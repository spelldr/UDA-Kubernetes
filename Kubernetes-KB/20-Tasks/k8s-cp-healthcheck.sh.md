```bash
#!/usr/bin/env bash
set -euo pipefail

echo "=== Kubernetes control-plane health check ==="

# 1. Swap status
echo "[1/6] Swap status..."
if swapon --show | grep -q .; then
    echo "  [FAIL] Swap is ENABLED. Disable with: swapoff -a and fix /etc/fstab."
else
    echo "  [OK]   Swap is disabled."
fi

# 2. Core services: containerd + kubelet
echo "[2/6] Systemd services..."
for svc in containerd kubelet; do
    if systemctl is-active --quiet "$svc"; then
        echo "  [OK]   $svc is active."
    else
        echo "  [FAIL] $svc is NOT active."
        systemctl status "$svc" --no-pager | tail -n 10 || true
    fi
done

# 3. Static pods (etcd, apiserver, controller, scheduler)
echo "[3/6] Control-plane static pods (via crictl)..."
if ! command -v crictl >/dev/null 2>&1; then
    echo "  [WARN] crictl not found; skipping static pod check."
else
    sudo crictl ps | grep -E "etcd|kube-apiserver|kube-controller-manager|kube-scheduler" || {
        echo "  [FAIL] One or more control-plane static pods are not running."
        echo "         Use: crictl ps -a and crictl logs <id> to investigate."
    }
fi

# 4. Kubelet node registration (local hostname)
echo "[4/6] Kubelet node registration..."
NODE_NAME=$(hostname -s)
if command -v kubectl >/dev/null 2>&1; then
    if kubectl get node "$NODE_NAME" >/dev/null 2>&1; then
        echo "  [OK]   Node '$NODE_NAME' is registered and reachable."
    else
        echo "  [FAIL] Node '$NODE_NAME' not found via kubectl."
    fi
else
    echo "  [WARN] kubectl not found; skipping node registration check."
fi

# 5. API server basic responsiveness
echo "[5/6] API server responsiveness..."
if command -v kubectl >/dev/null 2>&1; then
    if kubectl get --raw=/healthz >/dev/null 2>&1; then
        echo "  [OK]   API server /healthz is OK."
    else:
        echo "  [FAIL] API server /healthz check failed."
    fi
else
    echo "  [WARN] kubectl not found; skipping API health check."
fi

# 6. Cluster-wide pod status (high-level)
echo "[6/6] Cluster-wide pod status..."
if command -v kubectl >/dev/null 2>&1; then
    kubectl get pods -A
else
    echo "  [WARN] kubectl not found; cannot list pods."
fi

echo "=== Health check complete ==="
```
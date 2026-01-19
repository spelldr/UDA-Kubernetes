---
author: Platform Team
last_verified: 2026-01-18
status: current
maintenance_cadence: event-driven
runbook_owner: Platform Team
---

# Runbook: Upgrade Kubernetes Cluster

_How to safely upgrade a Kubernetes cluster from one version to the next._

## Overview

This runbook walks you through upgrading a Kubernetes cluster with zero downtime. Applies to kubeadm-managed clusters or managed services with kubeadm tooling.

## Prerequisites

- [ ] Cluster is healthy: `kubectl cluster-info` works
- [ ] All Nodes are Ready: `kubectl get nodes` (no `NotReady`)
- [ ] Backup etcd completed
- [ ] Current Kubernetes version: `kubectl version --short`
- [ ] Target version planned (e.g., 1.26 → 1.27)
- [ ] Read release notes: [Release & Upgrade](../80-ReleaseUpgrade/README.md) for breaking changes

## Timeline

- **Estimated duration:** 30-60 minutes for small clusters
- **Best time:** Off-peak hours (early morning, weekends)
- **Estimated downtime:** 0 (rolling update)
- **Rollback time:** 5-10 minutes

## Step-by-Step

### Phase 1: Pre-Upgrade Checks (5 minutes)

```bash
# 1. Check cluster health
kubectl cluster-info
echo "Status: Should show control plane is running"

# 2. Check all Nodes Ready
kubectl get nodes
echo "Status: All should be Ready"

# 3. Check system Pods healthy
kubectl get pods -n kube-system
echo "Status: All should be Running"

# 4. Note current version
kubectl version --short
echo "Status: Write down BEFORE and AFTER versions"

# 5. Check for Pod disruption budgets
kubectl get pdb --all-namespaces
echo "Status: Plan around any strict PDBs"
```

### Phase 2: Backup (5 minutes)

**Critical: Always backup etcd before upgrading!**

```bash
# On a master Node, backup etcd
ETCD_POD=$(kubectl get pods -n kube-system -l component=etcd -o jsonpath='{.items[0].metadata.name}')

# Snapshot etcd (example for kubeadm)
sudo etcdctl --endpoints=127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot save /backup/etcd-backup-$(date +%Y%m%d-%H%M%S).db

echo "Status: etcd backed up to /backup/"
```

Or if using managed service:
```bash
# Managed Kubernetes (EKS, AKS, GKE) handles backup automatically
echo "Status: Managed service handles backup"
```

### Phase 3: Drain Control Plane Nodes (if multi-control-plane)

For HA clusters with multiple control planes:

```bash
# Get control plane nodes
kubectl get nodes --selector=node-role.kubernetes.io/control-plane

# For each control plane except last one:
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data

echo "Status: Workload Pods moved away from node"
```

### Phase 4: Upgrade Control Plane (15-20 minutes)

**On master Node:**

```bash
# 1. Get kubeadm
sudo apt-get update
sudo apt-get install -y kubeadm=1.27.0-00  # Target version
# OR for apt-mark (prevent auto-upgrade):
sudo apt-mark hold kubelet kubeproxy

# 2. Plan the upgrade
sudo kubeadm upgrade plan v1.27.0

# 3. Apply upgrade
sudo kubeadm upgrade apply v1.27.0

# 4. Watch for success
echo "Status: kubeadm upgrade apply completed"

# 5. Upgrade kubelet on master
sudo apt-get install -y kubelet=1.27.0-00 kubectl=1.27.0-00

# 6. Restart kubelet
sudo systemctl restart kubelet

# 7. Verify master is upgraded
kubectl version --short
echo "Status: Master control plane upgraded"
```

### Phase 5: Uncordon Control Plane (if drained)

```bash
# Bring node back into service
kubectl uncordon <node>

echo "Status: Control plane node back online"
```

### Phase 6: Upgrade Worker Nodes (20-30 minutes)

For each worker node:

```bash
# Get list of worker nodes
kubectl get nodes --selector='!node-role.kubernetes.io/control-plane'

# For EACH worker node:
for NODE in <node1> <node2> <node3>; do
  echo "Upgrading $NODE..."
  
  # 1. Drain (move workloads off)
  kubectl drain $NODE --ignore-daemonsets --delete-emptydir-data
  echo "Status: Workloads evicted from $NODE"
  
  # 2. SSH into node and upgrade
  ssh $NODE "
    sudo apt-get update
    sudo apt-get install -y kubelet=1.27.0-00 kubectl=1.27.0-00
    sudo systemctl restart kubelet
  "
  echo "Status: kubelet upgraded on $NODE"
  
  # 3. Bring back online
  kubectl uncordon $NODE
  echo "Status: $NODE back in service"
  
  # 4. Wait for Pods to reschedule
  sleep 30
  kubectl get pods -o wide | grep $NODE
  echo "Status: Workloads rescheduled"
done

echo "Status: All worker nodes upgraded"
```

### Phase 7: Post-Upgrade Verification (5 minutes)

```bash
# 1. Check all Nodes Ready
kubectl get nodes
echo "Status: All should be Ready"

# 2. Check version
kubectl version --short
echo "Status: Should show new version (1.27.0)"

# 3. Check system Pods
kubectl get pods -n kube-system
echo "Status: All system Pods should be Running"

# 4. Check workload Pods
kubectl get pods --all-namespaces
echo "Status: No unusual restarts or errors"

# 5. Check API
kubectl api-versions | head
echo "Status: API responding normally"
```

## Rollback Plan

**If something goes wrong:**

### Quick Rollback (Within Minutes)

If cluster is still partially healthy:

```bash
# 1. Stop kubectl changes
# Cancel any ongoing operations

# 2. Restore etcd backup (if available)
sudo etcdctl --endpoints=127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot restore /backup/etcd-backup-TIMESTAMP.db

# 3. Restart control plane
sudo systemctl restart kubelet

echo "Status: Cluster should revert to previous version"
```

### Full Rollback

If using infrastructure-as-code (Terraform, CloudFormation):

```bash
# Restore from infrastructure backup
terraform apply -auto-approve -var=k8s_version="1.26.0"

# Or via cloud console
# Rollback cluster to previous snapshot
```

## Monitoring During Upgrade

Watch real-time:

```bash
# Terminal 1: Watch nodes
watch kubectl get nodes

# Terminal 2: Watch pods
watch kubectl get pods --all-namespaces

# Terminal 3: Watch events
kubectl get events --all-namespaces --watch
```

Check metrics:

```bash
# Node metrics
kubectl top nodes

# Pod metrics
kubectl top pods --all-namespaces

# API server responsiveness
time kubectl get pods
# Should respond in <1 second
```

## Troubleshooting During Upgrade

### Control Plane Won't Start

```bash
# Check kubelet status
sudo systemctl status kubelet

# Check logs
sudo journalctl -u kubelet -n 50

# Check etcd status
kubectl get pods -n kube-system | grep etcd

# Manual restore if needed
sudo kubeadm reset -f
# Then manual restore from backup
```

### Node Won't Uncordon

```bash
# Check node status
kubectl describe node <node>

# If stuck in SchedulingDisabled:
kubectl uncordon <node>

# If pods won't schedule:
# Check resources: kubectl top nodes
# Check taints: kubectl describe node
```

### Pods Stuck in Pending After Upgrade

```bash
# Check why scheduling failed
kubectl describe pod <pod>

# Common fixes:
# 1. Insufficient resources
kubectl top nodes

# 2. Node affinity changed
kubectl get nodes --show-labels

# 3. PVC unmounted
kubectl get pvc --all-namespaces
```

## Success Criteria

Upgrade successful when:
- [ ] All Nodes show `Ready` status
- [ ] All system Pods (`kube-system` namespace) `Running`
- [ ] Workload Pods stable (no unusual restarts)
- [ ] API responsive (`kubectl get pods` < 1 second)
- [ ] Version matches target: `kubectl version --short`
- [ ] Breaking changes addressed (see release notes)
- [ ] Monitoring/alerts working
- [ ] Business applications functional

## Post-Upgrade Tasks

1. **Update documentation**
   - New cluster version in runbooks
   - Feature availability notes

2. **Update CI/CD**
   - Update base images if needed
   - Test against new API version

3. **Monitor for 24 hours**
   - Watch error rates
   - Monitor resource usage
   - Check for deprecation warnings

4. **Communication**
   - Notify teams of successful upgrade
   - Share release notes highlights

## Related Runbooks

- Runbook - Backup etcd (planned)
- Troubleshooting - Upgrade Issues (planned)

## See Also

- [Release & Upgrade](../80-ReleaseUpgrade/README.md) for version-specific breaking changes
- [Concept - Architecture Overview](../30-Concepts/Concept%20-%20Architecture%20Overview.md) for cluster understanding
- Release notes: https://kubernetes.io/releases/

---

**Last run:** [Date]
**Run by:** [Name]
**From version:** 1.26.0
**To version:** 1.27.0
**Status:** ✓ Success / ✗ Failed
**Notes:** [Any issues or deviations]

---
author: Platform Team
last_verified: 2026-01-18
status: current
symptom_keywords: [stuck, pending, crashloop, error, failing, won't start, not running]
severity: common
---

# Troubleshooting: Pod Won't Start

_Answer: How do I fix a Pod that won't start or keeps crashing?_

## Symptoms

You run `kubectl get pods` and see a Pod with one of these statuses:
- `Pending` — Stuck, not scheduled
- `CrashLoopBackOff` — Container keeps crashing and restarting
- `ImagePullBackOff` — Can't pull the container image
- `Error` — One-time failure
- `Waiting` — Not ready yet

## Quick Diagnosis

```bash
# Get Pod status
kubectl describe pod my-pod

# See the problem
kubectl logs my-pod
```

The output will usually tell you the issue.

## Root Causes & Fixes

### Cause 1: Pod Stuck in "Pending" (Not Scheduling)

**How to identify:**
```bash
kubectl describe pod my-pod
```

Look for: "0/1 nodes are available"

**Common reasons:**
- Insufficient CPU/memory on Nodes
- Node selector or affinity can't be satisfied
- Taints on Nodes

**Fix:**

Check Node resources:
```bash
kubectl top nodes
kubectl describe nodes
```

Look for:
- `MemoryPressure`
- `DiskPressure`
- `NotReady`

**Solution:**
1. Add more Nodes to the cluster
2. Reduce `resources.requests` in Pod spec
3. Delete unused Pods/Deployments

### Cause 2: "ImagePullBackOff" (Can't Pull Image)

**How to identify:**
```bash
kubectl describe pod my-pod
```

Look for: "Failed to pull image" or "ImagePullBackOff"

**Common reasons:**
- Image doesn't exist
- Wrong image name or tag
- No access to private registry
- Typo in image name

**Fix:**

Check the image name:
```bash
kubectl get pod my-pod -o yaml | grep image:
```

Verify the image exists and is spelled correctly:
```bash
# Pull manually to test
docker pull nginx:1.24

# Or check registry
# (docker pull myregistry.azurecr.io/myapp:1.0)
```

**Solution:**
1. Fix the image name in Deployment/Pod
2. For private registries, add imagePullSecrets
3. Reapply: `kubectl apply -f deployment.yaml`

### Cause 3: "CrashLoopBackOff" (Container Keeps Crashing)

**How to identify:**
```bash
kubectl describe pod my-pod
```

Look for: "CrashLoopBackOff" or "Container exited with code X"

**Common reasons:**
- Application errors (process exits immediately)
- Missing configuration or files
- Port already in use
- Out of memory
- Permission denied

**Fix:**

Check logs:
```bash
kubectl logs my-pod
kubectl logs my-pod --previous  # Previous crash
```

Read the output carefully. The error message usually explains what's wrong.

**Examples:**

```
# Port already in use
"error: listen tcp :8080: bind: address already in use"
→ Change containerPort or kill process

# File not found
"FileNotFoundError: /app/config.yaml"
→ Mount the file with a Volume or ConfigMap

# Application error
"NameError: name 'db' is not defined"
→ Missing environment variable or startup order issue

# Permission denied
"Permission denied: /app/data"
→ Check file ownership or use securityContext
```

**Solution:**
1. Read logs carefully
2. Fix the application issue
3. Redeploy

### Cause 4: "Error" Status (One-Time Failure)

**How to identify:**
```bash
kubectl describe pod my-pod
```

Look for: "Error" status with an event

**Common reasons:**
- Resource exhausted
- Node evicted the Pod
- Kernel out of memory

**Fix:**

Check events:
```bash
kubectl describe pod my-pod
```

Look at bottom for recent events and error messages.

### Cause 5: "Waiting" (Taking Too Long to Start)

**How to identify:**

Pod shows `0/1` in READY status.

**Common reasons:**
- Large image (downloading)
- Slow startup application
- Health check failing

**Fix:**

Wait longer, then check:
```bash
kubectl get pods --watch
```

If still waiting after 5 minutes:
```bash
kubectl logs my-pod
```

If logs empty, image might still be pulling:
```bash
kubectl describe pod my-pod
# Look for "Pulling image" or "Downloaded image"
```

## Diagnosis Flowchart

```
Pod not running?
│
├─ Status = Pending?
│  └─ Check: kubectl describe pod
│     └─ "nodes are available"?
│        └─ → Cause 1 (No resources)
│
├─ Status = ImagePullBackOff?
│  └─ → Cause 2 (Image doesn't exist)
│
├─ Status = CrashLoopBackOff?
│  └─ Check: kubectl logs
│     └─ → Cause 3 (App is crashing)
│
├─ Status = Error?
│  └─ → Cause 4 (One-time failure)
│
└─ Status = Waiting?
   └─ → Cause 5 (Slow startup)
```

## Debugging Commands

```bash
# Full Pod description (includes events)
kubectl describe pod my-pod

# Logs from container
kubectl logs my-pod

# Logs from previous crashed container
kubectl logs my-pod --previous

# Logs with timestamps
kubectl logs my-pod --timestamps=true

# Get Pod YAML to verify configuration
kubectl get pod my-pod -o yaml

# Watch Pod for status changes
kubectl get pods --watch

# Check Node status
kubectl get nodes

# Check Node details
kubectl describe node node-name

# Check cluster events
kubectl get events --all-namespaces
```

## Prevention

To avoid these issues:

### Resource Requests
```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "100m"
```

Helps scheduler place Pod correctly.

### Health Checks
```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 80
  initialDelaySeconds: 10
```

Detects crashes early.

### Image Pull Secrets
```yaml
imagePullSecrets:
- name: registry-secret
```

For private registries.

### Init Containers
```yaml
initContainers:
- name: setup
  image: setup-tool
  # Runs before main container
```

For pre-flight setup.

## Quick Test

Deploy a simple working Pod:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
spec:
  containers:
  - name: test
    image: nginx:latest
```

If this works, the cluster is fine. Adjust your Pod gradually.

## Related Pages

- [[Concept - Pods]] to understand Pod lifecycle
- [[Reference - Pod API]] for all Pod fields
- [[Task - Deploy Your First Application]] for working example
- [[Troubleshooting - Service Can't Reach Pod]] if Pod won't respond

## See Also

- Official Kubernetes troubleshooting: https://kubernetes.io/docs/tasks/debug/debug-pod-deployment/
- Check [[50-Troubleshooting]] for other issues

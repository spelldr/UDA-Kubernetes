---
author: Platform Team
last_verified: 2026-01-18
status: current
prerequisites: ["kubectl installed and configured", "Access to a Kubernetes cluster"]
difficulty: Beginner
estimated_time: 10 minutes
---

# Task: Deploy Your First Application

_Answer: How do I run a containerized application in Kubernetes?_

## Quick Summary

You'll create a Deployment that runs nginx (a web server) with 3 replicas, then verify it's running. This is the "Hello World" of Kubernetes.

## Before You Start

- [ ] `kubectl` is installed (`kubectl version --client`)
- [ ] You have cluster access (`kubectl cluster-info`)
- [ ] You have a text editor (VS Code, nano, etc.)

## Step 1: Create a Deployment Manifest

Create a file named `nginx-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
```

**What this means:**
- `kind: Deployment` — This is a Deployment
- `replicas: 3` — Run 3 copies of nginx
- `image: nginx:1.24` — Use nginx version 1.24
- `containerPort: 80` — Container listens on port 80

## Step 2: Deploy It

```bash
kubectl apply -f nginx-deployment.yaml
```

**Expected output:**
```
deployment.apps/nginx-app created
```

## Step 3: Verify It's Running

```bash
kubectl get deployments
```

**Expected output:**
```
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
nginx-app   3/3     3            3           10s
```

All three replicas should be ready and available.

## Step 4: Check the Pods

```bash
kubectl get pods
```

**Expected output:**
```
NAME                        READY   STATUS    RESTARTS   AGE
nginx-app-d8545b4f-2pqwx   1/1     Running   0          15s
nginx-app-d8545b4f-4lx9v   1/1     Running   0          15s
nginx-app-d8545b4f-jk8hm   1/1     Running   0          15s
```

You should see 3 Pods all in `Running` status.

## Step 5: Verify One Pod

Get detailed information about one Pod:

```bash
kubectl describe pod nginx-app-d8545b4f-2pqwx
```

**Look for:**
- `Status: Running`
- `Image: nginx:1.24`
- No errors in Events section

## Step 6: View Logs

See what nginx is outputting:

```bash
kubectl logs nginx-app-d8545b4f-2pqwx
```

You should see nginx startup messages.

## Success

Your Deployment is running! You now have:
- ✓ 3 nginx containers running
- ✓ Each in its own Pod
- ✓ Automatically monitored (if one crashes, a new one starts)

## Next: Expose It

Right now, nginx is running but not accessible from outside the cluster. To access it, create a Service:

[Task - Expose an Application with Services](Task%20-%20Expose%20an%20Application%20with%20Services.md)

## Common Pitfalls

**Pitfall 1: "ImagePullBackOff" status**
- **Why it happens:** The image can't be found or pulled
- **Fix:** Check the image name and tag are correct: `kubectl describe pod [pod-name]` for details

**Pitfall 2: "Pending" status (stuck)**
- **Why it happens:** Pod can't be scheduled (usually insufficient resources)
- **Fix:** Check Node resources: `kubectl describe nodes`

**Pitfall 3: "CrashLoopBackOff"**
- **Why it happens:** Container keeps crashing
- **Fix:** Check logs: `kubectl logs [pod-name]`

## Cleanup

When you're done, delete the Deployment:

```bash
kubectl delete deployment nginx-app
```

This removes the Deployment and all its Pods.

## See Also

- [Concept - Deployments](../30-Concepts/Concept%20-%20Deployments.md) to understand what you just created
- [Concept - Pods](../30-Concepts/Concept%20-%20Pods.md) to understand the individual Pods
- [Task - Expose an Application with Services](Task%20-%20Expose%20an%20Application%20with%20Services.md) to make it accessible
- Reference - Deployment API (planned) for all Deployment options
- [Troubleshooting - Pod Won't Start](../50-Troubleshooting/Troubleshooting%20-%20Pod%20Won%27t%20Start.md) if things go wrong

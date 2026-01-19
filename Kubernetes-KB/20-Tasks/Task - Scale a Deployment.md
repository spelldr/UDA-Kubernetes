---
author: Platform Team
last_verified: 2026-01-18
status: current
prerequisites: ["Running Deployment", "kubectl installed"]
difficulty: Beginner
estimated_time: 3 minutes
---

# Task: Scale a Deployment

_Answer: How do I run more or fewer copies of my application?_

## Quick Summary

Scaling a Deployment means changing the number of replicas (copies running). Kubernetes automatically creates or deletes Pods to match your desired count.

## Before You Start

- [ ] You have a running Deployment ([Task - Deploy Your First Application](Task%20-%20Deploy%20Your%20First%20Application.md))
- [ ] kubectl installed

## Option 1: Edit the Manifest (Recommended)

### Step 1: Edit the File

Change this in your `deployment.yaml`:

```yaml
spec:
  replicas: 3    # ← Change this number
```

To:

```yaml
spec:
  replicas: 5    # ← New count
```

### Step 2: Apply the Change

```bash
kubectl apply -f deployment.yaml
```

### Step 3: Verify

```bash
kubectl get deployment my-app
```

**Expected output:**
```
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
my-app   5/5     5            5           2m
```

Now you have 5 replicas.

## Option 2: Scale via Command

Scale without editing the file:

```bash
kubectl scale deployment my-app --replicas=5
```

Or:

```bash
kubectl scale deployment/my-app --replicas=5
```

## Check Pods

List all Pods:

```bash
kubectl get pods
```

You should see 5 running Pods (or your new count).

Watch Pods being created in real-time:

```bash
kubectl get pods --watch
```

Press Ctrl+C to stop watching.

## Scale Down

Reduce replicas:

```bash
kubectl scale deployment my-app --replicas=2
```

Kubernetes immediately terminates 3 Pods (and their containers).

## Autoscaling (Advanced)

Kubernetes can automatically scale based on CPU/memory load. See Concept - Horizontal Pod Autoscaling for details.

## Common Pitfalls

**Pitfall 1: Scaled to 0 accidentally**
- **Effect:** All Pods deleted
- **Fix:** Scale back up: `kubectl scale deployment my-app --replicas=1`

**Pitfall 2: Scaling slowly**
- **Cause:** Pod startup takes time (image pull, initialization)
- **Check:** `kubectl get pods --watch` to monitor
- **Expected:** Usually 5-30 seconds per Pod

**Pitfall 3: Not enough Node resources**
- **Symptom:** New Pods stuck in Pending
- **Check:** `kubectl top nodes` to see capacity
- **Fix:** Add more Nodes or scale down

## See Scaling in Action

Deploy, then scale:

```bash
# Deploy 1 replica
kubectl apply -f deployment.yaml

# Wait a few seconds
sleep 5

# Scale to 5
kubectl scale deployment my-app --replicas=5

# Watch them create
kubectl get pods --watch
```

You'll see Pods go from Pending → Running over time.

## Next Steps

- [Concept - Deployments](../30-Concepts/Concept%20-%20Deployments.md) to understand replicas
- Concept - Horizontal Pod Autoscaling for automatic scaling
- Reference - Deployment API (planned) for all options

## See Also

- [Task - Deploy Your First Application](Task%20-%20Deploy%20Your%20First%20Application.md) (prerequisite)
- [Concept - Deployments](../30-Concepts/Concept%20-%20Deployments.md) for explanation
- [Troubleshooting - Pod Won't Start](../50-Troubleshooting/Troubleshooting%20-%20Pod%20Won%27t%20Start.md) if Pods don't start

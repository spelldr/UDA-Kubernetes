---
author: Platform Team
last_verified: 2026-01-18
status: current
prerequisites: ["kubectl installed and configured", "Understanding of Deployments"]
difficulty: Beginner
estimated_time: 5 minutes
---

# Task: Create a Deployment

_Answer: How do I create a Deployment with my own container image?_

```ad-question
What is a container image?
```

## Quick Summary

A Deployment describes desired state: "Run N replicas of this container with this configuration." You'll create a Deployment from scratch using a YAML manifest.

## Before You Start

- [ ] kubectl installed
- [ ] Access to Kubernetes cluster
- [ ] Know your container image name (e.g., `myapp:1.0` or `nginx:latest`)

## Step 1: Write the Deployment Manifest

Create `my-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: app
        image: nginx:1.24
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "64Mi"
            cpu: "100m"
          limits:
            memory: "128Mi"
            cpu: "200m"
```

**Key fields explained:**

| Field | Meaning |
|---|---|
| `name: my-app` | Deployment name (what you'll reference) |
| `replicas: 3` | Run 3 copies |
| `image: nginx:1.24` | Container image to use |
| `containerPort: 8080` | Port container listens on |
| `memory/cpu requests` | Minimum resources needed |
| `memory/cpu limits` | Maximum resources allowed |

## Step 2: Customize for Your App

Replace these values:

```yaml
metadata:
  name: my-app           # ← Your app name
spec:
  containers:
  - name: app
    image: myapp:1.0     # ← Your image name:tag
    ports:
    - containerPort: 8080 # ← Your app's port
```

## Step 3: Create the Deployment

```bash
kubectl apply -f my-deployment.yaml
```

**Expected output:**
```
deployment.apps/my-app created
```

## Step 4: Verify It

```bash
kubectl get deployment my-app
```

**Expected output:**
```
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
my-app   3/3     3            3           5s
```

All 3 replicas should be ready.

## Step 5: Check Pods

```bash
kubectl get pods -l app=my-app
```

**Expected output:**
```
NAME                      READY   STATUS    RESTARTS   AGE
my-app-5d4f7c6b8-abc12   1/1     Running   0          10s
my-app-5d4f7c6b8-def45   1/1     Running   0          10s
my-app-5d4f7c6b8-ghi78   1/1     Running   0          10s
```

All should be `Running`.

## Step 6: Verify One Pod in Detail

```bash
kubectl describe pod my-app-5d4f7c6b8-abc12
```

Look for:
- Status: Running
- Image: Your specified image
- No error events

## Update the Deployment

### Change the Image

```yaml
spec:
  template:
    spec:
      containers:
      - name: app
        image: myapp:2.0    # ← Changed version
```

Then:
```bash
kubectl apply -f my-deployment.yaml
```

Kubernetes automatically does a rolling update (one Pod at a time).

### Change Replica Count

```yaml
spec:
  replicas: 5    # ← Changed from 3
```

Then:
```bash
kubectl apply -f my-deployment.yaml
```

Kubernetes scales to 5 Pods.

## Common Pitfalls

**Pitfall 1: Image pull fails**
- **Symptom:** Status shows `ImagePullBackOff`
- **Cause:** Image doesn't exist or isn't accessible
- **Fix:** Verify image name: `kubectl describe pod [pod-name]`

**Pitfall 2: Pod stuck in Pending**
- **Cause:** Not enough resources to schedule Pod
- **Fix:** Check Node resources: `kubectl top nodes`

**Pitfall 3: Port conflicts**
- **Symptom:** Container won't start (`CrashLoopBackOff`)
- **Cause:** Multiple containers using same port
- **Fix:** Use different ports: `containerPort: 8080`, `containerPort: 8081`

**Pitfall 4: Resources too high**
- **Symptom:** Pod can't be scheduled
- **Cause:** Requested resources exceed Node capacity
- **Fix:** Lower `resources.requests`

## Next Steps

- [Task - Expose an Application with Services](Task%20-%20Expose%20an%20Application%20with%20Services.md) to access it from outside
- [Task - Scale a Deployment](Task%20-%20Scale%20a%20Deployment.md) to change replica count
- Reference - Deployment API (planned) to see all options
- [Concept - Deployments](../30-Concepts/Concept%20-%20Deployments.md) to understand how it works

## See Also

- [Task - Deploy Your First Application](Task%20-%20Deploy%20Your%20First%20Application.md) for basic example
- Reference - Deployment API (planned) for complete field reference
- [Troubleshooting - Pod Won't Start](../50-Troubleshooting/Troubleshooting%20-%20Pod%20Won%27t%20Start.md) if it fails

---
author: Platform Team
last_verified: 2026-01-18
status: current
audience: Beginner
---

# Concept: Deployments

_Answer: What is a Deployment and how does it manage Pods?_

## Definition

**A Deployment is a Kubernetes resource that describes the desired state for your application.** It specifies: "I want N replicas of this container image, with this configuration, and update them using this strategy."

Kubernetes automatically manages Pods to match your Deployment specification.

## Why It Matters

Deployments handle:
- **Creating Pods:** Based on your template
- **Scaling:** Increasing/decreasing replicas
- **Updates:** Rolling out new versions without downtime
- **Rollbacks:** Reverting to previous versions
- **Self-healing:** Restarting failed Pods

Without Deployments, you'd manage all this manually.

## Pod Template Inside Deployment

A Deployment contains a Pod template (what each Pod should look like):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3  # Run 3 copies
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: app
        image: myapp:1.24
        ports:
        - containerPort: 8080
```

This says: "Create 3 Pods, each running myapp:1.24 listening on port 8080."

## How Deployments Work

```
┌─────────────────────────────────────────┐
│       Deployment (desired state)        │
│   replicas: 3                           │
│   image: myapp:1.24                     │
└─────────────┬───────────────────────────┘
              │
   Kubernetes reconciliation loop:
   "Do we have 3 running Pods?"
              │
    ┌─────────┴────────────┐
    │                      │
   YES                    NO
    │                      │
  Done              Create/fix Pods
    │                      │
    └─────────────┬────────┘
                  │
         ┌────────▼──────────┐
         │  3 Pods running   │
         └───────────────────┘
```

## Replicas: Creating Multiple Copies

```yaml
spec:
  replicas: 3
```

This means: "Keep 3 Pods running at all times."

If one crashes:
1. Kubernetes detects it's gone
2. Creates a new Pod automatically
3. You're back to 3 running

## Updates: Rolling Out New Versions

Old way (manual):
1. Deploy new version
2. Stop old Pods one by one
3. Hope nothing breaks

Deployment way (automatic):
```yaml
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
```

This updates one Pod at a time:
- Run 4 Pods temporarily (3 + 1 new one)
- Stop one old Pod
- Repeat until all are updated
- Back to 3 Pods with new version

Zero downtime. Automatic. Reversible.

## Rollback: Reverting to Previous Version

If the new version has a bug:

```bash
kubectl rollout undo deployment my-app
```

Instantly goes back to the previous version. Kubernetes maintains history.

## Labels and Selectors

Deployments use labels to identify which Pods belong to them:

```yaml
spec:
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app  # Must match selector
```

This tells Kubernetes: "Manage all Pods labeled `app: my-app`"

## Deployment vs. Pod vs. ReplicaSet

These are related but different:

| Object | Purpose | Manages |
|---|---|---|
| **Pod** | Individual container(s) | Nothing (lowest level) |
| **ReplicaSet** | Keep N Pods running | Pods |
| **Deployment** | Declare desired state + updates | ReplicaSets (you don't manage directly) |

**In practice:** You create Deployments (not ReplicaSets, not Pods directly).

## Common Patterns

### Stateless Web Server
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-server
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
```

### Scaling Database Replicas
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 5  # Increased from 3
  template:
    spec:
      containers:
      - name: app
        image: myapp:1.24
```

### Updating to New Version
```yaml
# Change image version
spec:
  template:
    spec:
      containers:
      - name: app
        image: myapp:2.0  # Changed from 1.24
```

Kubernetes automatically does rolling update.

## Key Concepts

### **Desired State**
What you declare in the Deployment spec.

### **Actual State**
What's currently running (Pods, their state).

### **Reconciliation**
Kubernetes continuously makes actual state match desired state.

## Limitations & Alternatives

**Deployments are great for:**
- Stateless applications
- Web servers, APIs, microservices

**Use StatefulSet instead if:**
- You need stable Pod identities
- You need persistent storage
- Running databases, caches

**Use DaemonSet if:**
- You need one Pod on every Node
- System tools, monitoring agents

## Your Next Steps

- [Task - Create a Deployment](../20-Tasks/Task%20-%20Create%20a%20Deployment.md) (hands-on: create your first Deployment)
- [Task - Scale a Deployment](../20-Tasks/Task%20-%20Scale%20a%20Deployment.md) (learn scaling)
- Reference - Deployment API (planned) (see all fields)

## Related Concepts

- [Concept - Pods](Concept%20-%20Pods.md) — Individual containers
- [Concept - Services](Concept%20-%20Services.md) — How to expose Deployments
- Concept - ReplicaSets — Lower-level Pod management (usually automatic)

## See Also

- [Task - Deploy Your First Application](../20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md) for practical example
- Reference - Deployment API (planned) for complete field reference

---
author: Platform Team
last_verified: 2026-01-18
status: current
audience: Beginner
---

# Concept: Pods

_Answer: What is a Pod and why does Kubernetes use them?_

## Definition

**A Pod is the smallest deployable unit in Kubernetes.** It's a wrapper around one or more containers that run together on the same machine.

In most cases, one Pod = one container. But a Pod **can** contain multiple containers if they need to work closely together.

## Why It Matters

Kubernetes doesn't manage containers directly. Instead, it manages Pods, and Pods contain containers. This indirection lets Kubernetes provide:
- **Networking:** Containers in a Pod share a network interface (same IP address, localhost)
- **Storage:** Containers can share storage volumes
- **Lifecycle:** All containers in a Pod start/stop together

## Key Concept: Pod = Container + Networking

Think of a Pod like a "box" around your container(s):

```
┌─────────────── Pod ──────────────┐
│                                   │
│  ┌──────────────────────────┐   │
│  │    Container (nginx)      │   │
│  │  - Code                   │   │
│  │  - Runtime                │   │
│  └──────────────────────────┘   │
│                                   │
│  Networking (shared by all)      │
│  - IP address                     │
│  - Ports                          │
│  - Volume mounts                  │
│                                   │
└───────────────────────────────────┘
```

## Containers vs. Pods vs. Deployments

These are different concepts:

| Concept | What It Is | Example |
|---|---|---|
| **Container** | Packaged application code | `docker build` creates a container image |
| **Pod** | 1+ containers + networking | Kubernetes' smallest unit |
| **Deployment** | Desired state for many Pods | "Run 3 replicas of this Pod" |

**Flow:** You build a container image → Kubernetes runs it in a Pod → A Deployment manages multiple Pods.

## How Pods Work

### Pod Lifecycle

```
┌──────────┐
│ Pending  │  (Image pulling, scheduling)
└────┬─────┘
     │
┌────▼──────┐
│  Running  │  (Containers are running)
└────┬──────┘
     │
┌────▼─────────┐
│  Succeeded   │  (Containers exited successfully)
│  or Failed   │  (Or containers failed)
└──────────────┘
```

### Pod Status Details

- **Pending:** Kubernetes is pulling the image or scheduling the Pod to a Node
- **Running:** Pod is on a Node and at least one container is running
- **Succeeded:** All containers exited successfully (job Pods)
- **Failed:** At least one container exited with error
- **CrashLoopBackOff:** Container keeps crashing and restarting

## Single-Container vs. Multi-Container Pods

### Most Common: Single Container

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-server
spec:
  containers:
  - name: nginx
    image: nginx:1.24
```

This Pod contains just nginx.

### Advanced: Multiple Containers (Sidecar Pattern)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-with-logger
spec:
  containers:
  - name: app
    image: myapp:1.0
  - name: logger
    image: logging-agent:1.0
```

Both containers run in the same Pod:
- Share the same IP address
- Can communicate via localhost
- Share a network namespace

**When to use:** Only when containers must share networking (e.g., logging sidecar, service mesh proxy).

## Pod Networking

Containers in a Pod share:
- **IP address:** All containers get the same IP
- **Ports:** If both containers try to use port 80, there's a conflict
- **Localhost:** Containers can reach each other via `localhost:port`

Example:
```yaml
containers:
- name: app
  image: myapp:1.0
  ports:
  - containerPort: 8080
- name: sidecar
  image: sidecar:1.0
  # Can reach app via localhost:8080
```

## Pod Volumes

Pods can mount storage (Volumes):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-storage
spec:
  containers:
  - name: app
    image: myapp:1.0
    volumeMounts:
    - name: data
      mountPath: /app/data
  volumes:
  - name: data
    emptyDir: {}  # Ephemeral storage
```

## Ephemeral vs. Persistent

- **Ephemeral Pod:** Dies when deleted, data is lost. Use for stateless apps.
- **Persistent Pod:** Data survives the Pod. Use for databases, caches (via StatefulSet).

## Key Limitations

- **Pods are ephemeral:** They're created, run, then deleted
- **Don't create Pods directly:** Use Deployments, StatefulSets, Jobs instead
- **Pods are not reliable:** If a Node crashes, Pods on that Node are lost
- **Not for direct scaling:** Don't manually create many Pods; use Deployments

## Common Misconceptions

### Myth: A Pod is a container

**Reality:** A Pod wraps container(s). The difference matters because Pods add networking and lifecycle management.

### Myth: You should create Pods directly

**Reality:** Create Deployments instead (which manage Pods for you). Direct Pod creation is only for debugging.

### Myth: Pods always run one container

**Reality:** Most do, but multi-container Pods are valid for tightly-coupled workloads (sidecars).

## Your Next Steps

- [[Task - Deploy Your First Application]] (practical: run your first Pod)
- [[Concept - Deployments]] (understand how to manage multiple Pods)
- [[Reference - Pod API]] (see all Pod fields)

## Related Concepts

- [[Concept - Deployments]] — How to manage multiple Pods
- [[Concept - Services]] — How to expose Pods
- [[Concept - Namespaces]] — How Pods are partitioned
- [[Concept - Architecture Overview]] — How Pods fit in the system

## See Also

- [[Task - Deploy Your First Application]] to try it
- [[Troubleshooting - Pod Won't Start]] if something goes wrong
- [[Reference - Pod API]] for complete field reference

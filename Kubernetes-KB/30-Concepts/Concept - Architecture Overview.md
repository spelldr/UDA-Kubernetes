---
author: Platform Team
last_verified: 2026-01-18
status: current
audience: Intermediate
---

# Concept: Architecture Overview

_Answer: How does Kubernetes work internally and what are its main components?_

## High-Level Architecture

```
┌──────────────────────────────────────────────────────────┐
│                    KUBERNETES CLUSTER                     │
├──────────────────────────────────────────────────────────┤
│                                                           │
│  ┌─────────────────────────────────────────────────┐    │
│  │         CONTROL PLANE (Master Nodes)             │    │
│  │                                                  │    │
│  │  ┌──────────────┐  ┌──────────────────┐         │    │
│  │  │  API Server  │  │ Controller       │         │    │
│  │  │ - REST API   │  │ Manager          │         │    │
│  │  │ - Validation │  │ - Reconciliation │         │    │
│  │  └──────────────┘  │ - Scaling        │         │    │
│  │                    └──────────────────┘         │    │
│  │                                                  │    │
│  │  ┌──────────────┐  ┌──────────────────┐         │    │
│  │  │  Scheduler   │  │     etcd          │         │    │
│  │  │ - Pod        │  │  (Cluster State) │         │    │
│  │  │   placement  │  │  (Database)      │         │    │
│  │  └──────────────┘  └──────────────────┘         │    │
│  │                                                  │    │
│  └──────────────────────────────────────────────────┘    │
│                         │                                │
│           ┌─────────────┴──────────────┐                │
│           ↓                            ↓                │
│  ┌──────────────────┐      ┌──────────────────┐        │
│  │  WORKER NODE 1   │      │  WORKER NODE 2   │        │
│  │                  │      │                  │        │
│  │ ┌──────────────┐ │      │ ┌──────────────┐ │        │
│  │ │ kubelet      │ │      │ │ kubelet      │ │        │
│  │ │ (Node agent) │ │      │ │ (Node agent) │ │        │
│  │ └──────────────┘ │      │ └──────────────┘ │        │
│  │                  │      │                  │        │
│  │ ┌──┐ ┌──┐ ┌──┐  │      │ ┌──┐ ┌──┐       │        │
│  │ │P1│ │P2│ │P3│  │      │ │P4│ │P5│       │        │
│  │ └──┘ └──┘ └──┘  │      │ └──┘ └──┘       │        │
│  │ (Pods running)  │      │ (Pods running)  │        │
│  │                  │      │                  │        │
│  └──────────────────┘      └──────────────────┘        │
│                                                           │
└──────────────────────────────────────────────────────────┘
```

## Key Components

### Control Plane

The "brain" of Kubernetes. Makes all decisions about the cluster.

#### **API Server**
- Entry point for all Kubernetes operations
- Validates requests
- Stores state in etcd
- All other components talk to it

#### **Controller Manager**
- Runs multiple controllers that watch cluster state
- Makes things happen: "Is actual state = desired state? If not, fix it."
- **Deployment Controller:** Ensures correct number of Pods
- **Replication Controller:** Keeps replicas healthy
- **Node Controller:** Monitors Node health

#### **Scheduler**
- Assigns Pods to Nodes
- Considers: resource requirements, affinity, taints, etc.
- Goal: pack Pods efficiently across available Nodes

#### **etcd**
- Distributed key-value store
- Kubernetes' database
- Stores ALL cluster state: Deployments, Pods, Services, etc.
- Must be backed up!
- Single source of truth

### Worker Nodes

Machines that run your Pods.

#### **kubelet**
- Node agent (one per Node)
- Watches API server: "What Pods should be on this Node?"
- Manages Pod lifecycle: creates, starts, monitors, stops
- Reports Node status back to control plane
- Communicates with container runtime (Docker, containerd)

#### **Container Runtime**
- Software that actually runs containers
- Docker, containerd, CRI-O, etc.
- kubelet tells it what to run

#### **kube-proxy**
- Network proxy (one per Node)
- Implements Services
- Handles network routing to Pods
- Manages iptables or IPVS rules

### Add-Ons (Optional)

- **DNS (CoreDNS):** Service discovery
- **Ingress Controller:** External routing to Services
- **Monitoring:** Prometheus, etc.
- **Logging:** Centralized logs

## Communication Flow

### Deploying an Application

```
1. User creates Deployment manifest
   kubectl apply -f deployment.yaml
            ↓
2. API Server validates and stores in etcd
            ↓
3. Controller Manager sees new Deployment
   "I need to create 3 Pods"
            ↓
4. API Server stores 3 Pod objects
            ↓
5. Scheduler sees unscheduled Pods
   "Assign these to Nodes based on resources"
            ↓
6. API Server stores Node assignments
            ↓
7. kubelet on each Node watches API Server
   "I have Pods to run!"
            ↓
8. kubelet talks to container runtime
   "Run this container with this image"
            ↓
9. Container runtime starts container
            ↓
10. Pods are running!
            ↓
11. Controller Manager continuously checks
    "Are we still at desired state?"
    (If a Pod crashes, create a new one)
```

### Day-2 Monitoring (Reconciliation Loop)

```
Controller Manager runs continuously:

Check desired state (from API Server)
              ↓
Check actual state (from kubelet reports)
              ↓
Are they equal?
     ↙     ↘
   YES     NO
    ↓       ↓
  Sleep   Fix something
            (create Pod, delete Pod, etc.)
    ↑       ↓
    └───────┘
  (Repeat forever)
```

This is **reconciliation**: continuous alignment of actual to desired state.

## Request Flow: Service Access

```
External Client
      ↓
Load Balancer (cloud)
      ↓
Service API object
(has list of Pods via Endpoints)
      ↓
kube-proxy routes to Pod
(using iptables/IPVS rules)
      ↓
Pod (container running)
```

## Persistence: Where Data Lives

| What | Where | Persistent? |
|---|---|---|
| **Cluster configuration** | etcd | Yes (must backup) |
| **Pod data** | Container filesystem | No (lost if Pod dies) |
| **Shared data** | Volumes (PersistentVolume) | Yes (survives Pod restart) |
| **Logs** | Node filesystem (kubelet) | No (lost if Node dies) |

## Multi-Node Cluster vs. Single-Node

### Single-Node (Development)
```
┌─────────────────────────┐
│  Control Plane + Node   │
│  (All in one)           │
│                         │
│  API, Scheduler,        │
│  Controller, etcd       │
│  ← AND →                │
│  kubelet, Pods          │
└─────────────────────────┘
```

Example: Docker Desktop, Minikube

### Multi-Node (Production)
```
┌─────────────────────┐
│   Control Plane     │
│ (1-3 Master Nodes)  │
│ HA setup            │
└──────────┬──────────┘
      │
┌─────┴──────┬─────────┐
↓            ↓         ↓
Node 1      Node 2    Node 3
(Worker)    (Worker)  (Worker)
Pods        Pods      Pods
```

Control plane is separate from worker nodes.
Scale: hundreds of nodes, thousands of Pods.

## Kubernetes Cluster Topology

```
Region
  ├─ Availability Zone 1
  │   └─ Master Nodes (3 for HA)
  │       - API Server (3 instances, load balanced)
  │       - etcd (3 instances, quorum)
  │       - Scheduler (1 active, 2 standby)
  │       - Controller Manager (1 active, 2 standby)
  │
  ├─ Availability Zone 2
  │   └─ Worker Nodes (1-100s)
  │       - Workload Pods
  │       - System Pods (DNS, monitoring)
  │
  └─ Availability Zone 3
      └─ Worker Nodes (1-100s)
          - Workload Pods
```

## How Self-Healing Works

```
Pod crashes
    ↓
kubelet detects death
    ↓
Notifies API Server
    ↓
Controller Manager sees mismatch
(desired: 3 Pods, actual: 2 Pods)
    ↓
Controller Manager creates new Pod
    ↓
API Server stores it
    ↓
Scheduler assigns to Node
    ↓
kubelet creates the Pod
    ↓
Back to desired state
```

Automatic. No manual intervention needed.

## Key Architectural Principles

1. **Desired State:** You declare what you want, Kubernetes makes it happen
2. **Reconciliation:** Continuous loop making actual = desired
3. **Declarative:** YAML files describe what, not how
4. **Distributed:** Components can run on different machines
5. **Resilient:** Failure of one component doesn't crash others
6. **Observable:** Every action is observable, auditable

## Your Next Steps

- [Concept - What is Kubernetes](Concept%20-%20What%20is%20Kubernetes.md) (if you haven't read it)
- [Concept - Pods](Concept%20-%20Pods.md), [Concept - Deployments](Concept%20-%20Deployments.md), [Concept - Services](Concept%20-%20Services.md) (understand key objects)
- [Task - Deploy Your First Application](../20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md) (see it in action)

## Related Concepts

- [Concept - Pods](Concept%20-%20Pods.md) — Smallest unit
- [Concept - Deployments](Concept%20-%20Deployments.md) — Pod management
- [Concept - Services](Concept%20-%20Services.md) — Pod exposure
- [99-Glossary](../99-Glossary/Glossary.md) — etcd, kubelet, API Server, etc.

## See Also

- Kubernetes official docs: Components
- [Reference - Pod API](../40-Reference/Reference%20-%20Pod%20API.md) for object details
- Reference - Deployment API (planned) for object details

---
author: Platform Team
last_verified: 2026-01-18
status: current
audience: Beginner
---

# Concept: Services

_Answer: What is a Service and why do you need it?_

## Definition

**A Service is a stable, permanent way to expose Pods.** It provides:
- **Stable DNS name:** Instead of connecting to individual Pod IP addresses (which change), you connect to a Service
- **Load balancing:** Automatically distributes traffic across multiple Pods
- **Port mapping:** Maps external traffic to internal container ports

## Why It Matters

Pods are ephemeral. Their IP addresses change constantly (old Pods die, new Pods are created). You need a stable way to access them. Services solve this.

**Without Service:**
```
Client → Pod IP address 10.0.0.5
         (Pod dies)
         Pod is replaced with IP 10.0.0.8
         Connection breaks!
```

**With Service:**
```
Client → Service (stable)
         ↓
         Service routes to Pod 10.0.0.5
         (Pod dies)
         Service routes to new Pod 10.0.0.8
         Connection continues!
```

## Service Types

### 1. **ClusterIP** (Default)
Accessible only within the cluster.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  type: ClusterIP
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: my-app
```

- Other Pods can reach it via DNS: `my-app`
- Not accessible from outside the cluster

### 2. **NodePort**
Accessible from outside via any Node's IP.

```yaml
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 30000  # Fixed port on Node
  selector:
    app: my-app
```

- Access via `<Node-IP>:30000` from outside
- Fixed port (30000-32767 range)

### 3. **LoadBalancer**
Exposes via a cloud load balancer (requires cloud provider).

```yaml
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: my-app
```

- Cloud provider creates a load balancer
- Public IP address provided
- Best for exposing to internet

### 4. **ExternalName**
Maps to external DNS name.

```yaml
spec:
  type: ExternalName
  externalName: external-api.example.com
```

- Used rarely, for routing to external services

## How Services Work

### Service Discovery via Selector

The Service uses a **selector** to find which Pods to route to:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app  # Find all Pods with this label
  ports:
  - port: 80
    targetPort: 8080
```

Pods need matching labels:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  template:
    metadata:
      labels:
        app: my-app  # Must match Service selector
    spec:
      containers:
      - name: app
        image: myapp:1.24
        ports:
        - containerPort: 8080
```

### Traffic Flow

```
External Client
       ↓
    Service (port 80)
       ↓
  Load Balancer
       ↓
   ┌─────┬─────┬─────┐
   ↓     ↓     ↓
  Pod   Pod   Pod   (targetPort 8080)
  10.0  10.1  10.2
```

Client connects to Service:port. Service routes to a Pod:targetPort using round-robin load balancing.

## DNS Names

Services get automatic DNS names:

```
Service name: my-service
Namespace: default

DNS: my-service.default.svc.cluster.local
     (or just: my-service within same namespace)
```

Other Pods can reach it:

```bash
# From same namespace
curl http://my-service

# From different namespace
curl http://my-service.default.svc.cluster.local
```

## Service Endpoints

Behind the scenes, a Service creates an **Endpoints** object listing which Pods it's routing to:

```bash
kubectl get endpoints my-service
# Shows IPs and ports of selected Pods
```

When Pods are added/removed, Endpoints update automatically.

## Choosing a Service Type

```
You want to access from:

├─ Other Pods in cluster?
│  └─> Use ClusterIP (default)
│
├─ Outside cluster, any Node?
│  └─> Use NodePort
│
├─ Internet via cloud provider?
│  └─> Use LoadBalancer
│
└─ External system/API?
   └─> Use ExternalName
```

## Common Example: Exposing Web App

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: web-app-service
spec:
  type: LoadBalancer
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 80
```

This exposes 3 Pods running nginx via a load balancer. Traffic is automatically distributed.

## Key Concepts

### **Selector**
Labels used to identify which Pods belong to this Service.

### **Port**
Port the Service listens on (external).

### **TargetPort**
Port on the Pod (internal).

### **EndPoints**
List of Pod IPs and ports the Service routes to. Updated automatically.

### **Load Balancing**
Distributes requests across multiple Pods. Default: round-robin.

## Limitations

- **No stickiness by default:** Requests from same client may go to different Pods
- **No session persistence:** Unless you configure it
- **May lose connection on Pod replacement:** Brief moment during redeployment

## Your Next Steps

- [Task - Expose an Application with Services](../20-Tasks/Task%20-%20Expose%20an%20Application%20with%20Services.md) (hands-on: create your first Service)
- Reference - Service API (planned) (see all Service fields)
- [Concept - Deployments](Concept%20-%20Deployments.md) (need this to understand complete example)

## Related Concepts

- [Concept - Pods](Concept%20-%20Pods.md) — What Services route to
- [Concept - Deployments](Concept%20-%20Deployments.md) — Creates Pods that Services expose
- Concept - Namespaces — Affect DNS names

## See Also

- [Task - Expose an Application with Services](../20-Tasks/Task%20-%20Expose%20an%20Application%20with%20Services.md) for step-by-step
- Reference - Service API (planned) for complete reference
- [Troubleshooting - Service Can't Reach Pod](../50-Troubleshooting/Troubleshooting%20-%20Service%20Can%27t%20Reach%20Pod.md) if something goes wrong

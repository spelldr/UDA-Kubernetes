---
author: Platform Team
last_verified: 2026-01-18
status: current
prerequisites: ["Running Deployment", "kubectl installed"]
difficulty: Beginner
estimated_time: 5 minutes
---

# Task: Expose an Application with Services

_Answer: How do I make my Pods accessible from outside the cluster?_

## Quick Summary

A Service exposes Pods with a stable IP/DNS name. This task creates a Service that routes traffic to your Deployment's Pods.

## Before You Start

- [ ] You have a running Deployment ([[Task - Deploy Your First Application]])
- [ ] kubectl installed

## Step 1: Create a Service Manifest

Create `nginx-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
```

**What this means:**
- `type: LoadBalancer` — Expose via load balancer (cloud provider)
- `selector: app: nginx` — Route to Pods with label `app: nginx`
- `port: 80` — Service listens on port 80
- `targetPort: 80` — Routes to Pod port 80

## Step 2: Deploy the Service

```bash
kubectl apply -f nginx-service.yaml
```

**Expected output:**
```
service/nginx-service created
```

## Step 3: Check the Service

```bash
kubectl get service nginx-service
```

**Expected output:**
```
NAME            TYPE          CLUSTER-IP      EXTERNAL-IP    PORT(S)   AGE
nginx-service   LoadBalancer  10.0.0.100      pending...     80/TCP    5s
```

Wait for `EXTERNAL-IP` to appear (might take 1-2 minutes on cloud providers).

## Step 4: Get the External IP

```bash
kubectl get service nginx-service --watch
```

When EXTERNAL-IP is ready, press Ctrl+C. It will look like:

```
NAME            TYPE          CLUSTER-IP      EXTERNAL-IP        PORT(S)   AGE
nginx-service   LoadBalancer  10.0.0.100      203.0.113.50      80/TCP    30s
```

## Step 5: Test Access

Using the external IP from above:

```bash
curl http://203.0.113.50
```

You should see nginx welcome page HTML.

## Service Types

### Option 1: LoadBalancer (Recommended for Internet Access)

```yaml
spec:
  type: LoadBalancer
```

- Cloud provider creates load balancer
- Public IP
- Good for public apps

### Option 2: NodePort (No Load Balancer)

```yaml
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30000
```

Access via `<Node-IP>:30000`:

```bash
# Find a Node IP
kubectl get nodes -o wide

# Then access via
curl http://192.168.1.100:30000
```

### Option 3: ClusterIP (Internal Only)

```yaml
spec:
  type: ClusterIP
```

Accessible only from within cluster. Other Pods can reach via DNS:

```bash
curl http://nginx-service
```

## How the Service Routes Traffic

```
External Client → Service (IP: 10.0.0.100, port: 80)
                     ↓
         Find all Pods with label app: nginx
                     ↓
      ┌────────────────┬────────────────┬────────────────┐
      ↓                ↓                ↓
   Pod A            Pod B            Pod C
 (10.0.1.10)      (10.0.1.11)      (10.0.1.12)
   port 80          port 80          port 80
      ↓                ↓                ↓
   nginx            nginx            nginx
```

Load balancer distributes requests across all Pods.

## Verify the Service is Routing Correctly

See which Pods the Service is routing to:

```bash
kubectl get endpoints nginx-service
```

**Expected output:**
```
NAME            ENDPOINTS                                 AGE
nginx-service   10.0.1.10:80,10.0.1.11:80,10.0.1.12:80  5s
```

Shows the 3 Pod IPs and port.

## Common Pitfalls

**Pitfall 1: EXTERNAL-IP stuck on "pending"**
- **Cause:** No load balancer available (minikube, kind, etc.)
- **Fix:** Use `type: NodePort` instead, or use minikube tunnel

**Pitfall 2: "Connection refused"**
- **Cause:** Pod isn't listening on the port
- **Check:** `kubectl logs [pod-name]` to see if app started
- **Check:** Port in Service matches Pod's `containerPort`

**Pitfall 3: Service can't find Pods**
- **Cause:** Labels don't match between Service selector and Pod
- **Check:** Service selector: `kubectl get service -o yaml nginx-service`
- **Check:** Pod labels: `kubectl get pods --show-labels`
- **Fix:** Make sure they match exactly

## Update the Service

Change the manifest and reapply:

```bash
kubectl apply -f nginx-service.yaml
```

Kubernetes updates the Service automatically.

## Delete the Service

```bash
kubectl delete service nginx-service
```

## Next Steps

- [[Task - Scale a Deployment]] to handle more traffic
- [[Concept - Services]] to understand routing
- [[Reference - Service API]] for all options
- [[Troubleshooting - Service Can't Reach Pod]] if it doesn't work

## See Also

- [[Task - Deploy Your First Application]] (prerequisite)
- [[Concept - Services]] for explanation
- [[Reference - Service API]] for complete reference
- [[Troubleshooting - Service Can't Reach Pod]] if connection fails

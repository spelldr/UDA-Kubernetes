---
author: Platform Team
last_verified: 2026-01-18
status: current
symptom_keywords: [service, unreachable, connection refused, timeout, no route]
severity: common
---

# Troubleshooting: Service Can't Reach Pod

_Answer: Why can't my Service reach my Pods?_

## Symptoms

- Service exists but returns "Connection refused"
- External client can reach Service but gets no response
- Service returns 503 or similar error
- Service shows endpoints but they're not responding

## Quick Diagnosis

```bash
# Check if Service has endpoints
kubectl get endpoints my-service

# Check if endpoints are actually running
kubectl get pods

# Test Service from inside a Pod
kubectl run -it debug --image=nginx -- bash
# Inside: curl http://my-service
```

## Root Causes & Fixes

### Cause 1: Service Selector Doesn't Match Pod Labels

**How to identify:**

```bash
kubectl get service my-service -o yaml
# Look at: spec.selector
```

```bash
kubectl get pods --show-labels
# Check if any Pod has matching labels
```

```bash
kubectl get endpoints my-service
# If empty, selector doesn't match
```

**Example:**

Service selector:
```yaml
selector:
  app: web
```

Pod labels:
```yaml
labels:
  app: webapp  # ← Doesn't match "web"
```

**Fix:**

Make labels match exactly:

```yaml
# Service
spec:
  selector:
    app: web

# Pod
metadata:
  labels:
    app: web  # ← Same
```

Then:
```bash
kubectl apply -f service.yaml
kubectl apply -f deployment.yaml
```

### Cause 2: Pod Port Doesn't Match Service TargetPort

**How to identify:**

```bash
kubectl get service my-service -o yaml
# Look at: spec.ports[].targetPort
```

```bash
kubectl get pod my-pod -o yaml
# Look at: spec.containers[].ports[].containerPort
```

**Example:**

Service targetPort:
```yaml
ports:
- port: 80
  targetPort: 8080  # Routes to Pod port 8080
```

Pod container port:
```yaml
ports:
- containerPort: 80  # ← Pod is listening on 80, not 8080
```

**Fix:**

Make ports match:

```yaml
# Service targets port 8080
spec:
  ports:
  - port: 80
    targetPort: 8080

# Pod listens on 8080
spec:
  containers:
  - name: app
    containerPort: 8080
```

### Cause 3: Pod Application Not Listening on Correct Port

**How to identify:**

```bash
# Get logs from Pod
kubectl logs my-pod

# Check if app started
# Look for "listening on port X" or similar
```

```bash
# Connect to Pod directly
kubectl port-forward my-pod 8080:8080
# From another terminal:
curl localhost:8080
```

**Example error in logs:**
```
error: Could not bind to port 8080
address already in use
```

**Fix:**

1. Verify app is actually listening:
   ```bash
   kubectl exec -it my-pod -- netstat -tlnp
   ```

2. Check app logs for errors:
   ```bash
   kubectl logs my-pod
   ```

3. Ensure app is configured for right port:
   - Check environment variables
   - Check config files
   - Check default port if not configured

### Cause 4: Pod Not Actually Running

**How to identify:**

```bash
kubectl get pods
# Check if Pod status is "Running"

kubectl describe pod my-pod
# Look for error events
```

**Fix:**

See [[Troubleshooting - Pod Won't Start]]

### Cause 5: Network Policy Blocking Traffic

**How to identify:**

```bash
# Check if NetworkPolicy exists
kubectl get networkpolicy
kubectl describe networkpolicy
```

**Fix:**

If NetworkPolicy blocks traffic:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web
spec:
  podSelector:
    matchLabels:
      app: web
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector: {}  # Allow from any Pod
```

Or temporarily disable to test:
```bash
kubectl delete networkpolicy --all
```

### Cause 6: Endpoints List is Empty

**How to identify:**

```bash
kubectl get endpoints my-service
# Output shows no endpoints
```

**This means:** Service selector isn't finding any Pods

**Fix:**

Check selector matches:
```bash
kubectl get service my-service -o yaml | grep -A 5 "selector:"
kubectl get pods --show-labels | grep -E "LABELS|matching-label"
```

Ensure they match exactly.

## Diagnosis Flowchart

```
Service can't reach Pods?
│
├─ Do Pods exist and are Running?
│  └─ kubectl get pods
│     └─ No → [[Troubleshooting - Pod Won't Start]]
│     └─ Yes → Continue
│
├─ Does Service have Endpoints?
│  └─ kubectl get endpoints my-service
│     └─ Empty → → Cause 1 (Labels don't match)
│     └─ Has Pods → Continue
│
├─ Do Service ports match Pod ports?
│  └─ kubectl get service -o yaml | grep targetPort
│     └─ kubectl get pod -o yaml | grep containerPort
│        └─ Don't match → → Cause 2 (Port mismatch)
│        └─ Match → Continue
│
├─ Is Pod app actually listening?
│  └─ kubectl logs my-pod
│     └─ No "listening" message → → Cause 3 (App not started)
│     └─ App listening → Continue
│
└─ Check NetworkPolicy
   └─ kubectl get networkpolicy
      └─ Blocks traffic → → Cause 5 (NetworkPolicy)
```

## Testing Commands

```bash
# Check if Service exists and has endpoints
kubectl get svc my-service
kubectl get endpoints my-service

# Check Pod is actually running
kubectl get pods -l app=web

# Check labels match
kubectl get service my-service -o yaml | grep -A 5 selector:
kubectl get pods --show-labels

# Get port information
kubectl get svc my-service -o yaml | grep -A 10 ports:
kubectl get pods -o yaml | grep -A 5 containerPort:

# Test connectivity from within cluster
kubectl run -it debug --image=nginx -- bash
curl http://my-service

# Test Pod directly (port-forward)
kubectl port-forward svc/my-service 8080:80
# Then from another terminal:
curl localhost:8080

# Check Pod logs
kubectl logs my-pod

# Check Pod networking
kubectl exec -it my-pod -- sh
netstat -tlnp  # See what it's listening on
curl localhost:8080  # Test from inside
```

## Prevention

### Use Descriptive Labels
```yaml
labels:
  app: web
  version: v1
  tier: frontend
```

### Match Exactly in Selector
```yaml
selector:
  app: web
  version: v1
```

### Be Explicit with Ports
```yaml
spec:
  containers:
  - name: web
    image: nginx:1.24
    ports:
    - name: http
      containerPort: 80
      protocol: TCP
```

### Use Named Ports (Optional but Clear)
```yaml
spec:
  ports:
  - port: 80
    targetPort: http  # References containers[].ports[].name
```

## See Also

- [[Concept - Services]] for explanation
- [[Reference - Service API]] for complete service spec
- [[Task - Expose an Application with Services]] for working example
- [[Troubleshooting - Pod Won't Start]] if Pods aren't running

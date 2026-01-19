---
author: Platform Team
last_verified: 2026-01-18
status: current
version_min: "1.24"
version_max: "1.28"
---

# Reference: Pod API

_Exact syntax, fields, and parameters for Kubernetes Pods._

## Basic Pod Syntax

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: default
  labels:
    app: web
spec:
  containers:
  - name: app
    image: nginx:1.24
    ports:
    - containerPort: 80
```

## Required Fields

| Field | Type | Description |
|---|---|---|
| `apiVersion` | string | `v1` for Pods |
| `kind` | string | `Pod` |
| `metadata.name` | string | Unique name within namespace (required) |
| `spec.containers` | array | List of containers (at least one, required) |

## Pod Metadata

| Field | Type | Example | Description |
|---|---|---|---|
| `metadata.name` | string | `my-pod` | Pod name (max 63 chars, lowercase alphanumeric + hyphens) |
| `metadata.namespace` | string | `default` | Namespace (defaults to `default`) |
| `metadata.labels` | map | `app: web` | Labels for organizing/selecting |
| `metadata.annotations` | map | `description: web server` | Non-identifying metadata |

## Pod Spec: Common Fields

| Field | Type | Default | Description |
|---|---|---|---|
| `spec.containers` | array | — | Container definitions (required) |
| `spec.restartPolicy` | string | `Always` | `Always`, `Never`, `OnFailure` |
| `spec.imagePullPolicy` | string | `IfNotPresent` | `Always`, `Never`, `IfNotPresent` |
| `spec.serviceAccountName` | string | `default` | Service account for auth |
| `spec.nodeSelector` | map | — | Schedule to Nodes with these labels |
| `spec.tolerations` | array | — | Node taints to tolerate |
| `spec.affinity` | object | — | Advanced scheduling rules |

## Container Spec

```yaml
containers:
- name: app
  image: nginx:1.24
  imagePullPolicy: IfNotPresent
  ports:
  - containerPort: 80
    name: http
    protocol: TCP
  env:
  - name: ENVIRONMENT
    value: production
  resources:
    requests:
      memory: "64Mi"
      cpu: "100m"
    limits:
      memory: "128Mi"
      cpu: "200m"
  volumeMounts:
  - name: config
    mountPath: /etc/config
  livenessProbe:
    httpGet:
      path: /health
      port: 80
    initialDelaySeconds: 10
    periodSeconds: 10
```

### Container Fields

| Field | Type | Example | Description |
|---|---|---|---|
| `name` | string | `app` | Container name (required, unique within Pod) |
| `image` | string | `nginx:1.24` | Container image (required) |
| `imagePullPolicy` | string | `IfNotPresent` | Pull policy for image |
| `ports` | array | `- containerPort: 80` | Ports to expose |
| `env` | array | `- name: VAR` | Environment variables |
| `resources` | object | See below | CPU/memory requests and limits |
| `volumeMounts` | array | `- name: data` | Mount volumes |
| `livenessProbe` | object | See below | Health check (restart if unhealthy) |
| `readinessProbe` | object | See below | Readiness check (remove from Service) |

## Resources: CPU and Memory

```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "100m"
  limits:
    memory: "128Mi"
    cpu: "500m"
```

| Field | Unit | Example | Description |
|---|---|---|---|
| `requests.memory` | bytes | `64Mi`, `512Mi`, `2Gi` | Minimum memory needed |
| `limits.memory` | bytes | `128Mi`, `1Gi` | Maximum memory allowed |
| `requests.cpu` | millicores | `100m`, `1`, `2000m` | Minimum CPU needed (1 = 1 core) |
| `limits.cpu` | millicores | `500m`, `2`, `4000m` | Maximum CPU allowed |

### CPU Examples
- `100m` = 100 millicores = 0.1 CPU core
- `500m` = 500 millicores = 0.5 CPU cores
- `1` = 1 core
- `2` = 2 cores

## Probes: Health Checks

### Liveness Probe (Restart if unhealthy)

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 80
  initialDelaySeconds: 10
  periodSeconds: 10
  failureThreshold: 3
  timeoutSeconds: 5
```

Pod is restarted if unhealthy.

### Readiness Probe (Remove from Service)

```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 80
  initialDelaySeconds: 5
  periodSeconds: 5
```

Pod is removed from Service if unhealthy (but not restarted).

### Probe Types

```yaml
# HTTP
httpGet:
  path: /health
  port: 80
  scheme: HTTP

# TCP
tcpSocket:
  port: 8080

# Exec
exec:
  command:
  - /bin/sh
  - -c
  - test -f /tmp/healthy
```

## Volume Mounts

```yaml
volumeMounts:
- name: data
  mountPath: /app/data

volumes:
- name: data
  emptyDir: {}
```

## Environment Variables

```yaml
env:
- name: APP_ENV
  value: production
- name: DEBUG
  value: "false"
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

## Complete Example: Multi-Container Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-with-sidecar
  labels:
    app: myapp
spec:
  containers:
  # Main container
  - name: app
    image: myapp:1.0
    ports:
    - containerPort: 8080
    env:
    - name: PORT
      value: "8080"
    resources:
      requests:
        memory: "128Mi"
        cpu: "100m"
      limits:
        memory: "256Mi"
        cpu: "500m"
    livenessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 10
  
  # Sidecar container
  - name: logger
    image: logging:1.0
    volumeMounts:
    - name: logs
      mountPath: /var/log
  
  volumes:
  - name: logs
    emptyDir: {}
```

## Common Commands

```bash
# Create
kubectl apply -f pod.yaml

# List
kubectl get pods
kubectl get pods -o wide  # More details
kubectl get pods -l app=web  # With label selector

# Describe
kubectl describe pod my-pod

# Get YAML
kubectl get pod my-pod -o yaml

# Logs
kubectl logs my-pod
kubectl logs my-pod -c container-name  # Specific container
kubectl logs my-pod -f  # Follow

# Execute command
kubectl exec -it my-pod -- /bin/sh

# Port forward
kubectl port-forward my-pod 8080:8080

# Delete
kubectl delete pod my-pod
```

## Pod Status

| Status | Meaning |
|---|---|
| Pending | Image pulling or scheduling |
| Running | Container is running |
| Succeeded | Container exited successfully |
| Failed | Container failed |
| CrashLoopBackOff | Container keeps crashing |
| ImagePullBackOff | Can't pull image |
| ErrImageNeverPull | Image pull policy is Never |

## Constraints

- **Name:** Max 63 characters, lowercase alphanumeric + hyphens only
- **Names must be unique** within a namespace
- **Container name:** Max 63 characters
- **Image:** Must be valid registry format
- **Port:** Valid port range 1-65535
- **Memory:** Must be valid Kubernetes format

## See Also

- [Concept - Pods](../30-Concepts/Concept%20-%20Pods.md) for explanation
- [Task - Deploy Your First Application](../20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md) for practical example
- [Troubleshooting - Pod Won't Start](../50-Troubleshooting/Troubleshooting%20-%20Pod%20Won%27t%20Start.md) for common issues

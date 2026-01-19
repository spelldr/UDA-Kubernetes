---
author: [Your Name/Team]
last_verified: YYYY-MM-DD
status: current
version_min: "1.24"
version_max: "1.28"
---

# Reference: [API or Command Name]

_Exact syntax, fields, and parameters for [this resource/command]._

## Syntax

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  # All fields explained below
```

Or command form:

```bash
kubectl [command] [resource] [name] [options]
```

## Required Fields

| Field | Type | Description |
|---|---|---|
| `metadata.name` | string | Unique identifier within namespace |
| `spec.containers` | array | Container definitions (at least one required) |

## Optional Fields

| Field | Type | Default | Description |
|---|---|---|---|
| `metadata.namespace` | string | `default` | Namespace for this resource |
| `spec.restartPolicy` | string | `Always` | Never/Always/OnFailure |
| `spec.imagePullPolicy` | string | `IfNotPresent` | Always/Never/IfNotPresent |

## Field Details

### metadata

Container for metadata about this resource.

**Example:**
```yaml
metadata:
  name: my-pod
  namespace: default
  labels:
    app: web
```

### spec.containers

List of containers to run in this Pod.

**Example:**
```yaml
spec:
  containers:
  - name: app
    image: nginx:1.24
    ports:
    - containerPort: 8080
```

## Complete Examples

### Example 1: [Use case]

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-server
spec:
  containers:
  - name: nginx
    image: nginx:1.24
    ports:
    - containerPort: 80
    env:
    - name: ENVIRONMENT
      value: production
```

### Example 2: [Use case]

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container
spec:
  containers:
  - name: app
    image: myapp:1.0
  - name: sidecar
    image: logging:1.0
```

## Common Commands

```bash
# Create
kubectl apply -f pod.yaml

# List
kubectl get pods

# Get details
kubectl describe pod my-pod

# View logs
kubectl logs my-pod

# Delete
kubectl delete pod my-pod
```

## Constraints & Limitations

- Pod names must be lowercase alphanumeric + hyphens
- Names cannot exceed 63 characters
- Names must start and end with alphanumeric

## Related References

- [[Reference - Deployment API]] (creates Pods)
- [[Reference - Service API]] (routes to Pods)

## See Also

- [Concept - Pods](../../30-Concepts/Concept%20-%20Pods.md) for explanation
- [Task - Deploy Your First Application](../../20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md) for practical example

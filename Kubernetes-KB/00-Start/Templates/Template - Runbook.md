---
author: [Your Name/Team]
last_verified: YYYY-MM-DD
status: current
maintenance_cadence: monthly
runbook_owner: [Team Name]
---

# Runbook: [What operational task?]

_How to [perform this operational task] safely and reliably._

## Overview

[What is this runbook for? When would you run it?]

Example: "This runbook walks a cluster operator through safely upgrading a Kubernetes cluster from 1.26 to 1.27 with zero downtime."

## Prerequisites

- [ ] Access to cluster (kubeconfig configured)
- [ ] Backup of etcd completed
- [ ] [Specific permission level]
- [ ] [Tool installed, e.g., kubectl 1.27+]

## Timeline

- **Estimated duration:** X hours
- **Best time to run:** Off-peak hours / during maintenance window
- **Estimated downtime:** X minutes (or zero-downtime)

## Step-by-Step

### Pre-Flight Checks

```bash
# Verify cluster health
kubectl cluster-info
kubectl get nodes
kubectl get pods --all-namespaces

# Check version
kubectl version --short
```

### Phase 1: [First phase]

**What this does:**
[Brief explanation]

**Steps:**
1. [Step 1]
   ```bash
   kubectl [command]
   ```

2. [Step 2]
   ```bash
   kubectl [command]
   ```

### Phase 2: [Second phase]

**What this does:**
[Explanation]

**Steps:**
1. [Step 1]
2. [Step 2]

### Post-Flight Verification

```bash
# Verify operation succeeded
kubectl get nodes
kubectl get pods --all-namespaces

# Check specific metrics
kubectl top nodes
```

## Rollback Plan

**If something goes wrong, here's how to roll back:**

```bash
# Rollback command(s)
```

## Monitoring During/After

Watch these metrics:
- Pod restart count (should be zero)
- Node status (should be Ready)
- API latency (should be normal)

```bash
# Watch command
watch kubectl get nodes
```

## Troubleshooting

**If step X fails:**
- See [[Troubleshooting - Common Issue]]
- Check logs: `kubectl logs -n kube-system [pod-name]`
- Rollback: [Rollback steps]

## References

- [[Concept - Cluster Architecture]]
- [[Task - Upgrade Kubernetes]]
- Related runbook: [[Runbook - Backup etcd]]

## Success Criteria

You know this runbook succeeded when:
- [ ] All nodes report Ready status
- [ ] All pods are Running
- [ ] API endpoints respond normally
- [ ] [Specific business metric is healthy]

---

**Last run:** [Date]
**Run by:** [Name]
**Status:** ✓ Success / ✗ Failed (see notes)
**Notes:** [Any deviations or issues encountered]

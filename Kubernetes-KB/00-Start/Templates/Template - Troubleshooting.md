---
author: [Your Name/Team]
last_verified: YYYY-MM-DD
status: current
symptom_keywords: [symptom1, symptom2, symptom3]
severity: common | rare
---

# Troubleshooting: [What's broken?]

_Answer: How do I fix [this problem]?_

## Symptoms

[How does the user know they have this problem?]

Example: "You run `kubectl get pods` and see a Pod with `CrashLoopBackOff` or `Pending` status."

## Quick Fix

[If there's a one-line fix, put it here]

```bash
# The command that usually fixes it
kubectl delete pod my-pod
```

## Root Causes (Choose Yours)

### Cause 1: [What's actually wrong]

**How to identify:**
```bash
# Diagnostic command
kubectl describe pod my-pod
```

Look for: [What specific error message or behavior]

**Fix:**
[Steps to resolve]

```bash
# Fix command(s)
```

### Cause 2: [Different cause]

**How to identify:**
```bash
kubectl logs my-pod
```

Look for: [Specific error pattern]

**Fix:**
[Explanation + steps]

### Cause 3: [Another possibility]

**How to identify:**
[Diagnostic approach]

**Fix:**
[Solution]

## Diagnosis Flowchart

```
Pod not running?
├─ Status = Pending → Check node resources ([[Cause 1]])
├─ Status = CrashLoopBackOff → Check logs ([[Cause 2]])
├─ Status = ImagePullBackOff → Check image exists ([[Cause 3]])
└─ Status = Error → Check YAML syntax
```

## Prevention

To avoid this problem in the future:
- [Best practice 1]
- [Best practice 2]
- [Best practice 3]

## Related Issues

- If you see X instead, see [[Troubleshooting - Other Issue]]
- Related concept: [[Concept - Pod Lifecycle]]
- How to debug: [[Task - Debug a Pod]]

## Still Stuck?

- Check the logs: `kubectl logs [pod-name]`
- Describe the resource: `kubectl describe pod [pod-name]`
- Check events: `kubectl get events`
- See [[Concept - Pods]] for background


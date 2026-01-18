---
author: [Your Name/Team]
last_verified: YYYY-MM-DD
status: current
difficulty: Beginner | Intermediate
estimated_time: X hours
---

# Tutorial: [Complete guided walkthrough topic]

_A hands-on, step-by-step guide to [accomplish this task] from scratch._

## Learning Objectives

By the end of this tutorial, you will:
- [Objective 1]
- [Objective 2]
- [Objective 3]

## Prerequisites

- [ ] [Requirement 1, e.g., kubectl installed]
- [ ] [Requirement 2, e.g., a running Kubernetes cluster]
- [ ] [Requirement 3, e.g., familiarity with YAML]

## What You'll Build

[Description of the end result]

[Optional: ASCII diagram or screenshot]

## Part 1: [Section title]

### 1.1 [Subsection]

[Explanation]

```bash
# Copy-paste command
kubectl [command]
```

**Expected output:**
```
actual output here
```

### 1.2 [Next subsection]

[Explanation]

```bash
kubectl [command]
```

## Part 2: [Section title]

### 2.1 [Subsection]

[Explanation and reasoning]

```yaml
# Complete YAML file
apiVersion: v1
kind: Pod
metadata:
  name: example
spec:
  containers:
  - name: app
    image: nginx
```

Save as `my-app.yaml`, then:

```bash
kubectl apply -f my-app.yaml
```

### 2.2 [Verify it works]

```bash
# Verify command
kubectl get pods
```

## Part 3: [Section title]

[Walkthrough of next logical section]

## Verification

You're done when you can:
- [ ] [Success criterion 1]
- [ ] [Success criterion 2]
- [ ] [Success criterion 3]

```bash
# Final verification command(s)
kubectl get all
```

## Troubleshooting

**If step X doesn't work:**
- See [[Troubleshooting - Common Issue]]
- Check: [Diagnostic approach]

## What's Next?

After this tutorial, explore:
- [[Task - Extend What You Built]] (next step)
- [[Concept - How This Works]] (understand the theory)
- [[Reference - Full API Reference]] (dive deeper)

## Key Concepts Learned

- **[[Concept 1]]**: [Brief explanation]
- **[[Concept 2]]**: [Brief explanation]

## Related Materials

- [[Task - Similar Hands-On Task]]
- [[Concept - Background Theory]]
- [[Reference - API Details]]

---

**Tutorial version:** 1.0
**Last tested:** YYYY-MM-DD with Kubernetes 1.28

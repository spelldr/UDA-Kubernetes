---
author: [Your Name/Team]
last_verified: YYYY-MM-DD
status: current
prerequisites: [List required knowledge, e.g., "Understanding of Pods", "kubectl installed"]
difficulty: Beginner | Intermediate
estimated_time: X minutes
---

# Task: [What are you accomplishing?]

_Answer: How do I [accomplish this specific thing]?_

## Before You Start

- [ ] Prerequisite 1 (e.g., kubectl installed and configured)
- [ ] Prerequisite 2 (e.g., access to a cluster)
- [ ] Prerequisite 3

## Quick Summary

[One sentence: what you're doing and why]

Example: "A Deployment creates and manages a set of identical Pods automatically, ensuring they stay running and scale as needed."

## Step-by-Step

### Step 1: [First action]

[Explanation and reasoning]

```bash
# Copy-paste ready command
kubectl apply -f deployment.yaml
```

### Step 2: [Second action]

[Explanation]

```bash
kubectl get deployments
```

### Step 3: [Verify success]

[How to know it worked]

```bash
kubectl describe deployment my-app
```

## Common Pitfalls

**Pitfall 1:** [Something people often get wrong]
- **Why it happens:** [Explanation]
- **How to avoid it:** [The right approach]

**Pitfall 2:** [Another common mistake]
- **Why it happens:** [Explanation]
- **How to avoid it:** [The right approach]

## Next Steps

- [Reference - Pod API](../../40-Reference/Reference%20-%20Pod%20API.md) for all Pod fields
- [Troubleshooting - Pod Won't Start](../../50-Troubleshooting/Troubleshooting%20-%20Pod%20Won%27t%20Start.md) if things go wrong
- [Concept - Deployments](../../30-Concepts/Concept%20-%20Deployments.md) if you want to understand how this works

## See Also

- [[Related Task 1]]
- [[Related Concept]]

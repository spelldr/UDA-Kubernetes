---
author: [Your Name/Team]
last_verified: YYYY-MM-DD
status: current
---

# Release Notes: Version X.Y.Z

_What changed in this release and how it affects you._

## Summary

[One sentence overview of this release]

Example: "Kubernetes 1.27 adds improved container lifecycle hooks and deprecates PodSecurityPolicy in favor of Pod Security Standards."

## Release Date

YYYY-MM-DD

## New Features

### Feature 1: [Feature name]

[Explanation of what it does and why it matters]

**New API fields:**
```yaml
spec:
  newField: value
```

**Example:**
```bash
# How to use it
```

**Docs:** [[Concept - Feature 1]], [[Task - Use Feature 1]]

### Feature 2: [Feature name]

[Explanation]

## Improvements

- [Improvement 1]: [Benefit]
- [Improvement 2]: [Benefit]
- [Performance improvement]: [Details]

## Deprecations

### Deprecated: [What's being phased out]

**Status:** Deprecated in 1.27, will be removed in 1.29

**Action required:**
- Migrate to: [[New Feature]]
- Timeline: [Migration deadline]

```yaml
# Old way (deprecated)
spec:
  oldField: value

# New way (replacement)
spec:
  newField: value
```

## Breaking Changes

### Breaking: [What changed]

**Impact:** [Who is affected and how]

**Migration:**
1. [Step 1]
2. [Step 2]
3. [Verify it works]

**Related:** [[ADR - Why we made this change]], [[Task - Migrate from oldway to newway]]

## Bug Fixes

- [Bug 1]: Fixed [description]
- [Bug 2]: Fixed [description]
- [Security fix]: [Brief description]

## Upgrade Path

**From 1.26 to 1.27:**

1. [[Runbook - Upgrade Kubernetes Cluster]]
2. Check [[Breaking Changes]] above
3. Follow [[Task - Migrate from oldway to newway]] if needed

**Supported versions:** 1.27.0 → 1.27.5 (current)

## Known Issues

| Issue | Workaround | Fix in |
|---|---|---|
| [Brief description] | [Workaround steps] | 1.27.1 |
| [Another issue] | [Workaround] | Investigating |

## Downloads & Resources

- [Download 1.27.0](https://kubernetes.io)
- [Release notes (official)](https://kubernetes.io/releases/notes)
- [[Concept - Version Support Policy]]

## Questions?

- See [[Troubleshooting - Upgrade Issues]]
- See [[FAQ - Kubernetes Versions]]
- Slack: #kubernetes-help

---

**Next version:** 1.28 (planned for Q3 2024)

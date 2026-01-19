# Release & Upgrade

Release notes, upgrade guides, and breaking changes documentation.

## What This Section Is For

Track what changed between Kubernetes versions and how to upgrade safely.

## When to Use This Section

- You're upgrading Kubernetes
- You need to know about breaking changes
- You're checking what features were added
- You want to understand deprecation notices

## When to Use Other Sections Instead

- **How to actually upgrade** → [Runbook - Upgrade Kubernetes Cluster](../60-PlatformOps/Runbook%20-%20Upgrade%20Kubernetes%20Cluster.md)
- **What is a feature** → [30-Concepts/README.md](../30-Concepts/README.md)
- **How to use a feature** → [20-Tasks/README.md](../20-Tasks/README.md)

## How This Section Is Organized

Each version gets its own Release Notes page with:
- **Summary** (one-line overview)
- **New features** (what was added)
- **Improvements** (performance, usability)
- **Deprecations** (being phased out, migration paths)
- **Breaking changes** (what changed and requires action)
- **Bug fixes** (important fixes)
- **Upgrade path** (how to go from previous version)

## Key Sections to Check Before Upgrading

1. **Breaking Changes:** Do you need to change anything?
2. **Deprecations:** What's being phased out?
3. **Migration steps:** Required changes to your configs/code
4. **Supported versions:** What versions are currently supported?

## Example Structure

```
v1.27 Release Notes
├─ New Features
├─ Deprecations
├─ Breaking Changes
├─ Migration Guide
└─ Upgrade Path (→ Runbook)
```

## Related Runbooks

- [Runbook - Upgrade Kubernetes Cluster](../60-PlatformOps/Runbook%20-%20Upgrade%20Kubernetes%20Cluster.md)

---

**Planning an upgrade?**
1. Read the Release Notes for your target version
2. Check the Breaking Changes section
3. Follow [Runbook - Upgrade Kubernetes Cluster](../60-PlatformOps/Runbook%20-%20Upgrade%20Kubernetes%20Cluster.md)

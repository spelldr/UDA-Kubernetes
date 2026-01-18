# Platform Operations

Operational runbooks for cluster administrators and SREs.

## What This Section Is For

Step-by-step procedures for cluster operations: upgrades, backups, maintenance, security, and day-to-day administration.

## When to Use This Section

- You need to perform an operational task safely
- You're upgrading or maintaining the cluster
- You're performing routine operations
- You need a verified procedure with rollback plans

## When to Use Other Sections Instead

- **Need explanation/theory** → [[30-Concepts]]
- **Quick how-to (not operational)** → [[20-Tasks]]
- **Something's broken** → [[50-Troubleshooting]]

## How Runbooks Are Structured

Each runbook includes:
- **Overview** (what is this, when to run it)
- **Prerequisites** (requirements and checklist)
- **Timeline** (estimated duration, best time, downtime)
- **Step-by-step phases** (detailed procedures with commands)
- **Post-flight verification** (how to confirm success)
- **Rollback plan** (how to undo if needed)
- **Monitoring during/after** (what to watch)
- **Success criteria** (how you know it worked)

## Who Should Run These

- Cluster administrators
- Site reliability engineers (SREs)
- Platform engineers with cluster access

## Example Topics

- [[Runbook - Upgrade Kubernetes Cluster]]

## Important Notes

- **Always backup before running runbooks**
- **Run during maintenance windows** (unless explicitly zero-downtime)
- **Test in non-production first**
- **Have a rollback plan**

## Ownership

Each runbook has an assigned owner. Contact them before making changes.

---

**Not sure if you should run this?** Check the prerequisites and timeline sections first.

# Kubernetes Knowledge Base

_A comprehensive Kubernetes reference built on Universal Documentation Architecture (UDA)._

**Simple to Intermediate Level** — Perfect for platform engineers, DevOps practitioners, and developers new to Kubernetes.

## Quick Start

- **New here?** Start with [00-Start/Welcome & Conventions.md](00-Start/Welcome%20&%20Conventions.md)
- **Looking for something?** Use search (Ctrl+Shift+F in Obsidian)
- **Want to contribute?** See [00-Start/Creating a New Page - Checklist.md](00-Start/Creating%20a%20New%20Page%20-%20Checklist.md)

## How to Use This KB

This KB is organized by **user intent**, not topic. Choose based on what you're trying to do:

| Intent                    | Section                | Examples                                               |
| ------------------------- | ---------------------- | ------------------------------------------------------ |
| **How do I...?**          | [20-Tasks/README.md](20-Tasks/README.md)           | Deploy an app, create a Deployment, expose a service   |
| **What is...?**           | [30-Concepts/README.md](30-Concepts/README.md)        | What's a Pod? What's Kubernetes? Architecture overview |
| **Need exact syntax?**    | [40-Reference/README.md](40-Reference/README.md)       | Pod API, Deployment specs, Service fields              |
| **Something broke**       | [50-Troubleshooting/README.md](50-Troubleshooting/README.md) | Pod won't start, service can't reach pod               |
| **Operate/maintain**      | [60-PlatformOps/README.md](60-PlatformOps/README.md)     | Upgrade cluster, monitor nodes                         |
| **What changed?**         | [80-ReleaseUpgrade/README.md](80-ReleaseUpgrade/README.md)  | v1.26 breaking changes, deprecation notices            |
| **Why did we choose...?** | [90-ADRs/README.md](90-ADRs/README.md)            | Architecture decisions and tradeoffs                   |
| **What does this mean?**  | [99-Glossary/Glossary.md](99-Glossary/Glossary.md)        | Terminology and definitions                            |

## Structure

```
Kubernetes-KB/
├── 00-Start/                    ← Start here: Welcome, guides, templates
├── 10-Tutorials/                ← Guided walkthroughs (coming soon)
├── 20-Tasks/                    ← How-to guides (quick answers)
├── 30-Concepts/                 ← Explanations (understand the why)
├── 40-Reference/                ← API specs and field reference
├── 50-Troubleshooting/          ← Diagnosis and fixes
├── 60-PlatformOps/              ← Operational runbooks
├── 80-ReleaseUpgrade/           ← Release notes and upgrade guides
├── 90-ADRs/                     ← Architecture decisions
└── 99-Glossary/                 ← Terminology
```

## Common Questions

**Where do I find how to deploy an app?**
→ [20-Tasks/Task - Deploy Your First Application.md](20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md)

**What's the difference between a Deployment and a Pod?**
→ Start with [30-Concepts/Concept - Pods.md](30-Concepts/Concept%20-%20Pods.md), then [30-Concepts/Concept - Deployments.md](30-Concepts/Concept%20-%20Deployments.md)

**My Pod won't start. What do I do?**
→ [50-Troubleshooting/Troubleshooting - Pod Won't Start.md](50-Troubleshooting/Troubleshooting%20-%20Pod%20Won%27t%20Start.md)

**What are all the fields in a Pod API?**
→ [40-Reference/Reference - Pod API.md](40-Reference/Reference%20-%20Pod%20API.md)

**How do I upgrade my Kubernetes cluster?**
→ [60-PlatformOps/Runbook - Upgrade Kubernetes Cluster.md](60-PlatformOps/Runbook%20-%20Upgrade%20Kubernetes%20Cluster.md)

## Governance

- **Confused about the KB structure?** See [DOCUMENTATION_DOCTRINE.md](../UDA%20-%20Kubernetes/DOCUMENTATION_DOCTRINE.md) (the rules)
- **Want to create a page?** Follow [00-Start/Creating a New Page - Checklist.md](00-Start/Creating%20a%20New%20Page%20-%20Checklist.md)
- **Who maintains this?** See [00-Start/Maintenance Workflow.md](00-Start/Maintenance%20Workflow.md)

---

Built with [Universal Documentation Architecture (UDA)](https://github.com/your-org/UDA).

**Last updated:** 2026-01-18

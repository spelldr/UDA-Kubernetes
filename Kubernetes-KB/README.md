# Kubernetes Knowledge Base

_A comprehensive Kubernetes reference built on Universal Documentation Architecture (UDA)._

**Simple to Intermediate Level** — Perfect for platform engineers, DevOps practitioners, and developers new to Kubernetes.

## Quick Start

- **New here?** Start with [[00-Start/Welcome & Conventions]]
- **Looking for something?** Use search (Ctrl+Shift+F in Obsidian)
- **Want to contribute?** See [[00-Start/Creating a New Page - Checklist]]

## How to Use This KB

This KB is organized by **user intent**, not topic. Choose based on what you're trying to do:

| Intent | Section | Examples |
|---|---|---|
| **How do I...?** | [[20-Tasks]] | Deploy an app, create a Deployment, expose a service |
| **What is...?** | [[30-Concepts]] | What's a Pod? What's Kubernetes? Architecture overview |
| **Need exact syntax?** | [[40-Reference]] | Pod API, Deployment specs, Service fields |
| **Something broke** | [[50-Troubleshooting]] | Pod won't start, service can't reach pod |
| **Operate/maintain** | [[60-PlatformOps]] | Upgrade cluster, monitor nodes |
| **What changed?** | [[80-ReleaseUpgrade]] | v1.26 breaking changes, deprecation notices |
| **Why did we choose...?** | [[90-ADRs]] | Architecture decisions and tradeoffs |
| **What does this mean?** | [[99-Glossary]] | Terminology and definitions |

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
→ [[20-Tasks/Task - Deploy Your First Application]]

**What's the difference between a Deployment and a Pod?**
→ Start with [[30-Concepts/Concept - Pods]], then [[30-Concepts/Concept - Deployments]]

**My Pod won't start. What do I do?**
→ [[50-Troubleshooting/Troubleshooting - Pod Won't Start]]

**What are all the fields in a Pod API?**
→ [[40-Reference/Reference - Pod API]]

**How do I upgrade my Kubernetes cluster?**
→ [[60-PlatformOps/Runbook - Upgrade Kubernetes Cluster]]

## Governance

- **Confused about the KB structure?** See [[DOCUMENTATION_DOCTRINE.md]] (the rules)
- **Want to create a page?** Follow [[00-Start/Creating a New Page - Checklist]]
- **Who maintains this?** See [[00-Start/Maintenance Workflow]]

---

Built with [Universal Documentation Architecture (UDA)](https://github.com/your-org/UDA).

**Last updated:** 2026-01-18

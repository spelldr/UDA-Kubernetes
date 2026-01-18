# Welcome & Conventions

Everything you need to know to navigate and use this Kubernetes Knowledge Base.

## What Is This KB?

This is a **Kubernetes Knowledge Base** built on the **Universal Documentation Architecture (UDA)**. It's organized by **user intent**, not by topic.

- **Level:** Simple to Intermediate (beginners through developers with some Kubernetes experience)
- **Focus:** Practical, actionable information
- **Organization:** By what you're trying to accomplish, not by alphabetical topic

## How to Navigate

### I Want to...

| Goal | Go here |
|---|---|
| **Learn what Kubernetes is** | [[30-Concepts/Concept - What is Kubernetes]] |
| **Deploy an app** | [[20-Tasks/Task - Deploy Your First Application]] |
| **Understand Pods** | [[30-Concepts/Concept - Pods]] |
| **Create a Deployment** | [[20-Tasks/Task - Create a Deployment]] |
| **Expose my app publicly** | [[20-Tasks/Task - Expose an Application with Services]] |
| **Look up Pod fields** | [[40-Reference/Reference - Pod API]] |
| **Debug a failing Pod** | [[50-Troubleshooting/Troubleshooting - Pod Won't Start]] |
| **Upgrade the cluster** | [[60-PlatformOps/Runbook - Upgrade Kubernetes Cluster]] |
| **Understand architecture** | [[30-Concepts/Concept - Architecture Overview]] |

### Browsing

**Table of Contents by Intent:**
- [[20-Tasks]] — How-to guides (step-by-step answers)
- [[30-Concepts]] — Explanations (understand the why)
- [[40-Reference]] — API documentation (exact syntax)
- [[50-Troubleshooting]] — Diagnosis and fixes
- [[60-PlatformOps]] — Operational procedures
- [[80-ReleaseUpgrade]] — What changed, how to upgrade
- [[90-ADRs]] — Why we made certain decisions
- [[99-Glossary]] — Terminology

### Search

Can't find something? Use **Ctrl+Shift+F** in Obsidian to search all pages. Search for:
- Specific error messages
- Command names (e.g., "kubectl apply")
- Concepts (e.g., "networking")
- Problem descriptions (e.g., "Pod stuck")

## How This KB Is Organized

### By User Intent (Not Topic)

Instead of a table of contents organized alphabetically ("Deployments", "Pods", "Services"), this KB is organized by **what you're trying to do**:

| User Intent | Section | Example |
|---|---|---|
| "How do I...?" | [[20-Tasks]] | [[Task - Deploy Your First Application]] |
| "What is...?" | [[30-Concepts]] | [[Concept - Pods]] |
| "Exact syntax?" | [[40-Reference]] | [[Reference - Pod API]] |
| "How do I fix...?" | [[50-Troubleshooting]] | [[Troubleshooting - Pod Won't Start]] |

This matches how your brain searches for information.

### Folder Structure

```
Kubernetes-KB/
├── 00-Start/                 ← You are here
│   ├── Templates/            ← All 8 canonical templates
│   ├── Welcome & Conventions ← This page
│   ├── Maintenance Workflow
│   └── Creating a New Page - Checklist
│
├── 20-Tasks/                 ← How do I...? (quick answers)
├── 30-Concepts/              ← What is...? (explanations)
├── 40-Reference/             ← Exact syntax and fields
├── 50-Troubleshooting/       ← How do I fix...? (diagnosis)
├── 60-PlatformOps/           ← Operational runbooks
├── 80-ReleaseUpgrade/        ← What changed, how to upgrade
├── 90-ADRs/                  ← Why we made decisions
└── 99-Glossary/              ← Terminology
```

Folders use **numeric prefixes** (00, 10, 20...) for consistent ordering.

## Page Conventions

### All Pages Have Metadata

Every page includes:
- **Author** (who wrote it)
- **Last verified** (when it was checked for accuracy)
- **Status** (current, needs_review, stale, superseded)

This helps you know if a page is trustworthy.

### Page Types

Each page uses one of 8 templates, which determines its structure:

| Type | Purpose | Structure |
|---|---|---|
| **Task** | How-to guide | Prerequisites → Steps → Verification → Common Pitfalls |
| **Concept** | Explanation | Definition → Why It Matters → How It Works → Key Relationships |
| **Reference** | API/command docs | Syntax → Required Fields → Optional Fields → Examples |
| **Troubleshooting** | Diagnosis/fixes | Symptoms → Root Causes → Flowchart → Prevention |
| **Runbook** | Operational procedure | Prerequisites → Phase 1 → Phase 2 → Verification → Rollback |
| **Release Notes** | What changed | Features → Deprecations → Breaking Changes → Upgrade Path |
| **ADR** | Decision record | Context → Options → Decision → Rationale → Consequences |
| **Glossary** | Terminology | Term → Definition → Links |

Understanding the page type helps you know what to expect.

### Linking Between Pages

Pages link according to **user intent routing**:

- **Task → Reference** (for exact syntax)
- **Task → Concepts** (for understanding)
- **Task → Troubleshooting** (for help)
- **Concept → Concepts** (related ideas)
- **Concept → Reference** (for specs)

Never Task → Task. This keeps navigation clear.

## How to Search

### Search Tips

1. **Search for the action:** "deploy" not "Deployment"
2. **Search for error messages:** Use verbatim error text
3. **Search for problem:** "pod won't start" not "pod lifecycle"
4. **Browse folders:** If search isn't helping, browse the folder matching your intent

### Examples

- Searching for "how do I deploy"? → [[20-Tasks]] folder
- Searching for "what is a service"? → [[30-Concepts]] folder
- Searching for "pod fields"? → [[40-Reference]] folder
- Searching for "pod stuck"? → [[50-Troubleshooting]] folder

## How to Contribute

Found an error? Want to add something? See [[Creating a New Page - Checklist]].

## Key Principles

This KB follows these non-negotiable rules:

1. **One page = one answer** (pages aren't bloated)
2. **Organized by intent** (not topic)
3. **Strict page types** (Task/Concept/Reference/etc.)
4. **Metadata required** (author, verification date, status)
5. **No duplication** (one source of truth)
6. **Verifiable facts** (we test examples)

See [[DOCUMENTATION_DOCTRINE.md]] for all the rules.

## Troubleshooting the KB Itself

**Can't find something?**
- Check [[20-Tasks]] first (most common questions answered there)
- Try search (Ctrl+Shift+F)
- Check related [[Concept]] pages for background

**A page is wrong or outdated?**
- Check the `last_verified` date in the metadata
- See [[Maintenance Workflow]] for how to flag stale content

**Want to contribute?**
- See [[Creating a New Page - Checklist]]
- Use a template from `00-Start/Templates/`
- Follow the routing rules in [[DOCUMENTATION_DOCTRINE.md]]

---

**Happy learning!** Start with [[20-Tasks/Task - Deploy Your First Application]] or [[30-Concepts/Concept - What is Kubernetes]].

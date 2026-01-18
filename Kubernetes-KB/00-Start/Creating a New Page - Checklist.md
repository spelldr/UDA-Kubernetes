# Creating a New Page - Checklist

Step-by-step checklist for contributing a new page to the KB.

## Step 1: Decide What You're Writing

**Question:** What is the user trying to do/understand/fix?

Use the [[DOCUMENTATION_DOCTRINE.md|routing matrix]] to choose the right page type:

| User Intent | Page Type | Go to Template |
|---|---|---|
| "How do I...?" | Task | [[Template - Task.md]] |
| "What is...?" | Concept | [[Template - Concept.md]] |
| "What are exact fields?" | Reference | [[Template - Reference.md]] |
| "How do I fix...?" | Troubleshooting | [[Template - Troubleshooting.md]] |
| "How do I operate...?" | Runbook | [[Template - Runbook.md]] |
| "What changed?" | Release Notes | [[Template - Release Notes.md]] |
| "Why did we choose...?" | ADR | [[Template - ADR.md]] |
| "What does this mean?" | Glossary | [[Template - Glossary.md]] |

**If it doesn't fit one type, you need to split it into multiple pages.**

## Step 2: Choose Your Folder

Based on page type:

| Page Type | Folder |
|---|---|
| Task | `20-Tasks/` |
| Concept | `30-Concepts/` |
| Reference | `40-Reference/` |
| Troubleshooting | `50-Troubleshooting/` |
| Runbook | `60-PlatformOps/` |
| Release Notes | `80-ReleaseUpgrade/` |
| ADR | `90-ADRs/` |
| Glossary | `99-Glossary/` |
| Tutorial | `10-Tutorials/` |

## Step 3: Copy the Template

1. Open `00-Start/Templates/Template - [Your Type].md`
2. Copy the entire content
3. Create a new file in the appropriate folder
4. Paste the template

**Naming convention:**
```
[Type] - [Question Being Answered].md
```

Examples:
- `Task - Deploy Your First Application.md`
- `Concept - What is Kubernetes.md`
- `Troubleshooting - Pod Won't Start.md`
- `Reference - Pod API.md`

## Step 4: Fill In the Template

Go section-by-section through the template:

- [ ] **Frontmatter metadata** (author, date, status, type-specific fields)
- [ ] **Title** (clear, searchable, matches user intent)
- [ ] **Main content** (follow template sections)
- [ ] **All required sections present** (don't skip or rename)
- [ ] **Examples complete and working** (test them!)
- [ ] **Code properly formatted** (syntax highlighting)

### Metadata Checklist

Every page must include:

```yaml
---
author: Your Name or Team
last_verified: YYYY-MM-DD (today's date)
status: current
---
```

Add type-specific metadata:

**Task pages:**
```yaml
prerequisites: [List of required knowledge]
difficulty: Beginner | Intermediate
estimated_time: X minutes
```

**Concept pages:**
```yaml
audience: Beginner | Intermediate | Advanced
```

**Reference pages:**
```yaml
version_min: "X.Y"
version_max: "X.Y"
```

**Troubleshooting pages:**
```yaml
symptom_keywords: [list, of, keywords]
severity: common | rare
```

**Runbook pages:**
```yaml
maintenance_cadence: monthly | quarterly | event-driven
runbook_owner: Team Name
```

## Step 5: Check Content Quality

- [ ] **Clear title:** Does it answer the user's question?
- [ ] **Jargon-free:** Is it accessible to the target audience?
- [ ] **Concise:** No unnecessary explanation?
- [ ] **Examples work:** Did you test the commands/code?
- [ ] **Complete:** Does it answer the complete question?
- [ ] **Links work:** All wikilinks `[[...]]` point to real pages

### Content Rules

✓ **DO:**
- Answer one specific question
- Use concrete examples
- Test every command/code example
- Link to related concepts for explanation
- Link to reference for syntax details

✗ **DON'T:**
- Mix multiple questions in one page
- Explain unnecessary background (link to Concept instead)
- Use unexamined jargon
- Include outdated information
- Duplicate content from other pages

## Step 6: Add Cross-Links

Following the routing matrix:

**Task pages** link to:
- [ ] [[40-Reference|Reference]] pages (for exact syntax)
- [ ] [[30-Concepts|Concept]] pages (for understanding)
- [ ] [[50-Troubleshooting|Troubleshooting]] pages (for help)
- Never other Task pages

**Concept pages** link to:
- [ ] Other [[30-Concepts|Concept]] pages (related ideas)
- [ ] [[40-Reference|Reference]] pages (for specs)
- Never Task pages

**Reference pages** link to:
- [ ] [[30-Concepts|Concept]] pages (for explanation)
- [ ] Never Task or Reference pages

**Troubleshooting pages** link to:
- [ ] [[20-Tasks|Task]] pages (for fixes)
- [ ] [[30-Concepts|Concept]] pages (for background)
- [ ] [[40-Reference|Reference]] pages (for specs)

**Use wikilink format:** `[[Folder/Page Name]]` or `[[Page Name]]`

## Step 7: Verify Links

In Obsidian:
1. Open the Command Palette (Ctrl+Shift+P)
2. Search for "Find unlinked references"
3. Fix any broken or unintended links

## Step 8: Final Checklist

Before you consider the page "done":

### Structure
- [ ] Correct template used (Task/Concept/Reference/etc.)
- [ ] All template sections present (none removed/renamed)
- [ ] Frontmatter metadata complete

### Content
- [ ] Answers one clear question
- [ ] Uses plain language
- [ ] Examples are tested and correct
- [ ] Code is properly formatted with syntax highlighting
- [ ] No broken wikilinks `[[...]]`
- [ ] No duplicated content from other pages

### Links
- [ ] Cross-links follow routing discipline
- [ ] Links point to real pages
- [ ] No circular dependencies (Concept ↔ Task)

### Metadata
- [ ] `author` is filled in
- [ ] `last_verified` is today's date
- [ ] `status` is one of: `current`, `needs_review`, `stale`, `superseded`
- [ ] Type-specific metadata present

## Step 9: Ask for Review

When your page is ready:

1. Mark `status: needs_review` in frontmatter
2. Ask the section owner (check [[Maintenance Workflow]] for assignments)
3. Or create a PR if this is in version control
4. Incorporate feedback
5. Change `status: current` when approved

## Step 10: Deploy

Once reviewed and approved:

1. [ ] Update metadata: `status: current`
2. [ ] Verify all links one more time
3. [ ] Publish/commit the page
4. [ ] Done! 🎉

---

## Examples

### Example Task Page

**File:** `20-Tasks/Task - Create a Deployment.md`

**Structure:** Prerequisites → Quick Summary → Step-by-Step → Common Pitfalls → Next Steps

### Example Concept Page

**File:** `30-Concepts/Concept - Pods.md`

**Structure:** Definition → Why It Matters → Core Concepts → How It Works → Key Relationships → Misconceptions

### Example Troubleshooting Page

**File:** `50-Troubleshooting/Troubleshooting - Pod Won't Start.md`

**Structure:** Symptoms → Root Causes → Flowchart → Prevention → Related Issues

## If You're Stuck

- **Confused about page type?** See [[DOCUMENTATION_DOCTRINE.md|routing matrix]]
- **Need a template?** `00-Start/Templates/`
- **Unsure about links?** Check [[DOCUMENTATION_DOCTRINE.md#8-Cross-Linking-Follows-Routing-Rules|routing rules]]
- **Want an example?** Look at an existing page in the KB

---

**You're ready!** Pick a page type and start writing.

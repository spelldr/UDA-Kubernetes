# Maintenance Workflow

How to keep the KB current, accurate, and useful.

## Section Ownership

Each section has an owner responsible for maintenance:

| Section | Owner | Review Cadence |
|---|---|---|
| 00-Start | Documentation Team | Monthly |
| 10-Tutorials | Platform Team | Quarterly |
| 20-Tasks | Platform Team | Monthly |
| 30-Concepts | Platform Team | Quarterly |
| 40-Reference | Platform Team | Event-driven (on API changes) |
| 50-Troubleshooting | SRE Team | Event-driven (after incidents) |
| 60-PlatformOps | SRE Team | Monthly |
| 80-ReleaseUpgrade | Platform Team | Event-driven (on releases) |
| 90-ADRs | Architecture Team | As decisions are made |
| 99-Glossary | Documentation Team | Quarterly |

**Assign yourself:** Update the table above with your actual team assignments.

## Half-Life Review Schedule

Each page type has an expected "freshness" threshold. When exceeded, auto-flag for review:

| Page Type | Half-Life | Threshold | Action |
|---|---|---|---|
| **Reference** | 6 months | Flag if > 6 months | Verify API still current |
| **Tasks** | 9-12 months | Flag if > 12 months | Test end-to-end |
| **Troubleshooting** | 12-18 months | Flag if > 18 months | Verify still relevant |
| **Runbooks** | 6-12 months | Flag after release | Test on current system |
| **Release Notes** | 3-6 months | Auto-flag on next release | Update with new info |
| **Concepts** | 18-24 months | Flag if > 24 months | Verify accuracy |
| **Tutorials** | 12 months | Flag if > 12 months | Walk through |
| **ADRs** | Permanent | Never auto-flag | Only mark superseded |

**How it works:**
- Check `last_verified` date in page metadata
- If older than threshold, page gets flagged `status: needs_review`
- Section owner verifies and updates `last_verified`

## Maintenance Checklist

### Weekly (All Sections)
- [ ] Review pages flagged `status: needs_review`
- [ ] Update pages affected by incidents or urgent issues

### Monthly (Primary Owners)
- [ ] Verify critical pages in your section still accurate
- [ ] Test code examples in Task/Reference pages
- [ ] Check for broken links: Obsidian → Find unlinked references
- [ ] Update metadata: `last_verified` to today's date if still current

### Quarterly (All Sections)
- [ ] Full section review: browse all pages
- [ ] Verify routing is still correct
- [ ] Update outdated information
- [ ] Mark pages as `superseded` if newer versions exist
- [ ] Archive pages no longer relevant

### Event-Driven (Immediate)

**New Kubernetes Release:**
- Update [[80-ReleaseUpgrade]] with new release notes
- Update [[40-Reference]] pages affected by API changes
- Update [[20-Tasks]] pages affected by new features
- Update [[30-Concepts]] pages with new capabilities

**Incident Occurs:**
- Same-day: Update [[50-Troubleshooting]] pages
- Create new troubleshooting page if gap identified
- Update [[20-Tasks]] pages to prevent future incidents

**CVE or Security Issue:**
- Flag [[50-Troubleshooting]] pages
- Update [[60-PlatformOps]] runbooks
- Create/update security-related pages

**API Deprecation:**
- Flag all pages mentioning deprecated API
- Create migration guide in [[20-Tasks]]
- Link from old pages to new pages

## How to Flag Pages for Review

1. Update `status` field in frontmatter:

```yaml
status: needs_review
```

2. Optional: Add comment explaining why

3. Section owner reviews and either:
   - Updates page and marks `status: current`
   - Marks `status: stale` or `status: superseded`

## Handling Stale Content

**Stale page:** Information is outdated but not replaced

1. Mark `status: stale`
2. Add notice at top of page:
   ```markdown
   > ⚠️ **This page is outdated.** See [[Newer Page]] instead.
   ```
3. Don't delete (leave it for historical reference)

**Superseded page:** Replaced by a newer page

1. Mark `status: superseded`
2. Add frontmatter: `supersedes: [[Old Page]]`
3. Add notice:
   ```markdown
   > **Superseded by [[New Page]].** See that page instead.
   ```

## Handling Broken Links

If you see `[[Page That Doesn't Exist]]`:

1. Check if the page should exist
2. If yes: Create it (or add to [[20-Tasks]] backlog)
3. If no: Fix the link to point to correct page
4. If correcting: Test that link works in Obsidian

## Handling Duplicated Content

If you find the same information in two pages:

1. Identify the authoritative source (usually the more recent)
2. Delete duplicate content from secondary page
3. Add link from secondary page to authoritative page:
   ```markdown
   > For details, see [[Authoritative Page]].
   ```
4. Update `last_verified` on both pages

## Verification Standards

When you verify a page:

- [ ] Test every command/code example (actually run it)
- [ ] Verify API fields against current Kubernetes version
- [ ] Check links for accuracy
- [ ] Read for clarity and correctness
- [ ] Verify metadata is accurate
- [ ] Update `last_verified: YYYY-MM-DD` to today's date

## Contributing a Page

For new pages, follow [[Creating a New Page - Checklist]]:

1. Use correct template
2. Fill in all sections
3. Add metadata (author, date, status)
4. Add cross-links
5. Verify all links
6. Mark `status: needs_review`
7. Ask section owner for review

## Tools

**Obsidian plugins that help:**
- **Dataview:** Auto-generate lists of stale pages (see [[DASHBOARDS]])
- **Find unlinked references:** Catch broken links

**Manual:**
- Search (Ctrl+Shift+F) for common terms
- Browse folders for scope verification
- Check git history if version controlled

## Escalations

**Who to contact if:**

| Issue | Contact |
|---|---|
| Page is wrong/misleading | Section owner |
| Multiple pages contradict each other | Section owner + Documentation team |
| Page is severely outdated | Section owner |
| Need new page created | Section owner |
| Disagree with a decision (ADR) | Architecture team |

---

## Status Definitions

| Status | Meaning | Action |
|---|---|---|
| **current** | Verified accurate, up-to-date | No action needed |
| **needs_review** | Flagged for section owner review | Review ASAP, then change to current or stale |
| **stale** | Outdated but not fully replaced | Link to replacement page, keep for history |
| **superseded** | Replaced by another page | Link to new page, keep for history |

---

**Last Updated:** 2026-01-18

Assign owners and cadences above, then follow this workflow.

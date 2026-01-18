# Weekly Dashboard

```ad-note
dataview code is given for reference. 
```

```js
table file.name as "Needs Review"
from ""
where status = "needs_review"
sort last_verified asc
```
```dataview
table file.name as "Needs Review"
from ""
where status = "needs_review"
sort last_verified asc
```

This lists all pages flagged `needs_review`.

---

# Monthly Dashboard

```js
table file.name as "Stale Metadata"
from ""
where status = "stale" or status = "superseded"
sort last_verified asc
```
```dataview
table file.name as "Stale Metadata"
from ""
where status = "stale" or status = "superseded"
sort last_verified asc
```

Shows pages with stale/superseded metadata for monthly review.

---

# Quarterly Dashboard

```js
table file.name as "Quarterly Review"
from ""
where status = "current"
sort last_verified asc
```
```dataview
table file.name as "Quarterly Review"
from ""
where status = "current"
sort last_verified asc
```

Lists all current pages, sorted by verification date, for quarterly section review.

---

# Event-driven Dashboards

## **New Release**

```js
table file.name as "Release-sensitive Pages"
from "40-Reference" or "80-ReleaseUpgrade" or "20-Tasks"
where status = "current"
```
```dataview
table file.name as "Release-sensitive Pages"
from "40-Reference" or "80-ReleaseUpgrade" or "20-Tasks"
where status = "current"
```

---

## **CVE**

```js
table file.name as "Security-sensitive Pages"
from "50-Troubleshooting" or "60-PlatformOps" or "70-Security"
```
```dataview
table file.name as "Security-sensitive Pages"
from "50-Troubleshooting" or "60-PlatformOps" or "70-Security"
```

---

## **Incident**

```js
table file.name as "Incident-linked Pages"
from "50-Troubleshooting"
```
```dataview
table file.name as "Incident-linked Pages"
from "50-Troubleshooting"
```

---

## **Major Change**

```js
table file.name as "Architecture Decisions"
from "90-ADRs"
```
```dataview
table file.name as "Architecture Decisions"
from "90-ADRs"
```

---

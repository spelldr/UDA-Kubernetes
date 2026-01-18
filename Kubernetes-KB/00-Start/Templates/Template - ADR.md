---
author: [Your Name/Team]
last_verified: YYYY-MM-DD
status: current
decision_date: YYYY-MM-DD
decides: [What this decision is about]
---

# ADR: [What decision was made?]

_**Decision:** [Brief statement of the decision]_

_**Status:** Accepted | Proposed | Superseded_

_**Supersedes:** [[ADR - Previous related decision (if any)]]_

## Context

[What problem were we facing? Why did we need to make a decision?]

Example: "We needed to choose how to manage network policies in our Kubernetes clusters. There are multiple approaches: native Kubernetes NetworkPolicy, third-party CNI plugins like Calico, or enterprise service mesh solutions like Istio."

## Options Considered

### Option 1: [First approach]

**Pros:**
- [Advantage 1]
- [Advantage 2]

**Cons:**
- [Disadvantage 1]
- [Disadvantage 2]

**Effort:** [High/Medium/Low] | **Cost:** [High/Medium/Low]

### Option 2: [Second approach]

**Pros:**
- [Advantage 1]

**Cons:**
- [Disadvantage 1]

**Effort:** [High/Medium/Low] | **Cost:** [High/Medium/Low]

### Option 3: [Third approach]

**Pros:**
- [Advantage 1]

**Cons:**
- [Disadvantage 1]

**Effort:** [High/Medium/Low] | **Cost:** [High/Medium/Low]

## Decision

**We chose: Option 1 (Native Kubernetes NetworkPolicy)**

**Reasoning:**
1. [Primary reason]
2. [Secondary reason]
3. [Constraint or requirement that drove the choice]

## Implementation

**Who:** [Team/person responsible]
**When:** [Timeline]
**How:**
1. [Step 1]
2. [Step 2]
3. [Success criteria]

## Consequences

### Positive

- [Benefit 1]
- [Benefit 2]

### Negative

- [Tradeoff 1]
- [Tradeoff 2]

### Mitigations

[How we'll address the negatives]

## Related Decisions

- [[ADR - Previous decision that informed this one]]
- [[ADR - Related decision]]

## Documentation

- [[Concept - NetworkPolicy Basics]]
- [[Task - Create a NetworkPolicy]]
- [[Reference - NetworkPolicy API]]

## Review Schedule

- **First review:** [3 months from decision]
- **Next review:** [Annual]
- **Reviewed by:** [Team/person]

---

**Decision made by:** [Name/Team]
**Date:** YYYY-MM-DD
**Last reviewed:** YYYY-MM-DD

# ADR, RFC, Decision Logs, and Architecture Governance

## Purpose

This file explains how to record and govern architecture decisions.

Architecture decisions should be visible, reviewable, and connected to implementation.

---

## ADR

An Architecture Decision Record captures one important decision.

Recommended ADR sections:

```text
Title
Status
Context
Decision
Options considered
Consequences
Implementation impact
Operational impact
Security impact
Evidence
Review date
```

---

## RFC

A Request for Comments is useful for larger design proposals.

RFC should include:

- problem statement
- goals
- non-goals
- design
- alternatives
- migration plan
- risks
- testing
- operations
- rollout
- open questions
- decision outcome

---

## Decision Log

A decision log summarizes decisions across a repo or platform.

Columns:

```text
ID
Decision
Status
Owner
Date
Related docs
Implementation evidence
Review date
```

---

## Decision Status

| Status | Meaning |
|---|---|
| Proposed | Under review. |
| Accepted | Approved decision. |
| Superseded | Replaced by later decision. |
| Deprecated | No longer recommended. |
| Rejected | Considered and not chosen. |
| Needs review | Current validity uncertain. |

---

## Architecture Governance

Good governance:

- lightweight enough to use
- clear decision thresholds
- evidence-based
- connected to implementation
- reviewed periodically
- avoids endless committees
- records trade-offs

---

## ADR Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Decision only in meeting notes | Lost context. |
| ADR written after implementation only | Rationale weak. |
| Alternatives omitted | Future debates repeat. |
| Consequences omitted | Trade-offs hidden. |
| ADR never updated when superseded | Stale decision. |
| Governance too heavy | Teams bypass process. |

---

## Review Checklist

- Does decision need ADR/RFC?
- Is context clear?
- Are alternatives considered?
- Is decision explicit?
- Are consequences honest?
- Are security/ops impacts included?
- Is implementation evidence linked?
- Is status current?
- Is review date defined?

---

## Summary

ADRs and RFCs preserve decision context.

They help future teams understand not only what was built, but why.

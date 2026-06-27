# ADR, RFC, Decision Governance, and Trade-Off Analysis

## Purpose

This file explains how leaders and architects should govern technical decisions.

Good decisions are explicit, contextual, and reviewable.

---

## ADR Versus RFC

| Format | Use |
|---|---|
| ADR | Record a specific decision. |
| RFC | Propose and discuss a larger design. |
| Decision log | Track decisions across a system. |
| Exception record | Document deviation from a standard. |

---

## ADR Template

```text
Title:
Status:
Date:
Context:
Decision:
Options considered:
Why this option:
Consequences:
Risks:
Security impact:
Operational impact:
Testing/evidence:
Review date:
```

---

## Trade-Off Analysis

Every meaningful architecture decision involves trade-offs.

Examples:

| Decision | Trade-Off |
|---|---|
| Synchronous API | Simple caller experience but timeout risk. |
| Async workflow | Durable and scalable but more complexity. |
| Cache | Faster reads but stale/invalidation risk. |
| Shared service | Reuse but coupling and governance needs. |
| Service split | Ownership clarity but operational overhead. |
| Monolith | Simpler deployment but boundary and scale limits. |

---

## Reversibility

Classify decisions:

| Type | Meaning |
|---|---|
| Easy to reverse | Low ceremony, local decision. |
| Moderately reversible | Team review and documentation. |
| Hard to reverse | ADR/RFC, stakeholder review, rollout plan. |
| Nearly irreversible | Formal governance and executive/risk awareness. |

---

## Decision Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Decision hidden in chat | Context lost. |
| Alternatives not recorded | Future debate repeats. |
| Consequences omitted | Teams surprised later. |
| No review date | Decisions go stale. |
| Exception without expiry | Permanent drift. |
| Over-document every small choice | Bureaucracy. |

---

## Review Checklist

- Does this need ADR or RFC?
- Is context clear?
- Are alternatives listed?
- Are consequences honest?
- Is reversibility assessed?
- Are risks documented?
- Is security/ops impact included?
- Is evidence defined?
- Is owner clear?
- Is review date defined?

---

## Summary

Decision governance is not paperwork.

It is how teams preserve reasoning and avoid rediscovering the same trade-offs.

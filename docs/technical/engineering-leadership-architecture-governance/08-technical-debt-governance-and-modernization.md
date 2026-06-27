# Technical Debt Governance and Modernization

## Purpose

This file explains how to identify, classify, prioritize, and reduce technical debt.

Technical debt is not all bad. Unmanaged technical debt is the problem.

---

## Technical Debt Types

| Type | Example |
|---|---|
| Architecture debt | Wrong service boundary or duplicated domain logic. |
| Code debt | High complexity, duplication, dead code. |
| Test debt | Missing contract or regression tests. |
| CI/CD debt | Weak gates, flaky checks, slow pipelines. |
| Security debt | Old dependencies, broad permissions, weak secrets handling. |
| Observability debt | Poor logs, missing metrics, stale runbooks. |
| Runtime debt | No probes, no resource limits, manual deployment. |
| Documentation debt | Stale README, missing ADRs, unsupported claims. |
| Data debt | unclear source ownership, weak lineage, no quality states. |

---

## Debt Classification

Classify debt by:

- severity
- business impact
- operational impact
- security impact
- delivery impact
- effort
- reversibility
- dependency
- urgency

---

## Prioritization

Prioritize debt that:

- blocks delivery
- causes incidents
- creates security risk
- creates audit/control risk
- increases cost
- prevents onboarding
- affects critical services
- is cheap to fix with high value

---

## Modernization Strategy

Good modernization is incremental.

Patterns:

- strangler pattern
- contract-first replacement
- compatibility wrapper
- dual-run
- expand-migrate-contract
- targeted refactoring
- deprecation plan
- feature flag
- migration waves

Avoid big-bang rewrites unless unavoidable.

---

## Debt Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Debt backlog with no owner | Never fixed. |
| Every improvement called critical | Prioritization impossible. |
| Big rewrite without proof | High delivery risk. |
| Only feature delivery | Debt compounds. |
| Debt hidden from product stakeholders | No capacity allocated. |
| Refactor without tests | Regression risk. |
| Debt reduction not measured | Value unclear. |

---

## Review Checklist

- What type of debt is this?
- What risk does it create?
- What evidence shows impact?
- Who owns it?
- What is smallest improvement slice?
- Can it be fixed incrementally?
- What tests protect refactor?
- What is business value?
- What happens if we do nothing?
- Is deprecation needed?

---

## Summary

Technical debt governance turns vague frustration into actionable modernization.

Debt should be visible, prioritized, and reduced through small evidence-backed slices.

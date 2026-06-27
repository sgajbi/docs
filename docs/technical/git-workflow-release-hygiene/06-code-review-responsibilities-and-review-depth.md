# Code Review Responsibilities and Review Depth

## Purpose

This file explains what reviewers should look for beyond syntax and style.

Enterprise code review is a control point for architecture, security, correctness, operations, and maintainability.

---

## Reviewer Responsibilities

Reviewers should assess:

- behavior correctness
- domain ownership
- architecture boundaries
- API compatibility
- test quality
- security posture
- sensitive-data handling
- observability
- performance risk
- migration safety
- documentation truth
- release risk

---

## Review Depth By Change Type

| Change Type | Review Focus |
|---|---|
| Domain logic | Business rules, edge cases, golden examples. |
| API change | Contract, errors, versioning, consumers. |
| BFF change | Upstream authority, degraded states, caller context. |
| UI change | Gateway-backed truth, permission states, accessibility where relevant. |
| Persistence change | Migration, transaction, rollback, data safety. |
| Worker change | Idempotency, retry, replay, dead-letter. |
| Security change | Auth, secrets, logs, permissions, tests. |
| CI change | Gate purpose, workflow permissions, runtime impact. |
| Docs change | Implementation-backed truth and link validity. |

---

## Good Review Comments

Good comments are:

- specific
- actionable
- tied to risk
- respectful
- clear about blocking versus suggestion
- supported by evidence or reasoning

Example:

```text
Blocking: this route returns raw upstream error detail. Please map it to the standard problem-details model and add a test for the 503 case.
```

---

## Review Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Only style comments | Important risks missed. |
| Approve without reading tests | Weak evidence. |
| Ignore docs impact | Truth drift. |
| Security left to security team only | Late findings. |
| Rubber-stamp urgent PR | Emergency risk. |
| Vague comments | Slow resolution. |
| Overly broad subjective comments | Review frustration. |

---

## Review Checklist

- Does code belong in the right layer?
- Is domain ownership preserved?
- Are contracts safe?
- Are tests meaningful?
- Are failure paths handled?
- Are sensitive fields safe?
- Are logs/metrics safe?
- Are migrations safe?
- Are docs updated?
- Is CI evidence sufficient?
- Are residual risks clear?

---

## Summary

Code review is engineering governance.

Reviewers protect future maintainability, production safety, and user trust.

# Migration Governance, Risk, Issue, Decision, and Evidence Management

## Purpose

This file explains migration governance and evidence management.

Large migrations need structured tracking of risks, issues, decisions, dependencies, exceptions, and sign-off evidence.

---

## RAID

Track:

| Area | Meaning |
|---|---|
| Risks | Potential future problems. |
| Assumptions | Things believed true that need validation. |
| Issues | Active problems. |
| Dependencies | External needs or blockers. |

---

## Decision Log

Migration decisions should capture:

- decision id
- decision
- context
- options
- owner
- date
- impact
- evidence
- review date

---

## Evidence Pack

A migration evidence pack may include:

- scope inventory
- architecture docs
- mapping catalogue
- transformation rules
- reconciliation output
- exception register
- test results
- mock cutover evidence
- security validation
- report/archive validation
- operational readiness
- sign-off records
- known limitations

---

## Exception Register

Track:

- exception id
- description
- affected scope
- severity
- owner
- status
- accepted/rejected
- resolution
- sign-off
- evidence

---

## Governance Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Risks tracked only in meetings | Lost ownership. |
| Decisions not recorded | Repeated debate. |
| Exceptions hidden in spreadsheets | Sign-off risk. |
| Evidence scattered | Audit and go-live friction. |
| Assumptions not validated | Cutover surprise. |
| Dependencies unmanaged | Timeline risk. |

---

## Review Checklist

- Is RAID active?
- Are decisions recorded?
- Are exceptions tracked?
- Are dependencies visible?
- Is evidence pack structured?
- Are owners assigned?
- Are accepted risks documented?
- Are sign-offs linked?
- Is governance lightweight but effective?

---

## Summary

Migration governance turns complexity into controlled execution.

Evidence, decisions, risks, and exceptions must be visible and owned.

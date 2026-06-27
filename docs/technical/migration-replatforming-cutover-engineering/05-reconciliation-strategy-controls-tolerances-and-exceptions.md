# Reconciliation Strategy, Controls, Tolerances, and Exceptions

## Purpose

This file explains how to reconcile migrated data and outputs.

Reconciliation provides evidence that migration results are correct enough for sign-off.

---

## Reconciliation Types

| Type | Example |
|---|---|
| Record count | Source and target count match. |
| Key match | Every source key maps to target key. |
| Field-level | Important fields match after transformation. |
| Total reconciliation | Balances, market values, or quantities match. |
| Hash reconciliation | Record or file hash comparison. |
| Business outcome | Reports, performance, or risk outputs match. |
| Sampling | Controlled sample reviewed manually. |
| Exception-based | Known breaks classified and accepted. |

---

## Tolerances

Some values may require tolerance.

Examples:

- rounding differences
- FX precision differences
- price decimal differences
- timing differences
- methodology change differences
- data-source enrichment differences

Tolerance must be approved and documented.

---

## Exception Workflow

An exception should include:

```text
exception id
data object
source value
target value
difference
reason
severity
owner
resolution
accepted by
evidence
```

---

## Sign-Off

Sign-off should be based on:

- reconciliation summary
- unresolved exceptions
- accepted tolerances
- business review
- risk review where needed
- cutover readiness
- rollback posture

---

## Reconciliation Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Only record count reconciliation | Business errors missed. |
| Tolerances undocumented | Sign-off disputes. |
| Exceptions tracked in emails | Loss of control. |
| Manual sample only | Weak evidence. |
| No owner for breaks | Stale exceptions. |
| Sign-off without unresolved break summary | Hidden risk. |

---

## Review Checklist

- What reconciliation types are needed?
- Are tolerances defined?
- Are exceptions tracked?
- Are owners assigned?
- Is severity defined?
- Are business outcomes reconciled?
- Are reports/analytics compared?
- Is sign-off evidence produced?
- Are unresolved breaks accepted formally?

---

## Summary

Reconciliation turns migration from hope into evidence.

It should compare the business meaning of data, not only technical counts.

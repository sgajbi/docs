# Domain Calculation, Analytics, and Methodology Continuity

## Purpose

This file explains how to preserve or intentionally change analytics and methodology during migration.

In wealth and banking platforms, numbers must remain explainable across old and new systems.

---

## Analytics Continuity Areas

Examples:

- portfolio valuation
- cash balances
- holdings
- transactions
- performance returns
- MWR/IRR
- contribution
- attribution
- risk metrics
- concentration
- benchmark comparison
- suitability results
- report totals

---

## Methodology Documentation

Document:

- old methodology
- new methodology
- differences
- expected breaks
- tolerances
- data input differences
- calculation version
- sign-off owner

---

## Golden Scenarios

Use golden scenarios for:

- simple portfolio
- multi-currency portfolio
- cashflow-heavy portfolio
- missing price
- late transaction
- benchmark comparison
- concentration breach
- report generation
- entitlement-denied case

---

## Tolerances

Define tolerances for:

- rounding
- FX precision
- price source difference
- timing difference
- benchmark revision
- methodology improvement

Tolerance must be approved.

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Output differences called "minor" without tolerance | Sign-off dispute. |
| Methodology changes hidden inside migration | Business trust issue. |
| No golden examples | Regression risk. |
| Analytics only reconciled at total level | Detail breaks missed. |
| Benchmark inputs changed silently | Performance mismatch. |
| Stale data differences not classified | Wrong root cause. |

---

## Review Checklist

- Which analytics must match?
- Are methodologies documented?
- Are differences expected?
- Are tolerances approved?
- Are golden scenarios defined?
- Are inputs comparable?
- Are results reproducible?
- Are exceptions classified?
- Is business sign-off obtained?

---

## Summary

Migration continuity includes meaning, not only data.

If numbers change, teams must know whether it is a defect, tolerance, source difference, or intentional methodology change.

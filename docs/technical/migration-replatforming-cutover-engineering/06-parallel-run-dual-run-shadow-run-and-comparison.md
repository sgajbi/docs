# Parallel Run, Dual Run, Shadow Run, and Comparison

## Purpose

This file explains comparison patterns used before migration cutover.

Running old and new systems together helps prove target behavior before switching users or production flow.

---

## Parallel Run

Old and new systems process same scope and outputs are compared.

Use for:

- calculations
- reports
- APIs
- batch outputs
- valuation
- holdings
- transactions
- user workflows

---

## Dual Run

Both systems actively process, sometimes with controlled routing.

Use carefully when both systems can create side effects.

---

## Shadow Run

Target system receives production-like inputs but does not affect production outputs.

Use for:

- performance testing
- output comparison
- operational readiness
- data quality validation

---

## Comparison Areas

Compare:

- record counts
- balances
- holdings
- market values
- transactions
- performance returns
- risk metrics
- reports
- API responses
- user permissions
- job statuses
- operational metrics

---

## Difference Classification

Classify differences:

| Classification | Meaning |
|---|---|
| Expected | Known methodology or mapping difference. |
| Tolerable | Within approved tolerance. |
| Defect | Must fix. |
| Source issue | Source data problem. |
| Timing issue | Due to update timing. |
| Unknown | Needs investigation. |
| Accepted risk | Formally accepted. |

---

## Parallel Run Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Run systems but no comparison criteria | No decision value. |
| Differences not classified | Sign-off blocked. |
| Side effects in dual run uncontrolled | Duplicate actions. |
| Shadow run not monitored | Failures missed. |
| Manual comparison only | Weak evidence. |
| Parallel run too short | Edge cases missed. |

---

## Review Checklist

- Which run pattern is used?
- What scope is compared?
- What outputs are compared?
- Are tolerances defined?
- Are differences classified?
- Are side effects controlled?
- Is evidence generated?
- Is run duration sufficient?
- Are sign-off criteria clear?

---

## Summary

Parallel run is only useful when comparison criteria, tolerances, and evidence are clearly defined.

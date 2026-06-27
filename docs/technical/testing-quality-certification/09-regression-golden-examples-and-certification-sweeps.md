# Regression, Golden Examples, and Certification Sweeps

## Purpose

This file explains how to use regression tests, golden examples, and certification sweeps to protect supported behavior.

Regression strategy prevents old bugs from returning and provides confidence for refactoring, migration, and release.

---

## Regression Tests

Add regression tests when:

- a bug is fixed
- a production incident occurs
- a migration issue is found
- a contract drift happens
- an edge case is discovered
- a supportability state was mishandled
- a security issue is fixed

A bug fix without a regression test is incomplete unless a clear reason is documented.

---

## Golden Examples

Golden examples are deterministic reference scenarios.

Use for:

- financial calculations
- performance metrics
- risk metrics
- attribution
- reporting
- lifecycle transitions
- data quality states
- entitlement behavior
- product certification

Golden examples should include:

- input
- expected output
- explanation
- assumptions
- version
- sensitive-data-safe identifiers

---

## Certification Sweeps

A certification sweep validates a supported API or product scope.

It should:

- seed deterministic synthetic data
- call supported endpoints
- assert expected values
- check health/readiness
- check supportability states
- generate evidence file
- avoid unsupported claims
- record known gaps

---

## Demo Readiness

Demo readiness is not production readiness.

Demo certification should prove:

- demo-critical routes work
- data is deterministic
- visible claims are supported
- boundaries are clear
- screenshots are safe
- unsupported features are not presented as supported

---

## Certification Evidence

Evidence should include:

- generated timestamp
- commit/ref
- commands run
- endpoints tested
- expected values
- actual values
- pass/fail status
- warnings
- known gaps
- evidence classification

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Golden data not documented | Future teams cannot trust it. |
| Certification uses random values | Non-repeatable evidence. |
| Demo screenshots without backend proof | Weak claims. |
| Regression only tests status 200 | Bug may return. |
| Certification claims unsupported features | Buyer/support trust risk. |
| Evidence contains sensitive data | Security risk. |

---

## Review Checklist

- Is regression added for bug fix?
- Are golden examples deterministic?
- Are assumptions documented?
- Is certification scope clear?
- Are unsupported states excluded?
- Is evidence machine-readable?
- Is data synthetic?
- Are expected values asserted?
- Are gaps documented?
- Is evidence safe to share with intended audience?

---

## Summary

Regression tests protect history. Golden examples protect meaning. Certification sweeps protect claims.

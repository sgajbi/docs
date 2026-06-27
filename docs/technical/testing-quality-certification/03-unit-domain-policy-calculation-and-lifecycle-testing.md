# Unit, Domain, Policy, Calculation, and Lifecycle Testing

## Purpose

This file explains how to test the core business logic of a backend service.

Domain tests should be fast, deterministic, and independent of infrastructure.

---

## What To Test

Test:

- value objects
- domain models
- policies
- calculations
- lifecycle transitions
- reason-code logic
- supportability classification
- eligibility rules
- boundary cases
- validation rules
- deterministic formatting or rounding

---

## Unit Test Characteristics

Good unit/domain tests:

- do not require database
- do not require HTTP server
- do not require container
- do not require external services
- use deterministic data
- assert business meaning
- run quickly
- fail clearly

---

## Calculation Testing

Calculation tests should define:

- input data
- expected output
- methodology notes
- precision and rounding
- date/currency assumptions
- unsupported inputs
- edge cases
- warnings/supportability behavior

Use golden examples for critical calculations.

---

## Lifecycle Testing

Lifecycle tests should prove:

- allowed transitions
- forbidden transitions
- terminal states
- idempotent repeated action
- invalid transition reason code
- audit/event intent where domain-level

Example:

```text
DRAFT -> SUBMITTED allowed
SUBMITTED -> APPROVED allowed
APPROVED -> DRAFT forbidden
CANCELLED -> RUNNING forbidden
```

---

## Policy Testing

Policy tests should cover:

- allowed case
- denied case
- boundary condition
- missing data
- stale data
- permission blocked
- unsupported input
- reason code

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Domain tests need database | Domain coupled to infrastructure. |
| Tests assert private implementation | Refactoring breaks tests unnecessarily. |
| Only one happy-path calculation | Edge-case defects. |
| No invalid lifecycle tests | Bad transitions reach production. |
| Floating random data | Non-deterministic evidence. |
| Test names vague | Failure hard to understand. |

---

## Review Checklist

- Are domain rules covered?
- Are calculations deterministic?
- Are edge cases included?
- Are unsupported states tested?
- Are lifecycle transitions tested?
- Are reason codes asserted?
- Are tests infrastructure-free?
- Are test names behavior-focused?
- Are golden examples documented?

---

## Summary

Domain tests protect the service's business truth.

If domain logic is hard to test, the architecture likely needs improvement.

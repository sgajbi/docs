# E2E, Browser, Product Flow, and Canonical Runtime Testing

## Purpose

This file explains how to test user-visible and cross-service product flows.

E2E and browser tests are expensive but valuable when they prove critical flows that cannot be trusted from lower-level tests alone.

---

## E2E Test Purpose

E2E tests prove:

- services work together
- user journey completes
- BFF contracts align with UI
- domain services return usable data
- authentication/caller context flows
- degraded states render correctly
- critical workflows are not broken

---

## Browser Test Purpose

Browser tests prove:

- page loads
- navigation works
- data renders
- loading/empty/error states appear
- permission-blocked states appear
- responsive layout works where required
- screenshots/evidence are captured
- UI consumes gateway/BFF instead of raw services

---

## Canonical Runtime

Canonical runtime means a controlled, repeatable environment used for product-flow validation.

It should define:

- service addresses
- seeded data
- startup commands
- validation commands
- expected routes
- evidence output path
- screenshots where useful
- cleanup

---

## What To Test E2E

Focus on:

- high-value product journeys
- release-critical paths
- previously broken flows
- cross-service integration
- entitlement and permission-blocked states
- degraded/partial/stale states
- report/render/archive flows
- BFF/UI contract alignment

Do not test every domain edge case through browser.

---

## Evidence

E2E evidence may include:

- JSON summary
- screenshots
- trace/correlation IDs
- API response excerpts
- route coverage
- assertion results
- environment details
- commit SHA

Evidence must be sensitive-data safe.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| All testing done through E2E | Slow and brittle. |
| Browser screenshots without assertions | Decorative proof. |
| E2E uses random data | Flaky evidence. |
| UI calls raw services in tests | BFF boundary bypassed. |
| Sensitive screenshots | Data leakage. |
| Test depends on manual setup | Poor repeatability. |
| E2E failure ignored as flaky | Real integration issue hidden. |

---

## Review Checklist

- Is E2E needed for this risk?
- Is canonical runtime documented?
- Is data synthetic and deterministic?
- Are assertions meaningful?
- Are degraded states tested?
- Is caller context tested?
- Are screenshots safe?
- Is evidence machine-readable?
- Are failures diagnosable?
- Are tests placed in appropriate CI lane?

---

## Summary

E2E and browser tests should prove critical integrated behavior, not replace lower-level tests.

Use them carefully and make their evidence repeatable.

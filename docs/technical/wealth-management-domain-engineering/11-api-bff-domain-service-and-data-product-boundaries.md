# API, BFF, Domain Service, and Data Product Boundaries

## Purpose

This file explains service and contract boundaries in wealth platforms.

Clear boundaries prevent duplicated logic, inconsistent analytics, and support confusion.

---

## Domain Service

A domain service owns business truth.

Examples:

- portfolio source data
- performance analytics
- risk analytics
- advisory workflow
- report generation
- archive lifecycle
- market/reference data

---

## BFF / Experience API

A BFF composes product-ready responses.

It may:

- aggregate services
- map degraded states
- forward caller context
- shape UI payloads
- hide upstream complexity

It should not:

- calculate official returns
- calculate official risk
- own source portfolio data
- invent report evidence
- bypass entitlements

---

## Data Product

A data product exposes governed reusable data.

It should include:

- producer
- version
- schema
- lineage
- freshness
- quality
- access policy
- SLO
- evidence
- supportability

---

## UI

UI owns:

- presentation
- workflow navigation
- user interaction
- display states
- client-side convenience

UI does not own official domain truth.

---

## Boundary Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| BFF calculates performance | Methodology drift. |
| UI joins raw service outputs into official report | Evidence risk. |
| Data product catalog becomes authority | Producer ownership weakened. |
| Report service recalculates holdings | Source confusion. |
| Risk service owns instrument master | Reference data drift. |
| Domain service returns UI-only layout | Coupling. |

---

## Review Checklist

- Which layer owns this logic?
- Does BFF only compose?
- Does UI avoid domain truth?
- Is data product producer clear?
- Are contracts versioned?
- Are supportability states preserved?
- Are entitlements enforced server-side?
- Are boundaries documented?
- Are tests aligned with boundaries?

---

## Summary

A wealth platform needs strong boundaries because many services use the same data for different purposes.

Ownership must be explicit.

# Testing, Contract Testing, Certification, and Regression

## Purpose

This file explains how to test data products and data mesh controls.

Testing should prove schema, semantics, quality, lineage, access, SLO, evidence, telemetry, and consumer impact.

---

## Test Categories

| Test Type | Purpose |
|---|---|
| Schema test | Validate fields, types, required values, enum values. |
| Semantic test | Validate business meaning, dates, units, currency, and vocabulary. |
| Quality test | Validate completeness, validity, freshness, reconciliation. |
| Lineage test | Validate source and evidence references. |
| Access test | Validate entitlement and policy behavior. |
| SLO test | Validate expected availability/freshness/latency posture where possible. |
| Contract test | Validate producer/consumer declarations. |
| API/event/batch test | Validate serving surfaces. |
| Telemetry test | Validate runtime trust snapshot shape and values. |
| Certification test | Validate maturity gate checks. |
| Regression test | Protect known business examples and previous breaks. |

---

## Producer Contract Tests

Test:

- product identity
- version
- producer
- schema reference
- vocabulary references
- source systems
- lineage fields
- freshness fields
- quality controls
- access/SLO/evidence policy references
- lifecycle state

---

## Consumer Contract Tests

Test:

- consumer identity
- product dependency
- version consumed
- fields used
- fallback behavior
- access scope
- impact if unavailable
- owner contact
- declared purpose

---

## Telemetry Tests

Test:

- product identity
- generatedAt
- certification state
- freshness bucket
- quality state
- supportability state
- evidence references
- no restricted fields
- schema validity
- timestamp recency where applicable

---

## Certification Tests

Certification gates may validate:

- producer contract
- consumer contract
- schema
- vocabulary
- access policy
- SLO policy
- evidence policy
- telemetry snapshot
- catalog publication
- gateway publication
- UI consumption
- no-sensitive-content rules
- runbook/dashboard references

---

## Regression Packs

Regression packs should include:

- known historical breaks
- golden product examples
- stale data scenario
- partial data scenario
- permission-blocked scenario
- reconciliation failure scenario
- schema compatibility scenario
- deprecated version scenario

---

## Test Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Only schema tests | Semantically wrong data passes. |
| No consumer contract test | Producer changes break consumers. |
| No access tests | Data leakage risk. |
| Static telemetry never validated | Trust posture drifts. |
| Certification gate ignores runtime evidence | False readiness. |
| Regression tests use real data | Privacy risk. |
| Quality tests not linked to supportability | Consumers cannot act. |

---

## Review Checklist

- Are schemas tested?
- Are semantics tested?
- Are quality rules tested?
- Is lineage tested?
- Is access policy tested?
- Are consumer contracts tested?
- Is telemetry tested?
- Is certification tested?
- Are regression scenarios included?
- Is data synthetic?
- Are tests in CI?
- Are failures actionable?

---

## Summary

Data-product testing must prove trust, not only shape.

A product is not mature until its contracts, quality, access, telemetry, and certification evidence are validated.

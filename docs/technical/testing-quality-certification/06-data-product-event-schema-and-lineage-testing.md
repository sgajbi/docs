# Data Product, Event, Schema, and Lineage Testing

## Purpose

This file explains how to test data products, event schemas, lineage, freshness, trust metadata, and quality controls.

Data-product tests prove that reusable data is meaningful, governed, and trustworthy.

---

## Data Product Tests

Test:

- product identity
- version
- producer declaration
- consumer declaration
- schema fields
- data dictionary references
- canonical vocabulary
- source systems
- access policy reference
- SLO policy reference
- evidence policy reference
- lifecycle status

---

## Event Schema Tests

Test:

- event type
- version
- producer
- required fields
- payload schema
- timestamp format
- correlation/causation fields
- backward compatibility
- no sensitive fields
- examples

---

## Lineage Tests

Test:

- source owner present
- source timestamp present
- business date present where needed
- transformation/calculation version present
- evidence reference present
- lineage reference safe
- generatedAt separate from asOfDate

---

## Freshness Tests

Test:

- current data classification
- stale data classification
- unknown freshness classification
- freshness bucket logic
- stale reason code
- consumer behavior on stale data

---

## Trust Metadata Tests

Test:

- supportability state
- quality state
- certification state
- evidence class
- telemetry timestamp
- no restricted fields
- schema validity

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Schema-only data product tests | Semantic defects pass. |
| No lineage tests | Data cannot be explained. |
| Freshness not tested | Stale data used as current. |
| Event compatibility ignored | Consumers break. |
| Static trust fixture never validated | False trust. |
| Sensitive source refs in evidence | Information leakage. |

---

## Review Checklist

- Are producer and consumer contracts tested?
- Are schemas validated?
- Are event schemas versioned and tested?
- Are lineage fields tested?
- Is freshness logic tested?
- Is trust metadata tested?
- Are access/SLO/evidence policies tested?
- Are examples synthetic?
- Are compatibility rules tested?

---

## Summary

Data-product and event tests protect trust.

They ensure shared data can be discovered, consumed, explained, and governed safely.

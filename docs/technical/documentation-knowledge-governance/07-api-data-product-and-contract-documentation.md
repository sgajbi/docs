# API, Data Product, and Contract Documentation

## Purpose

This file explains how to document APIs, events, data products, schemas, and contracts.

Contracts are durable promises. Documentation should make those promises clear and testable.

---

## API Documentation

API docs should include:

- route
- method
- purpose
- owner
- request schema
- response schema
- examples
- error model
- idempotency
- pagination
- caller context
- authorization
- supportability states
- versioning
- deprecation notes

---

## Event Documentation

Event docs should include:

- event type
- version
- producer
- consumers
- payload schema
- ordering expectations
- idempotency
- retry/dead-letter
- replay
- examples
- compatibility rules

---

## Data Product Documentation

Data-product docs should include:

- product name
- version
- producer
- consumers
- schema
- data dictionary
- source systems
- lineage
- freshness
- quality controls
- access policy
- SLO policy
- evidence policy
- certification state
- known limitations

---

## Contract Examples

Examples should be:

- synthetic
- valid
- minimal but meaningful
- current
- safe from sensitive data
- aligned with tests

---

## Contract Drift

Prevent drift with:

- OpenAPI snapshots
- schema validation
- contract tests
- data-product gates
- docs CI
- review checklist
- evidence links

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| API docs omit errors | Clients fail unpredictably. |
| Examples stale | Consumer confusion. |
| Data product has schema but no meaning | Misuse. |
| Event schema undocumented | Consumer breakage. |
| Planned fields shown as supported | False claim. |
| No versioning notes | Compatibility risk. |

---

## Review Checklist

- Is contract owner clear?
- Is schema documented?
- Are examples valid?
- Are errors documented?
- Is version clear?
- Is supportability included?
- Are access rules included?
- Are tests/evidence linked?
- Are deprecated fields marked?
- Are unsupported states honest?

---

## Summary

Contract documentation helps consumers integrate safely.

It should be treated as part of the API, event, or data product itself.

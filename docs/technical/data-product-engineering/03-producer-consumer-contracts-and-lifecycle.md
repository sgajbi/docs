# Producer, Consumer Contracts, and Lifecycle

## Purpose

This file explains how to define producer and consumer contracts for data products.

Contracts make ownership, usage, compatibility, evidence, and support expectations explicit.

---

## Producer Declaration

A producer declaration describes the data product from the producer's perspective.

It should include:

```text
producer
product name
version
business description
schema reference
semantic vocabulary
source systems
freshness expectation
quality controls
lineage fields
access policy reference
SLO policy reference
evidence policy reference
supported API/event/batch surfaces
supportability states
certification posture
known gaps
deprecation policy
```

---

## Consumer Declaration

A consumer declaration describes how another system uses the data product.

It should include:

```text
consumer
producer product
version consumed
purpose
fields used
freshness requirement
fallback behavior
impact if unavailable
access scope
downstream users
reporting/AI/client impact
test expectations
owner contact
```

---

## Why Consumer Contracts Matter

Without consumer declarations:

- producers do not know impact of change
- breaking changes surprise downstream teams
- unused fields cannot be safely removed
- operational impact is unclear
- certification scope is vague
- audit trails are incomplete

---

## Lifecycle States

| State | Meaning |
|---|---|
| Draft | Initial idea; not ready for consumers. |
| Proposed | Contract exists but not fully implemented. |
| Implementation-backed | Code and tests exist. |
| Runtime-evidenced | Runtime telemetry exists. |
| Certified | Required gates and evidence pass. |
| Degraded | Certified product currently operating with reduced quality. |
| Deprecated | Still available but planned for retirement. |
| Retired | No longer supported. |

---

## Compatibility Rules

Backward-compatible changes usually include:

- adding optional fields
- adding new optional metadata
- adding new quality warnings
- adding new consumer-declared fields
- adding new endpoint while retaining old one

Potentially breaking changes include:

- removing fields
- renaming fields
- changing field type
- changing units
- changing date semantics
- changing freshness semantics
- changing quality-state meaning
- changing access policy
- changing SLO
- removing event or API surface

---

## Versioning

Data products should be versioned.

Version may apply to:

- product contract
- schema
- event
- API
- vocabulary
- telemetry snapshot
- evidence manifest

Version should be visible to consumers.

---

## Deprecation

Deprecation plan should include:

- old product/version
- replacement product/version
- affected consumers
- migration date
- support window
- compatibility aliases
- evidence for consumer migration
- removal criteria

---

## Contract Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Producer declares product but no consumers | Product may not solve real need. |
| Consumer uses product without declaration | Producer cannot assess impact. |
| Contract has schema but no semantics | Fields misused. |
| Lifecycle state vague | Consumers over-trust product. |
| Certification claimed without telemetry | False readiness. |
| Breaking change without consumer review | Production failures. |
| Deprecated product never retired | Platform complexity grows. |

---

## Review Checklist

- Is producer declaration complete?
- Is consumer declaration complete?
- Is product versioned?
- Are fields semantically defined?
- Are compatibility rules clear?
- Is lifecycle state honest?
- Are known gaps documented?
- Are access/SLO/evidence policies linked?
- Are consumers notified of changes?
- Is deprecation plan defined?

---

## Summary

Producer and consumer contracts turn data sharing into governed collaboration.

They make dependencies visible, changes safer, and certification meaningful.

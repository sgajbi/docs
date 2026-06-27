# Catalog, Discovery, Publication, and Consumption

## Purpose

This file explains how data products should be discovered and published through a governed catalog and consumption layer.

A catalog helps consumers find data products, but it does not replace producer ownership.

---

## Catalog Purpose

A catalog should answer:

- what data products exist?
- who produces them?
- what version is available?
- what is the certification state?
- what consumers exist?
- what dependencies exist?
- what access policy applies?
- what SLO applies?
- what evidence exists?
- where are docs and APIs?
- who supports the product?

---

## Catalog Entry

A catalog entry may include:

```text
product name
version
producer
description
domain
schema reference
API/event/batch surfaces
certification state
trust summary
freshness expectation
quality posture
access policy reference
SLO reference
evidence reference
consumer list
dependency graph
support owner
documentation links
```

---

## Discovery APIs

A platform may expose discovery APIs such as:

```text
GET /data-products/catalog
GET /data-products/{producer}/{name}/{version}
GET /data-products/dependency-graph
GET /data-products/trust-certification
```

These APIs publish governed metadata but do not become the data-product authority.

---

## Dependency Graph

A dependency graph shows:

```text
producer product
  -> consumer product/service
      -> downstream report/UI/AI workflow
```

It supports:

- change impact analysis
- outage impact analysis
- migration planning
- consumer notification
- risk assessment

---

## Gateway Publication

A gateway or BFF may publish catalog and trust posture to product clients.

Rules:

- gateway may expose catalog metadata
- gateway must preserve producer authority
- gateway must not fabricate trust
- gateway should return unavailable/degraded posture if platform evidence is missing
- gateway should not read private files in ways that bypass governance

---

## UI Consumption

A UI may display:

- data products
- producers
- certification state
- dependencies
- trust posture
- freshness
- access state
- evidence links where allowed

UI must not:

- invent certification
- read platform files directly
- show restricted internal evidence to unauthorized users
- treat catalog visibility as product readiness

---

## Catalog Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Catalog becomes source of truth | Producer ownership weakened. |
| UI reads catalog files directly | Access and governance bypassed. |
| Catalog lists products without lifecycle state | Consumers over-trust. |
| Certification shown without evidence | False readiness. |
| Dependency graph missing | Change impact unknown. |
| Customer sees internal source paths | Information leakage. |
| Gateway invents fallback certification | Misleading trust posture. |

---

## Review Checklist

- Is catalog entry complete?
- Is producer authority preserved?
- Is certification state explicit?
- Are dependency relationships available?
- Is access policy linked?
- Is evidence safe to display?
- Does gateway publish without owning?
- Does UI consume through governed APIs?
- Are unavailable/degraded evidence states handled?
- Is catalog generated or validated?

---

## Summary

Catalogs make data products discoverable. Governance makes them trustworthy.

Never confuse discovery with certification.

# Caching, Materialized Views, Precomputation, and Invalidation

## Purpose

This file explains how to use caching and precomputed data safely.

Caching improves performance but creates correctness, freshness, invalidation, and entitlement risks.

---

## Cache Use Cases

Use caching for:

- reference data
- product configuration
- capability posture
- expensive read models
- stable lookup data
- repeated computed summaries
- catalog metadata
- safe short-lived UI support data

Be careful caching:

- entitlements
- rapidly changing positions
- transactions
- prices
- client-specific data
- AI outputs
- source freshness
- sensitive documents

---

## Cache Design

Define:

```text
key:
value:
owner:
TTL:
invalidation:
stale behavior:
security scope:
size limit:
metrics:
fallback:
```

---

## Materialized Views

Materialized views are precomputed read models.

Use when:

- query is expensive
- data can be refreshed on schedule/event
- consumers need fast reads
- lineage/freshness can be shown
- stale state can be represented

---

## Precomputation

Precompute when:

- calculation expensive
- same result reused
- batch window available
- result can be versioned
- consumers can accept freshness boundary

---

## Invalidation

Invalidation options:

- time-based TTL
- event-based invalidation
- manual invalidation
- version-based cache key
- write-through update
- refresh-ahead
- full rebuild

Invalidation must be documented.

---

## Cache Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Cache with no TTL | Stale data. |
| Cache key missing caller scope | Data leakage. |
| Cache treated as source of truth | Incorrect output. |
| No invalidation strategy | Wrong results. |
| Cache hides source failure | False readiness. |
| Sensitive data cached broadly | Security risk. |
| No cache metrics | Effectiveness unknown. |

---

## Review Checklist

- Is cache needed?
- What is cached?
- Is data sensitive?
- Is key scoped correctly?
- Is TTL defined?
- Is invalidation defined?
- Is stale behavior explicit?
- Are metrics present?
- Is fallback defined?
- Are tests present?

---

## Summary

Caching is a correctness feature as much as a performance feature.

Never add cache without defining freshness, invalidation, and security scope.

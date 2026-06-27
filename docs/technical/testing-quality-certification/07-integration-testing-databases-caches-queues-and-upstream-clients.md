# Integration Testing: Databases, Caches, Queues, and Upstream Clients

## Purpose

This file explains how to test real infrastructure adapters and dependencies.

Integration tests prove behavior that unit and fake-port tests cannot.

---

## What Integration Tests Should Cover

Integration tests should cover:

- database repositories
- migrations
- transaction behavior
- unique constraints
- connection handling
- cache TTL/invalidation
- queue publish/consume
- outbox delivery
- stream/event schema
- object storage adapter
- HTTP upstream client
- timeout/retry behavior
- dependency error mapping

---

## Database Integration Tests

Test:

- repository CRUD
- query filters
- pagination
- transaction rollback
- unique constraints
- idempotency records
- migration apply
- migration rollback or compensation
- data type mapping
- connection failures where possible

---

## Upstream Client Tests

Use controlled test servers or mocks that simulate real HTTP behavior.

Test:

- success response
- 400/403/404/409/500
- timeout
- malformed response
- retryable failure
- non-retryable failure
- caller-context headers
- correlation/trace propagation

---

## Queue and Stream Tests

Test:

- publish
- consume
- duplicate event
- retry
- dead-letter
- schema validation
- ordering assumptions
- consumer group behavior
- idempotent processing

---

## Cache Tests

Test:

- cache hit
- cache miss
- TTL expiry
- invalidation
- stale behavior
- security scope
- serialization errors

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Integration tests depend on shared dev DB | Flaky and unsafe. |
| No migration tests | Release failures. |
| HTTP clients tested only with mocks of own code | Real protocol errors missed. |
| Queue tests ignore duplicates | Retry issues. |
| Cache behavior untested | Stale data bugs. |
| Integration tests too broad | Slow and hard to diagnose. |

---

## Review Checklist

- Are real adapters tested?
- Are migrations tested?
- Are constraints tested?
- Are timeout/retry paths tested?
- Are caller headers tested?
- Are queue duplicates tested?
- Are cache TTLs tested?
- Are tests isolated?
- Are dependencies containerized or controlled?
- Are failures deterministic?

---

## Summary

Integration tests prove the seams between application logic and real infrastructure.

Use them where fake tests cannot provide enough confidence.

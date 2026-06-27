# Stateful Dependencies: Databases, Caches, Queues, and Streams

## Purpose

This file explains how runtime services should depend on databases, caches, queues, streams, and other stateful components.

Stateful dependencies are often where production incidents occur.

---

## Dependency Types

| Dependency | Purpose |
|---|---|
| Database | Durable transactional or analytical state. |
| Cache | Fast access to derived or temporary data. |
| Queue | Background work and async processing. |
| Stream | Event propagation and ordered processing. |
| Object store | Documents, reports, evidence, large artifacts. |
| Search index | Search and retrieval. |
| External API | Source or downstream integration. |

---

## Database Runtime Concerns

Define:

- connection string secret
- connection pool size
- migration strategy
- readiness dependency
- backup/restore
- failover behavior
- query timeouts
- indexes
- retention
- data classification
- encryption

---

## Cache Runtime Concerns

Define:

- cache purpose
- key format
- TTL
- invalidation
- stale behavior
- security scope
- maximum size
- fallback behavior
- metrics

Do not treat cache as source of truth unless deliberately designed as such.

---

## Queue/Stream Runtime Concerns

Define:

- topic/queue name
- consumer group
- ordering requirements
- retry policy
- dead-letter behavior
- idempotency
- replay
- consumer lag monitoring
- back pressure
- message schema
- retention

---

## Dependency Readiness

Readiness should include required dependencies.

If dependency is optional, readiness may stay true but supportability should degrade.

Example:

```text
database down -> not ready
optional analytics source down -> ready but degraded for analytics route
```

---

## Dependency Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No connection timeout | Hanging requests. |
| Unbounded connection pool | Database overload. |
| Cache with no TTL | Stale data and memory growth. |
| Queue messages not idempotent | Duplicate side effects. |
| No dead-letter path | Failed work hidden. |
| No consumer lag alert | Backlog grows silently. |
| Migrations not aligned with deployment | Runtime failures. |
| Object store paths exposed to clients | Security risk. |

---

## Review Checklist

- Are dependencies documented?
- Are secrets externalized?
- Are timeouts defined?
- Are connection pools sized?
- Are migrations safe?
- Are backups defined?
- Is cache behavior explicit?
- Are queue retries bounded?
- Is dead-letter behavior defined?
- Is lag monitored?
- Is replay safe?
- Are dependency failures represented in readiness/supportability?

---

## Summary

Stateful dependencies need runtime design.

A service is only as reliable as its dependency handling.

# Persistence, Transactions, Migrations, and State Ownership

## Purpose

This file explains how backend services should own durable state safely.

If a service persists data, it must own the lifecycle, migrations, transaction behavior, recovery posture, and operational support of that state.

## State Ownership

A service owns state when it is authoritative for creating, changing, or interpreting that state.

Owning state means owning:

- schema
- migrations
- validation
- data lifecycle
- retention
- transaction rules
- concurrency behavior
- recovery
- operational diagnostics
- runbooks
- test evidence

## Persistence Model

Persistence models should be separate from:

- API DTOs
- domain entities
- application result objects

A database row is not the same as a business object.

## Repository Responsibilities

Repositories should:

- map persistence records to domain objects
- hide SQL/ORM details
- expose domain-oriented methods
- handle not-found cases
- support transaction boundaries
- avoid leaking database exceptions
- support tests through fake or in-memory implementations

## Transaction Boundaries

A transaction should protect a consistent unit of state change.

Questions:

- Which writes must happen together?
- Can this transaction be retried?
- Does it call external systems?
- Can it be split into state write plus outbox event?
- What happens if the process crashes after commit?
- What happens if event publication fails?

Avoid long transactions around external calls.

## Outbox Pattern

Use an outbox when state change and event publication must be reliable.

Pattern:

```text
1. Begin transaction.
2. Write domain state.
3. Write outbox event record.
4. Commit transaction.
5. Outbox worker publishes event.
6. Mark event delivered or retry.
```

This avoids losing events when the database write succeeds but publisher fails.

## Migrations

Migrations should be versioned and testable.

Good migration rules:

- backward-compatible first
- expand-migrate-contract for breaking changes
- idempotent where practical
- dry-run where possible
- rollback or compensating plan
- data checks before and after
- migration smoke tests
- documented owner and procedure

## Expand-Migrate-Contract

For schema changes:

```text
1. Expand: add new nullable column/table/path.
2. Migrate: populate or dual-write data.
3. Contract: remove old column/path after consumers migrate.
```

This supports rolling deployments.

## Concurrency

Stateful services must handle concurrency.

Concerns:

- duplicate requests
- simultaneous updates
- stale version updates
- worker races
- retry collisions
- idempotency conflicts
- lock contention

Tools:

- optimistic locking
- unique constraints
- idempotency table
- transaction isolation
- queue leasing
- status transition checks

## Retention

Durable state needs retention rules.

Define:

- what is retained
- how long
- why
- who can access
- cleanup process
- audit constraints
- archive requirement
- legal hold behavior where relevant

## Persistence Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| ORM object returned through API | Internal schema becomes public contract. |
| Manual schema changes | Environments drift. |
| Migration without rollback plan | Release risk. |
| External calls inside DB transaction | Long locks and partial failure. |
| No idempotency for writes | Duplicate records. |
| No unique constraints for identity | Data corruption. |
| No retention policy | Unbounded growth and compliance risk. |
| Worker state only in memory | Lost progress on restart. |

## Review Checklist

- What state does the service own?
- Is schema versioned?
- Are migrations tested?
- Is transaction boundary explicit?
- Are external calls outside transactions?
- Is idempotency enforced?
- Are unique constraints aligned with business identity?
- Is concurrency handled?
- Is retention defined?
- Are repository methods domain-oriented?
- Are persistence models hidden from API?
- Is recovery possible after partial failure?

## Summary

Persistence is not just storage. It is ownership.

A service that writes durable state must also own migration, consistency, recovery, retention, and operational evidence.

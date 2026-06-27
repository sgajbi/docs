# Database Query, Index, Migration, and Storage Performance

## Purpose

This file explains database performance concerns for enterprise services.

Many application performance problems are database access problems.

---

## Query Design

Good query design:

- filters early
- uses indexes
- avoids unbounded scans
- returns only needed columns
- avoids N+1 queries
- uses stable sorting
- supports pagination
- has clear timeout
- is measured with query plan

---

## Indexing

Indexes should support:

- where filters
- join keys
- sort order
- pagination keys
- uniqueness constraints
- idempotency keys

Indexes are not free. They add write overhead and storage usage.

---

## N+1 Queries

N+1 happens when one query loads parent records and then one query per parent loads children.

Fix with:

- joins
- batch loading
- explicit projections
- precomputed read models
- query redesign

---

## Migration Performance

Migrations can create incidents.

Safe migration practices:

- backward-compatible first
- avoid long locks
- add indexes concurrently where supported
- batch backfills
- measure on realistic volume
- separate schema and data migration
- include rollback/compensation
- monitor migration duration

---

## Storage Growth

Define:

- retention
- archive
- partitioning
- compaction
- cleanup
- indexes
- storage alerts
- growth forecasts

---

## Database Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Query without limit | Large scan and memory pressure. |
| Offset pagination on huge table | Slow response. |
| No index for filter/sort | Full table scan. |
| Migration backfills all rows in one transaction | Locking/outage. |
| ORM lazy loading unnoticed | N+1. |
| No query timeout | Hanging requests. |
| No storage retention | Cost and performance growth. |

---

## Review Checklist

- Are critical queries indexed?
- Are list APIs paginated?
- Are projections used?
- Are N+1 risks checked?
- Are query plans reviewed?
- Are timeouts defined?
- Are migrations safe?
- Are backfills chunked?
- Is retention defined?
- Are database metrics monitored?

---

## Summary

Database performance should be designed into APIs, workflows, and migrations.

Do not wait for production data volume to reveal query design problems.

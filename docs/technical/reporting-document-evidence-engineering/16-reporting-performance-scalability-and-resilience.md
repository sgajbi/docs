# Reporting Performance, Scalability, and Resilience

## Purpose

This file explains performance and resilience concerns for reporting systems.

Report generation can be expensive, data-heavy, and dependency-heavy. It must be designed for scale and failure.

---

## Performance Concerns

Common bottlenecks:

- data collection
- analytics calculation
- rendering engine
- PDF generation
- chart generation
- archive storage
- large batch concurrency
- document download
- database writes
- template asset loading

---

## Scalability Patterns

Use:

- async jobs
- worker pools
- bounded queues
- chunked batch processing
- section-level caching where safe
- precomputed snapshots
- content model reuse
- rate limits
- back-pressure
- archive throughput limits

---

## Resilience Patterns

Define:

- retry policy
- dead-letter state
- replay
- partial generation rules
- render failure handling
- archive failure handling
- timeout behavior
- dependency fallback
- cancellation
- retention cleanup

---

## Report Generation Limits

Document:

- max portfolios per batch
- max pages
- max sections
- max rendering time
- max file size
- max concurrent jobs
- retention and cleanup limits

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Heavy report generated synchronously | Timeout. |
| Unlimited batch concurrency | System overload. |
| Render worker no queue metrics | Backlog invisible. |
| Failed archive not retried | Lost report. |
| Large report no size limit | Memory/storage risk. |
| No cancellation | Wasted resources. |
| No replay | Manual repair. |

---

## Review Checklist

- Is report generation async?
- Are queues bounded?
- Are workers scalable?
- Are timeouts defined?
- Are retries bounded?
- Is replay supported?
- Is archive throughput considered?
- Are file/page limits defined?
- Are performance tests present?
- Are dashboards available?

---

## Summary

Reporting performance is workflow performance.

Design for expensive data collection, rendering, archive, and batch generation from the start.

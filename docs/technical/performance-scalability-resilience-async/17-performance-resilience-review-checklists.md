# Performance and Resilience Review Checklists

## Purpose

This file provides practical checklists for performance, scalability, resilience, async execution, and capacity reviews.

Use it during architecture reviews, PR reviews, production-readiness checks, incident reviews, and engineering-lead assessments.

---

## Performance Design Checklist

- [ ] Expected load documented.
- [ ] Expected data volume documented.
- [ ] Latency targets defined.
- [ ] Throughput targets defined.
- [ ] Critical APIs identified.
- [ ] Response sizes bounded.
- [ ] Database query paths reviewed.
- [ ] Dependency limits known.
- [ ] Performance evidence planned.
- [ ] Known limits documented.

---

## API Performance Checklist

- [ ] List APIs paginated.
- [ ] Max page size enforced.
- [ ] Stable sort defined.
- [ ] Filters validated.
- [ ] Filters indexed.
- [ ] Date windows used where needed.
- [ ] Summary/detail split considered.
- [ ] Response fields minimized.
- [ ] Large-volume tests present.
- [ ] Timeouts defined.

---

## Timeout and Retry Checklist

- [ ] External calls have timeouts.
- [ ] Retryable errors defined.
- [ ] Non-retryable errors defined.
- [ ] Max attempts defined.
- [ ] Backoff used.
- [ ] Jitter used.
- [ ] Retry budget considered.
- [ ] Idempotency guaranteed where needed.
- [ ] Retry metrics emitted.
- [ ] Circuit breaker considered.

---

## Back-Pressure Checklist

- [ ] Queue bounds defined.
- [ ] Concurrency limits defined.
- [ ] Rate limits considered.
- [ ] Load shedding defined.
- [ ] Critical/non-critical work isolated.
- [ ] Overload response safe.
- [ ] Saturation metrics present.
- [ ] Alerts defined.
- [ ] Tests present.

---

## Idempotency and Concurrency Checklist

- [ ] Write-like operations identified.
- [ ] Idempotency keys required where needed.
- [ ] Request hash stored.
- [ ] Duplicate behavior defined.
- [ ] Conflict behavior defined.
- [ ] Unique constraints present.
- [ ] Lifecycle transitions atomic.
- [ ] Locks short-lived.
- [ ] Concurrent worker safety tested.
- [ ] Race conditions considered.

---

## Async and Worker Checklist

- [ ] Async justified.
- [ ] Job state durable.
- [ ] Status endpoint defined.
- [ ] Result endpoint defined.
- [ ] Retention defined.
- [ ] Worker lease model defined.
- [ ] Retry/dead-letter defined.
- [ ] Replay authorized and audited.
- [ ] Queue depth monitored.
- [ ] Worker shutdown safe.

---

## Cache Checklist

- [ ] Cache purpose clear.
- [ ] Key includes security scope.
- [ ] TTL defined.
- [ ] Invalidation defined.
- [ ] Stale behavior explicit.
- [ ] Sensitive data considered.
- [ ] Metrics present.
- [ ] Fallback defined.
- [ ] Tests present.

---

## Database Checklist

- [ ] Queries bounded.
- [ ] Indexes aligned with filters/sorts.
- [ ] Query plans reviewed for critical paths.
- [ ] N+1 risks checked.
- [ ] Connection pool sized.
- [ ] Query timeouts defined.
- [ ] Migrations chunked where needed.
- [ ] Backfills safe.
- [ ] Storage retention defined.
- [ ] Database metrics monitored.

---

## Resilience Checklist

- [ ] Required dependencies identified.
- [ ] Optional dependencies identified.
- [ ] Fallback defined.
- [ ] Partial response safe.
- [ ] Stale data labelled.
- [ ] Unknown state not treated as ready.
- [ ] Permission-blocked state safe.
- [ ] Degraded states tested.
- [ ] Runbook updated.

---

## Capacity and Scaling Checklist

- [ ] CPU/memory requests defined.
- [ ] Limits defined.
- [ ] Autoscaling trigger appropriate.
- [ ] Dependency capacity considered.
- [ ] Connection pools tuned.
- [ ] Batch windows considered.
- [ ] Cost impact understood.
- [ ] Known limits documented.
- [ ] Capacity test evidence exists.

---

## Observability Checklist

- [ ] Latency percentiles measured.
- [ ] Throughput measured.
- [ ] Error rate measured.
- [ ] Dependency latency measured.
- [ ] Queue depth measured.
- [ ] Saturation measured.
- [ ] Supportability measured.
- [ ] Dashboards current.
- [ ] Alerts actionable.
- [ ] SLOs defined where needed.

---

## Testing Checklist

- [ ] Performance baseline exists.
- [ ] Load test defined.
- [ ] Large data test defined.
- [ ] Retry/failure tests present.
- [ ] Back-pressure tested.
- [ ] Async recovery tested.
- [ ] Cache stale behavior tested.
- [ ] Database migration performance considered.
- [ ] Results retained.
- [ ] Findings tracked.

---

## Engineering Lead Questions

- What breaks first under load?
- Which dependency is most fragile?
- What is the expected p95 latency?
- What happens on duplicate request?
- What happens when downstream times out?
- What is the queue backlog threshold?
- Is this synchronous work actually long-running?
- What is safe fallback?
- What evidence proves capacity?
- What incident would be hardest to diagnose?

---

## Summary

Performance and resilience review should be explicit, evidence-based, and repeatable.

Use these checklists to catch design weaknesses before production finds them.

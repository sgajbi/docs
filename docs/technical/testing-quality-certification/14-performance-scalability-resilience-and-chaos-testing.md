# Performance, Scalability, Resilience, and Chaos Testing

## Purpose

This file explains how to test non-functional behavior: speed, scalability, resilience, and failure response.

Enterprise systems must handle realistic volume, dependency failures, retries, timeouts, and recovery.

---

## Performance Testing

Measure:

- latency
- throughput
- CPU
- memory
- database query time
- queue processing time
- batch completion time
- startup time
- response size

---

## Scalability Testing

Test:

- higher request volume
- larger datasets
- more concurrent users
- larger batch sizes
- worker concurrency
- queue backlog
- database connection pool limits
- autoscaling behavior

---

## Resilience Testing

Test:

- dependency unavailable
- dependency slow
- timeout
- retry behavior
- back pressure
- circuit breaker
- partial response
- stale data
- worker crash
- replay/recovery
- duplicate events

---

## Chaos-Style Testing

Chaos testing intentionally introduces failure.

Use carefully and only in controlled environments.

Examples:

- kill worker
- stop dependency
- inject latency
- drop messages
- fail database connection
- return malformed upstream response
- exhaust queue
- simulate stale source

---

## Performance Gates

For critical APIs, define:

- max p95 latency
- max response size
- max query count
- max memory usage
- expected throughput
- batch completion threshold

Thresholds should be realistic and tied to user/operational impact.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No performance baseline | Regressions invisible. |
| Load test unrealistic data | False confidence. |
| Retry behavior untested | Retry storm. |
| Queue backlog untested | Worker failure under load. |
| Chaos in uncontrolled env | Production impact. |
| Performance test not in CI/schedule | Results stale. |

---

## Review Checklist

- Are critical paths identified?
- Is performance baseline defined?
- Are large datasets tested?
- Are timeouts tested?
- Are retries tested?
- Is back pressure tested?
- Are partial/degraded states tested?
- Is recovery tested?
- Are thresholds meaningful?
- Are results retained?

---

## Summary

Non-functional testing proves whether the system behaves under pressure and failure.

Correct behavior at small scale is not enough.

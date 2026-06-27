# Performance, Scalability, and Resilience Mindset

## Purpose

This file explains the mindset behind performance, scalability, and resilience engineering.

The goal is not only to make individual requests faster. The goal is to keep systems predictable, safe, and recoverable under real operating conditions.

---

## Key Definitions

| Term | Meaning |
|---|---|
| Performance | How efficiently a system handles work, usually measured by latency, throughput, and resource usage. |
| Scalability | Ability to handle increased load or data volume by adding resources or improving design. |
| Resilience | Ability to continue, degrade safely, or recover when things fail. |
| Availability | Ability to serve supported requests when needed. |
| Saturation | Degree to which a resource is fully used. |
| Capacity | Maximum sustainable workload under defined conditions. |

---

## Common Engineering Trap

A feature can pass functional tests and still fail in production because:

- API returns too much data
- database query scans large table
- no timeout on downstream call
- retries amplify outage
- duplicate request creates duplicate job
- worker cannot keep up
- cache goes stale silently
- readiness ignores dependency failure
- logs do not show bottleneck
- no known capacity limit documented

Performance and resilience must be designed, not discovered accidentally.

---

## Non-Functional Requirements

Define non-functional requirements early:

```text
Expected request rate:
Expected data volume:
Expected p95 latency:
Expected p99 latency:
Max response size:
Max page size:
Worker throughput:
Batch completion target:
Recovery time:
Retry policy:
Back-pressure behavior:
Known limits:
```

---

## Design Trade-Offs

| Choice | Trade-Off |
|---|---|
| Synchronous API | Simple caller flow, but risk of timeout for long work. |
| Async execution | Better for heavy work, but needs durable state and polling. |
| Cache | Faster reads, but stale/invalidation complexity. |
| Batch | Efficient throughput, but latency and partial-failure complexity. |
| More replicas | Higher throughput, but dependency pressure and cost. |
| Strong consistency | Simpler correctness, but higher latency/locking. |
| Eventual consistency | Better scalability, but needs supportability and reconciliation. |

---

## Performance Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Optimize only after production incident | Late and expensive. |
| No data-volume assumptions | Demo design fails at scale. |
| Unlimited API response | Slow UI and memory pressure. |
| Retry without idempotency | Duplicate side effects. |
| Cache without stale-state design | Incorrect results. |
| Add replicas without DB capacity | Bottleneck moves to database. |
| No saturation metrics | Capacity issues invisible. |

---

## Review Questions

- What is expected load?
- What is expected data volume?
- What is the latency target?
- What can fail?
- What should be async?
- What must be idempotent?
- What happens under retry?
- How is back-pressure applied?
- Which resource saturates first?
- What evidence proves this design?

---

## Summary

Performance and resilience are architecture qualities.

They should be designed into the service from the start and validated continuously.

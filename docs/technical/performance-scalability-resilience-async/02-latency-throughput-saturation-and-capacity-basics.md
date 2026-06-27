# Latency, Throughput, Saturation, and Capacity Basics

## Purpose

This file explains the core measurement concepts used in performance and capacity engineering.

A team cannot improve what it cannot measure.

---

## Latency

Latency is how long one operation takes.

Useful latency metrics:

- p50
- p90
- p95
- p99
- max
- timeout rate

Average latency is not enough. Tail latency often drives user experience.

---

## Throughput

Throughput is how much work the system processes in a time period.

Examples:

- requests per second
- jobs per minute
- records per second
- reports per hour
- events per second
- batch items per run

---

## Saturation

Saturation shows how full a resource is.

Examples:

- CPU usage
- memory usage
- database connections
- thread pool usage
- queue depth
- worker lag
- disk IO
- network bandwidth
- cache memory
- rate limit utilization

High saturation usually appears before failure.

---

## Capacity

Capacity is the maximum sustainable workload under defined constraints.

Capacity should specify:

- workload type
- data volume
- environment
- resource limits
- dependency limits
- latency target
- error threshold

---

## Bottlenecks

Common bottlenecks:

- database query
- connection pool
- upstream API
- CPU-heavy calculation
- memory-heavy response
- serialization
- network latency
- queue worker throughput
- lock contention
- file/object storage
- external rate limit

---

## Measurement Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Only average latency tracked | Tail pain hidden. |
| No throughput metric | Capacity unknown. |
| No queue depth | Worker backlog invisible. |
| No dependency metrics | Root cause unclear. |
| No p95/p99 | Production experience misunderstood. |
| Measure demo data only | False confidence. |
| No baseline | Regressions invisible. |

---

## Review Checklist

- Are latency percentiles measured?
- Is throughput measured?
- Is saturation measured?
- Are dependency latencies measured?
- Are queue depths measured?
- Are capacity assumptions documented?
- Is baseline recorded?
- Are critical-path metrics visible?
- Are dashboards available?

---

## Summary

Latency, throughput, saturation, and capacity are the language of performance engineering.

Use them before guessing.

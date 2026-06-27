# Resources, Limits, Autoscaling, Capacity, and Performance

## Purpose

This file explains how to define runtime resources and scaling behavior for enterprise workloads.

Resource posture affects reliability, cost, performance, and cluster stability.

---

## Requests and Limits

| Term | Meaning |
|---|---|
| CPU request | CPU expected/reserved for scheduling. |
| CPU limit | Maximum CPU a container can use. |
| Memory request | Memory expected/reserved for scheduling. |
| Memory limit | Maximum memory before termination. |

Requests help scheduling. Limits protect the platform.

---

## Why Resources Matter

Good resource definitions:

- prevent noisy-neighbor issues
- improve scheduling
- reduce OOM kills
- support capacity planning
- improve cost visibility
- enable autoscaling
- make performance expectations explicit

---

## Autoscaling

Horizontal Pod Autoscaler can scale replicas based on:

- CPU
- memory
- custom metrics
- queue depth
- request rate
- latency indicators where supported

Autoscaling must consider:

- startup time
- downstream capacity
- database connection limits
- queue semantics
- idempotency
- statefulness
- cost impact

---

## Capacity Planning

Capacity planning should consider:

- expected traffic
- peak traffic
- batch windows
- report schedules
- async worker throughput
- dependency limits
- database connections
- memory growth
- cache size
- storage growth
- failover capacity

---

## Performance Signals

Useful signals:

- CPU saturation
- memory usage
- request latency
- queue depth
- worker lag
- database connection pool usage
- error rate
- retry rate
- garbage collection where applicable
- container restart count
- throttling

---

## Performance Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No resource requests | Poor scheduling. |
| No memory limit | Node instability. |
| Limit too low | OOM kills. |
| Autoscale only API but not workers | Queue backlog persists. |
| Scale out without DB capacity | Dependency overload. |
| No performance gate for critical API | Regression discovered late. |
| Batch jobs run during peak traffic | User impact. |
| No known limits documented | Operators surprised. |

---

## Review Checklist

- Are CPU/memory requests defined?
- Are limits defined?
- Are values based on measurement?
- Are autoscaling rules defined?
- Are dependencies able to handle scale?
- Are connection pools configured?
- Are batch windows considered?
- Are saturation metrics monitored?
- Are known limits documented?
- Are performance tests or smoke checks present?

---

## Summary

Scaling is not only adding replicas.

A scalable service must understand resources, dependencies, throughput, limits, and cost.

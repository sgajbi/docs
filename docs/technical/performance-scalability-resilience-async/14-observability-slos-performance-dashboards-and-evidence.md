# Observability, SLOs, Performance Dashboards, and Evidence

## Purpose

This file explains how to observe performance and resilience.

If performance cannot be measured, it cannot be managed.

---

## Performance Metrics

Track:

- request rate
- latency percentiles
- error rate
- timeout rate
- retry rate
- dependency latency
- queue depth
- worker lag
- CPU/memory
- database query time
- connection pool saturation
- cache hit rate
- batch duration

---

## SLOs

Example SLOs:

```text
99% of supported portfolio summary requests complete under 500ms over 30 days.
95% of report jobs complete within 5 minutes during business hours.
99% of certified data products have CURRENT freshness by 08:00 business time.
```

---

## Dashboards

Dashboards should show:

- latency
- throughput
- error rate
- saturation
- dependencies
- queues
- stale/degraded states
- cache effectiveness
- batch progress
- recent deployments
- known limits

---

## Performance Evidence

Evidence may include:

- load test report
- baseline benchmark
- query plan
- capacity test output
- dashboard snapshot
- CI performance gate
- certification sweep
- incident review
- optimization note

Evidence should be tied to environment and commit where possible.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No latency percentiles | Tail issues hidden. |
| No dependency metrics | Bottleneck unclear. |
| Dashboard not tied to implemented metrics | False confidence. |
| Performance evidence not retained | Regression analysis weak. |
| No SLOs | Expectations unclear. |
| No saturation metrics | Capacity issues surprise. |

---

## Review Checklist

- Are key metrics defined?
- Are latency percentiles tracked?
- Are dependency metrics tracked?
- Are queues monitored?
- Are SLOs defined?
- Are dashboards current?
- Are alerts actionable?
- Is performance evidence retained?
- Is evidence tied to release?
- Are known limits visible?

---

## Summary

Performance engineering requires measurement.

Dashboards and SLOs turn non-functional expectations into operational reality.

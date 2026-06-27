# Resource Limits, Autoscaling, Cost, and Capacity Planning

## Purpose

This file explains how runtime resources, autoscaling, cost, and capacity planning fit into performance engineering.

Scaling is both a technical and economic design concern.

---

## Resources

Define:

- CPU request
- CPU limit
- memory request
- memory limit
- ephemeral storage
- connection pool size
- worker concurrency
- queue limits

---

## Autoscaling

Autoscaling can use:

- CPU
- memory
- request rate
- queue depth
- worker lag
- custom business metrics

Autoscaling must consider dependency capacity.

---

## Capacity Planning

Capacity plan should include:

- expected users
- request rate
- data volume
- peak windows
- batch windows
- worker throughput
- dependency limits
- storage growth
- failover headroom
- cost constraints

---

## Cost Controls

Cost controls:

- right-size resources
- scheduled scale down for non-prod
- retention cleanup
- log retention policy
- cache sizing
- storage lifecycle
- batch window optimization
- performance regression detection

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No resource limits | Platform instability. |
| Limit too low | OOM/restarts. |
| Scale API but not DB | Bottleneck moves. |
| Autoscale on CPU only for queue workload | Backlog persists. |
| Non-prod always full size | Waste. |
| No storage retention | Cost growth. |
| No capacity forecast | Surprise incidents. |

---

## Review Checklist

- Are resource requests/limits defined?
- Are values measured?
- Are autoscaling triggers appropriate?
- Are dependency limits known?
- Are connection pools tuned?
- Is cost monitored?
- Is storage growth tracked?
- Are non-prod resources controlled?
- Is capacity tested?
- Are known limits documented?

---

## Summary

Capacity is not infinite and scaling is not free.

Good engineering balances performance, resilience, and cost.

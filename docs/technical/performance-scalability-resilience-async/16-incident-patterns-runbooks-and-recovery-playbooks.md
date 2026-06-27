# Incident Patterns, Runbooks, and Recovery Playbooks

## Purpose

This file explains common performance and resilience incidents and how to diagnose, mitigate, recover, and improve.

Incidents should produce learning and stronger controls.

---

## Common Incident Patterns

| Incident | Common Causes |
|---|---|
| Slow API | unbounded query, missing index, dependency latency, large response |
| Gateway timeout | downstream slow, no async path, serialization cost |
| Queue backlog | worker under-scaled, dependency failing, poison message |
| Retry storm | dependency outage plus uncontrolled retries |
| OOM restart | large response, memory leak, unbounded batch |
| Database saturation | full scans, connection pool exhaustion, lock contention |
| Stale data | ingestion delay, source failure, cache invalidation issue |
| Duplicate jobs | missing idempotency, retry after timeout |
| Report batch delay | worker throughput, dependency issue, large batch |
| Cache serving wrong data | key scope or invalidation bug |

---

## Diagnosis Steps

1. Check recent deployments.
2. Check latency/error dashboards.
3. Check dependency metrics.
4. Check saturation metrics.
5. Check queue depth/lag.
6. Check logs by operation and reason code.
7. Check database query metrics.
8. Check supportability states.
9. Check alerts and incident timeline.
10. Identify mitigation.

---

## Mitigation Options

- disable feature flag
- reduce traffic
- apply rate limit
- pause batch/worker
- scale worker/API
- increase resource limit temporarily
- fail over dependency
- clear or replay stuck work
- roll back deployment
- apply hotfix
- communicate degraded state

---

## Recovery

Recovery should validate:

- service ready
- queue backlog decreasing
- latency normal
- error rate normal
- data freshness restored
- duplicate side effects absent
- supportability ready
- alerts clear
- users/operators informed

---

## Post-Incident Improvements

Add or improve:

- test
- alert
- dashboard
- timeout
- retry policy
- back-pressure
- idempotency
- pagination
- index
- cache invalidation
- runbook
- capacity plan
- SLO

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Only scale up without root cause | Cost and recurrence. |
| Clear queue without audit | Data loss. |
| Disable alert instead of fixing noise | Future incident hidden. |
| No post-incident action | Repeat failures. |
| Manual DB patch undocumented | Audit and integrity risk. |
| No duplicate check after retry incident | Hidden data issue. |

---

## Review Checklist

- Is incident pattern known?
- Is runbook available?
- Are dashboards useful?
- Is mitigation safe?
- Is recovery verified?
- Is evidence preserved?
- Are duplicate effects checked?
- Are follow-up controls created?
- Are tests added?
- Are docs updated?

---

## Summary

Performance incidents should become stronger design, tests, alerts, and runbooks.

Do not waste incidents by only restoring service.

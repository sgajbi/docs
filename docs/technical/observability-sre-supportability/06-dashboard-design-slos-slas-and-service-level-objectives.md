# Dashboard Design, SLOs, SLAs, and Service-Level Objectives

## Purpose

This file explains how to design dashboards and service-level objectives that support real operations.

Dashboards should answer operational questions. SLOs should define measurable reliability expectations.

---

## Dashboard Audiences

| Audience | Needs |
|---|---|
| On-call engineer | Current incident diagnosis. |
| SRE | Reliability trends and saturation. |
| Engineering lead | Service health and quality posture. |
| Product owner | User journey impact. |
| Operations support | Actionable status and runbook links. |
| Executive | High-level availability and business impact. |

---

## Dashboard Layers

### Service Dashboard

Shows:

- request rate
- latency
- error rate
- health/readiness
- dependency failures
- saturation
- supportability states
- deployment version

### Workflow Dashboard

Shows:

- job states
- queue depth
- retry rates
- replay activity
- stuck work
- SLA breaches

### Data Product Dashboard

Shows:

- freshness
- quality
- certification
- telemetry recency
- consumer errors

### Product Journey Dashboard

Shows:

- UI/API path health
- gateway/domain dependency posture
- degraded panels
- permission-blocked states
- user-visible impact

---

## SLIs, SLOs, and SLAs

| Term | Meaning |
|---|---|
| SLI | Service-level indicator, the measurement. |
| SLO | Service-level objective, the target. |
| SLA | Service-level agreement, often contractual or formal. |

Example:

```text
SLI: percentage of successful supported portfolio summary requests under 500ms.
SLO: 99.5% over 30 days.
SLA: contractual commitment, if applicable.
```

---

## Useful SLIs

- availability
- latency
- error rate
- freshness
- completeness
- async completion time
- queue lag
- successful report generation rate
- dependency availability
- data-quality pass rate
- certification recency

---

## Error Budgets

An error budget is the allowed unreliability within an SLO.

Use error budgets to decide:

- whether to slow releases
- whether to prioritize reliability
- whether to improve dependencies
- whether to tune alerting
- whether to invest in resilience

---

## Dashboard Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Dashboard shows metrics not implemented | False confidence. |
| Too many panels | Operators cannot focus. |
| No service version | Deployment impact hard to assess. |
| No dependency view | Root cause unclear. |
| No supportability state | Business impact hidden. |
| Dynamic IDs as labels | Slow/expensive dashboards. |
| Dashboard not linked from runbook | Incident friction. |

---

## Review Checklist

- Who is the dashboard for?
- What questions does it answer?
- Are metrics implemented?
- Are labels safe?
- Are service versions shown?
- Are dependencies visible?
- Are supportability states shown?
- Are SLOs defined?
- Are alerts linked?
- Are runbooks linked?
- Are panels actionable?

---

## Summary

Dashboards should reduce uncertainty.

A good dashboard helps the right person make the right decision quickly.

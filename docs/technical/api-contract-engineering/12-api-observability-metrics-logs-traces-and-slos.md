# API Observability, Metrics, Logs, Traces, and SLOs

## Purpose

This file explains how to make APIs observable and supportable.

API observability should help teams understand usage, failures, latency, dependency health, supportability posture, and consumer impact.

## API Operation Vocabulary

Each route should map to a stable operation name.

Examples:

```text
portfolio.summary.read
performance.calculation.submit
report.job.replay
document.download
data_product.catalog.read
```

Stable operation names improve logs, metrics, dashboards, alerts, and runbooks.

## API Logs

Structured API logs should include:

- operation
- method
- route template
- status class
- reason code
- duration bucket
- caller application where safe
- dependency name where relevant
- supportability state
- correlation id

Avoid raw payloads and dynamic identifiers.

## API Metrics

Useful metrics:

```text
api_requests_total{operation,status_class}
api_request_duration_seconds_bucket{operation}
api_supportability_total{operation,state,reason}
api_dependency_calls_total{operation,dependency,status_class}
api_permission_blocked_total{operation,reason}
api_idempotency_conflicts_total{operation}
```

Labels must be bounded.

## Tracing

Trace propagation should connect:

```text
UI
  -> BFF
  -> domain service
  -> shared capability service
  -> database / upstream / queue
```

Trace attributes should be safe and bounded.

## SLOs

An API SLO defines expected service level.

Examples:

- availability
- latency
- error rate
- readiness
- freshness
- data completeness
- async completion time

Example:

```text
99.5% of portfolio summary API calls complete within 500ms over 30 days for supported requests.
```

## Error Budget

An error budget represents allowed unreliability.

Use it to decide:

- when to slow feature delivery
- when to focus on reliability
- when to improve dependencies
- when to tune alerts
- when to optimize performance

## Dashboards

API dashboards should show:

- request rate
- latency percentiles
- error rate
- status codes
- dependency failures
- supportability states
- permission-blocked rate
- stale/partial/degraded states
- idempotency conflicts
- async queue depth if applicable

## Alerts

Good API alerts are actionable.

Alert examples:

- high 5xx rate
- readiness failure
- dependency unavailable
- latency above SLO
- async work stuck
- stale data above threshold
- permission-blocked spike
- idempotency conflict spike

Each alert should link to a runbook.

## Observability Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Logs use raw paths with IDs | Hard aggregation and possible data leakage. |
| Metrics use dynamic IDs | Cardinality explosion. |
| No operation vocabulary | Dashboards inconsistent. |
| Traces not propagated through BFF | Cross-service debugging broken. |
| Dashboard uses non-existing metrics | False confidence. |
| Alert has no runbook | Incident response weak. |
| SLO not tied to user impact | Reliability goals become abstract. |

## Review Checklist

- Is operation vocabulary defined?
- Are route templates logged?
- Are metrics low-cardinality?
- Are supportability states measured?
- Is trace context propagated?
- Are BFF-to-domain calls visible?
- Are dependency failures measured?
- Are SLOs defined for critical APIs?
- Are dashboards accurate?
- Are alerts actionable?
- Are logs, metrics, and traces sensitive-data safe?

## Summary

API observability is the bridge between contract and operation.

A well-designed API can explain how it behaves in production without exposing sensitive data.

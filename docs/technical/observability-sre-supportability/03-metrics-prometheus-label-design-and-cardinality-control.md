# Metrics, Prometheus Label Design, and Cardinality Control

## Purpose

This file explains how to design metrics that are useful, safe, and scalable.

Metrics measure system behavior over time. Poor metric design can create high cost, noisy dashboards, and sensitive-data leakage.

---

## Metric Types

| Type | Use |
|---|---|
| Counter | Count events that only increase. |
| Gauge | Measure current value. |
| Histogram | Measure distributions such as latency. |
| Summary | Client-side distribution summaries where appropriate. |

---

## Metric Naming

Good names are:

- clear
- stable
- service or domain scoped
- unit-aware
- lowercase with underscores
- consistent

Examples:

```text
api_requests_total
api_request_duration_seconds
dependency_calls_total
worker_items_total
supportability_state_total
replay_attempts_total
```

---

## Label Design

Labels should be bounded.

Good labels:

- operation
- status_class
- state
- reason
- dependency
- worker
- freshness_bucket
- result

Bad labels:

- client_id
- account_id
- portfolio_id
- transaction_id
- document_id
- user_id
- trace_id
- correlation_id
- raw_error_message
- request_path_with_ids
- prompt text

---

## Cardinality

High cardinality means too many unique label combinations.

High-cardinality metrics can:

- overload monitoring systems
- increase cost
- make dashboards slow
- hide useful patterns
- leak sensitive identifiers

Keep labels limited and controlled.

---

## Common Metrics

### API Metrics

```text
api_requests_total{operation,status_class}
api_request_duration_seconds_bucket{operation}
api_permission_blocked_total{operation,reason}
```

### Dependency Metrics

```text
dependency_calls_total{dependency,operation,status_class}
dependency_call_duration_seconds_bucket{dependency,operation}
```

### Worker Metrics

```text
worker_items_total{worker,state}
worker_queue_depth{worker}
worker_lag_seconds{worker}
```

### Supportability Metrics

```text
supportability_state_total{operation,state,reason,freshness_bucket}
```

---

## Metric Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Label includes portfolio id | Sensitive-data and cardinality risk. |
| Label includes raw error | Cardinality explosion. |
| Metric name changes frequently | Dashboard breakage. |
| Dashboard uses planned metric | False confidence. |
| Counter used for current value | Incorrect interpretation. |
| Gauge used for event count | Incorrect trend. |
| No unit in metric name | Confusing dashboards. |

---

## Review Checklist

- Are metric names stable?
- Are units clear?
- Are labels bounded?
- Are sensitive identifiers excluded?
- Are histograms used for latency?
- Are dependency metrics present?
- Are supportability states measured?
- Are worker metrics present where applicable?
- Are dashboards tied to implemented metrics?
- Are metrics tested or validated?

---

## Summary

Metrics should help teams see system behavior safely and cheaply.

A metric that leaks sensitive data or creates uncontrolled cardinality is a production risk.

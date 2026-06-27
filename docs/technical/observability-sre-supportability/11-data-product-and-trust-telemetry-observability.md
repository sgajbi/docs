# Data Product and Trust Telemetry Observability

## Purpose

This file explains how to observe data products, trust telemetry, freshness, quality, certification, and consumer impact.

Data products need operational visibility just like services.

---

## Data Product Observability Questions

- Is the product available?
- Is it fresh?
- Did quality checks pass?
- Is lineage available?
- Is certification current?
- Are consumers failing?
- Is access policy working?
- Is telemetry present?
- Is evidence safe and current?

---

## Trust Telemetry

Trust telemetry may include:

- product name
- version
- producer
- generated timestamp
- supportability state
- freshness bucket
- quality state
- lineage availability
- evidence reference
- certification state
- SLO posture
- access policy posture
- known blockers

---

## Metrics

Examples:

```text
data_product_freshness_state_total{product,state}
data_product_quality_state_total{product,state}
data_product_certification_state_total{product,state}
data_product_telemetry_age_seconds{product}
data_product_consumer_errors_total{product,consumer,status_class}
```

Labels must be bounded and safe.

---

## Dashboards

A data-product dashboard should show:

- certified products
- stale products
- quality failures
- missing telemetry
- expired certification
- consumer error rates
- access denials
- dependency failures
- evidence recency

---

## Alerts

Useful alerts:

- certified product becomes stale
- quality check fails
- telemetry missing
- certification expired
- consumer error rate spikes
- access-policy validation fails
- lineage unavailable

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Certification not monitored | Trust decays. |
| Static telemetry treated as live | False posture. |
| Consumer impact missing | Outage triage weak. |
| Freshness hidden | Stale data used as current. |
| Data quality only in batch logs | Consumers cannot react. |
| Trust dashboard exposes restricted evidence | Security risk. |

---

## Review Checklist

- Is trust telemetry produced?
- Is freshness measured?
- Is quality measured?
- Is certification recency monitored?
- Are consumers visible?
- Are access failures measured?
- Are dashboards safe?
- Are alerts actionable?
- Are evidence references current?
- Are runbooks linked?

---

## Summary

A data product is not truly trusted unless its trust posture is observable.

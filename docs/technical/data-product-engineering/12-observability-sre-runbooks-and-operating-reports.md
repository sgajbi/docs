# Observability, SRE, Runbooks, and Operating Reports

## Purpose

This file explains how data products should be operated and supported.

A certified data product must remain observable after certification. Operators need to understand current posture, degradation, consumers, incidents, and historical trend.

---

## Data Product Observability

Useful signals:

- product availability
- freshness
- quality state
- validation failures
- reconciliation failures
- telemetry recency
- access denials
- consumer errors
- API latency
- event delivery lag
- batch delivery status
- certification status
- degradation trend

---

## Metrics

Example metrics:

```text
data_product_freshness_state_total{product,state}
data_product_quality_state_total{product,state}
data_product_certification_state_total{product,state}
data_product_consumer_errors_total{product,consumer,status_class}
data_product_reconciliation_failures_total{product}
data_product_access_denials_total{product,reason}
```

Use bounded labels. Avoid client/account/portfolio identifiers.

---

## Dashboards

Dashboard should answer:

- which products are certified?
- which products are stale?
- which products failed quality checks?
- which products have consumer errors?
- which certifications expired?
- which telemetry snapshots are missing?
- which products have access-policy issues?
- which consumers are impacted?

---

## Alerts

Alert examples:

- certified product becomes stale
- quality validation fails
- reconciliation fails
- telemetry missing
- certification expires
- access-policy validation fails
- high consumer error rate
- batch delivery missed SLA
- event lag exceeds threshold

Alerts should link to runbooks.

---

## Runbooks

A data-product runbook should include:

- product identity
- producer owner
- consumers
- source systems
- freshness expectation
- quality checks
- dashboards
- alerts
- first diagnosis steps
- common failure modes
- mitigation
- consumer notification
- rollback/replay
- escalation contacts
- evidence locations
- data-safety notes

---

## Operating Report

An operating report summarizes product posture.

It may include:

- certification state
- current quality state
- freshness posture
- recent incidents
- consumer impact
- drift trend
- telemetry availability
- policy compliance
- residual risks
- recommended actions

Operating reports are internal operational evidence, not automatically customer-facing evidence.

---

## SRE Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Certified once, never monitored | Trust decays silently. |
| Telemetry missing but product still shown certified | False trust. |
| Alerts without runbooks | Operators cannot act. |
| Quality failures hidden from consumers | Incorrect downstream behavior. |
| Operating report exported to customer without filtering | Information leakage. |
| No consumer impact view | Outage triage weak. |

---

## Review Checklist

- Are data-product metrics defined?
- Are labels bounded and safe?
- Are dashboards current?
- Are alerts actionable?
- Are runbooks complete?
- Is telemetry recency monitored?
- Is certification expiry monitored?
- Is consumer impact visible?
- Are operating reports generated?
- Is customer evidence filtered?

---

## Summary

Data products need operations, not only contracts.

A trusted product should stay trusted through monitoring, runbooks, evidence, and continuous review.

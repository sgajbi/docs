# Alerting, Severity, Routing, and Noise Control

## Purpose

This file explains how to design actionable alerts.

Alerts should call humans only when action is required. Noisy alerts train teams to ignore real problems.

---

## Alert Design

A good alert includes:

- condition
- threshold
- duration
- severity
- owner
- impact
- runbook link
- dashboard link
- routing rule
- suppression/deduplication where appropriate

---

## Severity Levels

Example severity model:

| Severity | Meaning |
|---|---|
| SEV1 | Major client/business impact or critical production outage. |
| SEV2 | Significant degradation or partial outage. |
| SEV3 | Limited impact, support action needed. |
| SEV4 | Low urgency, backlog or business-hours action. |

Severity should reflect impact, not technical emotion.

---

## Alert Types

Useful alerts:

- readiness failure
- high 5xx rate
- latency SLO burn
- dependency unavailable
- queue lag
- worker stuck
- stale data
- reconciliation failure
- certification expired
- high retry rate
- high permission-blocked spike
- storage/capacity threshold
- container crash loop

---

## Alert Routing

Routing should consider:

- owning team
- service
- severity
- business hours/on-call
- region
- platform versus application ownership
- escalation path

---

## Noise Reduction

Reduce noise through:

- sensible thresholds
- duration windows
- grouping
- deduplication
- maintenance windows
- suppress downstream symptom alerts when root alert exists
- alert on user impact rather than every internal fluctuation
- regularly review noisy alerts

---

## Alert Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Alert without runbook | Human does not know what to do. |
| Alert on every warning log | Alert fatigue. |
| Alert routes to wrong team | Slow response. |
| No severity model | Poor prioritization. |
| Alert on symptoms only | Multiple noisy pages. |
| No alert review | Noise accumulates. |
| No business impact in alert | Escalation unclear. |

---

## Review Checklist

- Is alert actionable?
- Is severity appropriate?
- Is owner defined?
- Is threshold justified?
- Is duration set?
- Is runbook linked?
- Is dashboard linked?
- Is routing correct?
- Is deduplication needed?
- Is alert safe from sensitive data?
- Is alert tested or validated?

---

## Summary

Good alerts protect people as well as systems.

Alert only when action is needed, and give responders the context to act.

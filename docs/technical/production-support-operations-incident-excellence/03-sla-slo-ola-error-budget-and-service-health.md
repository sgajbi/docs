# SLA, SLO, OLA, Error Budget, and Service Health

## Purpose

This file explains service-level concepts used to manage reliability and support expectations.

SLAs, SLOs, OLAs, and error budgets help align business expectations with engineering operations.

---

## Definitions

| Term | Meaning |
|---|---|
| SLA | Contractual commitment to customer or business. |
| SLO | Internal reliability target used to manage service health. |
| SLI | Measurable indicator used to evaluate an SLO. |
| OLA | Internal operational agreement between teams. |
| Error budget | Allowed unreliability under an SLO. |

---

## Common SLIs

Examples:

- availability
- request latency
- error rate
- report generation success
- batch completion time
- data freshness
- reconciliation completion
- incident response time
- restore time
- successful archive retrieval

---

## Example SLOs

```text
99.9% of portfolio-summary requests return successfully over 30 days.

95% of supported report jobs complete within 5 minutes during business hours.

Critical EOD data products reach CURRENT freshness by 08:00 local business time.

99% of archived documents are retrievable within 2 seconds during business hours.
```

---

## Error Budget

Error budgets help balance reliability and change.

If service burns too much error budget:

- slow risky releases
- prioritize reliability work
- review incidents
- improve tests/alerts/runbooks
- reduce operational risk

---

## Service Health

Service health should combine:

- technical availability
- latency
- errors
- dependency state
- data freshness
- job health
- supportability states
- user impact
- known incidents

---

## SLA/SLO Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| SLA promised before system measured | Unrealistic commitments. |
| SLO not tied to user impact | Poor reliability focus. |
| Only uptime measured | Data/report failures hidden. |
| Error budget ignored | No governance effect. |
| OLA missing for dependencies | Internal escalation weak. |
| Metrics cannot measure SLO | SLO decorative. |

---

## Review Checklist

- Are SLIs defined?
- Are SLOs measurable?
- Are SLAs realistic?
- Are OLAs defined for dependencies?
- Are error budgets used?
- Are data/report SLOs included where relevant?
- Are dashboards aligned with SLOs?
- Are alerts tied to user impact?
- Is service health reviewed regularly?

---

## Summary

Reliability expectations should be measurable and operationally meaningful.

SLOs turn support from reactive firefighting into managed service health.

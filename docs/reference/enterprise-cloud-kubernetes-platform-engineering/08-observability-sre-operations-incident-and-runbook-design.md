<!--
Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems Reference Pack
Generated: 2026-06-27
Purpose: Professional study, architecture, platform design, project delivery, advisory/reporting/analytics integration, and future reference.
Disclaimer: Educational and architecture reference only. Not cloud, security, regulatory, legal, or investment advice.
-->

# 08 - Observability, SRE, Operations, Incident and Runbook Design

## 1. Observability purpose

Observability answers:

- Is the service healthy?
- Is the business capability healthy?
- What changed?
- What data is stale?
- Which dependency is failing?
- Which portfolios/reports/jobs are affected?
- Can support explain the issue to business users?
- Can we recover safely?

## 2. Three pillars plus business observability

| Signal | Example |
|---|---|
| Logs | structured events, errors, audit details |
| Metrics | latency, throughput, failures, job duration |
| Traces | request flow across services |
| Business metrics | stale prices, failed reports, unmatched breaks, delayed EOD |
| Audit events | who did what, when and why |
| Data quality metrics | completeness, freshness, reconciliation status |

## 3. Wealth observability metrics

| Capability | Metrics |
|---|---|
| Portfolio API | latency, error rate, cache freshness, data-source age |
| Transactions API | page latency, query time, result count, pagination mode |
| Performance engine | job duration, failed portfolios, cashflow classification errors |
| Report generation | queue depth, render time, PDF failure, archive failure |
| Market data | price freshness, missing instruments, vendor delay |
| Buying power | collateral feed age, exposure feed age, rule failures |
| Advisory | proposal failures, suitability check outcomes |
| AI copilot | retrieval precision, groundedness, unsafe answer blocks, tool failures |
| Reconciliation | breaks by type, aged breaks, certification status |

## 4. Logs

Structured logs should include:

- timestamp
- service name
- environment
- version/build SHA
- correlation ID
- trace ID
- client/account/portfolio identifiers where allowed
- business date
- event type
- severity
- source system
- error code
- sanitized message
- no secrets or sensitive raw payloads unless approved

## 5. Tracing

Trace important workflows:

```text
Advisor UI
  -> API gateway
  -> Portfolio API
  -> Entitlement service
  -> Holdings service
  -> Valuation service
  -> Market data cache
  -> Database
```

For async workflows, propagate correlation IDs through events and jobs.

## 6. Alerts

Alert on symptoms, not noise.

Good alerts:

- business capability unavailable
- p95 latency above SLO
- error rate above threshold
- report generation backlog exceeds SLA
- market data freshness breach
- EOD job not complete by cut-off
- reconciliation breaks exceed threshold
- portfolio valuation missing
- AI guardrail blocks spike unexpectedly
- secret/certificate expiry approaching

Bad alerts:

- every pod restart
- every transient timeout
- debug-level logs
- raw CPU alert without service impact
- duplicate alerts without ownership

## 7. SLO examples

| Service | SLO |
|---|---|
| Portfolio summary API | 99.9% availability during advisory hours, p95 < 500 ms |
| Transactions API | p95 < 1.5 s for paginated query |
| Performance calculation | complete EOD within agreed batch window |
| Report generation | 99% reports generated within SLA |
| Buying power | p95 < 300 ms for pre-trade check |
| AI portfolio explanation | cite grounded sources for 99% approved-answer outputs |

## 8. Runbook structure

Every runbook should include:

- symptom
- impact
- dashboard links
- alert names
- first checks
- dependency checks
- safe restart steps
- rollback steps
- data validation checks
- business communication template
- escalation path
- recovery validation
- post-incident evidence

## 9. Incident management

Incident phases:

1. detect
2. triage
3. assess business impact
4. contain
5. restore service
6. validate financial correctness
7. communicate
8. investigate root cause
9. fix permanently
10. update controls/runbooks/tests

For wealth systems, restoration is not enough. You must validate data correctness.

## 10. Problem management

Common root causes:

- missing market prices
- stale FX rates
- source feed delay
- schema mismatch
- entitlement data issue
- unhandled corporate action
- resource exhaustion
- downstream timeout
- bad release
- non-idempotent replay
- cache inconsistency

Problem records should identify prevention:

- test added
- monitor added
- runbook updated
- control strengthened
- data contract improved
- replay made idempotent
- deployment guardrail added

## 11. Production support dashboard

Recommended panels:

- service health
- API latency/error rate
- dependency health
- job status
- report queue
- stale data indicators
- reconciliation breaks
- source-feed arrival
- pod/node health
- recent deployments
- error budget
- top client/portfolio impact
- incidents and aged problems

## 12. Practitioner summary

A strong answer:

> Observability for wealth platforms must go beyond infrastructure metrics. It must include business-date health, data freshness, report-generation status, calculation completeness, reconciliation breaks and audit lineage. SRE practices should define SLOs, alerts, runbooks and incident response, but recovery must always include validation of financial correctness.

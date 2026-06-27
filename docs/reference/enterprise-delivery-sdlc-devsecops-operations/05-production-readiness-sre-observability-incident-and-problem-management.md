# Production Readiness, SRE, Observability, Incident and Problem Management

## 1. Production readiness mindset

A feature is not production-ready just because tests pass. It is production-ready when the team can:

- deploy it safely
- detect failures quickly
- understand impact
- recover within expected time
- explain the issue to stakeholders
- prevent recurrence

For wealth platforms, production issues can affect client reporting, advisory workflows, order readiness, valuation, performance returns, suitability checks, cash availability and regulatory evidence.

## 2. Production readiness checklist

| Area | Checklist |
|---|---|
| Functionality | Acceptance criteria and regression complete. |
| Data | Source ownership, lineage and reconciliation defined. |
| Performance | Latency, throughput and batch windows validated. |
| Resilience | Retry, idempotency, replay and failure handling tested. |
| Observability | Metrics, logs, traces and dashboards ready. |
| Alerts | Alert thresholds, severity and routing defined. |
| Runbooks | Triage, recovery and escalation documented. |
| Security | Access, secrets, vulnerability and logging controls checked. |
| Release | Deployment, rollback and feature flags documented. |
| Support | L1/L2/L3 ownership and escalation path agreed. |
| Business | Known limitations and stakeholder communications ready. |

## 3. SRE concepts for wealth platforms

| Concept | Meaning |
|---|---|
| Service Level Indicator | Quantitative measure of service behaviour, such as latency or success rate. |
| Service Level Objective | Target level for an SLI. |
| Error budget | Acceptable unreliability within a time window. |
| Toil | Manual repetitive work that should be automated. |
| Blameless postmortem | Incident review focused on system improvement, not personal blame. |
| Reliability engineering | Designing services to withstand expected failures. |

## 4. Observability model

Observability needs three technical signals plus business context.

| Signal | Example |
|---|---|
| Metrics | request count, latency, error rate, job duration, queue lag. |
| Logs | structured events with correlation ID and business identifiers. |
| Traces | end-to-end request flow across services. |
| Business events | portfolio ID, report ID, valuation date, product type, source system, rule version. |

## 5. Wealth-specific observability examples

| Service | Key metrics |
|---|---|
| Portfolio API | latency, error rate, cache hit ratio, result count, page size. |
| Transaction ingestion | events consumed, duplicates, rejects, lag, replay count. |
| Performance engine | calculation duration, failed portfolios, restatement count. |
| Report renderer | report generation time, failed PDFs, archive success. |
| Suitability engine | pass/block counts, override counts, rule version. |
| Valuation service | stale prices, missing FX, pricing source fallback. |
| Collateral engine | LTV breaches, margin calls, in-flight order impacts. |

## 6. Log design

Good logs are structured and actionable.

Minimum fields:

- timestamp
- service
- environment
- correlation_id
- request_id
- user/system actor
- portfolio/account/instrument identifiers where allowed
- event type
- status
- error code
- source system
- rule/model/version
- latency/duration
- sanitized message

Do not log sensitive client data unless explicitly allowed and masked.

## 7. Alert design

Bad alert: "API error."

Good alert:

```text
Portfolio transaction API p95 latency > 2s for 10 minutes
Impact: advisor transaction history may be delayed
Scope: SG booking centre
Runbook: /runbooks/portfolio-transaction-api-latency
Owner: Portfolio API L2
```

Alert fields:

- condition
- severity
- business impact
- affected service
- affected region/booking centre
- runbook link
- escalation owner
- dashboard link

## 8. Incident management

Incident phases:

| Phase | Action |
|---|---|
| Detect | Alert, user report, monitoring or reconciliation identifies issue. |
| Triage | Determine scope, severity, impacted clients/business process. |
| Contain | Stop spread, disable feature, rollback, pause job, block report. |
| Communicate | Inform stakeholders with clear status and next update. |
| Recover | Restore service or correct data. |
| Validate | Confirm recovery with technical and business checks. |
| Close | Document timeline and residual risks. |
| Learn | Conduct postmortem and create prevention actions. |

## 9. Incident severity model

| Severity | Example |
|---|---|
| Sev 1 | Client reports materially wrong across many clients; order submission blocked globally. |
| Sev 2 | Major booking centre affected; performance unavailable; valuation batch missed cut-off. |
| Sev 3 | Limited portfolios affected; workaround available. |
| Sev 4 | Minor issue, internal-only, no client impact. |

Severity should consider business impact, not just technical error count.

## 10. Problem management

Problem management prevents recurrence.

A problem record should include:

- incident summary
- root cause
- contributing factors
- detection gap
- prevention action
- regression test added
- monitoring improvement
- documentation/runbook update
- owner
- due date

## 11. Runbook structure

Runbook template:

```markdown
# Runbook: <service / issue>

## Symptoms

## Business impact

## Dashboards

## Common causes

## Triage steps

## Recovery steps

## Data correction / replay steps

## Escalation

## Communication template

## Post-recovery validation

## Related incidents
```

## 12. Wealth operations examples

### Stale price before report generation

Expected behaviour:

- report generated with stale-price warning, or blocked based on policy
- affected instruments identified
- price source and timestamp shown
- operations workflow created
- report archive records degraded state
- no silent substitution without lineage

### Duplicate transaction event

Expected behaviour:

- idempotency key detects duplicate
- duplicate is rejected or ignored
- audit record created
- position not doubled
- reconciliation remains clean

### Failed performance recalculation

Expected behaviour:

- job status visible
- failed portfolio isolated
- retry/replay supported
- previous approved result remains available with data-as-of label
- report shows exception if needed

## 13. Service ownership

Each service should have:

- product owner
- engineering owner
- support owner
- source-data owner
- runbook owner
- escalation channel
- service catalog entry
- SLOs
- dashboards
- dependencies

## 14. Operational readiness review

Before go-live ask:

- What happens if upstream is late?
- What happens if downstream is unavailable?
- What happens if the job partially fails?
- Can we replay safely?
- Can we rollback safely?
- How do we know clients are impacted?
- Can support explain the issue?
- Is there a manual workaround?
- What data corrections might be required?
- What report disclaimers are needed?

## 15. Practitioner explanation

> "For production readiness, I look beyond deployment. I want service ownership, SLOs, dashboards, structured logs, correlation IDs, business-impact alerts, runbooks, rollback or replay procedures, and post-incident regression. In wealth platforms, operational readiness must include product and data impact: stale prices, failed valuations, duplicate transactions, performance restatements and report exceptions need clear handling and lineage."

# 07 - Resilience, Observability, Security and Operational Controls

## 1. Why operational architecture matters

A wealth platform is client-facing and control-sensitive. A small failure can create incorrect statements, wrong buying power, unsuitable trades, mandate breaches or advisor confusion.

Architecture must support:

- high availability
- controlled degradation
- disaster recovery
- data recovery
- observability
- secure access
- audit trail
- incident response
- reconciliation
- operational dashboards

## 2. Reliability principles

| Principle | Wealth-platform meaning |
|---|---|
| Fail safely | Do not show misleading portfolio values or allow unsuitable orders. |
| Degrade explicitly | Show stale/provisional/missing data states. |
| Isolate failure | A report-rendering issue should not stop transaction intake. |
| Retry carefully | Avoid duplicate orders, transactions or client notifications. |
| Recover deterministically | Use idempotency, replay and lineage. |
| Monitor business health | Technical uptime is not enough; monitor data freshness and output correctness. |

## 3. Critical platform SLOs

| Capability | Example SLO |
|---|---|
| Advisor portfolio summary | 99.9% availability during business hours. |
| Order capture | High availability with strict idempotency and audit. |
| Pre-trade checks | Low latency and deterministic response. |
| EOD positions | Available by defined operational cut-off. |
| Market prices | Freshness by asset class/source. |
| Performance calculation | Completed before reporting cut-off. |
| Statement generation | Completed within batch window. |
| Event processing | Lag below defined threshold. |
| Reconciliation | Breaks identified before sign-off. |

## 4. Resilience patterns

| Pattern | Use |
|---|---|
| Timeout | Avoid hanging user/API calls. |
| Retry with backoff | Recover transient dependency failures. |
| Circuit breaker | Stop cascading failures. |
| Bulkhead | Isolate heavy report/calculation workloads. |
| Idempotency key | Prevent duplicate state changes. |
| Outbox/inbox | Reliable event publishing/consumption. |
| Dead-letter queue | Isolate poison messages. |
| Snapshot and replay | Recover derived state. |
| Graceful degradation | Continue with caveats where safe. |
| Feature flags | Controlled rollout and rollback. |

## 5. Business degradation examples

| Failure | Safe behavior |
|---|---|
| Missing price for one illiquid bond | Show valuation warning or block official report depending policy. |
| Market data delayed | Display stale flag and source timestamp. |
| Performance job failed | Show prior result only if clearly marked and allowed. |
| Mandate engine unavailable | Block discretionary order placement or route to manual approval. |
| Entitlement service unavailable | Fail closed for sensitive data access. |
| Report rendering failed | Keep snapshot, allow regeneration without recalculating inputs. |
| Event consumer lagging | Show operational alert and lag metrics. |

## 6. Observability model

Use three layers:

| Layer | Examples |
|---|---|
| Technical telemetry | Logs, metrics, traces, latency, error rate, saturation. |
| Data telemetry | Feed freshness, record counts, validation failures, stale prices, missing FX. |
| Business telemetry | Report readiness, calculation completion, mandate breaches, reconciliation breaks. |

OpenTelemetry-style instrumentation helps standardize traces, metrics and logs across services.

## 7. Key metrics

### API metrics

- request count
- error rate
- latency p50/p95/p99
- timeout count
- rate-limit count
- entitlement-denied count
- dependency latency

### Event metrics

- topic lag
- consumer lag
- dead-letter count
- event validation failures
- duplicate event count
- replay job status

### Data-quality metrics

- missing price count
- stale price count
- missing FX count
- unmatched instrument count
- transaction reconciliation breaks
- position breaks
- NAV lag count
- benchmark missing dates

### Calculation metrics

- job duration
- failed jobs
- recalculation jobs pending
- portfolios impacted by correction
- outlier returns
- material restatement count

### Reporting metrics

- reports generated
- reports failed
- reports blocked by data quality
- average generation time
- archive failures
- client delivery failures

## 8. Logging standards

Structured logs should include:

```text
timestamp
level
service
environment
correlation_id
trace_id
span_id
user_or_service_id
client_id/account_id/portfolio_id where allowed
event_id/job_id/report_id/order_id
business_date
status
error_code
message
```

Avoid logging sensitive data such as full account numbers, personal identifiers, secrets or unrestricted client details.

## 9. Security architecture

Security should cover:

- identity and authentication
- authorization and data scoping
- service-to-service authentication
- secrets management
- encryption in transit and at rest
- secure API gateway policies
- audit logging
- least privilege
- privileged access management
- data masking/redaction
- secure SDLC
- vulnerability scanning
- dependency management
- incident response

## 10. Access-control model

| Access dimension | Example |
|---|---|
| User role | Advisor, portfolio manager, assistant, operations, compliance. |
| Relationship | Advisor assigned to client or household. |
| Booking centre | User can access only permitted booking locations. |
| Product permission | User can view/trade only approved product classes. |
| Action permission | View, propose, trade, approve, override, generate report. |
| Data sensitivity | PII, tax, suitability, trust beneficiary, portfolio value. |
| Delegation | Temporary delegation or assistant access. |

Entitlements must be enforced in APIs, not just UI.

## 11. Operational runbooks

Every critical capability should have runbooks for:

- feed delay
- stale prices
- missing FX
- failed event consumer
- stuck calculation jobs
- failed report batch
- reconciliation break spike
- corrupted input file
- duplicate transactions
- entitlement outage
- incident communication
- rollback/replay procedure

## 12. Disaster recovery and recovery objectives

Define:

| Term | Meaning |
|---|---|
| RTO | Maximum acceptable recovery time. |
| RPO | Maximum acceptable data loss. |
| Backup strategy | What data is backed up, frequency, retention. |
| Replay strategy | Which streams/files can rebuild state. |
| Regional failover | How application and data fail over. |
| Report recovery | How official statements are recovered/regenerated. |

## 13. Control dashboards

Recommended dashboards:

- platform health
- source feed freshness
- event lag
- calculation job status
- reconciliation breaks
- report readiness
- stale price/NAV dashboard
- API latency/error dashboard
- mandate breach dashboard
- failed order/pre-trade check dashboard
- data override dashboard

## 14. Production readiness checklist

- Health checks implemented.
- Readiness/liveness checks meaningful.
- Metrics and traces instrumented.
- Logs structured and searchable.
- Runbooks available.
- Alerts tested.
- Idempotency implemented for mutating operations.
- Backup/replay tested.
- DR test performed.
- Security review completed.
- Access-control tests passed.
- Reconciliation checks implemented.
- Data-quality dashboard available.
- Support ownership and escalation path defined.

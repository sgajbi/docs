# Operational Incident And Supportability Case Study

## Purpose

This case study shows how observability, SRE practice, supportability states, runbooks, incident management, QA and engineering follow-up work together during a real platform incident.

The example is application-neutral, but it is shaped around common wealth-management workflows: report generation, portfolio snapshots, data freshness, async workers, client-facing delivery and regulated operational evidence.

## Scenario

A batch of quarterly portfolio reports is delayed. The report request API is healthy, but report generation workers are falling behind because one upstream price-source feed delivered late corrections for funds and structured products. Advisors can still open portfolios, but some reports show stale valuation posture and cannot be delivered to clients until snapshots are regenerated.

## Systems Involved

| System Area | Responsibility |
|---|---|
| Report request API | Accepts report requests, returns request status and exposes supportability state. |
| Portfolio snapshot service | Builds point-in-time holdings, valuation, performance and disclosure snapshots. |
| Market data service | Supplies prices, NAVs, FX rates and stale/freshness metadata. |
| Async report worker | Renders reports and stores output artifacts. |
| Notification/delivery service | Sends report-ready notifications and delivery status. |
| Operator dashboard | Shows queue depth, failed jobs, stale valuation counts and report delivery posture. |

## Expected Supportability States

| Surface | Normal State | Incident State | User Meaning |
|---|---|---|---|
| Portfolio overview | `READY` | `PARTIAL` | Holdings are visible, but some valuation-dependent sections may be pending. |
| Report request API | `READY` | `DEGRADED` | Requests are accepted, but completion may be delayed. |
| Report status | `READY` | `STALE` or `PARTIAL` | Existing report snapshot may not reflect latest accepted prices. |
| Report delivery | `READY` | `UNAVAILABLE` for affected reports | Delivery is held until regenerated snapshot passes validation. |
| Operator dashboard | `READY` | `DEGRADED` | Incident is active; queue and data freshness require operator attention. |

## Detection

| Signal | Example Threshold | Why It Matters |
|---|---|---|
| `report_worker_queue_depth` | Above normal baseline for 20 minutes. | Shows report generation is not keeping up. |
| `report_job_duration_seconds` | 95th percentile above SLO. | Shows client-facing report readiness is delayed. |
| `market_data_stale_instrument_count` | Increases after upstream correction feed. | Explains why snapshots are held or recomputed. |
| `report_snapshot_regeneration_total` | Spike after price correction event. | Confirms reports are being reprocessed. |
| `report_delivery_blocked_total` | Non-zero for approved reports. | Indicates client delivery is blocked by quality controls. |
| Error logs with `reason_code=PRICE_FRESHNESS_BLOCKED` | Repeated for same batch. | Identifies a controlled degradation, not a rendering defect. |

## Structured Log Example

```json
{
  "event": "report.snapshot.generation.blocked",
  "operation": "generate_portfolio_report",
  "correlation_id": "corr-2026-06-28-1042",
  "report_request_id": "rr-842991",
  "portfolio_id_hash": "pfh_7f3a91",
  "as_of_date": "2026-06-30",
  "supportability_state": "STALE",
  "reason_code": "PRICE_FRESHNESS_BLOCKED",
  "dependency": "market_data",
  "affected_product_family": "funds",
  "retryable": true,
  "safe_for_client_delivery": false
}
```

Do not log client names, account numbers, full portfolio identifiers, holdings-level sensitive details or report document contents.

## Incident Timeline

| Time | Event | Owner | Evidence |
|---|---|---|---|
| T+00 | Alert fires for report worker backlog and blocked deliveries. | On-call engineer | Alert ID, dashboard snapshot and queue metric. |
| T+05 | Triage confirms API is available but report generation is degraded. | Incident commander | Incident note with scope and supportability state. |
| T+10 | Market data correction feed identified as trigger. | Data operations | Feed arrival timestamp and stale instrument count. |
| T+20 | Delivery hold confirmed for affected report requests. | Operations lead | Blocked delivery count and report status sample. |
| T+30 | Worker concurrency increased within approved limit. | Platform/SRE | Change record and worker metric improvement. |
| T+45 | Snapshot regeneration completes for first affected segment. | Reporting service owner | Regeneration metric and QA checks. |
| T+75 | Report delivery resumes for reconciled reports. | Operations lead | Delivery success metric and supportability state update. |
| T+120 | Incident resolved; post-incident review opened. | Incident commander | Resolution note, action items and evidence links. |

## Runbook Actions

| Step | Action | Decision Rule |
|---|---|---|
| 1 | Confirm whether the incident is API availability, worker backlog, upstream data freshness or renderer failure. | Do not restart services until the failure mode is known. |
| 2 | Check supportability states returned by API and operator dashboard. | If states are hidden or inconsistent, escalate as supportability defect. |
| 3 | Identify affected product families, report types and as-of dates. | Prioritize client-facing delivery windows and regulated statements. |
| 4 | Hold delivery for reports with stale or unproven valuation inputs. | Never deliver a report if freshness controls mark it unsafe. |
| 5 | Verify whether worker scaling is safe under database, storage and renderer limits. | Scale only within tested concurrency limits. |
| 6 | Trigger or monitor snapshot regeneration after source freshness is restored. | Regenerated snapshots must pass valuation and report QA checks. |
| 7 | Communicate status using business language. | Say reports are delayed due to valuation freshness controls, not vague technical backlog. |
| 8 | Close only when queue, snapshot, delivery and supportability metrics prove recovery. | Resolution requires evidence across the workflow, not one green health endpoint. |

## Customer And Advisor Communication

| Audience | Message Shape |
|---|---|
| Advisor | Some report packages are delayed while valuation corrections are incorporated. Portfolio views remain available, but delivery is held until report snapshots pass freshness checks. |
| Operations | Affected requests are grouped by as-of date, product family and report type. Delivery should not be manually forced unless exception approval is recorded. |
| Product owner | The workflow protected clients from stale valuation reports. Recovery depends on source correction adoption, snapshot regeneration and delivery retry. |
| Engineering lead | The incident validates the need for queue SLOs, source-freshness telemetry, controlled degradation and supportability-state tests. |

## Recovery Evidence

| Evidence | Required Result |
|---|---|
| API health | Liveness and readiness are healthy. |
| Queue metrics | Queue depth and job age return within SLO. |
| Source freshness | Affected instruments have accepted current prices or approved override posture. |
| Snapshot regeneration | Affected snapshots regenerated with latest source version. |
| Report QA | Totals, holdings, performance, disclosures and delivery metadata pass checks. |
| Delivery metrics | Blocked delivery count returns to zero or documented accepted exceptions. |
| Supportability state | Affected APIs and dashboards return `READY` or documented `PARTIAL` for known exceptions. |
| Incident record | Timeline, impact, mitigation, root cause and follow-up actions are recorded. |

## Regression Tests To Add

| Test | Purpose |
|---|---|
| Report request remains accepted during worker backlog. | Prevents unnecessary API outage behavior. |
| Stale valuation blocks client delivery. | Proves freshness controls protect reports. |
| Report status exposes degraded reason code. | Makes supportability state explainable. |
| Snapshot regeneration uses latest source version. | Prevents stale report output after correction. |
| Delivery retry is idempotent. | Prevents duplicate notifications or duplicate archive records. |
| Worker scaling respects concurrency limits. | Protects downstream dependencies during recovery. |
| Telemetry contains no sensitive client content. | Prevents incident logs from becoming data leakage. |

## Post-Incident Improvements

| Improvement | Owner | Why It Matters |
|---|---|---|
| Add dashboard panel for report backlog by as-of date and product family. | SRE/platform | Speeds triage and business impact assessment. |
| Add explicit `PRICE_FRESHNESS_BLOCKED` supportability reason to report status API. | Reporting service owner | Makes delay explainable to operators and advisors. |
| Add source-correction replay runbook. | Data operations | Standardizes response to late price and NAV corrections. |
| Add delivery-hold certification test. | QA | Prevents unsafe report delivery after stale data. |
| Add incident drill for quarterly reporting. | Operations and engineering leads | Builds readiness before critical reporting windows. |
| Add capacity scorecard for report workers. | Platform/SRE | Prevents queue backlog during known reporting peaks. |

## Engineering Lessons

1. A healthy API does not prove the workflow is healthy.
2. Supportability state must describe the business capability, not only process uptime.
3. Report delivery should depend on data freshness and snapshot evidence.
4. Queue metrics need age, depth, retry, failure and business-scope labels.
5. Scaling workers is a controlled mitigation, not a substitute for source and snapshot correctness.
6. Incidents should produce better tests, runbooks, dashboards and reason codes.
7. Sensitive client and portfolio content must stay out of logs, traces, alerts and incident tickets.

## Review Checklist

Use this checklist when reviewing an incident or production-readiness package:

1. Can operators identify the affected business workflow within five minutes?
2. Do APIs and dashboards expose a consistent supportability state?
3. Are reason codes stable enough for support, alerting and QA?
4. Is delivery blocked when report evidence is stale or incomplete?
5. Are async workers observable by queue age, retries, failures and business scope?
6. Can recovery be proven from metrics, logs, traces, report snapshots and QA checks?
7. Are incident communications understandable by business and technical stakeholders?
8. Did the incident produce durable improvements to tests, gates, runbooks or architecture?

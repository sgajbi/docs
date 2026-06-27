# Observability, SRE, Runbooks, and Report Operations

## Purpose

This file explains how to operate and support reporting systems.

Report failures are often workflow, data, rendering, archive, entitlement, or dependency issues. Observability must reflect this.

---

## Reporting Metrics

Useful metrics:

```text
report_jobs_total{report_type,status}
report_job_duration_seconds_bucket{report_type}
report_render_failures_total{report_type,reason}
report_archive_failures_total{report_type,reason}
report_downloads_total{report_type,status_class}
report_batch_items_total{batch_type,status}
report_supportability_total{report_type,state,reason}
```

Use bounded labels only.

---

## Logs

Structured report logs should include:

- report job id or safe reference
- report type
- operation
- lifecycle state
- reason code
- dependency
- duration
- correlation id
- archive id where safe
- supportability state

Avoid raw report payloads.

---

## Dashboards

Dashboards should show:

- job counts by state
- render success/failure
- archive success/failure
- batch progress
- queue depth
- retry/dead-letter count
- average/p95 generation time
- download failures
- supportability states
- recent deployments

---

## Alerts

Useful alerts:

- report job failure spike
- rendering failure spike
- archive failure
- batch missed SLA
- queue backlog
- dead-letter count
- stale data blocking reports
- download authorization spike
- retention cleanup failure

---

## Runbooks

Runbooks should cover:

- report stuck queued
- renderer failure
- archive failure
- stale source data
- missing template
- failed batch
- download access issue
- document corruption/hash mismatch
- replay failed report
- retention/legal hold issue

---

## Operations Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Failed report only visible to user | Support reactive. |
| No reason code | Slow triage. |
| Queue depth not monitored | Backlog surprise. |
| Renderer logs raw content | Sensitive-data leak. |
| Archive failure hidden | Report lost. |
| No replay runbook | Manual repair. |
| Alert without dashboard | Diagnosis slow. |

---

## Review Checklist

- Are report metrics defined?
- Are lifecycle states observable?
- Are logs structured?
- Are dashboards available?
- Are alerts actionable?
- Are runbooks current?
- Is replay documented?
- Are sensitive report fields excluded?
- Are support owners clear?
- Are incidents feeding improvements?

---

## Summary

Report operations require visibility across data collection, rendering, archive, retrieval, and batch workflow.

A reporting system must explain why a report failed.

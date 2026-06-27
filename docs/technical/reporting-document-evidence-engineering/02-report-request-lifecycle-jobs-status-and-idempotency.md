# Report Request Lifecycle, Jobs, Status, and Idempotency

## Purpose

This file explains how report requests should be modelled as durable jobs.

Report generation is often long-running, dependency-heavy, and failure-prone. It should usually be asynchronous and recoverable.

---

## Report Lifecycle

Common lifecycle:

```text
REQUESTED
  -> ACCEPTED
  -> QUEUED
  -> COLLECTING_DATA
  -> RENDERING
  -> ARCHIVING
  -> SUCCEEDED
  -> FAILED_RETRYABLE
  -> FAILED_FINAL
  -> CANCELLED
  -> EXPIRED
```

---

## Report Request

A report request should include:

- report type
- requested by
- caller context
- client/account/portfolio scope
- business date or reporting period
- sections requested
- output format
- language/locale
- template preference where allowed
- idempotency key
- delivery/archive preference
- purpose or workflow reference

---

## Idempotency

Report creation should normally be idempotent.

Use idempotency to prevent:

- duplicate report jobs
- duplicate archive documents
- duplicate client delivery
- duplicate batch entries
- retry-after-timeout duplicates

Store:

- idempotency key
- request hash
- caller scope
- job id
- current status
- result/archive reference
- expiry

---

## Status API

A report status response should include:

```json
{
  "reportJobId": "synthetic-report-job-id",
  "status": "RENDERING",
  "supportability": {
    "state": "READY",
    "reason": "DATA_COLLECTION_COMPLETE"
  },
  "progress": {
    "currentStep": "RENDERING",
    "completedSections": 5,
    "totalSections": 6
  },
  "links": {
    "result": null
  }
}
```

---

## Cancellation

Cancellation should define:

- who can cancel
- which states are cancellable
- what happens to partial artifacts
- audit event
- worker behavior
- user-visible status

---

## Retry and Replay

Retry/replay should be:

- authorized
- audited
- idempotent
- bounded
- visible in status
- connected to runbook

---

## Lifecycle Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Report generation synchronous for heavy reports | Gateway/client timeout. |
| No idempotency | Duplicate jobs/documents. |
| No durable status | User/support cannot track progress. |
| Retry creates new document each time | Archive duplication. |
| Failed jobs have no reason code | Support cannot diagnose. |
| Cancellation not defined | User confusion. |
| Job state only in logs | Operational gap. |

---

## Review Checklist

- Is report generation async?
- Is job state durable?
- Is idempotency required?
- Is request hash stored?
- Is status API defined?
- Are lifecycle states clear?
- Are retry/replay rules defined?
- Is cancellation supported?
- Is audit captured?
- Are tests present?

---

## Summary

Report jobs are workflows.

They need durable lifecycle, idempotency, status visibility, retry/replay, and supportability.

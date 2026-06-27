# Async Execution, Job State, Polling, and Result Retrieval

## Purpose

This file explains how to design asynchronous APIs for long-running or expensive work.

Async execution lets systems accept work quickly and complete it outside the request thread.

---

## When To Use Async

Use async when:

- work exceeds normal request timeout
- calculation is expensive
- downstream dependency is slow
- batch processing is needed
- user can poll for result
- workflow needs durable tracking
- retry/recovery is required
- result generation is separate from request acceptance

---

## Async API Pattern

```text
POST /jobs
  -> returns 202 Accepted with jobId

GET /jobs/{jobId}
  -> returns status

GET /jobs/{jobId}/result
  -> returns result when complete
```

---

## Job State Model

| State | Meaning |
|---|---|
| ACCEPTED | Request accepted. |
| QUEUED | Waiting for worker. |
| RUNNING | Worker processing. |
| SUCCEEDED | Result ready. |
| FAILED_RETRYABLE | Failed but retry allowed. |
| FAILED_FINAL | Failed permanently. |
| CANCELLED | Cancelled by authorized caller. |
| EXPIRED | Result no longer retained. |

---

## Result Retrieval

Result endpoint should define:

- result available state
- not-ready response
- expired response
- permission checks
- lineage/evidence
- supportability
- retention period

---

## Polling Guidance

Polling should include:

- status endpoint
- retry-after suggestion where useful
- terminal states
- current progress where safe
- result path when complete
- error/reason code on failure

---

## Async Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Long work in request thread | Timeouts. |
| Job state only in memory | Lost on restart. |
| No status endpoint | Caller blind. |
| Result path process-local | Result lost after deploy. |
| No retention policy | Storage growth or surprise expiry. |
| Polling too frequent | Load amplification. |
| No idempotency on submission | Duplicate jobs. |

---

## Review Checklist

- Is async justified?
- Is job state durable?
- Is submission idempotent?
- Is status endpoint defined?
- Is result endpoint defined?
- Are terminal states clear?
- Is retention defined?
- Is polling guidance provided?
- Are failures represented safely?
- Are tests present?

---

## Summary

Async execution is not just background processing.

It requires durable state, clear status, safe polling, result retrieval, and recovery.

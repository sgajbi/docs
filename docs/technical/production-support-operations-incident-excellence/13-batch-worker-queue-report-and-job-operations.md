# Batch, Worker, Queue, Report, and Job Operations

## Purpose

This file explains operational support for asynchronous jobs, workers, queues, and batch/report workflows.

Many enterprise workflows are not user-triggered APIs. They run as background jobs that need monitoring, recovery, and evidence.

---

## Job Operations

Track:

- job id
- job type
- business date
- state
- owner
- started/completed time
- retry count
- failure reason
- dependency state
- output/evidence reference
- replay eligibility

---

## Queue Operations

Monitor:

- queue depth
- age of oldest message
- worker lag
- dead-letter count
- retry count
- processing duration
- consumer health
- throughput
- poison messages

---

## Batch Operations

Batch operations should show:

- batch id
- item count
- success count
- failure count
- skipped count
- partial count
- current step
- SLA status
- reconciliation status
- evidence

---

## Recovery Actions

Common actions:

- retry job
- replay batch
- dead-letter investigation
- cancel stuck job
- release lock/lease safely
- reprocess source
- regenerate report
- quarantine bad item
- scale workers temporarily

---

## Job Operations Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Job status only in logs | Support cannot track. |
| Dead letters ignored | Data loss/recovery gap. |
| Replay not idempotent | Duplicate side effects. |
| Batch has no item-level status | Partial failures hidden. |
| Worker lag not monitored | SLA breach surprise. |
| Stuck locks not detectable | Workflow blocked. |
| Manual database updates for recovery | High operational risk. |

---

## Review Checklist

- Are job states durable?
- Are queues monitored?
- Are dead letters visible?
- Are retries bounded?
- Is replay safe and audited?
- Is batch progress visible?
- Are failure reason codes defined?
- Are operator tools available?
- Are runbooks current?
- Are SLAs monitored?

---

## Summary

Async operations need first-class support.

If a job can fail, support must know where it failed, why, and how to recover safely.

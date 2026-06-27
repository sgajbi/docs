# Workers, Queues, Leasing, Dead Letter, Replay, and Recovery

## Purpose

This file explains how to design reliable background workers and queue-based processing.

Workers must be observable, idempotent, bounded, and recoverable.

---

## Worker Responsibilities

A worker should define:

- input source
- work item identity
- lease/concurrency model
- retry policy
- idempotency behavior
- dead-letter behavior
- recovery path
- metrics
- logs
- shutdown behavior
- runbook

---

## Leasing

A lease prevents multiple workers from processing the same item at the same time.

Lease design:

- lease owner
- lease duration
- renewal
- expiry
- retry after expiry
- stuck item detection

---

## Dead Letter

Dead-letter state captures work that cannot be processed after retries.

Dead-letter record should include:

- work item id
- failure reason
- retry count
- last error class
- first failed at
- last failed at
- replay eligibility
- safe diagnostic summary

---

## Replay

Replay reprocesses failed work.

Replay must be:

- authorized
- audited
- idempotent
- bounded
- observable
- documented
- safe for downstream effects

---

## Worker Metrics

Examples:

```text
worker_items_total{worker,state}
worker_processing_duration_seconds_bucket{worker}
worker_retries_total{worker,reason}
worker_dead_letters_total{worker,reason}
worker_replays_total{worker,result}
worker_queue_depth{worker}
worker_lag_seconds{worker}
```

---

## Worker Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Infinite retry | Resource waste. |
| No dead-letter | Failed work hidden. |
| No idempotency | Duplicate side effects. |
| No lease | Parallel duplicate processing. |
| Worker ignores shutdown | Lost work. |
| No replay audit | Unsafe operations. |
| Queue depth not monitored | Backlog invisible. |

---

## Review Checklist

- Are work states explicit?
- Is lease model defined?
- Are retries bounded?
- Is dead-letter defined?
- Is replay authorized?
- Is processing idempotent?
- Are metrics present?
- Are logs structured?
- Is shutdown safe?
- Is runbook available?

---

## Summary

Workers are production services.

They need the same engineering discipline as APIs, plus stronger recovery and duplicate-safety controls.

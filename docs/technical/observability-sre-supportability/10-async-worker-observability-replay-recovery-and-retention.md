# Async Worker Observability, Replay, Recovery, and Retention

## Purpose

This file explains observability for asynchronous workflows, workers, queues, replay, recovery, and retention.

Async work is harder to operate than synchronous APIs because the user may not be waiting and failure may happen later.

---

## Async Observability Needs

Async workflows should expose:

- submitted count
- queued count
- running count
- succeeded count
- failed retryable count
- failed final count
- retry count
- queue depth
- worker lag
- processing duration
- dead-letter count
- replay attempts
- retention cleanup count

---

## Work Item State Model

Example states:

| State | Meaning |
|---|---|
| ACCEPTED | Request accepted. |
| QUEUED | Waiting for worker. |
| RUNNING | Worker processing. |
| SUCCEEDED | Completed. |
| FAILED_RETRYABLE | Failed but retry allowed. |
| FAILED_FINAL | Failed permanently. |
| DEAD_LETTERED | Moved to dead-letter path. |
| REPLAY_REQUESTED | Operator or system requested replay. |
| EXPIRED | Result removed by retention. |

---

## Worker Logs

Worker logs should include:

- worker name
- work item id or safe reference
- operation
- state transition
- retry count
- reason code
- duration
- correlation id
- dependency failure
- replay status

Avoid raw payloads.

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

## Replay and Recovery

Replay should be visible.

Track:

- who requested replay
- why
- which item
- previous state
- new state
- outcome
- failure reason
- audit reference

---

## Retention

Retention observability should track:

- records eligible
- records removed
- records retained due to policy
- cleanup duration
- cleanup failures
- storage reclaimed
- last successful cleanup

---

## Async Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Work state only in memory | Lost progress on restart. |
| No queue depth metric | Backlog invisible. |
| Infinite retry | Resource waste and noise. |
| Dead-letter not monitored | Failed work hidden. |
| Replay not audited | Unsafe recovery. |
| Result retention undocumented | Consumers lose data unexpectedly. |
| Worker shutdown unsafe | Duplicate/lost work. |

---

## Review Checklist

- Are work states explicit?
- Are queue and lag metrics present?
- Are retries bounded?
- Is dead-letter behavior monitored?
- Is replay visible and audited?
- Is recovery documented?
- Is retention measured?
- Are worker logs structured?
- Are sensitive payloads excluded?
- Are runbooks available?

---

## Summary

Async workflows need first-class observability.

If work can happen in the background, operators need foreground visibility into its state.

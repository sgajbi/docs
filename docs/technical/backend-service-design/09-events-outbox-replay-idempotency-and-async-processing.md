# Events, Outbox, Replay, Idempotency, and Async Processing

## Purpose

This file explains how to design event-driven and asynchronous backend workflows.

Enterprise systems need durable processing, retry, replay, idempotency, and operator visibility. Fire-and-forget is not enough.

## When To Use Async Processing

Use asynchronous processing when:

- work is long-running
- computation is expensive
- external dependencies are slow
- results can be polled
- work needs retries
- workflow must survive restart
- throughput needs worker scaling
- batch processing is required
- operator recovery is required

## Async Request Pattern

```text
Client submits request
  -> API validates and creates command
  -> use case creates execution record
  -> service returns 202 Accepted
  -> worker processes work item
  -> status endpoint exposes progress
  -> result endpoint returns final output
```

Required:

- execution id
- status model
- durable work item
- idempotency key
- polling route
- result route
- recovery/replay route where needed
- retention policy

## Event Design

Good events are:

- meaningful
- versioned
- schema-controlled
- idempotent
- correlated
- safe
- documented
- testable

Event should include:

- event id
- event type
- version
- occurred at
- producer
- correlation id
- causation id if applicable
- subject id
- payload
- schema version

Avoid raw internal objects as events.

## Outbox Pattern

Use outbox when event publication must align with state change.

```text
transaction:
  write business state
  write outbox event

worker:
  read pending outbox event
  publish event
  mark delivered
  retry on failure
```

Benefits:

- avoids lost events
- supports retry
- supports audit
- supports replay
- decouples request from broker availability

## Idempotency

Idempotency prevents duplicate side effects.

For commands:

- idempotency key required
- request hash stored
- duplicate same request returns same result/status
- duplicate different request returns conflict
- retention window defined

For consumers:

- processed event ids tracked
- duplicate event ignored or returns previous effect
- side effects protected by unique constraints

## Replay

Replay allows failed or missed work to be safely reprocessed.

Replay requires:

- stable identity
- source evidence
- idempotent handler
- authorization
- audit log
- bounded scope
- metrics and logs
- operator runbook

Replay is not a manual database patch.

## Dead-Letter Handling

Dead-letter queues or states capture work that cannot be processed.

Dead-letter record should include:

- item id
- failure reason code
- retry count
- last error class
- first failed at
- last failed at
- correlation id
- next action
- supportability state

Do not store sensitive raw payloads unless explicitly approved and protected.

## Worker Design

Workers should define:

- lease model
- concurrency
- batch size
- retry policy
- backoff
- timeout
- idempotency
- dead-letter state
- metrics
- structured logs
- shutdown behavior
- health/readiness if deployed separately

## Async Status Model

Example:

| Status | Meaning |
|---|---|
| ACCEPTED | Request accepted. |
| QUEUED | Waiting for worker. |
| RUNNING | Work in progress. |
| SUCCEEDED | Result ready. |
| FAILED_RETRYABLE | Failed but retry allowed. |
| FAILED_FINAL | Failed permanently. |
| CANCELLED | Cancelled by authorized actor. |
| EXPIRED | Result no longer retained. |

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Fire-and-forget event | Lost work cannot be recovered. |
| No idempotency | Duplicate effects on retry. |
| Process-local async state | State lost on restart. |
| No polling route | Client cannot know progress. |
| Worker logs only raw errors | Operators cannot triage. |
| No dead-letter path | Failed work disappears. |
| Replay without audit | Unsafe operational control. |
| Event payload is database row | Schema and internal state leak. |
| Infinite retries | System churn and alert noise. |

## Review Checklist

- Is async needed?
- Is execution state durable?
- Is request idempotent?
- Is event schema versioned?
- Is outbox needed?
- Are retries bounded?
- Is dead-letter behavior defined?
- Can operators see status?
- Is replay authorized and audited?
- Are payloads source-safe?
- Are metrics and logs present?
- Is retention defined?
- Is shutdown safe?

## Summary

Async processing is not just background work.

In enterprise systems, async work must be durable, idempotent, observable, replayable, and supportable.

# Events, AsyncAPI, and API/Event Integration Patterns

## Purpose

This file explains how REST APIs and events should work together in enterprise systems.

Not every interaction should be synchronous. Events are useful for lifecycle notification, downstream propagation, replay, audit, and asynchronous workflow.

## REST Versus Events

| REST API | Event |
|---|---|
| Request/response | Asynchronous notification |
| Caller expects immediate response | Consumers react later |
| Good for queries and commands | Good for lifecycle changes |
| Tight timing | Loose coupling |
| Synchronous failure handling | Retry/replay handling |
| OpenAPI | AsyncAPI/event schema |

## Common Integration Pattern

```text
POST /report-jobs
  -> creates report job
  -> writes outbox event ReportJobCreated
  -> returns 202 Accepted

Outbox worker
  -> publishes ReportJobCreated

Consumers
  -> react to event
  -> process idempotently
```

## Event Schema

An event should include:

```json
{
  "eventId": "synthetic-event-id",
  "eventType": "ReportJobCreated",
  "eventVersion": "v1",
  "occurredAt": "2026-04-10T12:00:00Z",
  "producer": "reporting-service",
  "subject": "report-job/synthetic-id",
  "correlationId": "synthetic-correlation-id",
  "payload": {}
}
```

## AsyncAPI

AsyncAPI can document:

- channels/topics
- event types
- payload schemas
- producer
- consumers
- version
- delivery semantics
- retry/dead-letter behavior
- examples
- security requirements

Use AsyncAPI or equivalent event schema docs for governed event contracts.

## Lifecycle Events

Good event candidates:

- job created
- job completed
- job failed
- document archived
- calculation completed
- proposal reviewed
- source ingestion completed
- data product certified
- work item retry exhausted

Avoid events for internal implementation noise unless consumers need it.

## Event Versioning

Events require versioning.

Breaking event changes include:

- removing field
- renaming field
- changing type
- changing meaning
- changing required fields
- changing event type semantics

Prefer additive changes and consumer-tolerant readers.

## Idempotent Consumers

Consumers must handle duplicate events.

Use:

- event id tracking
- natural business id
- unique constraints
- processed-event table
- idempotent state transitions
- deduplication window

## Outbox and Replay

Outbox supports reliable publication.

Replay supports recovery.

Replay must be:

- authorized
- audited
- idempotent
- bounded
- observable
- documented

## Event Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Publish event before DB commit | Consumers see state that does not exist. |
| Event payload is raw database row | Schema leaks implementation. |
| No event version | Compatibility impossible. |
| Consumer not idempotent | Duplicate processing. |
| No dead-letter path | Failed events disappear. |
| REST API and event disagree | Inconsistent contract. |
| AsyncAPI missing | Consumers guess schema. |

## Review Checklist

- Should this be REST, event, or both?
- Is event type meaningful?
- Is event schema versioned?
- Is producer clear?
- Are consumers known?
- Is outbox needed?
- Are consumers idempotent?
- Is replay defined?
- Is dead-letter behavior defined?
- Is event documented?
- Is event tested?
- Are payloads source-safe?

## Summary

REST APIs and events should complement each other.

Use APIs for commands and queries. Use events for lifecycle notification, propagation, and asynchronous integration with reliable delivery and replay.

# Idempotency, Concurrency, and Write Safety

## Purpose

This file explains how to design write APIs that remain safe under retries, duplicate submissions, concurrent actions, and partial failures.

In enterprise platforms, users, browsers, clients, workers, gateways, and networks can retry. Write APIs must expect that.

## Idempotency

An operation is idempotent when repeating the same request does not create additional side effects.

Use idempotency for:

- create workflow
- submit calculation
- request report
- start batch
- acknowledge action
- review item
- replay job
- publish command
- external handoff

## Idempotency-Key

A common pattern is an `Idempotency-Key` header.

```text
Idempotency-Key: synthetic-key-123
```

The service stores:

- idempotency key
- caller scope
- operation
- request hash
- result identity
- status
- created at
- expiry

## Duplicate Handling

| Scenario | Behavior |
|---|---|
| Same key, same request, completed | Return previous result. |
| Same key, same request, still processing | Return current status. |
| Same key, different request | Return 409 conflict. |
| Key expired | Treat as new or reject according to policy. |
| Missing key for required operation | Return 400. |

## Request Hash

Request hash helps detect mismatched duplicate usage.

Hash should include:

- relevant request body
- target resource
- caller scope where needed
- operation type

Do not include volatile metadata that would break legitimate retry.

## Concurrency

Concurrent writes can conflict.

Examples:

- two users approve same item
- worker and operator replay same job
- duplicate batch submitted
- status transition races
- stale update overwrites new state

Controls:

- optimistic locking
- version field
- status transition checks
- unique constraints
- idempotency records
- transactional updates
- compare-and-set
- leases for workers

## Lifecycle Conflict

Use 409 Conflict when action cannot proceed because state changed.

Example:

```json
{
  "title": "Lifecycle conflict",
  "status": 409,
  "reasonCode": "INVALID_LIFECYCLE_TRANSITION",
  "detail": "The item is already in a terminal state."
}
```

## Retry Safety

A write operation is retry-safe when:

- idempotency is enforced
- side effects are protected
- events use outbox or duplicate detection
- external calls are not repeated unsafely
- response can return previous result/status
- audit remains clear

## Write API Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Frontend disables button only | Backend still receives duplicates. |
| No idempotency for create | Duplicate records. |
| Retry after timeout creates second job | User and support confusion. |
| Same idempotency key accepted with different payload | Data corruption. |
| Lifecycle update ignores current state | Invalid transitions. |
| External side effect before durable state | Lost audit and replay risk. |
| No unique constraints | Race conditions. |

## Review Checklist

- Is the operation state-changing?
- Is idempotency required?
- Where is the key passed?
- What is the idempotency scope?
- Is request hash stored?
- What happens on duplicate?
- What happens on conflict?
- Are unique constraints present?
- Are lifecycle transitions checked?
- Are retries safe?
- Are external side effects protected?
- Are tests present for duplicate and conflict cases?

## Summary

Write APIs must be designed for reality: retries, timeouts, double-clicks, concurrent workers, and partial failures.

Idempotency and concurrency controls turn those realities into safe behavior.

# Error Handling, Supportability, Reason Codes, and Degraded States

## Purpose

This file explains how backend services should handle errors and communicate supportability.

In enterprise systems, failures are not exceptional. Dependencies become stale, partial, unavailable, permission-blocked, or unsupported. Services must represent those states safely and consistently.

## Error Categories

| Category | Meaning |
|---|---|
| Validation error | Request shape or value is invalid. |
| Unsupported operation | Request is valid shape but not supported. |
| Permission blocked | Caller is not allowed. |
| Not found | Resource absent or invisible. |
| Conflict | Version, lifecycle, duplicate, or idempotency conflict. |
| Dependency unavailable | Required upstream or infrastructure unavailable. |
| Timeout | Dependency or internal operation exceeded timeout. |
| Partial data | Some sources available, others missing. |
| Stale data | Data exists but freshness is outside expected boundary. |
| Internal error | Unexpected service failure. |

## Supportability States

Supportability states explain usability.

| State | Meaning |
|---|---|
| READY | Fully supported for requested operation. |
| PARTIAL | Response available but incomplete. |
| STALE | Data is older than acceptable threshold. |
| DEGRADED | Service can respond with reduced quality. |
| UNAVAILABLE | Required capability/dependency is unavailable. |
| PERMISSION_BLOCKED | Caller cannot access the operation/data. |
| UNSUPPORTED | Capability not supported for this request. |
| UNKNOWN | Service cannot determine supportability. |

## Reason Codes

Reason codes should be bounded, stable, and documented.

Examples:

```text
SOURCE_READY
SOURCE_STALE
SOURCE_PARTIAL
DEPENDENCY_UNAVAILABLE
DEPENDENCY_TIMEOUT
PERMISSION_BLOCKED
UNSUPPORTED_INPUT_MODE
IDEMPOTENCY_CONFLICT
INVALID_LIFECYCLE_TRANSITION
REPLAY_NOT_ALLOWED
```

Do not use raw exception text as reason code.

## Warning Model

Warnings are useful when response is successful but quality is affected.

Warning should include:

- code
- severity
- message
- affected section
- supportability impact
- safe remediation hint

Avoid sensitive data in warning text.

## Problem Details

API errors should use a structured format.

Example:

```json
{
  "type": "https://example.com/problems/dependency-unavailable",
  "title": "Dependency unavailable",
  "status": 503,
  "detail": "The required source service is unavailable.",
  "correlationId": "synthetic-correlation-id",
  "reasonCode": "DEPENDENCY_UNAVAILABLE"
}
```

## Error Mapping

| Internal Condition | API Response |
|---|---|
| Invalid request field | 400/422 |
| Unsupported input mode | 400 |
| Missing entitlement | 403 |
| Resource not visible | 404 |
| Duplicate idempotency key with different payload | 409 |
| Upstream unavailable | 503 |
| Upstream timeout | 504 |
| Unexpected exception | 500 with safe message |

## Partial And Degraded Responses

Sometimes a service should return partial data instead of failing completely.

Use partial/degraded response when:

- optional upstream source fails
- non-critical section unavailable
- cached/stale data can be clearly marked
- UI can render a truthful partial state
- reporting/export does not require full correctness

Do not return partial data when:

- output would be misleading
- calculation would be materially wrong
- compliance or suitability control is missing
- caller lacks permission
- source authority cannot be established

## Permission-Blocked State

Permission-blocked is not a system error.

It should be:

- safely represented
- auditable
- free of raw entitlement details
- clear enough for UI rendering
- tracked in metrics/logs with bounded labels

## Error Logging

Log enough for diagnosis, not enough to leak data.

Log:

- event name
- operation
- status
- reason code
- dependency
- safe exception class
- correlation id
- duration bucket

Do not log:

- request body
- response body
- secrets
- client/account/portfolio identifiers unless approved
- raw entitlement failure
- SQL values
- prompts and model outputs

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Everything returns 500 | Clients cannot respond correctly. |
| Raw exception returned to caller | Security leakage. |
| Free-form reason text | Metrics and support become inconsistent. |
| Partial response without supportability | Users trust incomplete data. |
| Permission denied as not handled | Poor UX and audit gaps. |
| Stale data silently shown as ready | Incorrect business decisions. |
| Logs contain raw payloads | Sensitive-data exposure. |

## Review Checklist

- Are error categories explicit?
- Are supportability states modelled?
- Are reason codes bounded?
- Are warnings structured?
- Are problem details safe?
- Are permission-blocked states first-class?
- Is partial data clearly marked?
- Are stale states visible?
- Are logs safe?
- Are metrics labels bounded?
- Are degraded states tested?

## Summary

Good error handling is part of product quality.

A service should explain not only that something failed, but whether the result is ready, partial, stale, degraded, unavailable, permission-blocked, unsupported, or unknown.

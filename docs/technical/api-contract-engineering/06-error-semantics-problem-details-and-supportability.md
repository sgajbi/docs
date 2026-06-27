# Error Semantics, Problem Details, and Supportability

## Purpose

This file explains how to design API error responses and supportability states.

Enterprise APIs should fail predictably and safely. They should also represent partial, stale, degraded, unavailable, permission-blocked, and unsupported states clearly.

## Error Model Goals

A good error model should be:

- structured
- safe
- predictable
- documented
- testable
- consistent across APIs
- useful for clients
- useful for support
- free of sensitive data

## Problem Details Shape

A common structure:

```json
{
  "type": "https://example.com/problems/validation-error",
  "title": "Invalid request",
  "status": 400,
  "detail": "The request contains unsupported values.",
  "reasonCode": "UNSUPPORTED_INPUT",
  "correlationId": "synthetic-correlation-id"
}
```

Optional extension fields:

```json
{
  "fieldErrors": [
    {"field": "period", "reason": "UNSUPPORTED_VALUE"}
  ],
  "supportability": {
    "state": "UNSUPPORTED",
    "reason": "UNSUPPORTED_PERIOD"
  }
}
```

## Status Code Guidance

| Status | Use |
|---|---|
| 400 | Invalid request or unsupported input combination. |
| 401 | Authentication missing or invalid. |
| 403 | Authenticated but not authorized. |
| 404 | Resource not found or not visible. |
| 409 | Conflict, duplicate, lifecycle conflict, idempotency mismatch. |
| 422 | Semantic validation error where framework convention applies. |
| 429 | Rate limit or back-pressure. |
| 500 | Unexpected internal error. |
| 502 | Upstream returned bad response. |
| 503 | Dependency unavailable or service not ready. |
| 504 | Timeout. |

## Supportability States

Supportability states describe whether a response can be used.

| State | Meaning |
|---|---|
| READY | Fully usable. |
| PARTIAL | Some sections available; others missing. |
| STALE | Data older than expected. |
| DEGRADED | Available with reduced quality. |
| UNAVAILABLE | Required dependency unavailable. |
| PERMISSION_BLOCKED | Caller not allowed. |
| UNSUPPORTED | Capability not supported. |
| UNKNOWN | Service cannot prove posture. |

## Warning Model

Warnings are useful for successful responses with caveats.

Example:

```json
{
  "code": "SOURCE_STALE",
  "severity": "WARNING",
  "message": "The source data is older than the expected freshness window.",
  "section": "risk",
  "supportabilityImpact": "STALE"
}
```

Warnings should be bounded and safe.

## Permission-Blocked Errors

Permission-blocked is not a system failure.

Good pattern:

- return 403 where the full operation is blocked
- return section-level `PERMISSION_BLOCKED` where partial UI rendering is appropriate
- avoid exposing raw entitlement rules
- log safe audit event
- emit bounded metric
- include correlation id

## Safe Detail Field

The `detail` field should be helpful but safe.

Avoid:

- raw SQL
- stack trace
- upstream response body
- entitlement engine details
- client/account identifiers
- file paths
- secret values
- prompts/model responses

## Error Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Return 500 for all errors | Clients cannot handle known conditions. |
| Return raw upstream error | Security and UX risk. |
| Error message includes sensitive data | Data leakage. |
| No reason codes | Metrics and support inconsistent. |
| Stale data returned as ready | Misleading output. |
| Unsupported feature returns fake success | False product claim. |
| Permission issue hidden as empty data | User confusion and audit gap. |

## Review Checklist

- Are error responses structured?
- Are reason codes bounded?
- Are problem details documented in OpenAPI?
- Are supportability states defined?
- Are warnings structured?
- Are permission-blocked states safe?
- Are stale/partial/degraded states explicit?
- Is sensitive data excluded?
- Are errors tested?
- Do clients know what to do for each state?

## Summary

Good error semantics help clients, users, operators, and auditors understand what happened.

A professional API does not just fail. It explains failure safely.

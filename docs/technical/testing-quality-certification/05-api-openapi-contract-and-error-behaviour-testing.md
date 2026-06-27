# API, OpenAPI, Contract, and Error Behaviour Testing

## Purpose

This file explains how to test API contracts and error behavior.

APIs are durable promises to consumers. Tests should protect those promises.

---

## API Test Areas

Test:

- route exists
- request validation
- response schema
- status codes
- error model
- OpenAPI metadata
- examples
- caller context
- idempotency
- pagination
- supportability states
- permission-blocked behavior
- backward-compatible aliases

---

## Request Validation Tests

Cover:

- missing required fields
- invalid enum
- invalid date
- unsupported combination
- invalid pagination size
- invalid cursor
- missing idempotency key
- malformed caller context
- unauthorized caller

---

## Response Contract Tests

Assert:

- required fields
- optional fields
- metadata
- warnings
- supportability
- lineage
- pagination metadata
- no internal fields
- no sensitive fields

---

## Error Tests

Test:

- 400 invalid request
- 403 permission blocked
- 404 not found
- 409 conflict
- 422 semantic validation where used
- 429 rate limit where applicable
- 503 dependency unavailable
- 504 timeout
- safe 500 response

---

## OpenAPI Tests

Validate:

- every route has summary
- every route has description
- tags exist
- response descriptions exist
- examples exist
- error responses documented
- operation IDs stable
- deprecated routes marked
- unsupported future claims absent

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Test only status 200 | Contract weak. |
| No error tests | Clients fail during real-world issues. |
| No OpenAPI gate | Docs drift. |
| Snapshot test of huge response | Changes approved without meaning. |
| API tests require production-like data | Fragile and unsafe. |
| Unsupported fields silently accepted | Contract ambiguity. |

---

## Review Checklist

- Are API contracts tested?
- Are errors tested?
- Is OpenAPI validated?
- Is caller context tested?
- Is idempotency tested?
- Is pagination tested?
- Are degraded states tested?
- Are sensitive fields excluded?
- Are examples current and synthetic?

---

## Summary

API tests protect consumers from drift.

They should prove success, failure, security, supportability, and documentation quality.

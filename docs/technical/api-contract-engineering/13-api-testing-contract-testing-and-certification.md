# API Testing, Contract Testing, and Certification

## Purpose

This file explains how to test APIs as contracts.

API tests should prove request validation, response shape, error behavior, security posture, compatibility, observability, and consumer expectations.

## API Test Types

| Test Type | Purpose |
|---|---|
| Unit tests | Test route helpers, mappers, DTO validation. |
| API tests | Call route with test client and validate behavior. |
| Contract tests | Protect schemas, field names, status codes, and examples. |
| OpenAPI tests | Validate documentation completeness and correctness. |
| Integration tests | Validate real adapters and upstream/downstream behavior. |
| E2E tests | Validate full business flow. |
| Security tests | Validate auth, permission, and sensitive-data behavior. |
| Observability tests | Validate logs, metrics, and forbidden labels. |
| Certification tests | Produce repeatable evidence for supported API claims. |

## Request Validation Tests

Test:

- missing required fields
- invalid enum
- unsupported combination
- invalid date
- page size too large
- invalid cursor
- missing idempotency key
- malformed caller context
- unauthorized scope

## Response Contract Tests

Test:

- response schema
- required fields
- optional fields
- supportability state
- warnings
- metadata
- pagination metadata
- error shape
- no unexpected sensitive fields

## OpenAPI Tests

Test:

- every route has summary
- every route has description
- tags exist
- response descriptions exist
- examples exist
- error responses documented
- schemas generated
- operation ids stable
- unsupported claims absent

## Error Tests

Test:

- 400 invalid request
- 403 permission-blocked
- 404 not found
- 409 conflict
- 429 rate limit if applicable
- 503 dependency unavailable
- 504 timeout
- safe 500 handling

## Idempotency Tests

Test:

- first request creates action
- duplicate same request returns same result/status
- duplicate different request returns 409
- missing key returns 400
- processing duplicate returns current status
- expired key behavior

## Pagination Tests

Test:

- default page size
- max page size
- next cursor
- stable ordering
- invalid cursor
- filter interactions
- no unbounded result
- authorization with pagination

## BFF Contract Tests

Test:

- upstream calls made correctly
- caller context forwarded
- partial upstream failure mapped
- permission-blocked section rendered
- supportability preserved
- no domain recomputation
- raw upstream errors not leaked

## Certification Tests

Certification tests provide repeatable proof for supported APIs.

A certification sweep may:

- seed deterministic synthetic data
- call supported endpoints
- assert expected values
- validate errors and degraded states
- check OpenAPI
- check health/readiness
- write machine-readable evidence

## API Testing Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Only happy-path tests | Failure behavior unproved. |
| Snapshot response without meaning | Incorrect changes approved. |
| No OpenAPI tests | Docs drift. |
| No permission-blocked tests | Security gaps. |
| No pagination tests | Performance issues. |
| No idempotency tests | Duplicate writes. |
| Contract tests depend on real external systems | Flaky CI. |
| Certification uses non-deterministic data | Evidence unreliable. |

## Review Checklist

- Are request validation tests complete?
- Are response schemas protected?
- Are errors tested?
- Are permission states tested?
- Is idempotency tested?
- Is pagination tested?
- Is OpenAPI validated?
- Are BFF partial/degraded states tested?
- Are examples current?
- Is certification evidence deterministic?
- Are tests in the right CI lane?

## Summary

API testing protects consumers.

Good API tests prove not just that the endpoint works, but that its contract remains safe, stable, documented, and supportable.

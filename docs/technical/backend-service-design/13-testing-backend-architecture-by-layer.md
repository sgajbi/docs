# Testing Backend Architecture by Layer

## Purpose

This file explains how to test backend services according to architectural layers.

Good tests prove behavior at the cheapest reliable layer.

## Layer-Based Test Map

| Layer | Test Type | What To Prove |
|---|---|---|
| Domain | Unit/domain tests | Policies, calculations, lifecycle, invariants. |
| Application | Use-case tests | Orchestration, ports, idempotency, workflow decisions. |
| API | Contract/API tests | DTOs, validation, errors, OpenAPI, caller context. |
| Infrastructure | Integration tests | Database, HTTP client, queue, cache, object store. |
| Workers | Worker tests | Retry, leasing, status transitions, replay, dead-letter. |
| Runtime | Docker/smoke tests | Service starts, health/readiness, config. |
| Product flow | E2E/browser tests | User-visible or cross-service behavior. |
| Docs/contracts | Documentation tests | Docs match implementation and contract truth. |

## Domain Tests

Should be:

- fast
- deterministic
- framework-free
- infrastructure-free

Test:

- calculation correctness
- policy decisions
- lifecycle transitions
- value-object validation
- reason-code mapping
- unsupported states

## Application Tests

Use fake ports.

Test:

- success path
- dependency unavailable
- stale data
- permission blocked
- idempotency duplicate
- idempotency conflict
- event/outbox created
- transaction decision
- supportability result

## API Tests

Test:

- request validation
- response shape
- error mapping
- status codes
- caller-context extraction
- idempotency header behavior
- OpenAPI route metadata
- deprecated aliases
- unsupported inputs

## Integration Tests

Test real infrastructure seams:

- repository with database
- migration apply/rollback
- HTTP client with mock server
- outbox publisher with broker/test double
- cache adapter
- object storage adapter

## Worker Tests

Test:

- state transitions
- lease acquisition
- retry policy
- dead-letter
- idempotency
- replay eligibility
- shutdown behavior
- retention cleanup

## Observability Tests

Test:

- metric names
- metric labels
- forbidden label rejection
- log event vocabulary
- safe diagnostics
- readiness behavior
- dashboard/alert references where machine-checkable

## Security Tests

Test:

- permission-blocked state
- caller-context enforcement
- write action authorization
- sensitive-data rejection
- no secrets in generated artifacts
- safe error responses

## Contract Tests

Protect:

- OpenAPI
- event schemas
- data-product schemas
- reason codes
- supportability vocabulary
- API aliases
- generated evidence shape

## Test Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Use full E2E for every rule | Slow and brittle. |
| Mock the method under test | No real proof. |
| Test implementation details only | Refactor breaks tests unnecessarily. |
| Use real external services in unit tests | Flaky tests. |
| Ignore degraded states | Production behavior untested. |
| Use production-like sensitive data | Compliance risk. |
| Coverage without assertions | False confidence. |

## Review Checklist

- Are domain rules tested without infrastructure?
- Are use cases tested with fake ports?
- Are API contracts tested?
- Are infrastructure adapters tested separately?
- Are workers tested for retry and replay?
- Are security states tested?
- Are observability fields tested?
- Are test names behavior-focused?
- Is data synthetic and deterministic?
- Does CI run tests in the right lanes?

## Summary

Architecture and testing are connected.

If a service is layered well, it is easier to test well. If tests are painful, the architecture may be too coupled.

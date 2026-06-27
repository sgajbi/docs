# API Routers, Controllers, DTOs, and Contract Boundaries

## Purpose

This file explains how to design the HTTP/API layer of backend services without letting it absorb business logic.

The API layer is the boundary between external callers and application use cases.

## API Layer Responsibilities

The API layer owns:

- route registration
- request parsing
- request DTO validation
- response DTO serialization
- HTTP status codes
- dependency injection
- authentication/caller context extraction
- error mapping
- OpenAPI metadata

It should not own:

- domain calculations
- lifecycle decisions
- SQL queries
- upstream HTTP calls
- business policies
- retry/recovery workflows
- persistence transactions

## Thin Controller Rule

A controller should usually do this:

```text
parse request
  -> extract caller context
  -> create command/query
  -> call use case
  -> map result to response DTO
```

If a controller has complex loops, business rules, database calls, or long conditionals, it is probably too fat.

## DTOs

DTO means Data Transfer Object.

DTOs are API contract shapes.

DTOs should:

- match public request/response contract
- use documented field names
- support examples
- validate transport-level constraints
- allow backward-compatible aliases only when governed
- stay separate from domain and persistence models

DTOs should not:

- become domain models
- include internal-only fields
- expose ORM fields accidentally
- hide unsupported states
- contain business calculation methods

## Mapping

Use explicit mapping between DTOs and application commands/results.

Example:

```text
CalculationRequestDTO
  -> SubmitCalculationCommand
  -> CalculationResult
  -> CalculationResponseDTO
```

Mapping is not waste. It is contract control.

## OpenAPI Metadata

Each route should define:

- summary
- description
- tags
- request model
- response model
- error responses
- examples
- status codes
- authorization/caller-context notes
- idempotency requirement where applicable

OpenAPI should describe current supported behavior.

## Error Mapping

Application/domain errors should be mapped to safe API errors.

Example mapping:

| Error | HTTP Status |
|---|---|
| ValidationError | 400 or 422 |
| UnsupportedOperation | 400 |
| PermissionBlocked | 403 |
| ResourceNotFound | 404 |
| IdempotencyConflict | 409 |
| DependencyUnavailable | 503 |
| UpstreamTimeout | 504 |
| UnexpectedError | 500 |

Never expose raw stack traces or internal exception messages to clients.

## Caller Context Extraction

The API layer may extract caller context from headers, token claims, or gateway-injected values.

It should produce a typed context object:

```text
CallerContext
  actor_id
  tenant_id
  region
  role
  capabilities
  correlation_id
  trace_context
```

The application layer should receive the typed context, not raw request headers.

## Idempotency-Key Header

Write routes should require idempotency where repeated calls could duplicate effects.

Examples:

- create job
- submit workflow
- acknowledge action
- request report
- start replay
- publish event

The API layer can extract the key. The application layer should enforce idempotency semantics.

## API Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Controller contains business calculation | Hard to test and review. |
| Controller accesses database directly | Persistence leaks into transport. |
| API returns ORM model | Internal schema becomes public contract. |
| DTO reused as domain entity | Transport concerns corrupt domain model. |
| OpenAPI examples describe future features | False contract claims. |
| Error handler exposes raw exception | Security and support risk. |
| Caller context passed as raw headers everywhere | Duplication and inconsistent security. |

## API Layer Review Checklist

- Are controllers thin?
- Are request and response DTOs explicit?
- Is caller context typed?
- Are application commands/queries used?
- Are domain and persistence models hidden?
- Are errors mapped safely?
- Are idempotency headers handled?
- Is OpenAPI current?
- Are examples synthetic?
- Are unsupported states documented?
- Are contract tests present?

## Summary

The API layer should be a clean doorway into the application.

It should protect the contract, not become the application.

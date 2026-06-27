# API as Product and System Contract

## Purpose

This file explains the most important API design mindset: an API is a contract.

In enterprise financial platforms, APIs connect product experiences, domain services, reporting, operations, security, data products, and audit evidence. A weak API can create incorrect user behavior, operational confusion, security leakage, and long-term integration debt.

## API Contract Mental Model

An API contract defines:

```text
Capability
  -> ownership
  -> request shape
  -> response shape
  -> error semantics
  -> security expectations
  -> supportability states
  -> operational behavior
  -> compatibility promise
```

The contract is not only the code. It includes:

- route path
- HTTP method
- request body
- query parameters
- headers
- response schema
- examples
- status codes
- error model
- pagination
- idempotency
- OpenAPI documentation
- tests
- runbooks
- consumer expectations

## API As Product Surface

Even when an API is internal, it behaves like a product for its consumers.

Consumers need to know:

- what it does
- how to call it
- what it returns
- what it does not support
- how stable it is
- how failures appear
- how to troubleshoot it
- whether it is certified, proposed, or planned

## API Ownership

Every API must have an owner.

Ownership should answer:

- Which service owns the capability?
- Which team owns the contract?
- Which data source is authoritative?
- Which methodology applies?
- Who approves breaking changes?
- Who supports incidents?
- Who updates documentation?
- Who validates consumer impact?

## API Types

| API Type | Primary Purpose | Typical Owner |
|---|---|---|
| Domain API | Expose source-owned business capability | Domain service |
| BFF / Experience API | Compose product-ready payloads | Gateway/BFF service |
| Control-plane API | Expose operator workflow and runtime controls | Owning service |
| Integration API | Support system-to-system exchange | Domain/integration service |
| Data-product API | Expose governed reusable data product | Producer domain plus platform governance |
| Health/platform API | Expose runtime posture | Owning service / platform conventions |
| AI workflow API | Execute bounded AI workflow | AI capability service or governed domain service |

## API Contract Layers

A mature API has several contract layers:

| Layer | Contract Question |
|---|---|
| Business | What capability is exposed? |
| Domain | What terms and rules apply? |
| Transport | What route, method, body, and status codes are used? |
| Security | Who can call it and with what scope? |
| Operational | How is it observed and supported? |
| Compatibility | What can change without breaking consumers? |
| Evidence | What tests and certification prove behavior? |

## What APIs Should Not Be

An API should not be:

- a direct dump of database rows
- a thin mirror of every internal service function
- a transport wrapper around unclear business logic
- a future-state marketing claim
- a UI-specific shortcut hidden in a domain service
- an unbounded data extraction route
- a place where sensitive diagnostics leak
- a contract without tests

## Example API Contract Statement

```text
POST /performance/calculations

Purpose:
Submit a performance calculation request.

Owner:
Performance analytics service.

Consumers:
BFF, reporting service, operations tooling.

Request:
Calculation request with portfolio, period, input mode, and optional benchmark options.

Response:
Final result or 202 Accepted with calculation id for async polling.

Supportability:
READY, PARTIAL, STALE, DEGRADED, UNAVAILABLE, UNSUPPORTED.

Idempotency:
Required for async submission and write-like calculation execution records.

Not owned:
Portfolio source truth, benchmark master truth, report rendering, client publication.
```

## API Contract Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| API has no clear owner | Breaking changes and support gaps. |
| API mirrors internal database | Consumers become coupled to storage. |
| API route names follow implementation class names | Contract is hard to understand. |
| API returns raw upstream errors | Security and support risk. |
| API accepts arbitrary dictionaries | Weak validation and hidden incompatibility. |
| API docs describe planned behavior | False support claims. |
| API has no contract tests | Drift is discovered by consumers. |
| API has no unsupported-state model | Consumers misuse partial or invalid responses. |

## Design Review Questions

- What capability does this API expose?
- Who owns it?
- Which consumers use it?
- Is the route product/domain meaningful?
- Are request and response schemas explicit?
- Are errors structured?
- Is idempotency needed?
- Is pagination needed?
- What supportability states can occur?
- Is caller context required?
- Are examples current?
- What tests protect the contract?
- What would break if this field changed?

## Summary

Treat every API as a durable promise.

The best APIs are not just technically callable. They are understandable, safe, testable, supportable, and honest about their boundaries.

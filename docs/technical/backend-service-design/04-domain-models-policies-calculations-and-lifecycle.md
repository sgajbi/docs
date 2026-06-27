# Domain Models, Policies, Calculations, and Lifecycle

## Purpose

This file explains how to structure domain logic inside backend services.

The domain layer should express business meaning and behavior clearly. It should be independent of transport, persistence, and infrastructure.

## Domain Layer Responsibilities

The domain layer owns:

- entities
- value objects
- policies
- calculations
- lifecycle transitions
- invariants
- domain events
- domain errors
- reason codes
- methodology rules

## Entities

An entity has identity and lifecycle.

Examples:

- report job
- calculation execution
- proposal
- portfolio action
- candidate idea
- archive document
- data product declaration
- ingestion batch

Entities should protect invariants.

Example:

```text
A report job cannot move from ARCHIVED back to RUNNING.
A cancelled execution cannot publish a final result.
A reviewed proposal cannot be silently overwritten without versioning.
```

## Value Objects

A value object has meaning but no independent identity.

Examples:

- money amount
- date range
- performance period
- freshness bucket
- supportability state
- reason code
- currency pair
- pagination cursor
- entitlement scope

Value objects should validate themselves.

## Policies

Policies encode decision logic.

Examples:

- whether a request is supported
- whether data is stale
- whether a user can perform a workflow action
- whether a calculation can run
- whether a report job can be replayed
- whether an AI output can be published
- whether an item needs attention

Policy modules make decision logic visible and testable.

## Calculations

Calculation modules should be deterministic.

A calculation should define:

- inputs
- outputs
- assumptions
- methodology
- edge cases
- unsupported combinations
- precision/rounding behavior
- currency treatment
- dates and timing rules
- error and warning behavior
- golden examples

Avoid calculations hidden in:

- controllers
- SQL queries only
- UI components
- BFF mappers
- templates
- one-off scripts

## Lifecycle

Lifecycle logic should be explicit.

Example lifecycle:

```text
DRAFT
  -> SUBMITTED
  -> REVIEWED
  -> APPROVED
  -> EXECUTED
  -> ARCHIVED
```

Or async execution:

```text
ACCEPTED
  -> QUEUED
  -> RUNNING
  -> SUCCEEDED
  -> FAILED_RETRYABLE
  -> FAILED_FINAL
  -> EXPIRED
```

Lifecycle modules should define:

- allowed transitions
- forbidden transitions
- transition reason codes
- audit requirements
- idempotency behavior
- replay behavior
- terminal states

## Domain Events

Domain events represent meaningful facts.

Examples:

- ReportJobCreated
- CalculationCompleted
- ProposalReviewed
- DataProductCertified
- DocumentArchived
- WorkItemRetryExhausted

Domain events should not be raw database change notifications. They should describe business or operational meaning.

## Domain Errors

Domain errors should be meaningful.

Examples:

- UnsupportedCalculationInput
- StaleSourceData
- PermissionScopeRequired
- InvalidLifecycleTransition
- DuplicateIdempotencyKey
- SourceEvidenceMissing

These errors can later be mapped into API error responses or supportability states.

## Reason Codes

Reason codes should be bounded and documented.

Good:

```text
SOURCE_READY
SOURCE_STALE
DEPENDENCY_UNAVAILABLE
PERMISSION_BLOCKED
UNSUPPORTED_INPUT_MODE
REPLAY_NOT_ALLOWED_TERMINAL_STATE
```

Poor:

```text
failed
bad
error
unknown because upstream said xyz raw payload...
```

## Domain Test Strategy

Domain tests should prove:

- invariants
- lifecycle transitions
- calculation correctness
- policy decisions
- edge cases
- unsupported states
- reason-code mapping
- deterministic behavior

Domain tests should not require:

- web server
- database
- container
- external service
- cloud resource

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Domain logic in controller | Hard to test and reuse. |
| Calculation spread across multiple mappers | Methodology becomes invisible. |
| Lifecycle encoded as random string updates | Invalid transitions slip through. |
| Reason codes free-form | Metrics and support become inconsistent. |
| Value objects are plain strings everywhere | Validation duplicated or missed. |
| Domain imports HTTP client | Business logic tied to infrastructure. |
| Policy decision hidden in UI | Backend cannot enforce rule. |

## Review Checklist

- Are domain concepts named clearly?
- Are entities and value objects separated?
- Are invariants enforced?
- Are lifecycle transitions explicit?
- Are policies testable?
- Are calculations deterministic?
- Are reason codes bounded?
- Are domain errors meaningful?
- Are unsupported states modelled?
- Is domain logic free of framework and infrastructure dependencies?
- Are golden examples present for critical calculations?

## Summary

The domain layer is where the service's real value lives.

If the domain model is weak, the service becomes a pile of routes, tables, and scripts. If the domain model is strong, the service becomes easier to explain, test, and operate.

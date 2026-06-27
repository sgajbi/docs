# Clean Architecture Layering and Dependency Rules

## Purpose

This file explains how to structure backend services so business logic remains independent of frameworks, databases, queues, cloud SDKs, and transport details.

The goal is not ceremony. The goal is maintainability, testability, and safe change.

## Core Rule

Dependencies should point inward.

```text
API / framework
  -> application use cases
      -> domain model and policies
      -> ports
Infrastructure adapters
  -> ports
```

The domain should not depend on infrastructure.

## Layers

### API Layer

Owns:

- routes
- request parsing
- response formatting
- HTTP status codes
- dependency injection
- OpenAPI metadata
- error handler registration

Does not own:

- business rules
- SQL
- HTTP client calls
- calculation logic
- workflow orchestration beyond calling a use case

### Application Layer

Owns:

- use cases
- commands and queries
- workflow orchestration
- transaction boundary coordination
- idempotency coordination
- port invocation
- result assembly
- application-level errors

Does not own:

- HTTP framework details
- ORM models
- raw external payloads
- UI formatting

### Domain Layer

Owns:

- entities
- value objects
- policies
- calculations
- lifecycle transitions
- domain events
- invariants
- domain errors

Does not own:

- environment variables
- databases
- HTTP clients
- message brokers
- cloud storage
- logging implementation
- API DTOs

### Ports Layer

Owns:

- interfaces/protocols for outside dependencies
- repository contracts
- upstream client contracts
- event publisher contracts
- clock/id generator contracts

Does not own:

- concrete infrastructure implementation

### Infrastructure Layer

Owns:

- repository implementations
- database sessions
- migrations integration
- HTTP clients
- event publishers/consumers
- cache clients
- object storage clients
- configuration adapters

Does not own:

- business rules
- use-case decisions
- public API vocabulary

## Dependency Rules

| Rule | Reason |
|---|---|
| API can depend on application | HTTP routes should invoke use cases. |
| Application can depend on domain and ports | Use cases orchestrate business behavior and external needs. |
| Domain depends on nothing external | Domain remains testable and durable. |
| Infrastructure implements ports | External tools are replaceable. |
| DTOs do not leak into domain | Transport shape should not drive business model. |
| ORM models do not leak into API | Persistence should not become public contract. |

## Allowed And Forbidden Examples

Allowed:

```text
router -> CreateReportUseCase
CreateReportUseCase -> ReportPolicy
CreateReportUseCase -> ReportRepository port
PostgresReportRepository -> ReportRepository port
```

Forbidden:

```text
ReportPolicy -> SQLAlchemy session
Domain model -> FastAPI Request
Controller -> raw SQL and business calculation
UI DTO -> database row
Application service -> direct environment variable reads everywhere
```

## Why This Matters

Good layering enables:

- fast unit tests
- easier refactoring
- safer framework upgrades
- clearer code reviews
- better domain documentation
- simpler mocking/faking
- lower coupling
- stronger architecture governance

Poor layering creates:

- fragile tests
- controller bloat
- hidden business rules
- duplicated logic
- slow feedback
- high migration risk

## Practical Compromise

Small services do not need excessive folders.

A small service can still follow clean architecture by keeping responsibilities separate:

```text
api.py
use_cases.py
domain.py
ports.py
infrastructure.py
```

The folder structure is less important than dependency direction.

## Layering Review Checklist

- Does the route call a use case rather than implement behavior?
- Are business rules in domain or application modules?
- Does domain import framework or infrastructure packages?
- Are external systems accessed through ports?
- Are DTOs mapped to commands or domain objects?
- Are persistence models hidden from response models?
- Are application services small enough to understand?
- Are domain rules testable without a server or database?
- Is configuration centralized?
- Are infrastructure adapters thin?

## Anti-Patterns

| Anti-Pattern | Result |
|---|---|
| Fat controller | Hard to test and maintain. |
| Service layer as giant utility file | No clear use-case ownership. |
| Domain imports ORM | Business logic tied to database. |
| Infrastructure calls API DTOs directly | Transport concerns leak downward. |
| Use cases return raw database rows | Persistence leaks upward. |
| Every layer logs differently | Observability inconsistent. |
| Direct dependency on current time/random ids | Tests become flaky. |

## Summary

Clean architecture is a dependency discipline.

The central test is:

```text
Can the business behavior be tested without the web framework, database, queue, container, or cloud runtime?
```

If yes, the architecture is likely healthy.

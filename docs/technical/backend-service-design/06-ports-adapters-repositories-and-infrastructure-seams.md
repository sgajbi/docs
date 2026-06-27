# Ports, Adapters, Repositories, and Infrastructure Seams

## Purpose

This file explains how to isolate backend services from databases, upstream APIs, queues, caches, clocks, object stores, and external tools.

Infrastructure should be replaceable and testable. Business logic should not depend directly on implementation details.

## Ports

A port is an interface that defines what the application needs from the outside world.

Examples:

```text
PortfolioRepository
PerformanceSourceClient
ReportArchiveClient
EventPublisher
Clock
IdGenerator
ObjectStore
CacheClient
```

Ports belong near the application/domain boundary, not inside infrastructure implementation.

## Adapters

An adapter implements a port using a real tool or dependency.

Examples:

| Port | Adapter |
|---|---|
| PortfolioRepository | PostgresPortfolioRepository |
| SourceDataClient | HttpSourceDataClient |
| EventPublisher | KafkaEventPublisher |
| ObjectStore | S3ObjectStore |
| Cache | RedisCache |
| Clock | SystemClock |
| IdGenerator | UuidGenerator |

Tests can use fake adapters.

## Repository Pattern

A repository hides persistence details.

A good repository:

- exposes domain-oriented methods
- hides SQL/ORM details
- maps persistence records to domain objects
- handles persistence exceptions
- supports transaction boundaries
- avoids leaking database rows to application/API layers

Poor repository:

- returns raw ORM objects to API
- exposes query builder everywhere
- mixes domain rules into SQL methods
- requires controller to understand database schema

## Upstream Client Ports

External service calls should be behind client ports.

The application layer should not know:

- exact URL
- HTTP library
- retry implementation details
- authentication headers
- JSON transport quirks
- timeout configuration internals

The port should expose business meaning:

```text
get_portfolio_snapshot(portfolio_id, as_of_date)
```

not:

```text
get("/api/v1/source-products/x/y?foo=bar")
```

## Clock and ID Ports

Time and identifiers should be controllable in tests.

Use ports for:

- current time
- business date
- generated UUID
- sequence/id generator

This avoids flaky tests and hidden non-determinism.

## Cache Ports

Cache should be optional and explicit.

A cache port should define:

- key shape
- value shape
- TTL
- miss behavior
- stale behavior
- security scope
- invalidation model

Never hide critical source-of-truth behavior only inside cache.

## Event Publisher Ports

Event publishers should define:

- event type
- schema version
- idempotency key
- correlation id
- ordering requirements
- failure handling
- outbox behavior

Do not publish raw internal objects as events.

## Infrastructure Configuration

Adapters need configuration.

Good configuration:

- typed
- centralized
- environment-specific
- redacted in diagnostics
- validated at startup
- test-overridable

## Fake Adapters

Fake adapters help unit and use-case tests.

Examples:

- InMemoryReportRepository
- FakeSourceClient
- CapturingEventPublisher
- FixedClock
- DeterministicIdGenerator

Fakes should be simple and behavior-focused. Do not recreate full production infrastructure in a fake.

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Controller opens database session directly | Persistence leaks into API layer. |
| Domain object calls HTTP service | Domain tied to infrastructure. |
| Application hardcodes URLs | Poor portability and testing. |
| Tests mock deep internal methods | Brittle tests. |
| Fake adapter has different behavior from real adapter | False confidence. |
| Cache treated as source of truth | Incorrect data and stale behavior. |
| Event publisher emits raw database row | Contract instability. |

## Review Checklist

- Are external dependencies behind ports?
- Are repositories domain-oriented?
- Are persistence models mapped to domain models?
- Are upstream clients business-oriented?
- Are time and IDs controllable in tests?
- Are adapters thin?
- Are fakes available for use-case tests?
- Are configuration values typed and safe?
- Are infrastructure exceptions mapped?
- Are events schema-controlled?

## Summary

Ports and adapters protect business logic from infrastructure churn.

They also make testing faster, refactoring safer, and service boundaries clearer.

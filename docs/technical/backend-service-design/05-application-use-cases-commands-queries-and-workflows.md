# Application Use Cases, Commands, Queries, and Workflows

## Purpose

This file explains how the application layer should orchestrate domain logic and external dependencies.

The application layer answers:

```text
What should happen for this business operation?
```

It coordinates work without becoming a dumping ground for all business rules.

## Application Layer Responsibilities

The application layer owns:

- use cases
- commands
- queries
- workflow orchestration
- transaction coordination
- idempotency coordination
- port calls
- result assembly
- application-level errors
- async job submission
- recovery/replay orchestration

It should not own:

- HTTP request objects
- raw database logic
- low-level client calls
- UI rendering
- domain calculation details
- infrastructure configuration

## Commands

A command changes state or starts work.

Examples:

- CreateReportJobCommand
- SubmitCalculationCommand
- ReviewProposalCommand
- ReplayWorkItemCommand
- PublishOutboxEventCommand
- StartSourceIngestionCommand

Command design should include:

- caller context
- idempotency key where required
- target identity
- requested action
- input payload
- source/evidence references
- validation posture

## Queries

A query reads state.

Examples:

- GetExecutionStatusQuery
- GetPortfolioSummaryQuery
- SearchReportJobsQuery
- GetCandidateDetailQuery
- GetDataProductCatalogQuery

Queries should:

- be side-effect free
- enforce caller scope
- support pagination where needed
- return supportability where relevant
- avoid loading unbounded data

## Use Cases

A use case is a named application operation.

Example:

```text
SubmitPerformanceCalculationUseCase
GenerateReportBatchUseCase
ReplayFailedReportJobUseCase
GetAdvisorQueueUseCase
```

A use case should:

- validate command/query semantics
- call domain policies
- call ports
- coordinate persistence
- emit domain events if needed
- return an application result
- be testable with fake ports

## Use Case Example

```text
SubmitReportJobUseCase
  1. Validate caller context.
  2. Validate idempotency key.
  3. Check request is supported.
  4. Create domain ReportJob.
  5. Persist job through repository port.
  6. Publish outbox event.
  7. Return job id and accepted status.
```

The use case does not know whether the repository is PostgreSQL or an in-memory fake.

## Workflow Orchestration

Workflows coordinate multiple steps.

Examples:

- source ingestion
- async calculation
- report generation
- document render and archive
- AI workflow-pack execution
- replay/recovery
- data-product certification

Workflow design should define:

- start condition
- state transitions
- dependency calls
- retry behavior
- idempotency
- failure modes
- recovery actions
- supportability output
- operator visibility

## Transaction Boundaries

The application layer often decides transaction boundaries.

Questions:

- Which writes must succeed together?
- Which external calls should occur after commit?
- Should events be written through outbox?
- What happens on partial failure?
- Is the operation idempotent?
- Can the workflow resume?

Avoid:

- long transactions around external calls
- publishing events before durable state is committed
- committing partial workflow state without recovery path
- hiding transaction behavior inside controllers

## Application Results

A use case should return a structured result.

Example:

```text
ApplicationResult
  data
  supportability
  warnings
  lineage
  audit_ref
  next_actions
```

Do not return raw infrastructure objects or framework responses from the application layer.

## Application Errors

Application errors represent orchestration-level issues.

Examples:

- dependency unavailable
- idempotency conflict
- unsupported operation
- stale source dependency
- missing required source evidence
- authorization failed
- replay not allowed

The API layer can map these to structured HTTP responses.

## Testing Use Cases

Use-case tests should use fake ports.

Test:

- successful path
- unsupported request
- stale dependency
- permission blocked
- dependency unavailable
- idempotency duplicate
- idempotency conflict
- partial data
- event published
- transaction behavior where possible
- recovery/replay decision

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Use case calls FastAPI Request | Application tied to HTTP. |
| Use case contains SQL | Infrastructure leaks upward. |
| Controller orchestrates workflow | Hard to test and maintain. |
| Domain policy hidden in use case | Rules become harder to reuse. |
| External call inside DB transaction | Long locks and failure coupling. |
| No idempotency in state-changing use case | Duplicate side effects. |
| Use case returns ORM model | Persistence leaks into API. |

## Review Checklist

- Is the use case named after a business operation?
- Are commands and queries explicit?
- Does the use case call domain policy rather than duplicate rules?
- Are external dependencies accessed through ports?
- Is idempotency handled where needed?
- Is transaction behavior clear?
- Are events published safely?
- Are results structured?
- Are errors meaningful?
- Are tests using fakes rather than full infrastructure when possible?

## Summary

The application layer is the conductor.

It should coordinate domain behavior and infrastructure ports without becoming the place where all logic is hidden.

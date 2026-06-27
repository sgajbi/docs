# Service Mission, Capability Ownership, and Context Map

## Purpose

This file explains how to define the mission of a backend service before designing APIs, databases, workflows, or code structure.

A backend service should exist because it owns a meaningful capability, not because a team needed somewhere to put code.

## Service Mission Statement

Every backend service should have a short mission statement.

Template:

```text
This service owns <capability> for <business/platform context>.
It is authoritative for <data/state/rules>.
It exposes <contracts>.
It consumes <upstream dependencies>.
It does not own <explicit exclusions>.
```

Example:

```text
This service owns portfolio performance analytics for advisory and reporting workflows.
It is authoritative for performance methodology, calculation execution, result lineage, and performance supportability.
It exposes synchronous and asynchronous calculation APIs plus operator status APIs.
It consumes source portfolio, valuation, benchmark, and cashflow data.
It does not own portfolio accounting truth, instrument master data, risk methodology, report rendering, or client publication.
```

## Why Mission Clarity Matters

Without a clear mission:

- services accumulate unrelated logic
- teams duplicate calculations
- APIs become inconsistent
- ownership debates happen late
- UI and BFF layers invent missing behavior
- support teams do not know who owns failures
- product claims exceed implementation evidence

With a clear mission:

- teams know where to implement changes
- architecture reviews are objective
- tests target real behavior
- documentation can be truthful
- runbooks identify correct owners
- future services can integrate safely

## Capability Ownership

A capability is a durable business or technical responsibility.

Examples:

- portfolio source data
- performance analytics
- risk analytics
- advisory proposal lifecycle
- report job orchestration
- document rendering
- archive retrieval
- AI workflow execution
- product-facing API composition
- platform governance
- data-product catalog generation

A capability is not:

- one endpoint
- one screen
- one table
- one batch file
- one temporary project
- one team's backlog item

## Ownership Dimensions

| Dimension | Question |
|---|---|
| Business meaning | What concept does this service own? |
| State | What durable state does it own? |
| Rules | What business policies or calculations does it own? |
| Contracts | What APIs, events, or data products does it expose? |
| Consumers | Who depends on it? |
| Sources | What upstream systems does it trust? |
| Operations | Who supports it? |
| Evidence | What proves current support? |
| Exclusions | What must stay outside this service? |

## Explicit Non-Ownership

A professional service README or architecture doc should state what the service does not own.

Non-ownership protects the architecture.

Example exclusions:

```text
This service does not own:
- source-of-record portfolio holdings
- official performance methodology
- risk analytics
- report rendering
- document archive storage
- AI provider infrastructure
- client-ready publication
```

These exclusions prevent accidental scope drift.

## Context Map

A context map shows how services relate.

Example neutral map:

```text
Product UI
  -> Experience API / BFF
      -> Portfolio source service
      -> Performance analytics service
      -> Risk analytics service
      -> Advisory workflow service
      -> Reporting service
      -> Archive service
      -> AI capability service

Platform governance
  -> standards
  -> validators
  -> CI templates
  -> data-product catalog
  -> trust evidence
```

The context map should identify:

- upstream dependencies
- downstream consumers
- authority boundaries
- data-product relationships
- operational dependencies
- platform-owned contracts

## Service Responsibility Register

A simple service responsibility register:

| Area | Service Position |
|---|---|
| Service type | Domain service / BFF / shared capability / platform / worker |
| Primary capability | Short capability phrase |
| Authoritative data | Data owned by service |
| Authoritative rules | Rules owned by service |
| Public contracts | APIs/events/data products |
| Upstreams | Services or systems consumed |
| Downstreams | Consumers |
| Runtime state | Database, cache, queue, object store |
| Non-owned areas | Explicit exclusions |
| Support owner | Team or function |
| Readiness status | Implemented / partial / planned / unsupported / unknown |

## Capability Boundary Example

Poor boundary:

```text
The service handles portfolios, performance, risk, recommendations, reports, and UI summaries.
```

Better boundary:

```text
The service owns product-facing portfolio workspace composition.
It consumes portfolio, performance, risk, advisory, and reporting services.
It preserves upstream supportability and does not calculate official performance or risk locally.
```

## Review Questions

- Can the service mission be explained in one sentence?
- What does the service own?
- What does it not own?
- What data is source-owned here?
- What is consumed from other services?
- What contract does it expose?
- What behavior is unsupported?
- What evidence proves current support?
- Who supports the service in production?
- Is the boundary stable beyond one project?

## Anti-Patterns

| Anti-Pattern | Problem |
|---|---|
| Service mission is "handles everything for screen X" | Screen-centric design creates weak domain ownership. |
| No explicit exclusions | Scope expands silently. |
| Service owns data because it caches it | Cache is mistaken for authority. |
| BFF owns business calculation | Composition layer becomes hidden domain service. |
| Platform repo stores app-local truth | Standards and implementation drift apart. |
| Mission statement uses marketing terms only | Engineers cannot make design decisions from it. |

## Summary

A backend service starts with ownership.

Before writing code, define:

```text
What do we own?
What do we expose?
What do we consume?
What do we explicitly not own?
What evidence proves it?
```

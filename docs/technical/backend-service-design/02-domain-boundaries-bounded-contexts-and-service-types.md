# Domain Boundaries, Bounded Contexts, and Service Types

## Purpose

This file explains how to design service boundaries using domain-driven thinking and practical enterprise service types.

A well-designed platform separates domain authority, product composition, shared capabilities, and platform governance.

## Bounded Context

A bounded context is an area where business terms have a precise meaning.

Examples:

| Term | Possible Context |
|---|---|
| Position | Portfolio/accounting context |
| Return | Performance analytics context |
| Exposure | Risk or analytics context |
| Proposal | Advisory workflow context |
| Report job | Reporting context |
| Document | Archive/render context |
| Capability | Platform governance context |

The same word can mean different things in different contexts. A service boundary should protect the meaning inside its context.

## Service Types

### Domain Service

Owns a business capability.

Examples:

- portfolio source data
- performance analytics
- risk analytics
- advisory proposals
- discretionary mandate workflows
- reporting jobs

Design emphasis:

- domain model
- calculations/rules
- lifecycle
- persistence
- source ownership
- public APIs/events/data products
- tests and methodology evidence

### Experience API / BFF

Owns product-facing composition.

Design emphasis:

- stable UI-facing contract
- caller context
- partial/degraded mapping
- aggregation
- safe response shaping
- no domain authority drift

### Product UI

Owns user experience.

Design emphasis:

- presentation
- interaction
- loading/empty/error states
- capability-gated navigation
- browser proof
- no local domain truth

### Shared Capability Service

Owns reusable technical or platform-adjacent capability.

Examples:

- rendering
- archive
- AI workflow execution
- notification
- search
- evidence generation

Design emphasis:

- explicit consumers
- audit/retention
- fallback behavior
- safe boundaries
- no consuming-domain authority

### Platform Governance Service/Repository

Owns standards and cross-repo controls.

Design emphasis:

- executable standards
- validators
- scaffolds
- CI governance
- runtime conventions
- context and documentation routing
- minimal duplication of app-local truth

### Worker Service

Owns background processing.

Examples:

- ingestion worker
- async calculation executor
- report batch worker
- retention worker
- outbox delivery worker
- replay worker

Design emphasis:

- durable state
- idempotency
- retry/recovery
- metrics
- dead-letter handling
- operator controls

## Boundary Decision Matrix

| Question | Domain Service | BFF | UI | Shared Capability | Platform |
|---|---|---|---|---|---|
| Owns business rule? | Yes | No | No | Only own capability rule | No |
| Owns product presentation? | No | Contract shape only | Yes | No | No |
| Owns source data? | If domain source | No | No | No | No |
| Owns cross-repo standard? | No | No | No | No | Yes |
| Owns workflow execution? | If domain workflow | Orchestrates only | Initiates only | If capability workflow | No |
| Owns supportability? | For own behavior | For composition | For UI rendering | For capability | For platform controls |

## Boundary Smells

Boundary smells indicate design risk.

| Smell | Likely Problem |
|---|---|
| UI has calculation methods | Domain logic moved into presentation. |
| BFF has large business policies | Composition layer becoming domain authority. |
| Domain service imports UI vocabulary | Domain tied to product surface. |
| Shared service knows too much about one domain | Capability service becoming domain-specific. |
| Platform standard copies app-specific docs | Governance layer duplicating local truth. |
| Multiple services calculate same metric | Ownership unclear. |
| Service README cannot state exclusions | Boundary not mature. |

## Domain Boundary Example

Capability: advisor performance review.

Possible components:

```text
UI
  renders performance review screen

BFF
  composes portfolio, performance, risk, and advisory summary

Performance service
  owns return methodology and performance result

Risk service
  owns risk measures

Portfolio service
  owns source holdings and valuation data

Advisory service
  owns proposal or recommendation workflow

Reporting service
  owns report package generation

Archive service
  owns final document retention and retrieval
```

No one service should silently own all of this.

## Boundary Governance

Service boundaries should be reinforced through:

- README ownership section
- architecture docs
- route maps
- API contracts
- code folder structure
- tests
- CI guards
- CODEOWNERS where useful
- review checklists
- supported-feature documentation
- runbooks

## Anti-Patterns

| Anti-Pattern | Consequence |
|---|---|
| Service per screen | Duplicated domain logic and tight UI coupling. |
| Service per database table | Technical model replaces business boundary. |
| Gateway as super-service | BFF becomes bottleneck and hidden monolith. |
| Shared service accepts any responsibility | Capability boundary disappears. |
| Worker writes source truth without domain owner | Background process bypasses governance. |
| Platform team owns all standards but no validators | Standards become prose only. |

## Review Questions

- Is this a stable bounded context?
- Is the service type clear?
- Is domain authority explicit?
- Are non-owned responsibilities documented?
- Does the BFF preserve upstream authority?
- Does the UI avoid business truth?
- Does shared capability stay capability-focused?
- Are workers tied to owning domain state?
- Does platform governance enforce rather than duplicate?

## Summary

A service boundary is a contract of responsibility.

Good boundaries reduce duplicated logic, clarify ownership, improve tests, and make production support easier.

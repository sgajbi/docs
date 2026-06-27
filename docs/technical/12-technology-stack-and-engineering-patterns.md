# 12 - Technology Stack And Engineering Patterns

This guide explains the technology choices commonly used in a modern enterprise wealth-platform stack and how to reason about them as an engineer or engineering lead. The goal is not to memorize tools. The goal is to understand why each tool exists, what boundary it supports, and what quality risks it introduces.

## Stack Overview

| Area | Common technology pattern | Why it fits |
|---|---|---|
| Backend API | Python, FastAPI, Uvicorn | Fast typed HTTP APIs, OpenAPI generation, async-friendly request handling. |
| Contracts and validation | Pydantic and typed models | Explicit request/response models, validation, schema generation and safer refactoring. |
| Persistence | PostgreSQL, SQLAlchemy, Alembic | Relational consistency, query control, migration discipline and transaction support. |
| Service clients | httpx or equivalent typed HTTP client | Bounded upstream calls, timeouts, response parsing and correlation propagation. |
| Observability | structured logs, Prometheus metrics, OpenTelemetry-style tracing | Runtime diagnosis, SLO measurement, fan-out visibility and incident evidence. |
| Frontend | React, TypeScript, framework-managed routing/build, query/cache libraries | Component-based UI, typed frontend logic, data-fetching discipline and browser validation. |
| Data grids and charts | grid and chart libraries | Dense portfolio, risk, reporting and operations views with sorting, filtering and visualization. |
| Testing | pytest, coverage, Vitest, Playwright | Layered unit, integration, E2E and browser proof. |
| Static quality | ruff, mypy, lint rules, architecture gates | Fast feedback for style, type safety, complexity, imports and boundary drift. |
| Security checks | dependency audit, source scan, workflow permission checks | Dependency, code and CI supply-chain risk control. |
| Packaging/runtime | Docker, compose, orchestration-ready images | Reproducible local, CI and deployed runtime behavior. |
| CI/CD | repository-native commands and GitHub Actions-style lanes | Consistent local/remote validation and auditable release evidence. |

## Backend API Pattern

FastAPI-style services work well when the platform needs:

1. explicit API contracts,
2. generated OpenAPI documentation,
3. typed request and response models,
4. async dependency calls,
5. middleware for correlation and observability,
6. simple deployment through ASGI servers.

Engineering risks:

1. route handlers can become too large,
2. generated OpenAPI can drift from useful documentation,
3. async code can hide unbounded dependency calls,
4. business rules can leak into routers.

Good practice:

```text
router -> service/use case -> domain rule -> repository/client -> typed response
```

## Contract And Validation Pattern

Use typed models for:

1. input validation,
2. response shape,
3. error payloads,
4. event schemas,
5. data-product declarations,
6. supportability states.

Typed contracts help engineers refactor safely, but only when field names are domain-correct and model boundaries are not duplicated across layers.

Avoid:

1. passing raw dictionaries across service boundaries,
2. creating multiple versions of the same status enum,
3. adding compatibility aliases without governance,
4. relying on examples that no longer match implementation.

## Persistence And Migration Pattern

Relational storage is useful for:

1. portfolios, accounts, transactions and lifecycle state,
2. operational runs and job state,
3. audit and lineage records,
4. report status and evidence metadata,
5. workflow state machines.

Migration discipline matters because banking workflows cannot rely on manual database changes.

Good migration practice:

1. schema changes are versioned,
2. migration smoke runs in CI,
3. rollback or forward-fix posture is known,
4. query changes are reviewed for performance,
5. data retention and archival assumptions are documented.

## Service Client Pattern

An upstream client should own:

1. base URL configuration,
2. timeout,
3. retry policy where safe,
4. correlation propagation,
5. response parsing,
6. error mapping,
7. fallback or degraded-state semantics.

The caller should not need to know raw upstream HTTP details.

Bad pattern:

```text
business service -> raw HTTP call -> raw JSON parsing -> generic exception
```

Better pattern:

```text
business service -> typed client -> typed upstream result -> supportability-aware response
```

## Frontend Pattern

A serious enterprise UI should:

1. call supported backend contracts,
2. preserve loading, ready, empty, partial, degraded, permission-blocked and error states,
3. use typed UI models,
4. avoid reconstructing domain methodology in the browser,
5. keep dense data views scannable,
6. test user-critical flows with browser automation.

Frontend technology should serve workflow clarity. Complex grids, charts and dashboards need data contracts and state semantics more than decorative UI.

## Observability Tooling Pattern

Use structured logs for narrative events, metrics for measurement and traces for request flow.

| Tooling concept | Engineering use |
|---|---|
| Structured log | What happened and why. |
| Counter | How many times something happened. |
| Histogram | How long something took. |
| Gauge | Current value such as queue depth. |
| Trace span | Where time was spent across services. |
| Correlation id | How to follow a request through logs. |

Do not treat observability as an after-release task. Add it when behavior is implemented.

## Testing Toolchain Pattern

Use different tools for different proof:

| Proof need | Tooling pattern |
|---|---|
| Domain rule | Unit test. |
| API schema | Contract or OpenAPI gate. |
| Database behavior | Integration test. |
| Cross-service flow | E2E or live smoke test. |
| Browser behavior | Playwright-style browser test. |
| Frontend component logic | Component/unit test. |
| Performance regression | Load, benchmark or characterization test. |

Quality improves when each test proves behavior at the cheapest layer that can honestly prove it.

## Static Quality Pattern

Static checks should be fast and actionable.

Useful checks:

1. lint,
2. typecheck,
3. import-boundary rules,
4. complexity and source-size gates,
5. duplicate-code detection,
6. dead-code scan with understood exceptions,
7. dependency hygiene,
8. monetary-float guard,
9. unsafe observability label guard.

Do not promote a noisy scanner to a blocking gate until false positives and exception policy are understood.

## AI And Automation Pattern

AI-assisted engineering can help with:

1. summarizing code,
2. drafting tests,
3. generating documentation,
4. finding duplication,
5. preparing review checklists,
6. producing migration plans.

It must be governed by:

1. code review,
2. tests,
3. repository-native gates,
4. security checks,
5. documentation truth,
6. bounded prompts and evidence.

AI output is not engineering evidence. Passing tests, contracts and runtime proof are evidence.

## Tool Choice Tradeoffs

| Choice | Strength | Risk to manage |
|---|---|---|
| Python backend | Fast development, rich ecosystem, clear API frameworks. | Runtime type safety depends on validation, tests and typecheck discipline. |
| FastAPI/OpenAPI | Good contract generation and interactive docs. | Docs can look complete while examples and errors are weak. |
| PostgreSQL | Strong transactional model and query capability. | Query performance and migrations need active governance. |
| React/TypeScript | Strong UI composition and type-aware frontend work. | UI can drift from backend truth if contracts are weak. |
| Docker | Reproducible runtime. | Bad images can hide security, size and startup problems. |
| GitHub Actions-style CI | Strong automation and evidence trail. | Workflow sprawl and soft-fail checks can weaken trust. |

## How To Read A New Repository

Use this sequence:

1. README for purpose and commands,
2. repository context for boundaries,
3. package or project file for stack and dependencies,
4. Makefile or scripts for local quality commands,
5. workflow files for CI lanes,
6. source folders for architecture pattern,
7. tests for behavior proof,
8. Dockerfile and compose files for runtime,
9. docs and runbooks for operating truth,
10. quality scorecards or ledgers for known gaps.

## Engineering Lead Questions

1. Do the tools reinforce the architecture, or are they just present?
2. Are typed contracts used consistently?
3. Is CI proving the right things at the right layer?
4. Does observability protect supportability without leaking sensitive data?
5. Can the service run in a clean environment?
6. Are tool versions and runtime assumptions documented?
7. Is there a clear path from code change to release evidence?

# REST Resource, Action, and Route Design

## Purpose

This file explains how to design REST-style APIs that are clear, predictable, and enterprise-friendly.

REST does not mean every route must be a pure CRUD resource. Enterprise platforms often need a mix of resources, searches, calculations, commands, and control-plane actions. The key is to make that mix deliberate and consistent.

## Route Design Principles

Good routes should be:

- meaningful
- stable
- resource or capability oriented
- consistent in naming
- version-aware
- easy to document
- easy to authorize
- easy to test
- not tied to implementation classes
- not tied to database tables unless the database table is genuinely the resource

## HTTP Method Guidance

| Method | Use |
|---|---|
| GET | Read a resource or query result without side effects. |
| POST | Create a resource, submit a command, start a calculation, or execute complex search. |
| PUT | Replace a resource where full replacement is supported. |
| PATCH | Partially update a resource where patch semantics are clear. |
| DELETE | Delete, cancel, or retire where deletion semantics are clear. |

Avoid using GET for state-changing actions.

## Resource Routes

Resource routes represent durable objects or collections.

Examples:

```text
GET /api/v1/report-jobs
GET /api/v1/report-jobs/{job_id}
POST /api/v1/report-jobs
GET /api/v1/documents/{document_id}
GET /api/v1/data-products/{producer}/{name}/{version}
```

## Action Routes

Some operations are actions rather than simple CRUD.

Examples:

```text
POST /api/v1/report-jobs/{job_id}:replay
POST /api/v1/rebalance-runs/{run_id}:approve
POST /api/v1/calculations:submit
POST /api/v1/source-ingestion:run-once
```

Action routes should be used intentionally and documented clearly.

## Search Routes

For complex searches, use POST when filters are too rich for query parameters.

Examples:

```text
GET /api/v1/transactions?portfolioId=...&fromDate=...&toDate=...
POST /api/v1/report-jobs:search
POST /api/v1/portfolio-actions:search
```

GET is fine for simple filter sets. POST search is better for complex body-based criteria.

## Calculation Routes

Calculations can be synchronous or asynchronous.

Synchronous:

```text
POST /api/v1/performance/twr
```

Asynchronous:

```text
POST /api/v1/performance/calculations
GET /api/v1/performance/calculations/{calculation_id}
GET /api/v1/performance/calculations/{calculation_id}/result
```

Use async when computation is expensive, may exceed request timeout, or needs durable tracking.

## Control-Plane Routes

Control-plane routes expose operational capability.

Examples:

```text
GET /api/v1/runtime/status
GET /api/v1/runtime/work-items
POST /api/v1/runtime/work-items/{work_item_id}:replay
POST /api/v1/runtime/retention:run
```

Control-plane APIs need strong authorization and audit.

## Naming Conventions

Choose one convention and apply consistently.

Common guidance:

- use nouns for resources
- use plural collections
- use lower kebab-case or snake_case consistently
- use canonical domain vocabulary
- avoid implementation terms such as manager, helper, util
- avoid exposing internal service names in product-facing routes
- avoid abbreviations unless domain-standard

## Status Codes

| Status | Meaning |
|---|---|
| 200 | Successful read or completed synchronous operation. |
| 201 | Resource created. |
| 202 | Accepted for async processing. |
| 204 | Successful action without body. |
| 400 | Invalid request or unsupported input. |
| 401 | Not authenticated. |
| 403 | Authenticated but not authorized. |
| 404 | Not found or not visible. |
| 409 | Conflict, duplicate, idempotency mismatch, lifecycle conflict. |
| 422 | Semantic validation error where framework convention uses it. |
| 429 | Rate limit or back-pressure. |
| 500 | Unexpected internal error. |
| 502 | Upstream bad response. |
| 503 | Dependency or service unavailable. |
| 504 | Timeout. |

## Route Design Anti-Patterns

| Anti-Pattern | Problem |
|---|---|
| `/doThing` everywhere | No resource or capability model. |
| GET changes state | Unsafe retries, caching, and audit issues. |
| Route mirrors database table | Storage leaks into API. |
| Route mirrors internal method name | Implementation leaks into contract. |
| Multiple names for same concept | Consumer confusion and drift. |
| No versioning | Compatibility becomes accidental. |
| Unbounded list route | Performance incident risk. |
| Action route without idempotency | Duplicate side effects. |

## Route Review Checklist

- Is the route capability-oriented?
- Is the method correct?
- Is the route stable?
- Is the naming consistent?
- Is versioning clear?
- Are identifiers meaningful?
- Are actions justified?
- Are list routes bounded?
- Is idempotency required?
- Are status codes documented?
- Are unsupported states documented?
- Are route examples current?

## Summary

REST design is about clarity and predictability.

A good route tells consumers what capability is being used, what object or action is involved, and what behavior they can expect.

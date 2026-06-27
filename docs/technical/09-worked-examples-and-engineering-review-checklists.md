# 09 - Worked Examples And Engineering Review Checklists

Use these examples when reviewing architecture, implementation, PRs, production readiness or technical KT. For reusable practitioner prompts across architecture, API, data, CI/CD, testing, security, observability and production readiness, use [`16-practitioner-review-prompts-and-checklists.md`](16-practitioner-review-prompts-and-checklists.md).

## 1. New Domain API Endpoint

Scenario:

- `portfolio-service` adds `GET /api/v1/portfolios/{portfolio_id}/holdings`.
- The endpoint serves current holdings with source lineage and supportability.

Expected design:

| Area | Good implementation |
|---|---|
| Router | Validates path/query shape and delegates to service. |
| Service | Loads holdings, validates source readiness, maps supportability. |
| Repository | Owns query and row mapping. |
| Contract | Response model includes holdings, as-of date, source timestamp and support state. |
| Tests | Unit tests for mapping/supportability, integration tests for query, contract tests for OpenAPI. |
| Observability | Logs operation and bounded state; metrics include endpoint template and status class. |

Review checklist:

1. Does the endpoint expose domain-owned truth?
2. Is pagination needed?
3. Are stale and partial states explicit?
4. Are errors product-safe?
5. Does OpenAPI include examples?
6. Does CI run the contract gate?

## 2. Gateway Composition Over Multiple Services

Scenario:

- `gateway-service` composes portfolio summary, performance summary and risk summary for a UI panel.

Good behavior:

1. call upstream services with timeouts,
2. preserve correlation headers,
3. preserve each upstream supportability state,
4. avoid recalculating performance or risk,
5. return partial state if one optional source is degraded and the business workflow allows it,
6. block when a mandatory source is unavailable,
7. log bounded fan-out state.

Bad behavior:

1. swallowing upstream errors,
2. returning `ready` when risk is stale,
3. calculating risk locally from holdings,
4. exposing raw upstream stack traces,
5. letting UI infer missing source states.

## 3. Data Product Certification

Scenario:

- `analytics-service` wants to publish `PerformanceReturns:v1` as a reusable data product.

Required evidence:

| Control | Evidence |
|---|---|
| Product identity | Stable product id, version, owner and schema. |
| Source ownership | Domain declaration points to owning service and source path. |
| Runtime trust | Freshness, completeness, quality and lineage telemetry. |
| Access policy | Roles, scopes, allowed use cases and denial behavior. |
| Evidence policy | Audience-filtered evidence without restricted payloads. |
| API publication | Read-only contract exposes product and trust posture. |
| Tests | Contract, policy, telemetry and certification gate tests. |

Definition of done:

```text
declared -> catalog-visible -> telemetry-backed -> policy-backed -> certified
```

## 4. Promoting A CI Gate

Scenario:

- Duplicate-code inventory shows zero accepted duplicate implementation hotspots.
- Team wants to make duplicate-code detection blocking.

Promotion checklist:

1. baseline is measured,
2. command is deterministic,
3. false positives are understood,
4. failure output points to files and rule,
5. target is repo-native,
6. gate is in the right lane,
7. focused pass/fail tests exist,
8. scorecard and docs are updated,
9. exception policy is documented.

Do not promote if the scanner is noisy, subjective or slow for the intended lane.

## 5. Production Incident Review

Scenario:

- A report-generation endpoint starts returning stale report data after a source ingestion delay.

Incident review:

| Question | Good answer |
|---|---|
| Detection | Alert fired on freshness SLO and report stale-state metric. |
| Impact | Affected report ids and business workflows are identified without exposing client data. |
| Containment | Client-ready publication is blocked for stale reports. |
| Diagnosis | Trace shows ingestion delay and report cache not invalidated. |
| Recovery | Ingestion replay and cache invalidation completed. |
| Prevention | Add regression test, metric and runbook update. |

Post-incident artifacts:

1. timeline,
2. root cause,
3. impacted workflows,
4. mitigation,
5. permanent fix,
6. validation evidence,
7. follow-up backlog.

## 6. PR Review Checklist

For any meaningful backend PR:

1. Is the scope narrow and understandable?
2. Does the code preserve ownership boundaries?
3. Are routers thin?
4. Are contracts explicit and documented?
5. Are tests meaningful and at the right level?
6. Are security and sensitive-data controls preserved?
7. Are logs and metrics bounded?
8. Are failure states tested?
9. Do docs and runbooks reflect changed truth?
10. Does the PR evidence list actual commands and CI lanes?

## 7. New Service Readiness Checklist

Before treating a new service as enterprise-ready, confirm:

1. repository role and ownership are documented,
2. local commands exist,
3. CI lanes are mapped,
4. OpenAPI or equivalent contract is generated,
5. health/readiness endpoints exist,
6. structured logs and metrics are present,
7. configuration is validated,
8. secrets are externalized,
9. Docker build or deployment smoke passes,
10. tests cover unit, integration and key E2E behavior,
11. runbook exists,
12. known gaps are explicit.

## 8. Engineering Leadership Self-Assessment

Use this as a personal checklist after reading a service:

1. Can I explain the service boundary in two minutes?
2. Can I identify the owning domain model and source of truth?
3. Can I run the local quality command?
4. Can I explain the CI lane model?
5. Can I find the OpenAPI contract?
6. Can I describe how the service logs, measures and traces requests?
7. Can I explain how it handles stale, partial, unavailable and permission-blocked states?
8. Can I identify the highest-risk failure mode?
9. Can I propose one small improvement slice with tests and evidence?

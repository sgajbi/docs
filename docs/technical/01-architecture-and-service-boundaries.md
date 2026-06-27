# 01 - Architecture And Service Boundaries

Good enterprise architecture starts with ownership. A service boundary should make it clear who owns data, business rules, runtime behavior, contracts, evidence and support.

## Common Platform Roles

| Role | Responsibility | Typical anti-pattern |
|---|---|---|
| Domain service | Owns business facts, calculations, domain workflow, persistence, source lineage and authoritative APIs. | Moving domain logic into the UI, gateway or batch scripts. |
| Experience API / gateway | Composes domain APIs into product-oriented responses for UI or external clients. | Becoming the hidden owner of upstream domain truth. |
| Product UI | Presents user workflows, states, decisions and evidence through backend-backed contracts. | Inventing trust, status, metrics or decisions locally in the browser. |
| Shared capability service | Provides reusable capability such as AI, rendering, archive, notification or search. | Claiming business ownership for content it only processes. |
| Platform governance layer | Owns standards, validators, CI templates, common contracts, runtime orchestration and cross-repo evidence. | Duplicating app-local implementation truth or hand-editing generated evidence. |

## Boundary Rules

1. A domain service owns domain truth.
2. A gateway composes and normalizes, but does not recalculate authoritative business methodology.
3. A UI consumes supported contracts, and should not infer business supportability from missing data.
4. Shared capability services execute bounded capabilities for approved callers.
5. Platform governance validates and publishes evidence, but generated evidence must derive from source-owned declarations or runtime proof.

## Domain Authority

A domain service is authoritative when it owns:

1. the canonical input model,
2. the calculation or workflow rule,
3. the persistence or source integration,
4. the lifecycle event semantics,
5. the API contract,
6. the tests that prove business behavior,
7. the supportability state and failure behavior.

Example:

```text
portfolio-service owns holdings, transactions and portfolio-source readiness.
risk-service owns risk methodology and risk output posture.
gateway-service can compose a portfolio risk view, but it must not recalculate risk locally.
```

## Composition Boundary

An experience API may:

1. call multiple upstream services,
2. normalize response envelopes,
3. map upstream errors to product-safe errors,
4. preserve source supportability,
5. expose bounded diagnostics for operators,
6. apply client-facing pagination and presentation shape.

It should not:

1. invent missing upstream facts,
2. silently convert a degraded upstream response into ready state,
3. perform domain calculations owned by another service,
4. hide partial or stale data,
5. bypass access policy or source lineage.

## Source Ownership Matrix

Use this matrix when reviewing a new capability.

| Question | Good answer |
|---|---|
| Who owns the source data? | A named domain service or external source owner. |
| Who owns the calculation? | The service with methodology, tests and evidence. |
| Who owns the API contract? | The service exposing the supported boundary. |
| Who owns UI composition? | The experience API and UI, without taking domain authority. |
| Who owns runtime proof? | The service for local behavior; the platform for cross-service proof. |
| Who owns support? | The operational owner named in docs/runbooks. |

## Architecture Smells

Watch for:

1. controllers with complex business logic,
2. gateway code that calculates domain metrics,
3. UI code that derives supportability from presentation-only fields,
4. duplicated DTOs, status enums or vocabulary across services,
5. hand-authored generated artifacts,
6. unsupported aliases kept alive without tests or retirement plan,
7. runbooks that claim behavior not backed by code or metrics.

## Engineering Lead Checklist

Before approving an architecture slice, confirm:

1. the service boundary is written down,
2. public contracts map to owning services,
3. degraded, stale, partial and permission-blocked states are explicit,
4. cross-service calls have timeouts and safe failure behavior,
5. tests exist at the correct layer,
6. docs, runbooks and CI evidence match the implementation.

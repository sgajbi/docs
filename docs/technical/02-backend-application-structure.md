# 02 - Backend Application Structure

Backend services should be boring in the best way: predictable folders, thin HTTP routes, clear domain modules, typed contracts, explicit repositories, and central runtime composition.

## Common Service Layout

```text
src/
  app/
    routers/
    services/
    contracts/
    middleware/
    repositories/
    integrations/
    observability/
    runtime/
tests/
  unit/
  contract/
  integration/
  e2e/
scripts/
docs/
```

Some services may use `src/api`, `src/core` and `src/infrastructure` instead. The exact folder names matter less than the ownership split.

## Layer Responsibilities

| Layer | Responsibility | Should avoid |
|---|---|---|
| Router / controller | HTTP shape, validation handoff, dependency injection, status codes. | Business rules, SQL, retries, complex mapping. |
| Service / application layer | Orchestrates use cases and domain workflow. | HTTP-specific assumptions and raw persistence details. |
| Domain / core | Business rules, calculations, policy decisions, deterministic transformations. | Framework dependencies and request objects. |
| Repository | Persistence queries and transaction boundaries. | Business interpretation outside query mapping. |
| Integration client | External service calls, timeouts, retries, response validation. | Domain recomputation and silent fallback. |
| Contract / DTO | Request, response, event and internal typed payload definitions. | Ambiguous field names and ungoverned aliases. |
| Runtime composition | Wires services, repositories, clients, config and background workers. | Importing API routes or UI concerns. |

## Thin Router Pattern

Good route handlers are short:

```python
@router.post("/rebalance/simulations", response_model=SimulationResponse)
async def create_simulation(request: SimulationRequest) -> SimulationResponse:
    return simulation_service.create(request)
```

The route should not contain:

1. domain calculations,
2. direct SQL,
3. raw environment reads,
4. unbounded retry loops,
5. ad hoc error vocabulary,
6. large response-shaping logic.

## Service Boundary Pattern

Use a service object or function when a use case has:

1. multiple input sources,
2. domain validation,
3. policy decisions,
4. persistence,
5. event publication,
6. supportability state,
7. multiple failure modes.

Example use-case flow:

```text
validate request -> load source context -> run domain rule -> persist result -> emit event -> return supported response
```

## Repository Pattern

Repositories should isolate persistence and query shape.

Good repository methods:

1. have clear business-oriented names,
2. return typed rows or domain objects,
3. keep SQL and database details local,
4. preserve source timestamps and lineage,
5. support focused unit or integration tests.

Avoid repository methods that mix:

1. query construction,
2. domain scoring,
3. error mapping,
4. response DTO assembly,
5. unrelated query families.

When a repository method becomes hard to review, extract helper functions for query clauses, row mapping and summary calculation.

## Integration Client Pattern

Upstream clients should define:

1. base URL from configuration,
2. bounded timeout,
3. bounded retries only where safe,
4. correlation header propagation,
5. typed response parsing,
6. product-safe error mapping,
7. supportability output when upstream data is unavailable.

Never let an upstream client silently return invented successful data.

## Configuration Pattern

Configuration should be:

1. centralized,
2. typed where possible,
3. environment-scoped,
4. documented,
5. safe to log only after redaction,
6. tested for missing or invalid required values.

Avoid direct `os.environ` reads across business logic. Use central settings providers so tests, docs and runtime behavior agree.

## Background Work

Use background workers or async jobs when:

1. work is long running,
2. source ingestion or calculation is heavy,
3. retries and recovery matter,
4. progress must be observable,
5. a user request should not block on completion.

Background work needs durable state, idempotency keys, health checks, metrics, operator diagnostics and replay rules.

## Maintainability Heuristics

Prefer refactoring when you see:

1. large route functions,
2. repeated mapping logic,
3. duplicated status enums,
4. mixed domain and infrastructure code,
5. untyped dictionaries crossing layers,
6. tests that only assert mocks were called,
7. broad exception catches that hide root causes.

## Practical Review Questions

1. Can a new engineer find the owning module in under five minutes?
2. Can the domain rule be tested without HTTP?
3. Can the route be understood without reading SQL?
4. Can an operator diagnose a failed request from logs and metrics?
5. Can a downstream consumer trust the OpenAPI contract?
6. Does the code preserve source lineage and supportability state?

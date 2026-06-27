# 08 - Testing, Performance And Resilience

Mature engineering teams use tests to prove behavior, not to decorate coverage dashboards. Tests should make refactoring safer, contracts clearer and production failures less likely.

## Test Pyramid

Target distribution:

| Level | Typical share | Purpose |
|---|---:|---|
| Unit | 70-85% | Domain rules, calculations, validation, transformations and edge cases. |
| Integration | 15-25% | Persistence, adapters, HTTP boundaries, queues and dependency wiring. |
| E2E | 5-10% | Critical user journeys and high-risk cross-service workflows. |

## Meaningful Coverage

Meaningful tests assert:

1. business outputs,
2. failure behavior,
3. error contracts,
4. source-lineage preservation,
5. supportability states,
6. edge cases,
7. realistic data relationships.

Weak tests only assert:

1. a mock was called,
2. a route returned status 200,
3. a branch was executed,
4. a placeholder field exists,
5. a superficial snapshot matches.

## Contract Tests

Contract tests should protect:

1. OpenAPI schema,
2. request and response models,
3. examples,
4. error semantics,
5. vocabulary,
6. deprecated aliases,
7. generated artifacts,
8. consumer compatibility.

## Integration Tests

Use integration tests when behavior depends on:

1. database queries,
2. migrations,
3. upstream clients,
4. queues,
5. filesystem or object storage,
6. async job state,
7. authentication/authorization middleware.

## E2E Tests

E2E tests should be few but valuable:

1. product-critical happy path,
2. permission-blocked path,
3. degraded upstream path,
4. report or artifact generation path,
5. browser-backed path for UI surfaces.

## Performance Hygiene

API performance guardrails:

1. pagination on collections,
2. maximum response limits,
3. query-plan review for critical queries,
4. indexes for high-cardinality filters,
5. bounded concurrency,
6. async processing for heavy jobs,
7. caching with owner, TTL and invalidation policy,
8. p95/p99 latency budgets for critical endpoints.

## Resilience Patterns

| Pattern | Use |
|---|---|
| Timeout | Prevent indefinite waits on dependencies. |
| Bounded retry | Retry transient failures without storming dependencies. |
| Circuit breaker | Stop repeated calls to a failing dependency. |
| Bulkhead | Isolate expensive or failing workloads. |
| Idempotency key | Prevent duplicate writes on retry. |
| Durable job state | Recover long-running work after process failure. |
| Back-pressure | Protect service under load. |
| Graceful degradation | Return partial or degraded state when business workflow allows. |

## Migration And Runtime Smoke

Services with persistence need:

1. migration smoke tests,
2. startup validation,
3. rollback awareness,
4. seed or fixture discipline,
5. schema drift detection,
6. migration documentation.

## Test Review Questions

1. Does the test prove business behavior?
2. Does it cover the failure mode?
3. Is it at the right pyramid level?
4. Does it protect the public contract?
5. Does it avoid brittle implementation details?
6. Does CI run it in the correct lane?

## Performance Review Questions

1. What is the latency budget?
2. What is the maximum payload size?
3. Which dependency dominates response time?
4. Is heavy work isolated from request/response paths?
5. Are retries bounded?
6. Can the system recover from partial failure?

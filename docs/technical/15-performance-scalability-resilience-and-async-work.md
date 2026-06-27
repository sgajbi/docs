# 15 - Performance, Scalability, Resilience And Async Work

Performance is not only speed. In enterprise platforms it means predictable behavior under load, safe degradation when dependencies fail, controlled recovery after incidents and clear evidence that the system can meet its operating assumptions.

Use this guide when designing high-volume APIs, background calculations, report generation, file production, data ingestion, portfolio analytics, notification workflows, event processing or any capability where work may exceed a normal request/response window.

## Core Design Concerns

Every critical service should define:

1. latency budgets,
2. throughput assumptions,
3. dependency timeouts,
4. retry policy,
5. back-pressure behavior,
6. concurrency limits,
7. idempotency rules,
8. async execution pattern,
9. durable execution state,
10. recovery and replay controls,
11. caching rules,
12. batching rules,
13. pagination and large-read limits,
14. performance gates,
15. operational runbooks.

If any of these are implicit, the system will usually discover the answer during production stress.

## Latency Budgets

A latency budget states how long a workflow may take before it becomes operationally or commercially unacceptable.

| Workflow type | Typical budget question |
|---|---|
| Interactive read | Can the user continue scanning without waiting too long? |
| Advisory calculation | Does the calculation return fast enough for a conversation or review meeting? |
| Report generation | Can the workflow be async while preserving status and auditability? |
| Batch ingestion | Can downstream consumers tolerate the completion window? |
| Operational replay | Can support recover data inside the agreed support window? |

Do not set budgets only at the aggregate endpoint. Break them down by dependency so teams know which call dominates the path.

## Timeouts

Every network and database call should have a timeout. Without timeouts, request queues build up, worker pools saturate, retries multiply damage and support teams cannot distinguish slow dependencies from dead ones.

Define:

1. connect timeout,
2. read timeout,
3. overall request timeout,
4. worker timeout,
5. database query timeout,
6. external dependency timeout,
7. queue visibility or lease timeout.

Timeouts should align with caller expectations. A UI-facing API should not wait longer than the product flow can usefully tolerate.

## Retries

Retries are useful only when the failure is transient and the action is safe to repeat.

Retry candidates:

1. temporary network failures,
2. retryable dependency status codes,
3. queue processing errors,
4. optimistic locking conflicts where the operation is designed for retry,
5. temporary database connection failures.

Avoid retries for:

1. validation errors,
2. permission failures,
3. unsupported requests,
4. deterministic business-rule failures,
5. non-idempotent writes without an idempotency key,
6. large fan-out workflows where retry storms can overload dependencies.

A retry policy should define maximum attempts, backoff, jitter, retryable errors, non-retryable errors, logging, metrics and dead-letter handling.

## Back Pressure

Back pressure protects a system by slowing or rejecting work before overload becomes a wider incident.

Common controls:

1. queue length limits,
2. rate limits,
3. worker concurrency limits,
4. circuit breakers,
5. bulkheads by workload type,
6. bounded batch size,
7. pagination,
8. request rejection with a safe error,
9. load shedding for optional work,
10. async handoff for heavy workflows.

Back pressure is better than uncontrolled failure. A clear `429`, `503` or domain-specific blocked state is easier to support than silent saturation.

## Idempotency

Idempotency ensures repeated calls do not create duplicate effects.

Use idempotency for:

1. create workflows,
2. submit report jobs,
3. start calculations,
4. publish events,
5. acknowledge actions,
6. replay jobs,
7. external command handoff.

An idempotency design needs:

1. stable key,
2. caller or tenant scope,
3. request hash,
4. duplicate detection,
5. conflict behavior for mismatched duplicate keys,
6. stored result or execution status,
7. retention period,
8. audit trail,
9. operational lookup path.

Do not rely on a disabled UI button as the only duplicate-write control.

## Async Execution Pattern

Use async execution when a workflow is too slow, expensive, fragile or dependency-heavy for a normal request thread.

```text
Client submits request
  -> service validates request
  -> service creates execution record
  -> service returns 202 Accepted with job id
  -> worker processes durable work item
  -> client polls status endpoint or receives event
  -> service returns result reference, final result or terminal failure
```

Required components:

1. execution id,
2. status model,
3. durable work item,
4. result storage,
5. polling or notification endpoint,
6. recovery path,
7. retention policy,
8. metrics and logs,
9. idempotency behavior,
10. authorization checks for status and result access.

## Durable Execution State

Use explicit execution states rather than vague booleans.

| State | Meaning |
|---|---|
| `ACCEPTED` | Request accepted and recorded. |
| `QUEUED` | Waiting for a worker or capacity. |
| `RUNNING` | Work is in progress. |
| `SUCCEEDED` | Work completed and result is available. |
| `FAILED_RETRYABLE` | Work failed but can be retried safely. |
| `FAILED_FINAL` | Work failed permanently. |
| `CANCELLED` | Work was cancelled by an authorized action. |
| `EXPIRED` | Result is no longer retained. |

Execution state should include created time, updated time, owner, input reference, result reference, failure reason, retry count and correlation id where applicable.

## Recovery And Replay

Recovery is part of the product, not an afterthought.

A replay or recovery action should be:

1. authorized,
2. audited,
3. idempotent or duplicate-safe,
4. bounded by scope and time,
5. observable through logs and metrics,
6. documented in a runbook,
7. tested at least through a focused smoke or integration scenario.

Avoid recovery designs that require manual database edits as the normal support path.

## Caching

Caching can improve performance, but it can also create correctness and privacy risk.

Good cache candidates:

1. reference data,
2. product configuration,
3. capability posture,
4. expensive read models,
5. stable lookup data,
6. non-sensitive generated summaries with clear source scope.

Risky cache candidates:

1. entitlement decisions,
2. rapidly changing positions,
3. transactions,
4. prices,
5. source freshness,
6. client-specific sensitive data,
7. AI outputs without provenance and retention policy.

Cache design should define key, TTL, invalidation, stale behavior, security scope, metric coverage and fallback behavior.

## Batching

Batching improves throughput when many small operations can be grouped, but it can hide partial failures if state is not explicit.

Batch design should define:

1. batch identity,
2. item identity,
3. item state,
4. partial success behavior,
5. retry and replay behavior,
6. concurrency limit,
7. progress status,
8. operator controls,
9. output retention,
10. evidence trail.

Prefer item-level visibility for operationally important batches.

## Pagination And Large Reads

Large reads should be bounded and predictable.

Use:

1. cursor or keyset pagination,
2. server-side maximum page size,
3. stable sort order,
4. indexed filters,
5. time-window queries,
6. next-token semantics,
7. payload-size limits,
8. partial-result behavior where appropriate.

Offset pagination may be acceptable for small administrative tables, but it becomes expensive and inconsistent for large mutable datasets.

## Performance Gates

Performance gates do not need to be elaborate at the start. A useful gate can be a focused benchmark or regression check that protects the highest-risk path.

Examples:

| Gate | Protects |
|---|---|
| Query-plan check | Prevents accidental table scans on critical reads. |
| Endpoint latency smoke | Detects obvious request path regressions. |
| Payload-size check | Prevents unbounded response growth. |
| Worker throughput smoke | Confirms background work still drains. |
| Browser performance smoke | Confirms a dense UI remains usable. |
| Migration timing smoke | Confirms deployment does not block for too long. |

Promote a performance gate to blocking only when it is deterministic, actionable and aligned with a real production risk.

## Resilience Anti-Patterns

| Anti-pattern | Risk |
|---|---|
| No timeout | Cascading hangs. |
| Retry everything | Failure amplification. |
| No idempotency for writes | Duplicate side effects. |
| Long work inside a request thread | Timeouts and poor user experience. |
| Process-local async result only | Result lost on restart. |
| Unbounded queries | Slow APIs and outages. |
| Cache without invalidation | Stale or incorrect decisions. |
| No dead-letter path | Failed work disappears. |
| No operator recovery | Manual database repair becomes normal. |
| No performance gate | Regressions are found late. |

## Review Checklist

1. What is the latency budget?
2. Which dependency dominates response time?
3. Are timeouts defined for every external call?
4. Are retries bounded and safe?
5. Is back pressure defined?
6. Are writes idempotent?
7. Should the workflow be async?
8. Is execution state durable?
9. Is status polling or notification supported?
10. Is recovery or replay documented?
11. Are results retained and cleaned up?
12. Are large reads paginated?
13. Are indexes aligned with query paths?
14. Are caches scoped safely?
15. Are performance gates present for critical paths?
16. Are known limits documented?

## Operating Principle

A mature service does not only handle the happy path. It knows how to slow down, reject safely, retry carefully, recover explicitly and explain what happened.

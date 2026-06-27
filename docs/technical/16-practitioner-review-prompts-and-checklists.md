# 16 - Practitioner Review Prompts And Checklists

Use these prompts for architecture reviews, PR reviews, production readiness reviews, KT sessions, coaching conversations and personal technical development. They are written to improve practical judgement: ownership, evidence, failure behavior, security, supportability and release quality.

## Architecture Review Checklist

1. What capability is being built or changed?
2. Which service owns the domain truth?
3. Which services consume that truth?
4. Is the experience layer only composing, or is it taking ownership?
5. Does the UI depend on a stable product contract?
6. Are domain models separate from transport models?
7. Are persistence models hidden from API responses?
8. Are unsupported states explicit?
9. Are architecture decisions documented?
10. Is any calculation or business rule duplicated?
11. Are future-state claims clearly marked as planned?

## API Review Checklist

1. Is the route owned by the correct layer?
2. Is the route name business meaningful?
3. Are request and response models explicit?
4. Is OpenAPI complete and current?
5. Are errors structured and safe?
6. Is idempotency required?
7. Is pagination needed?
8. Are caller-context headers defined?
9. Are degraded states represented?
10. Are examples synthetic and current?
11. Are legacy aliases governed?
12. Are contract tests present?

## Data Product Review Checklist

1. Who is the producer?
2. What is the product name and version?
3. What does the data mean?
4. What source systems feed it?
5. What freshness is expected?
6. What lineage is available?
7. What quality checks exist?
8. Who can consume it?
9. What SLO applies?
10. What evidence proves it?
11. Is it proposed, visible, certified, deprecated or retired?
12. Are consumers declared?
13. Does a gateway or UI incorrectly become product authority?

## CI/CD Review Checklist

1. Which lane should run?
2. Are local checks fast and useful?
3. Are PR checks sufficient for merge confidence?
4. Are heavy checks placed in the right lane?
5. Are security scans included?
6. Are dependency checks included?
7. Is Docker build or runtime proof required?
8. Are migrations tested?
9. Are OpenAPI and contract gates present?
10. Are report-only checks tracked?
11. Is release evidence generated?

## Testing Review Checklist

1. What behavior changed?
2. Which unit tests prove local rules?
3. Which domain tests prove business correctness?
4. Which contract tests protect API shape?
5. Which integration tests prove adapters?
6. Which browser or E2E tests prove product flows?
7. Are degraded, stale, partial, unavailable and permission-blocked states tested?
8. Is test data synthetic?
9. Are assertions meaningful?
10. Is coverage used as a signal rather than a vanity number?

## Security Review Checklist

1. Are secrets excluded from source and artifacts?
2. Is configuration typed and redacted?
3. Are authentication assumptions clear?
4. Are authorization rules explicit?
5. Are write actions protected?
6. Is caller context propagated safely?
7. Are permission-blocked responses safe?
8. Are logs and metrics free of sensitive data?
9. Are dependency and container scans handled?
10. Are workflow permissions least privilege?
11. Are AI prompts, outputs and evidence governed?

## Observability Review Checklist

1. Are logs structured?
2. Are event names bounded?
3. Are metric labels low-cardinality?
4. Are sensitive fields excluded?
5. Are traces propagated?
6. Is correlation handled consistently?
7. Is readiness meaningful?
8. Are supportability states explicit?
9. Are dashboards based on implemented metrics?
10. Are alerts actionable?
11. Are runbooks current?
12. Are recovery actions observable and audited?

## Production Readiness Checklist

1. Does the service start in a supported runtime profile?
2. Are health and readiness endpoints meaningful?
3. Are dependencies documented?
4. Are environment variables documented?
5. Are secrets handled safely?
6. Are resource requests and limits defined where applicable?
7. Are migrations safe?
8. Are logs, metrics and traces available?
9. Are dashboards and alerts defined?
10. Are runbooks written?
11. Is rollback documented?
12. Is support ownership clear?
13. Is release evidence available?
14. Are residual risks accepted or backlogged?

## Engineering Lead Prompts

Use these prompts to test whether a design is implementation-backed and operationally credible.

| Prompt | Strong answer pattern | Weak answer pattern |
|---|---|---|
| Who owns this business capability? | A named domain service owns business truth; the experience layer composes it; the UI renders it; shared platform code governs standards. | The screen owns it because users see it there. |
| How do you know this capability is implemented? | Executable code, tests, contracts, runtime evidence, documentation and CI proof agree with each other. | It is mentioned in a README. |
| What happens if an upstream service is unavailable? | The caller maps the failure into a truthful degraded, unavailable or blocked state, logs safely, emits bounded metrics and preserves correlation. | The request fails. |
| How do you prevent duplicate writes? | Write operations use an idempotency key, request hash, stored result or status, retention and conflict behavior for mismatched duplicate keys. | The frontend disables the button. |
| What makes an API enterprise-grade? | Explicit models, OpenAPI, safe errors, idempotency, pagination, caller context, versioning, supportability states, contract tests and observability. | It returns JSON quickly. |
| How should a team promote a new CI gate? | Measure baseline, make output actionable, prove determinism, place it in the right lane, document exceptions and update scorecards. | Add another required check because quality is important. |
| What does production readiness require? | Startup proof, health/readiness, config, secrets, tests, runtime evidence, rollback, runbook, ownership and accepted residual risk. | The app works locally. |

## Practitioner Depth Topics

A senior practitioner should be able to reason about:

1. service boundaries,
2. clean backend structure,
3. experience API responsibilities,
4. OpenAPI governance,
5. idempotency,
6. pagination,
7. async execution,
8. data lineage,
9. data-product trust posture,
10. supportability states,
11. observability,
12. CI/CD lanes,
13. security and secrets,
14. production readiness,
15. rollback,
16. runbooks,
17. testing strategy,
18. release evidence,
19. documentation truth,
20. operational ownership.

## Personal Review Exercise

Pick one service, API or UI surface and answer:

1. What truth does it own?
2. What truth does it consume?
3. Which public contracts does it expose?
4. Which tests prove current behavior?
5. Which failure states are explicit?
6. Which logs, metrics and traces would help support it?
7. Which security controls protect it?
8. Which CI gates should block a risky change?
9. Which runbook would an operator use?
10. What is the smallest improvement that would raise confidence?

## Operating Principle

Strong practitioners do not only ask whether code works. They ask whether the capability is correctly owned, tested, documented, secured, observable, releasable and supportable.

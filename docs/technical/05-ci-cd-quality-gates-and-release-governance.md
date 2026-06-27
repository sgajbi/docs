# 05 - CI/CD, Quality Gates And Release Governance

High-quality CI is an engineering control system. It should catch real regressions early, produce useful evidence, and make it hard to ship undocumented or unsupported behavior.

## Repository-Native Command Policy

Every service should expose local commands for:

1. install,
2. lint,
3. typecheck,
4. unit tests,
5. integration tests,
6. E2E tests where applicable,
7. coverage,
8. security audit,
9. OpenAPI or contract gates,
10. Docker build or runtime validation.

CI should call these commands instead of re-implementing logic in workflow YAML.

Example:

```text
make lint
make typecheck
make test-unit
make test-integration
make openapi-gate
make security-audit
make docker-build
make ci
```

## Lane Model

| Lane | Purpose | Typical checks |
|---|---|---|
| Feature lane | Fast branch feedback. | install, lint, typecheck, fast unit tests, fast contract checks. |
| PR merge gate | Prevent unsafe merge. | integration, coverage, security, contract governance, migration smoke, Docker build. |
| Main releasability | Keep main deployable. | PR-grade rerun, release artifacts, retained evidence. |
| Platform E2E | Prove cross-service runtime. | stack bring-up, readiness, seed data, gateway/upstream comparisons, browser smoke. |

## Good Gate Design

A gate should be blocking only when:

1. the baseline is measured,
2. the failure has real engineering consequence,
3. the command is deterministic,
4. false positives are understood,
5. the gate is fast enough for its lane,
6. failures point to exact remediation,
7. exceptions are explicit and time-bounded.

Do not block on subjective style preferences, flaky checks or noisy inventories without policy.

## Useful Quality Gates

| Gate | Prevents |
|---|---|
| Architecture boundary gate | Domain logic leaking into routers, gateways or UI adapters. |
| Router thinness gate | Large controller functions and unreviewable HTTP handlers. |
| Duplicate implementation gate | Copy-paste service logic and repeated mappings. |
| OpenAPI gate | Contract drift and undocumented endpoints. |
| API vocabulary gate | Inconsistent domain vocabulary. |
| No-alias gate | Accidental compatibility aliases. |
| Monetary float guard | Unsafe floating-point money calculations. |
| Security audit | Known dependency and source-code security findings. |
| Observability contract gate | Unsafe logs, metrics, labels or missing health signals. |
| Documentation truth gate | Docs claiming support not backed by implementation evidence. |

## Release Evidence

Release evidence should answer:

1. what changed,
2. which tests ran,
3. which contracts changed,
4. which gates passed,
5. what artifacts were produced,
6. whether Docker/runtime behavior was validated,
7. whether security posture changed,
8. what residual risks remain.

Machine-readable artifacts are better than prose alone.

## Branch And PR Hygiene

Strong branch hygiene:

1. start from clean main,
2. keep commits small and meaningful,
3. avoid mixing unrelated refactors,
4. update docs with contract changes,
5. push regularly,
6. inspect CI failures,
7. fix forward,
8. merge only when required gates are green,
9. keep main releasable.

## Merge Governance

For a serious engineering platform:

1. direct push to main should be blocked,
2. pull requests should be required,
3. required checks should be enforced,
4. merge strategy should preserve useful history,
5. auto-merge must not bypass checks,
6. stale branches with durable docs or contract truth should be reconciled.

## CI Anti-Patterns

Avoid:

1. green CI that only runs superficial smoke tests,
2. workflow YAML duplicating local command logic,
3. `continue-on-error` for critical checks,
4. broad allowlists without owner and expiry,
5. generated report artifacts rewritten by normal blocking gates,
6. expensive checks hidden in fast lanes,
7. PR summaries that overstate evidence.

## Engineering Lead Review Questions

1. Can a developer run the same gate locally?
2. Is the failure actionable?
3. Is this check in the correct lane?
4. Does it prevent a real regression?
5. Does the PR evidence tell the truth?
6. Is main still releasable after merge?

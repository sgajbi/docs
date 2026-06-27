# Filled Prompt Examples

These examples show how to use the placeholder-based prompts. They are examples only; update dates, RFC IDs, branch names, and paths before use.

## Example 1: RFC Planning For Lotus Risk

Source prompt: [`copy-paste-workflows.md`](copy-paste-workflows.md#rfc-planning-only)

```text
You are working on `lotus-risk` at `C:\Users\Sandeep\projects\lotus-risk`.

Goal:
Draft or tighten `RFC-0047` before implementation. Do not write code yet.

Use Lotus context in AGENTS order, then read:
1. `C:\Users\Sandeep\projects\lotus-risk\REPOSITORY-ENGINEERING-CONTEXT.md`
2. `docs/rfcs/RFC-0047-risk-review.md`
3. `C:\Users\Sandeep\Downloads\risk-review-reference-pack` if present
4. the relevant Lotus standard, skill, or playbook for this work

First perform an implementation-readiness review:
1. current implementation state,
2. missing API contracts,
3. missing domain models,
4. source-authority and data-mesh rules,
5. upstream and downstream integration needs,
6. OpenAPI and supported-features expectations,
7. observability, audit, security, and supportability gaps,
8. test and evidence gaps,
9. documentation, wiki, context, and skill implications.

Then produce an implementation-ready RFC with:
1. business goal,
2. scope and non-scope,
3. architecture direction,
4. ownership boundaries,
5. slice-by-slice plan,
6. acceptance criteria per slice,
7. evidence and validation per slice,
8. risks and dependencies,
9. second-last hardening and review slice,
10. final documentation/context/wiki/supported-features/branch-hygiene slice,
11. deliberate skills/guidance review,
12. post-completion LinkedIn draft slice only if requested.

Use Lotus private-banking language. Do not leave generic source documentation unconverted.
```

## Example 2: Backend Refactor For Lotus Gateway

Source prompt: [`copy-paste-workflows.md`](copy-paste-workflows.md#backend-refactor)

```text
You are working on backend app `lotus-gateway` at `C:\Users\Sandeep\projects\lotus-gateway`.

Goal:
Refactor `DPM command center Gateway routes and clients` so the app is cleaner, more modular, more secure, more observable, more performant, and closer to the Lotus bank-buyable engineering standard.

Read AGENTS context order, repo engineering context, and relevant Lotus backend delivery/refactoring standards before editing.

Focus on:
1. dead code removal,
2. clearer repository structure,
3. domain-correct models and naming,
4. reduced duplication,
5. API quality and OpenAPI completeness,
6. proper error handling and validation,
7. business logic outside routers/controllers,
8. correlation, idempotency, lineage, audit, metrics, logs, tracing, health, and readiness,
9. performance and database access,
10. dependency hygiene and security,
11. meaningful unit, integration, contract, and e2e tests,
12. implementation-backed README/docs/wiki/supported-features updates.

Do not make cosmetic-only changes. Preserve behavior unless a change is intentional, tested, documented, and downstream-safe.
```

## Example 3: Downstream Realization For Gateway And Workbench

Source prompt: [`copy-paste-workflows.md`](copy-paste-workflows.md#gateway-and-workbench-downstream-realization)

```text
You are the Lotus Gateway + Workbench downstream realization agent.

Target apps:
`lotus-gateway and lotus-workbench`

Scope:
`Manage campaign-definition retire/supersede downstream realization`

Do not touch source-owner apps unless you discover source-owner drift and get explicit approval.

Start with Phase 0 in each target repo:
1. read AGENTS and repo engineering context,
2. run `git fetch origin --prune`,
3. run `git status --short --branch`,
4. run `git branch -r --no-merged origin/main`,
5. inspect open PRs with `gh pr list --state open --json number,headRefName,title,url,statusCheckRollup --limit 100`.

Gateway work:
1. verify current route/client behavior,
2. add typed client methods only where the source app already supports the capability,
3. add Gateway routes that preserve source payloads, supportability, lineage, reason codes, content hashes, and boundaries,
4. do not calculate source-owned business state in Gateway,
5. add unit, integration, contract, OpenAPI, vocabulary, docs, and wiki coverage as needed.

Workbench work:
1. add Gateway-only API helpers,
2. add focused UI controls or panels for the supported capability,
3. fail closed when prerequisites are missing or Gateway reports unsupported/blocked posture,
4. refresh data after successful commands,
5. render returned evidence clearly,
6. do not add unsupported workflows, order routing, OMS, execution, fills, settlement, client contact, or external workflow claims unless explicitly in scope and source-backed.

Validation:
1. Gateway lint/type/OpenAPI/vocabulary/no-alias/focused tests,
2. Workbench lint/type/focused component or integration tests/build,
3. browser or canonical validation if visible product surface changed,
4. PR checks green before merge.
```

## Example 4: Live Screenshot Proof

Source prompt: [`copy-paste-workflows.md`](copy-paste-workflows.md#live-runtime-proof-and-screenshots)

```text
Bring up the governed canonical Lotus runtime for `lotus-gateway, lotus-workbench, lotus-risk, lotus-performance, lotus-core, lotus-advise, and lotus-manage` with deterministic seeded data.

Evidence output:
`C:\Users\Sandeep\AppData\Local\Temp\lotus-risk-module-shots`

Use the documented canonical runtime path. For front-office proof, use `PB_SG_GLOBAL_BAL_001` unless the task explicitly requires another dataset.

Before screenshots:
1. validate APIs,
2. validate calculations,
3. validate Workbench panels,
4. confirm data is populated,
5. confirm Gateway and backend responses are implementation-backed,
6. confirm unsupported or degraded states are labelled honestly.

If validation fails, capture only diagnostic screenshots with a `diagnostic-` prefix, fix the issue, rerun validation, and then capture demo-ready screenshots.

Screenshots must clearly show the actual product surfaces and useful data, not empty placeholders.
```

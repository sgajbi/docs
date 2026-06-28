# Copy-Paste Workflow Prompts

These are cleaned prompts meant for direct reuse. Replace placeholders from [`placeholder-guide.md`](placeholder-guide.md), then paste into Codex.

## RFC Planning Only

```text
You are working on `<APP_NAME>` at `<APP_PATH>`.

Goal:
Draft or tighten `<RFC_ID>` before implementation. Do not write code yet.

Use Lotus context in AGENTS order, then read:
1. `<APP_PATH>\REPOSITORY-ENGINEERING-CONTEXT.md`
2. `<RFC_PATH>`
3. `<SOURCE_DOCS_PATH>` if present
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

## RFC Implementation

```text
You are working on `<APP_NAME>` at `<APP_PATH>`.

Goal:
Implement `<RFC_ID>` from `<RFC_PATH>` on branch `<FEATURE_BRANCH>` to a gold standard.

Start with Lotus Phase 0:
1. read context in AGENTS order,
2. read `<APP_PATH>\REPOSITORY-ENGINEERING-CONTEXT.md`,
3. read the RFC and any referenced standards/playbooks,
4. run `git fetch origin --prune`,
5. run `git status --short --branch`,
6. run `git branch -r --no-merged origin/main`,
7. inspect open PRs with `gh pr list --state open --json number,headRefName,title,url,statusCheckRollup --limit 100`.

Implementation rules:
1. work slice by slice,
2. do not move to the next slice until the current slice is implemented, validated, reviewed, and documented,
3. preserve source ownership boundaries,
4. update downstream consumers when contracts change,
5. improve structure, readability, modularity, reliability, and tests where it materially helps,
6. remove dead code, stale aliases, duplicate logic, and misleading claims in scope,
7. add meaningful tests, not superficial coverage,
8. update README/docs/wiki/context/supported-features when truth changes,
9. keep normal small meaningful commits,
10. use GitHub CI asynchronously and fix forward promptly.

Definition of done:
1. implemented slices merged to main,
2. local repo-native gates pass,
3. GitHub required checks pass,
4. wiki published if wiki source changed,
5. durable truth is not stranded on branches,
6. local main and remote main are aligned,
7. worktree is clean.
```

## Backend Refactor

```text
You are working on backend app `<APP_NAME>` at `<APP_PATH>`.

Goal:
Refactor `<SCOPE>` so the app is cleaner, more modular, more secure, more observable, more performant, and closer to the Lotus bank-buyable engineering standard.

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

## Enterprise Backend Refactoring Program

```text
You are working on backend app `<APP_NAME>` at `<APP_PATH>`.

Mission:
Refactor `<APP_NAME>` into a modular, reusable, maintainable, performant, secure, observable, enterprise-grade, production-ready, bank-buyable backend application.

This is a refactoring program, not cosmetic cleanup. Preserve behavior unless a change is intentional, tested, documented, downstream-safe, and explicitly called out.

Follow `<REFACTOR_PLAYBOOK_PATH>` fully for architecture, API governance, quality gates, CI measurement, testing, security, observability, documentation, commit strategy, PR expectations, and definition of done.

Phase 0 - governed ramp-up:
1. read AGENTS context order,
2. read `<APP_PATH>\REPOSITORY-ENGINEERING-CONTEXT.md`,
3. read `<REFACTOR_PLAYBOOK_PATH>`,
4. load relevant backend delivery, CI governance, PR pre-merge, codebase-review, and documentation/wiki skills,
5. run `git fetch origin --prune`,
6. run `git status --short --branch`,
7. run `git branch -r --no-merged origin/main`,
8. inspect open PRs and current CI posture.

Phase 1 - baseline before refactor:
1. create the baseline quality report described in the playbook,
2. establish the CI/reporting foundation needed to measure progress,
3. capture current architecture, OpenAPI, tests, security, observability, documentation, and maintainability posture,
4. define the before/after scorecard and commit it before broad code movement.

Phase 2 - refactor execution:
1. work on `<FEATURE_BRANCH>`,
2. make small, meaningful, truthful commits,
3. target roughly `<TARGET_COMMIT_COUNT>` well-scoped commits for a large refactor,
4. preserve normal history because the PR is intended for `<MERGE_STRATEGY>`,
5. keep the branch current and CI under control,
6. avoid broad unreviewable commits,
7. do not start the next major slice until the current slice has focused tests, docs/context decisions, and repo-native validation evidence.

Architecture rules:
1. distinguish design modularity from runtime modularity,
2. first create clear internal bounded modules with stable interfaces,
3. keep domain logic out of routers, controllers, CLI glue, infrastructure clients, persistence plumbing, and UI/BFF composition layers,
4. introduce separately scalable process or service boundaries only when workload, failure isolation, ownership, deployment, security, or operability evidence justifies it,
5. preserve source ownership and upstream/downstream contracts,
6. recommend cross-repository ownership moves when evidence supports them, but implement cross-repo moves only when they are explicitly in scope or separately approved.

For every refactor slice:
1. reread the mission and keep the work aligned,
2. state the slice objective and expected measurable improvement,
3. validate private-banking domain correctness and industry-standard vocabulary,
4. validate edge cases, degraded states, supportability, and failure modes,
5. validate API contract truth, OpenAPI quality, examples, errors, compatibility, and downstream consumers,
6. validate production operability: logs, metrics, traces, health, readiness, correlation, idempotency, lineage, audit, runbooks, and operator diagnostics,
7. remove dead code, duplicate logic, stale aliases, weak abstractions, misleading docs, and unsupported claims encountered in scope,
8. add meaningful unit, integration, contract, API, regression, security, observability, and performance tests where appropriate,
9. run repo-native focused checks before committing,
10. update README, docs, wiki source, context, supported-features, scorecards, contracts, and API inventories when truth changes,
11. record a deliberate no-change decision when docs/wiki/context/skills do not need updates.

Regularly review whether skills, guidance, automation, scaffolding, documentation, prompts, or agent context should be improved to support better future work, faster ramp-up, and stronger agent effectiveness.

Non-goals unless separately justified:
1. large rewrites without measurable improvement,
2. new service boundaries created only for aesthetic modularity,
3. breaking API changes without compatibility plan and downstream migration,
4. client-ready, production-ready, bank-buyable, or methodology-complete claims without evidence,
5. documentation-only progress presented as implementation progress.

Final PR requirements:
1. before/after scorecard for code health, architecture, OpenAPI quality, tests, security, observability, performance, and documentation,
2. polished audience-aware documentation for business, engineering, sales, marketing, operations, QA, architecture, and support where relevant,
3. clean branch and PR hygiene,
4. required GitHub checks green or resolved according to repo policy,
5. wiki published after merge when wiki source changed,
6. local `main` synced and clean.
```

## Workbench UI Refactor

```text
You are working on `lotus-workbench` at `<APP_PATH>`.

Goal:
Refactor or audit `<SCOPE>` so the UI is coherent, modular, fast, dense, useful, and enterprise-grade.

Use Lotus frontend delivery governance and Workbench repo context.

Rules:
1. every UI feature must be backed by supported Gateway/backend capability,
2. do not invent backend support in the browser,
3. keep API integration behind focused client/helper modules,
4. reduce technical leakage in user-facing text,
5. use private-banking front-office language,
6. improve shared patterns for typography, spacing, tables, badges, metrics, statuses, filters, actions, and empty/error states,
7. break large screens into focused panels, hooks, and reusable components where it improves maintainability,
8. add meaningful component/integration/browser tests,
9. use populated canonical runtime proof before demo-ready screenshots.

Think from the user journey:
1. what must the user see,
2. what decision must they make,
3. what action is allowed,
4. what is blocked and why,
5. what evidence supports the screen.
```

## Endpoint Certification

```text
You are working on `<APP_NAME>` at `<APP_PATH>`.

Goal:
Certify `<ENDPOINT_OR_FAMILY>` end to end.

Use the relevant Lotus backend delivery and endpoint certification workflow.

For each endpoint:
1. verify source authority,
2. test all supported options, output fields, branches, and error states,
3. validate business figures, state transitions, lineage, reason codes, supportability, and content hashes where applicable,
4. inspect Swagger/OpenAPI for grouping, what/when/how guidance, request examples, response examples, field descriptions, types, and errors,
5. review unit, integration, contract, e2e, and downstream coverage,
6. validate Gateway and Workbench use where relevant,
7. identify duplicate or stale endpoints,
8. create issues for downstream fixes that are out of scope,
9. update docs/wiki/supported-features only for implementation-backed truth,
10. run repo-native checks and summarize evidence.

Do not certify an endpoint based only on happy-path status codes.
```

## Gateway And Workbench Downstream Realization

```text
You are the Lotus Gateway + Workbench downstream realization agent.

Target apps:
`<TARGET_APPS>`

Scope:
`<SCOPE>`

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

## Live Runtime Proof And Screenshots

```text
Bring up the governed canonical Lotus runtime for `<TARGET_APPS>` with deterministic seeded data.

Evidence output:
`<EVIDENCE_PATH>`

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

## PR Closure

```text
Run the full PR closure loop for `<APP_NAME>` at `<APP_PATH>`.

1. Confirm the feature branch is current and scoped.
2. Run focused local repo-native checks.
3. Push commits.
4. Open or update the PR.
5. Include summary, tests, evidence, risks, docs/wiki/context changes, and unsupported scope.
6. Monitor GitHub checks asynchronously.
7. Inspect failures and fix forward.
8. Merge only after required checks are green.
9. Publish wiki after merge if wiki source changed.
10. Delete local and remote feature branches when safe.
11. Sync local main.
12. Verify clean status.
13. Verify no unclassified unmerged durable-truth branches remain.

Done means merged to main, validated, published where applicable, and clean.
```

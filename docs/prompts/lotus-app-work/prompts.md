# Canonical Lotus App Work Prompts

These prompts consolidate the useful parts of the pasted prompt dump into reusable, lower-duplication patterns.

Replace placeholders before use:

- `<repo>`
- `<repo-path>`
- `<rfc-id>`
- `<feature-branch>`
- `<docs-path>`
- `<evidence-path>`
- `<target-apps>`
- `<specific-scope>`

## 1. Phase 0 Ramp-Up

Use this when starting meaningful work in any Lotus repository.

```text
You are working in `<repo>` at `<repo-path>`.

Start with Lotus Phase 0:
1. Read `AGENTS.md`.
2. Read `lotus-platform/context/LOTUS-QUICKSTART-CONTEXT.md`.
3. Read `lotus-platform/context/LOTUS-ENGINEERING-CONTEXT.md`.
4. Read this repository's `REPOSITORY-ENGINEERING-CONTEXT.md`.
5. Read `lotus-platform/context/CONTEXT-REFERENCE-MAP.md`.
6. Load only the RFC, skill, playbook, or standard needed for this task.

Then:
1. Run `git fetch origin --prune`.
2. Run `git status --short --branch`.
3. Run `git branch -r --no-merged origin/main`.
4. Inspect open PRs with `gh pr list --state open --json number,headRefName,title,url,statusCheckRollup --limit 100`.
5. Summarize repo, branch, task intent, applicable standards, validation lane, and immediate risks before editing.

Use the smallest correct working set. Do not load broad context blindly.
```

## 2. Gold-Standard RFC Planning

Use this when implementation should not begin until an RFC is strong enough to execute.

```text
Do not start implementation yet.

Review `<docs-path>` and the current implementation of `<repo>` deeply. Draft or tighten RFC `<rfc-id>` so it is implementation-ready and can guide a high-quality agent without ambiguity.

The RFC must include:
1. problem statement and business goal,
2. current implementation assessment,
3. target architecture and ownership boundaries,
4. source-authority and data-mesh posture where relevant,
5. API and contract implications,
6. upstream and downstream integration requirements,
7. slice-by-slice implementation plan,
8. acceptance criteria per slice,
9. tests and evidence required per slice,
10. security, observability, CI, documentation, and wiki expectations,
11. explicit unsupported or deferred scope,
12. risks, dependencies, and rollback considerations.

The RFC must include a second-last hardening/review slice and a final closure slice covering documentation, context, wiki, supported-features, branch hygiene, and a deliberate skills/guidance review. If post-completion communication is requested, add a final LinkedIn draft slice that uses only implementation-backed outcomes.

Use Lotus vocabulary and private-banking language. Do not leave generic vendor-style documentation in place.
```

## 3. RFC Implementation Loop

Use this for executing an approved RFC.

```text
Implement RFC `<rfc-id>` in `<repo>` to a gold standard.

Work slice by slice. Do not move to the next slice until the current slice is implemented, validated, reviewed, documented, and in a solid state.

For each slice:
1. confirm scope and affected ownership boundaries,
2. implement the smallest correct change,
3. improve structure, readability, modularity, tests, and documentation where materially relevant,
4. remove dead code, duplication, stale aliases, or misleading claims encountered in scope,
5. add meaningful tests across the right test-pyramid layers,
6. update README, docs, wiki source, supported-features, contracts, context, and skills when truth changes,
7. run repo-native focused checks,
8. push small meaningful commits when requested,
9. monitor GitHub checks and fix forward.

Do not claim completion until implementation is merged to `main`, required checks are green, wiki is published when needed, durable truth is not stranded on unmerged branches, and local status is clean.
```

## 4. Backend Bank-Buyable Refactor

Use this for backend repositories such as `lotus-core`, `lotus-performance`, `lotus-risk`, `lotus-advise`, `lotus-manage`, `lotus-gateway`, `lotus-report`, `lotus-render`, `lotus-archive`, `lotus-ai`, or `lotus-idea`.

```text
Refactor `<repo>` into a modular, reusable, maintainable, performant, secure, observable, enterprise-grade, production-ready, bank-buyable backend application.

Read the Lotus context in AGENTS order, the target repo engineering context, and the relevant backend delivery/refactoring standards. If this is a large refactor, first create a baseline quality report and CI/reporting foundation.

Focus on:
1. removing dead code and unused paths,
2. breaking monolithic modules into clear domain, API, service, infrastructure, and test boundaries,
3. improving domain modeling and private-banking vocabulary,
4. reducing duplication across DTOs, services, mappers, validators, and tests,
5. improving API design, versioning, consistency, OpenAPI, examples, and error handling,
6. keeping business logic out of routers/controllers and infrastructure layers,
7. strengthening validation, idempotency, correlation, auditability, lineage, and supportability,
8. improving logging, metrics, tracing, health, readiness, and operator diagnostics,
9. optimizing latency, batching, pagination, caching, and database access,
10. hardening security, sensitive-data handling, configuration, and dependency hygiene,
11. improving meaningful unit, integration, contract, and end-to-end tests,
12. updating README, docs, wiki, supported-features, context, and scorecards when truth changes.

Preserve behavior unless a change is intentional, tested, documented, and downstream consumers are updated.
```

## 5. Workbench UI Refactor And Audit

Use this for `lotus-workbench` product-surface work.

```text
Refactor or audit `lotus-workbench` so the UI is modular, reusable, maintainable, performant, accessible, and enterprise-grade.

Start with the frontend delivery governance and relevant Workbench context. Every visible feature must be backed by supported Gateway/backend functionality.

Focus on:
1. removing dead code and unused components,
2. breaking large screens into domain-focused panels, hooks, and reusable components,
3. improving shared UI patterns for typography, spacing, tables, badges, metrics, status states, navigation, filters, and action controls,
4. reducing duplicate API, state, formatting, and supportability handling,
5. keeping Gateway integration abstracted away from UI components,
6. translating technical source/supportability posture into front-office decision language,
7. improving rendering performance and avoiding avoidable latency,
8. strengthening unit, component, integration, and browser validation,
9. updating documentation and wiki when product truth changes.

Think from the front-office user journey:
1. what does the user need to see,
2. what decision do they need to make,
3. what action is permitted,
4. what is blocked and why,
5. what evidence supports the view.

Use browser screenshots and canvas/pixel checks where visual quality matters. Do not call a surface demo-ready until populated canonical validation passes.
```

## 6. Endpoint Certification

Use this to certify one API endpoint or endpoint family.

```text
Certify `<endpoint-or-family>` in `<repo>` end to end using the relevant Lotus backend delivery and endpoint certification workflow.

For each endpoint:
1. verify current implementation, source authority, and downstream consumers,
2. test every supported option, branch, error case, and important output field,
3. validate figures, state transitions, supportability posture, lineage, and reason codes,
4. inspect OpenAPI/Swagger for what/when/how guidance, examples, every attribute description, request/response models, and errors,
5. check meaningful unit, integration, contract, and e2e coverage,
6. identify duplicate, stale, or non-strategic endpoints,
7. validate Gateway and Workbench consumption where relevant,
8. create GitHub issues for downstream fixes that are out of scope,
9. update docs/wiki/supported-features only for implementation-backed truth,
10. run repo-native certification and CI gates.

Do not certify superficial endpoint behavior. Certify the business contract and operational supportability.
```

## 7. Demo And Live Runtime Proof

Use this for populated stack validation or screenshot evidence.

```text
Bring up the governed canonical Lotus runtime for `<target-apps>` with deterministic seeded data.

Use the documented Lotus runtime path, not an improvised stack path. For Workbench/front-office proof, use the governed `lotus-workbench` canonical runtime and `PB_SG_GLOBAL_BAL_001` unless a different dataset is explicitly required.

Before capturing demo-ready screenshots:
1. run canonical API validation,
2. run calculation and panel validation,
3. confirm seeded data is populated,
4. verify Gateway and backend APIs return real implementation-backed data,
5. confirm unsupported or degraded states are labelled honestly.

Capture screenshots or evidence under `<evidence-path>`.

If validation fails, label any capture as diagnostic, fix the underlying issue, rerun validation, and only then capture demo-ready evidence.
```

## 8. PR Loop And Branch Hygiene

Use this when closing implementation work.

```text
Run the full PR loop for `<repo>`.

1. Confirm local feature branch is current and scoped.
2. Run focused local repo-native checks.
3. Push the feature branch.
4. Open or update the PR with a clear summary, tests, evidence, risks, and docs/wiki/context changes.
5. Monitor GitHub checks asynchronously.
6. Inspect failures and fix forward.
7. Keep commits small, meaningful, and non-squashed when normal history is required.
8. Merge only after required checks are green and review expectations are satisfied.
9. Publish wiki after merge if wiki source changed.
10. Delete local and remote feature branches when safe.
11. Sync local `main`.
12. Verify clean status and no unclassified unmerged durable-truth branches.

Definition of done is merged to `main`, validated, published where applicable, and clean.
```

## 9. Durable Truth Reconciliation

Use this before and after RFC/docs/wiki/context/contract/supported-features/CI workflow work.

```text
Before starting or closing durable-truth work in `<repo>`, verify that required truth is not stranded on unmerged branches.

Run:
1. `git fetch origin --prune`
2. `git branch -r --no-merged origin/main`

Inspect unmerged branches that touch:
1. `docs/rfcs/`
2. `wiki/`
3. `README.md`
4. `REPOSITORY-ENGINEERING-CONTEXT.md`
5. `AGENTS.md`
6. `contracts/`
7. `platform-contracts/`
8. `context/`
9. `docs/standards/`
10. `.github/workflows/`
11. migrations
12. OpenAPI snapshots
13. API vocabulary inventories
14. supported-features material

Classify each relevant branch as `must-merge`, `cherry-pick`, `superseded`, `delete`, or `active`.

Do not claim closure while unique durable truth exists only on an unmerged branch.
```

## 10. Documentation, Context, Wiki, And Skill Sync

Use this when implementation changes durable Lotus truth.

```text
Keep Lotus operating guidance current and synchronized.

When workflow, governance, repository responsibility, validation command, delivery pattern, supported feature, or repeatable lesson changes, update the right durable sources:
1. repo `AGENTS.md` when the operating contract changes,
2. `lotus-platform/context/*` when platform truth changes,
3. target `REPOSITORY-ENGINEERING-CONTEXT.md` when repo truth changes,
4. relevant Codex skills under platform-owned skill source when agent behavior should change,
5. repo-local `wiki/` when operator, developer, business, or product truth changes,
6. supported-features material when implementation-backed capabilities change.

Use sync and validation automation rather than hand-maintaining deployed local copies when automation exists.

If no context, wiki, or skill update is needed, record that as a deliberate no-change decision.
```

## 11. GitHub Issue For Integration Gap

Use this when a needed change belongs in another Lotus app.

```text
Create a GitHub issue in `<affected-repo>` for an upstream or downstream integration gap discovered while working in `<current-repo>`.

The issue must include:
1. problem observed,
2. affected upstream or downstream app,
3. impacted API, contract, workflow, or integration,
4. why it matters,
5. expected behavior,
6. suggested fix or direction,
7. evidence such as logs, screenshots, API responses, file paths, or tests,
8. whether the current app has a workaround, fail-closed state, or blocked capability.

Write it so another agent can act without needing extra clarification.
```

## 12. Post-Completion Retrospective

Use this after an RFC or major slice closes.

```text
Review what was learned from implementing `<rfc-id>` or `<specific-scope>`.

Cover:
1. what improved,
2. what is still not gold standard,
3. where dead code, duplication, or poor implementation choices remain,
4. what should be improved in skills, guidance, documentation, context, or automation,
5. which improvements should be immediate follow-up work versus backlog,
6. whether any docs/wiki/context/supported-features claims are too strong or stale.

No implementation yet. Return a clear assessment and recommended next prompt.
```

## 13. Executive Clean Handoff

Use this before starting a new chat or handing work to another agent.

```text
Prepare an executive clean handoff for `<repo>` / `<specific-scope>`.

Before writing the handoff:
1. verify no leftover local work,
2. verify no unmerged feature branches with useful durable truth,
3. verify CI is green or explicitly document failures,
4. verify README/docs/wiki/context/supported-features are updated and published where needed,
5. verify local main and remote main are aligned.

The handoff must include:
1. objective,
2. current repo and branch status,
3. completed work,
4. remaining WIP items,
5. exact files, PRs, commits, RFC IDs, issue IDs, and evidence paths,
6. validation already run,
7. known risks,
8. next recommended actions,
9. rules the next agent must follow.
```

## 14. LinkedIn Post-Completion Draft

Use this after implementation is actually complete.

```text
Draft a LinkedIn post based only on what was actually implemented and proven in `<specific-scope>`.

Use the `lotus-linkedin-thought-leadership` workflow. Read the content ledger, themes, voice/style guide, and recent drafts before drafting.

The post must be:
1. employer-safe,
2. non-confidential,
3. non-promotional,
4. grounded in implementation-backed outcomes,
5. written in a practical, calm, senior, domain-aware voice,
6. free of claims that any bank or employer uses Lotus,
7. free of unsupported product, regulatory, investment, or AI claims.

Draft under the governed LinkedIn drafts folder and update the content ledger if the workflow requires it.
```

## 15. Bank-Buyable Product Assessment

Use this when asking whether an app or app pair is bank-buyable or a crown-jewel candidate.

```text
Assess `<target-apps>` honestly against Lotus bank-buyable expectations.

Cover:
1. what is strong now,
2. what is implementation-backed,
3. what is still aspirational or unsupported,
4. architecture and ownership boundaries,
5. API quality and downstream realization,
6. UI/user journey quality where relevant,
7. test, CI, security, observability, and supportability posture,
8. documentation and wiki usefulness,
9. evidence gaps,
10. highest-value next improvements.

Use careful language. Do not call an app bank-buyable, data-mesh certified, production-ready, or a crown jewel unless the evidence supports that claim.
```

## 16. Enterprise Backend Refactoring Program

Use this for a large, multi-slice backend refactor where success must be measurable, reviewable, and suitable for a non-squash PR history. It is stronger than the compact backend refactor prompt because it requires baseline evidence, phase gates, scorecards, documentation/context upkeep, and a final before/after proof package.

```text
You are working on `<APP_NAME>` at `<APP_PATH>`.

Mission:
Refactor `<APP_NAME>` into a modular, reusable, maintainable, performant, secure, observable, enterprise-grade, production-ready, bank-buyable backend application.

This is a refactoring program, not cosmetic cleanup. Preserve behavior unless a change is intentional, tested, documented, downstream-safe, and explicitly called out.

Primary instruction source:
Follow `<REFACTOR_PLAYBOOK_PATH>` fully for architecture, API governance, quality gates, CI measurement, testing, security, observability, documentation, commit strategy, PR expectations, and definition of done.

Phase 0 - governed ramp-up:
1. read `AGENTS.md`,
2. read `lotus-platform/context/LOTUS-QUICKSTART-CONTEXT.md`,
3. read `lotus-platform/context/LOTUS-ENGINEERING-CONTEXT.md`,
4. read `<APP_PATH>/REPOSITORY-ENGINEERING-CONTEXT.md`,
5. read `lotus-platform/context/CONTEXT-REFERENCE-MAP.md`,
6. read `lotus-platform/context/PROCEDURAL-MEMORY-INDEX.md` if execution process is material,
7. read `<REFACTOR_PLAYBOOK_PATH>`,
8. load relevant backend delivery, CI governance, PR pre-merge, codebase-review, documentation/wiki, and endpoint-certification skills when applicable,
9. run `git fetch origin --prune`,
10. run `git status --short --branch`,
11. run `git branch -r --no-merged origin/main`,
12. inspect open PRs and current CI posture.

Phase 1 - baseline before refactor:
1. create the baseline quality report required by the playbook,
2. establish the CI/reporting foundation needed to measure improvement,
3. capture current posture for architecture, module boundaries, OpenAPI, tests, security, observability, performance, documentation, wiki, context, and maintainability,
4. identify high-risk modules, dead code, duplicate logic, weak contracts, missing tests, security gaps, observability gaps, documentation drift, and domain-language problems,
5. define the before/after scorecard and commit it before broad code movement.

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
1. reread this mission before editing,
2. state the slice objective, affected boundaries, and expected measurable improvement,
3. validate private-banking domain correctness and industry-standard vocabulary,
4. validate edge cases, degraded states, supportability, and failure modes,
5. validate API contract truth, OpenAPI quality, request/response examples, errors, compatibility, and downstream consumers,
6. validate production operability: logs, metrics, traces, health, readiness, correlation, idempotency, lineage, audit, runbooks, and operator diagnostics,
7. improve readability, maintainability, repository organization, security, performance, and tests,
8. remove dead code, duplicate logic, stale aliases, weak abstractions, misleading docs, and unsupported claims encountered in scope,
9. add meaningful unit, integration, contract, API, regression, security, observability, and performance tests where appropriate,
10. run repo-native focused checks before committing,
11. update README, docs, wiki source, context, supported-features, scorecards, contracts, and API inventories when truth changes,
12. record a deliberate no-change decision when docs/wiki/context/skills do not need updates.

Quality bar:
1. prevent AI slop, superficial fixes, weak implementation, and unproven changes,
2. prefer source-backed and methodology-backed truth over optimistic claims,
3. use domain-correct naming for APIs, attributes, functions, variables, models, files, and user-facing vocabulary,
4. make the implementation feel designed by experts in private banking, enterprise software, and production-grade engineering,
5. improve skills, prompts, context, automation, scaffolding, and documentation when the work reveals a repeatable improvement.

Non-goals unless separately justified:
1. large rewrites without measurable improvement,
2. new service boundaries created only for aesthetic modularity,
3. breaking API changes without compatibility plan and downstream migration,
4. client-ready, production-ready, bank-buyable, or methodology-complete claims without evidence,
5. documentation-only progress presented as implementation progress.

Final PR requirements:
1. before/after scorecard proving measurable improvement in code health, architecture, OpenAPI quality, tests, security, observability, performance, and documentation,
2. clear PR summary, risk assessment, validation evidence, docs/wiki/context changes, unsupported scope, and migration or rollback notes where relevant,
3. evidence that private-banking domain correctness, edge cases, API truth, and production operability were validated,
4. clean branch hygiene and no stranded durable truth,
5. required GitHub checks green or resolved according to repo policy,
6. wiki published after merge when wiki source changed,
7. local `main` synced and clean after merge.
```

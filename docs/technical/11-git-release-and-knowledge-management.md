# 11 - Git, Release Hygiene And Engineering Knowledge Management

Engineering scale depends on disciplined source control and durable knowledge. The goal is not bureaucracy; the goal is that another engineer can understand what changed, why it changed, how it was proven, and what remains risky.

## Git Working Model

Use Git as the durable history of engineering decisions.

Good habits:

1. start from an up-to-date main branch,
2. create a focused branch for one slice,
3. keep commits small and truthful,
4. avoid unrelated formatting churn,
5. do not rewrite other people's work without explicit agreement,
6. push regularly,
7. keep main releasable.

## Commit Quality

A good commit:

1. changes one coherent thing,
2. has a clear message,
3. preserves buildability where practical,
4. includes tests or docs when behavior changes,
5. does not include secrets, local paths or generated junk,
6. can be reviewed without reading unrelated changes.

Weak commit messages:

```text
fix
updates
changes
misc
```

Better messages:

```text
Add holdings supportability contract
Enforce OpenAPI vocabulary gate
Split report rendering from report status lookup
Document retry and timeout policy
```

## Branch Hygiene

Branch hygiene matters when repositories carry durable truth in docs, contracts, migrations, runbooks or CI workflows.

Before claiming closure:

1. confirm your branch includes required main changes,
2. check whether unmerged branches contain newer durable truth,
3. classify branches that touch governance paths,
4. merge, cherry-pick or supersede unique durable truth,
5. avoid leaving important docs or contracts stranded.

Governance paths include:

1. README,
2. architecture docs,
3. runbooks,
4. API contracts,
5. event schemas,
6. migrations,
7. CI workflows,
8. quality scorecards,
9. wiki source,
10. repository context.

## Pull Request Evidence

A strong PR explains:

1. what changed,
2. why it changed,
3. what behavior is preserved,
4. what behavior intentionally changed,
5. which commands ran,
6. which CI lanes passed,
7. what docs or contracts changed,
8. what risks remain,
9. what follow-up is needed.

PR evidence should be precise. "Tests passed" is weaker than naming the actual commands.

## Review Discipline

Review for:

1. behavior correctness,
2. contract quality,
3. ownership boundaries,
4. test value,
5. security posture,
6. observability,
7. error handling,
8. maintainability,
9. documentation truth,
10. deployment and rollback impact.

Avoid review comments that only express preference without explaining the engineering consequence.

## Release Hygiene

A release should answer:

1. what version or commit is being released,
2. what changed,
3. what gates passed,
4. what artifacts were produced,
5. what migrations or configuration changes are required,
6. what rollback path exists,
7. what monitoring should be watched,
8. who owns support.

## Documentation Layers

Use documentation layers intentionally.

| Layer | Purpose |
|---|---|
| README | Fast orientation, commands, scope and navigation. |
| Engineering context | Current repository role, boundaries, commands and constraints. |
| Architecture docs | Durable decisions, service boundaries and integration patterns. |
| API docs | Current request/response contracts and examples. |
| Runbooks | Operational diagnosis, mitigation and escalation. |
| Quality scorecards | Current control posture, evidence, gaps and next slices. |
| Wiki/source publication | Operator-facing or onboarding truth published from authored source. |
| Reference KB | General learning material and reusable patterns. |

## Documentation Truth Rule

Documentation must track implementation truth.

Do:

1. update docs when contracts change,
2. mark future work as future work,
3. keep examples executable where possible,
4. preserve unsupported boundaries,
5. remove stale claims,
6. link to evidence when a claim matters.

Do not:

1. claim production support from a prototype,
2. claim certification from catalog inclusion,
3. claim client readiness from screenshots alone,
4. leave old commands after CI changes,
5. publish private paths or local machine details,
6. duplicate generated artifacts by hand.

## Knowledge Management System

A strong engineering knowledge base should provide:

1. role-based navigation,
2. stable file names,
3. clear reading order,
4. reusable examples,
5. review checklists,
6. source-neutral explanations,
7. links from overview to deep references,
8. maintenance rules,
9. quality rubric,
10. backlog of future enrichment.

## Decision Records

Use decision records when a choice has future cost.

Capture:

1. context,
2. decision,
3. alternatives considered,
4. consequences,
5. evidence,
6. follow-up conditions.

Decision records are useful for service boundaries, storage choices, API versioning, deployment strategy, security exceptions, major refactors and cross-service contracts.

## Engineering Leadership Practices

As an engineering lead, build the habit of asking:

1. Is the current truth discoverable?
2. Can a new engineer run and validate this service?
3. Are claims backed by code and evidence?
4. Are repeated patterns documented once?
5. Are exceptions explicit and owned?
6. Are quality gates preventing real regression?
7. Is the next improvement slice obvious?

## Release And Knowledge Review Checklist

Before closing a meaningful slice:

1. worktree is clean,
2. commits are scoped,
3. tests and gates are named,
4. docs match changed behavior,
5. unsupported states remain explicit,
6. release or deployment evidence is retained where relevant,
7. follow-up backlog is recorded,
8. remote main contains the durable truth.


# Master Checklists and Execution Playbook

## Purpose

This file provides master checklists for applying the full knowledge-base series.

Use it as the final execution guide.

---

## Personal Study Checklist

- [ ] Read foundation guides.
- [ ] Read backend/API/testing/Git guides.
- [ ] Read CI/CD/runtime/security/observability guides.
- [ ] Read performance/data/documentation guides.
- [ ] Read leadership guide.
- [ ] Read AI/domain/reporting/productization/migration/support guides.
- [ ] Summarize each guide in your own words.
- [ ] Convert each guide into one practical action.

---

## Knowledge-To-Action Checklist

Use this checklist after reading any guide so the learning produces durable engineering value.

- [ ] Identify one concept that changes how a system should be designed, tested or operated.
- [ ] Find the matching evidence in a repository, architecture note, runbook, API contract or delivery workflow.
- [ ] Write one concise improvement action with owner, scope and expected evidence.
- [ ] Decide whether the action belongs in code, tests, CI, documentation, operations or team KT.
- [ ] Keep the action small enough to land as a reviewable change.
- [ ] Add a follow-up only when the remaining work is real, named and traceable.

---

## Repo Adoption Checklist

- [ ] README current.
- [ ] Architecture docs current.
- [ ] API contracts current.
- [ ] Data contracts current.
- [ ] Testing strategy documented.
- [ ] CI/CD gates documented.
- [ ] Runtime docs available.
- [ ] Security docs available.
- [ ] Observability/runbooks available.
- [ ] Supported-feature matrix available.
- [ ] ADR/RFC folder active.
- [ ] Evidence map available.

---

## Architecture Review Checklist

- [ ] Capability clear.
- [ ] Ownership clear.
- [ ] Boundaries clear.
- [ ] Contracts clear.
- [ ] Data lineage clear.
- [ ] Security clear.
- [ ] Failure modes clear.
- [ ] Testing strategy clear.
- [ ] Observability clear.
- [ ] Support model clear.
- [ ] Evidence expected.
- [ ] Decision recorded.

---

## PR Review Checklist

- [ ] Scope reviewable.
- [ ] Commits meaningful.
- [ ] Logic in correct layer.
- [ ] API/data contracts updated.
- [ ] Tests meaningful.
- [ ] Error paths covered.
- [ ] Security considered.
- [ ] Performance considered.
- [ ] Observability added.
- [ ] Docs updated.
- [ ] CI evidence included.
- [ ] Follow-up risks documented.

---

## Production Readiness Checklist

- [ ] Owner clear.
- [ ] Runtime validated.
- [ ] Health/readiness meaningful.
- [ ] Dashboards ready.
- [ ] Alerts actionable.
- [ ] Runbooks current.
- [ ] Security reviewed.
- [ ] Tests/certification passed.
- [ ] Rollback defined.
- [ ] Support trained.
- [ ] Known issues documented.
- [ ] Go/no-go evidence captured.

---

## KT Checklist

- [ ] Audience defined.
- [ ] Learning path selected.
- [ ] Guide sequence chosen.
- [ ] Hands-on exercise prepared.
- [ ] Repo example selected.
- [ ] Checklist shared.
- [ ] Teach-back required.
- [ ] Follow-up repo action created.
- [ ] Feedback collected.

---

## Productization Checklist

- [ ] Capability catalog.
- [ ] Supported-feature matrix.
- [ ] Reference architecture.
- [ ] Deployment guide.
- [ ] Integration guide.
- [ ] Security evidence pack.
- [ ] Data governance guide.
- [ ] Support model.
- [ ] Test/certification evidence.
- [ ] Implementation guide.
- [ ] Due-diligence responses.
- [ ] Exit plan.

---

## Migration Checklist

- [ ] Current state documented.
- [ ] Target state documented.
- [ ] Transition state documented.
- [ ] Inventory complete.
- [ ] Data mapping complete.
- [ ] Reconciliation defined.
- [ ] Parallel run planned.
- [ ] Cutover runbook ready.
- [ ] Rollback/fallback ready.
- [ ] Security/access migration ready.
- [ ] Report/archive continuity ready.
- [ ] Hypercare ready.
- [ ] Sign-off evidence ready.

---

## AI Adoption Checklist

- [ ] Use case classified.
- [ ] Risk tier assigned.
- [ ] Sources approved.
- [ ] Retrieval entitlement-aware.
- [ ] Prompt versioned.
- [ ] Output grounded.
- [ ] Human review defined.
- [ ] Tools authorized.
- [ ] Evaluations ready.
- [ ] Security tests ready.
- [ ] Observability ready.
- [ ] User guidance ready.

---

## Leadership Execution Checklist

- [ ] Service ownership map current.
- [ ] Architecture review rhythm active.
- [ ] Quality scorecards active.
- [ ] Production-readiness reviews active.
- [ ] Technical debt backlog prioritized.
- [ ] Incident learning loop active.
- [ ] KT curriculum active.
- [ ] Roadmap includes foundation work.
- [ ] Stakeholder communication clear.
- [ ] Evidence-driven decisions used.

---

## Evidence Artifacts By Work Type

Use this table to decide what durable evidence should exist after a learning, delivery, review or operational activity.

| Work type | Minimum evidence artifacts |
|---|---|
| Service design | Boundary note, API contract, dependency map, error model, data ownership note and test strategy. |
| API change | OpenAPI or contract update, compatibility note, consumer-impact review, examples, contract tests and deprecation plan when needed. |
| Data product | Data contract, lineage map, quality rules, freshness SLO, access policy, consumer guide and certification result. |
| CI/CD improvement | Gate purpose, owner, trigger, failure behavior, bypass policy, rollout plan and before/after failure evidence. |
| Runtime deployment | Configuration source, environment assumptions, health/readiness checks, rollback approach, resource expectations and validation command. |
| Security control | Threat or risk statement, control owner, enforcement point, test evidence, monitoring signal and exception process. |
| Observability change | Log fields, metric names, trace boundaries, dashboard panel, alert route, runbook link and sensitive-data review. |
| Production readiness | Owner, SLO/SLA expectations, runbooks, support model, incident route, DR/BCP posture, known risks and go/no-go evidence. |
| Migration | Inventory, mapping rules, reconciliation report, mock-cutover result, rollback decision tree, sign-off and hypercare plan. |
| AI-assisted capability | Source policy, entitlement model, prompt/version record, grounding checks, evaluation set, human review and tool authorization evidence. |
| KT or onboarding | Audience, reading path, exercises, teach-back questions, repo walkthrough, follow-up actions and feedback loop. |

## Reuse Pattern

When applying any checklist, keep the output compact:

1. capture the decision,
2. link the evidence,
3. identify the owner,
4. record the next action,
5. revisit the artifact after the next material change.

---

## Maturity Review Rubric

Use this rubric when reviewing a team, repository or platform area.

| Level | Signal | Evidence |
|---:|---|---|
| 1 | Knowledge is individual and inconsistent. | People can explain parts of the system, but durable docs, tests, runbooks or contracts are missing. |
| 2 | Core practices exist but are uneven. | README, API docs, tests and runbooks exist, but gaps are found during review or incidents. |
| 3 | Practices are repeatable. | PRs, releases, runbooks, dashboards and documentation follow a visible standard. |
| 4 | Practices are governed and measured. | Quality gates, scorecards, ownership, risk acceptance and review cadence are active. |
| 5 | Practices improve from evidence. | Incidents, reviews, migrations, buyer feedback and delivery metrics produce durable improvements. |

## Review Output Template

Keep review output short and action-oriented:

```text
Area reviewed:
Current maturity:
Evidence inspected:
Top strengths:
Top risks:
Decision or recommendation:
Owner:
Next action:
Follow-up date:
```

---

## Final Operating Principle

Every guide should ultimately become one of the following:

- a better design decision
- a better checklist
- a better test
- a better CI gate
- a better runbook
- a better document
- a better KT session
- a better product evidence artifact
- a better leadership narrative
- a better production outcome

That is how knowledge becomes engineering maturity.

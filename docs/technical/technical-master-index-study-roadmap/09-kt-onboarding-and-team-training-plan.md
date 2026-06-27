# KT, Onboarding, and Team Training Plan

## Purpose

This file provides a KT and onboarding model using the full pack series.

Use it to train developers, architects, QA, SRE, BA, product, and support teams.

---

## KT Principles

Good KT should be:

- role-based
- hands-on
- documented
- scenario-driven
- repeated
- evidence-backed
- followed by teach-back
- connected to real repos

---

## Suggested KT Tracks

| Track | Audience | Packs |
|---|---|---|
| Backend engineering | Developers | 2, 3, 9, 10, 11 |
| Platform engineering | DevOps/SRE | 5, 6, 7, 8, 19 |
| Data and reporting | Data/BA/reporting teams | 4, 15, 16, 18 |
| Architecture leadership | Leads/architects | 12, 13, 17, 18, 19 |
| AI governance | AI/product/security teams | 14, 8, 9, 17 |
| Productization | Founders/product/GTM | 17, 12, 8, 19 |

---

## KT Session Format

Each session:

1. objective
2. business context
3. architecture concept
4. engineering pattern
5. failure mode
6. example checklist
7. hands-on exercise
8. Q&A
9. teach-back
10. repo action item

---

## 10-Session KT Plan

| Session | Topic |
|---:|---|
| 1 | Enterprise engineering foundation and architecture map |
| 2 | Backend, API, and contract design |
| 3 | Testing, CI/CD, Git, and delivery evidence |
| 4 | Runtime, containers, Kubernetes, and deployment |
| 5 | Observability, SRE, and production support |
| 6 | Security, secrets, cyber resilience, and sensitive data |
| 7 | Data products, lineage, and trust |
| 8 | Wealth domain and reporting evidence |
| 9 | AI, RAG, copilots, and agentic governance |
| 10 | Migration, productization, and engineering leadership |

---

## Hands-On Exercises

- create a service ownership map
- write an ADR
- add one API contract checklist
- build one runbook
- define one golden test
- create one supported-feature matrix
- define one production-readiness checklist
- create one migration reconciliation checklist
- create one AI use-case risk classification

---

## Role-Based KT Outcomes

Each audience should leave KT with a concrete capability, not only awareness of the reading material.

| Audience | Should be able to explain | Should be able to produce |
|---|---|---|
| Backend developer | Service boundaries, use-case flow, contract ownership, error behavior and test placement. | Small service change with updated tests, API examples and release evidence. |
| Senior engineer | Tradeoffs across API, data, runtime, security, observability and performance. | Design note, risk list, test strategy and PR review comments that improve the implementation. |
| QA or quality engineer | Test pyramid, certification scope, fixture quality, contract testing and regression risk. | Meaningful test plan, gap list, defect evidence and release certification note. |
| SRE or platform engineer | Runtime model, probes, dashboards, alerts, runbooks, rollback and incident flow. | Operational readiness review, runbook update and monitoring gap list. |
| Business analyst or product technologist | Capability boundaries, data lineage, reporting evidence, supported features and acceptance criteria. | Domain scenario, acceptance example, source-owner note and reconciliation expectation. |
| Architect or engineering lead | Decision rights, standards, cross-system tradeoffs, maturity gaps and stakeholder narrative. | Architecture review note, decision record, maturity backlog and KT follow-up plan. |

## Teach-Back Question Bank

Use these questions after a KT session to verify practical understanding.

| Topic | Teach-back question |
|---|---|
| Architecture | What capability does this service own, and what should remain outside its boundary? |
| API contracts | Which consumers depend on this contract, and how would a breaking change be detected? |
| Data products | What is the source of truth, what quality rules apply, and how is lineage proven? |
| CI/CD | Which checks must run locally, which checks belong in CI, and what evidence proves release readiness? |
| Runtime | How does the service start, become ready, fail, recover and roll back? |
| Observability | Which logs, metrics, traces and alerts would help diagnose the top three failure modes? |
| Security | Where are the authorization, sensitive-data, secret and dependency controls enforced? |
| Testing | Which behavior needs unit, service, contract, integration, E2E or performance coverage? |
| Production support | Who owns the service during an incident, what runbook is used and how is learning captured? |
| Leadership | What standard or operating rhythm would prevent this issue from recurring across teams? |

## KT Evidence Pack

For important KT, capture a small evidence pack so the session becomes reusable.

| Evidence item | Why it matters |
|---|---|
| Audience and objective | Keeps the session scoped and role-appropriate. |
| Reading path | Shows which curated guides support the session. |
| Repo walkthrough notes | Connects general concepts to real implementation. |
| Exercise output | Proves the audience practiced the skill. |
| Teach-back answers | Shows whether judgment was built, not just attendance. |
| Follow-up backlog | Converts learning gaps into concrete repo or process improvements. |
| Owner and revisit date | Keeps KT from becoming stale. |

## Training Anti-Patterns

Avoid these patterns when using the knowledge base for onboarding or team training.

| Anti-pattern | Better approach |
|---|---|
| Long slide-only walkthrough | Use short concept explanation plus repo walkthrough and exercise. |
| Same content for every role | Tailor the reading path and output to the audience. |
| No teach-back | Ask participants to explain a decision, failure mode or tradeoff in their own words. |
| No repo action | End with one concrete documentation, test, runbook or evidence improvement. |
| One-time KT | Revisit after incidents, releases, architecture changes and recurring review comments. |

---

## Summary

KT is not a presentation series.

It is a structured system to build shared engineering judgment and local repo improvements.

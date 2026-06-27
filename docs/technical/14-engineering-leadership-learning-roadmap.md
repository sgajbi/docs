# 14 - Engineering Leadership Learning Roadmap

This roadmap turns the technical knowledge base into a practical self-learning and KT path for engineers, architects and engineering leads working on enterprise banking and wealth-platform systems.

The goal is not to become expert in every tool at once. The goal is to build judgement: knowing what matters, where evidence lives, how to review work, how to improve systems safely and how to lead technical delivery without relying on surface-level confidence.

## Learning Outcomes

After working through this roadmap, you should be able to:

1. explain service ownership and architecture boundaries,
2. read a backend repository and identify its main layers,
3. evaluate API contract quality,
4. understand data-product and trust-posture concepts,
5. run or reason about CI gates and release evidence,
6. review observability, SRE and runbook posture,
7. spot sensitive-data and security risks,
8. judge test quality and resilience posture,
9. understand container and deployment readiness,
10. review frontend supportability and browser proof,
11. communicate technical risk and next steps clearly.

## 30-Day Foundation

Focus: understand the platform shape and build reading fluency.

| Week | Read | Practice |
|---|---|---|
| 1 | [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md), [`12-technology-stack-and-engineering-patterns.md`](12-technology-stack-and-engineering-patterns.md) | Pick one service and map its role: domain service, gateway, UI, shared capability or platform governance. |
| 2 | [`02-backend-application-structure.md`](02-backend-application-structure.md), [`03-api-contract-governance.md`](03-api-contract-governance.md), [`backend-service-design/`](backend-service-design/README.md) and [`api-contract-engineering/`](api-contract-engineering/README.md) as deeper references | Trace one endpoint from route to service to repository/client to response model. |
| 3 | [`05-ci-cd-quality-gates-and-release-governance.md`](05-ci-cd-quality-gates-and-release-governance.md), [`08-testing-performance-and-resilience.md`](08-testing-performance-and-resilience.md) | Identify local quality commands and classify tests into unit, integration and E2E. |
| 4 | [`06-observability-sre-and-operational-support.md`](06-observability-sre-and-operational-support.md), [`07-security-configuration-and-sensitive-data.md`](07-security-configuration-and-sensitive-data.md) | Review one runbook or service README for health checks, logs, metrics, secrets and supportability. |

30-day checkpoint:

1. Can you explain what each major service type owns?
2. Can you find the local quality command?
3. Can you identify whether an endpoint is domain-owned or experience-composed?
4. Can you describe at least three supportability states?
5. Can you spot one documentation or test gap?

## 60-Day Applied Delivery

Focus: review and improve real slices with evidence.

| Week | Read | Practice |
|---|---|---|
| 5 | [`04-data-mesh-and-data-product-engineering.md`](04-data-mesh-and-data-product-engineering.md) | Review one data product or source contract and identify owner, schema, trust posture and consumers. |
| 6 | [`09-worked-examples-and-engineering-review-checklists.md`](09-worked-examples-and-engineering-review-checklists.md) | Use the PR review checklist on a recent change or draft change. |
| 7 | [`10-infrastructure-containers-and-deployment.md`](10-infrastructure-containers-and-deployment.md) | Review one Dockerfile or compose file for runtime, health, ports, secrets and startup posture. |
| 8 | [`13-frontend-experience-api-and-ui-supportability.md`](13-frontend-experience-api-and-ui-supportability.md) | Review one UI screen or panel for backing API, loading, empty, partial, degraded, blocked and error states. |

60-day checkpoint:

1. Can you write a narrow technical review with evidence?
2. Can you identify whether a UI is backed by supported API truth?
3. Can you explain what CI lane should catch a given failure?
4. Can you distinguish catalog-visible data from certified data?
5. Can you propose one small improvement with tests and docs?

## 90-Day Engineering Lead Depth

Focus: lead quality, architecture and operational decisions.

| Week | Read | Practice |
|---|---|---|
| 9 | [`11-git-release-and-knowledge-management.md`](11-git-release-and-knowledge-management.md) | Review a PR or release note for scope, evidence, risk and follow-up clarity. |
| 10 | [`technical-knowledge-coverage-matrix.md`](technical-knowledge-coverage-matrix.md) | Pick one weak technical area and plan an enrichment slice. |
| 11 | Re-read [`06-observability-sre-and-operational-support.md`](06-observability-sre-and-operational-support.md), [`08-testing-performance-and-resilience.md`](08-testing-performance-and-resilience.md) and [`15-performance-scalability-resilience-and-async-work.md`](15-performance-scalability-resilience-and-async-work.md) | Convert one failure mode into a test, alert, runbook update or gate proposal. |
| 12 | Re-read [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md), [`05-ci-cd-quality-gates-and-release-governance.md`](05-ci-cd-quality-gates-and-release-governance.md), [`backend-service-design/14-backend-refactoring-and-architecture-review-playbook.md`](backend-service-design/14-backend-refactoring-and-architecture-review-playbook.md), [`api-contract-engineering/15-api-review-checklists-and-governance-playbook.md`](api-contract-engineering/15-api-review-checklists-and-governance-playbook.md), [`16-practitioner-review-prompts-and-checklists.md`](16-practitioner-review-prompts-and-checklists.md) and [`17-implementation-evidence-reading-map.md`](17-implementation-evidence-reading-map.md) | Write an architecture or delivery review for a proposed change. |

90-day checkpoint:

1. Can you lead a review without relying only on personal opinion?
2. Can you tie risk to evidence, tests and operational controls?
3. Can you explain what should be fixed now versus tracked as a follow-up?
4. Can you prevent documentation from drifting away from implementation truth?
5. Can you coach another engineer through the repository reading path?

## Repository Reading Exercise

Use this exercise for any new repository. For deeper evidence patterns, use [`17-implementation-evidence-reading-map.md`](17-implementation-evidence-reading-map.md).

| Step | Question |
|---|---|
| 1 | What is the repository role? |
| 2 | What business or technical boundary does it own? |
| 3 | What are the local install, lint, typecheck, test and CI commands? |
| 4 | What are the public APIs, events, jobs or UI surfaces? |
| 5 | Where are contracts and schemas defined? |
| 6 | Where are tests located and what do they prove? |
| 7 | What are the health, readiness, metrics and logging conventions? |
| 8 | What configuration and secrets does it require? |
| 9 | How does it run locally and in a container? |
| 10 | What are the known gaps, scorecards or follow-up items? |

## Technical Review Exercise

For a planned change, write a short review with the prompts in [`16-practitioner-review-prompts-and-checklists.md`](16-practitioner-review-prompts-and-checklists.md):

1. owner and boundary,
2. behavior changed or preserved,
3. API or contract impact,
4. data and source ownership,
5. security and sensitive-data impact,
6. tests required,
7. CI lane required,
8. runtime and observability impact,
9. docs/runbook impact,
10. residual risks.

## Leadership Habits

Build these habits:

1. ask "where is the evidence?",
2. separate current support from future intent,
3. insist on clear ownership,
4. prefer small improvement slices,
5. make failure states explicit,
6. connect code, tests, CI, docs and operations,
7. keep examples source-safe,
8. improve the knowledge base when a repeated pattern appears.

## Self-Assessment Rubric

| Level | Signal |
|---|---|
| Beginner | Can read docs but needs help tracing implementation and evidence. |
| Developing | Can trace endpoints, tests and CI commands for a focused area. |
| Proficient | Can review changes across code, contracts, tests, docs and supportability. |
| Lead-ready | Can identify systemic gaps, prioritize slices, coach others and improve durable standards. |
| Architecture-ready | Can reason across service boundaries, runtime, data products, security, operations and release evidence. |

## KT Session Template

Use this agenda for a one-hour KT session:

1. repository or topic purpose,
2. architecture boundary,
3. key contracts,
4. local commands,
5. important tests,
6. runtime and observability,
7. security and sensitive-data posture,
8. common failure modes,
9. current gaps,
10. next learning exercise.

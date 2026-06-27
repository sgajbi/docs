# Master Pack Index and Knowledge Map

## Purpose

This file provides the master knowledge map across the full enterprise technical pack series.

Use it to decide which pack to read or apply for a specific engineering problem.

---

## Knowledge Map By Theme

| Theme | Primary Packs |
|---|---|
| Engineering foundations | Pack 1, 2, 3, 9, 10 |
| Backend architecture | Pack 2, 3, 8, 11 |
| API and contract governance | Pack 3, 4, 9, 11 |
| Data products and lineage | Pack 4, 12, 15, 18, 19 |
| CI/CD and DevSecOps | Pack 5, 8, 9, 10 |
| Runtime and Kubernetes | Pack 6, 7, 11, 19 |
| Observability and SRE | Pack 7, 11, 19 |
| Security and cyber resilience | Pack 8, 14, 17, 19 |
| Testing and certification | Pack 9, 16, 18 |
| Git and delivery discipline | Pack 10, 13 |
| Performance and async | Pack 11, 7, 19 |
| Documentation and KT | Pack 12, 13 |
| Engineering leadership | Pack 13, 12, 19 |
| AI and agentic engineering | Pack 14, 13, 17 |
| Wealth domain | Pack 15, 16, 18 |
| Reporting and archive | Pack 16, 15, 18 |
| Productization | Pack 17, 5, 8, 9, 12, 19 |
| Migration and cutover | Pack 18, 15, 16, 19 |
| Production support | Pack 19, 7, 11 |

---

## Knowledge Map By Engineering Question

| Question | Best Packs |
|---|---|
| How should I structure backend services? | 2 |
| How should APIs be designed and governed? | 3 |
| How do we build trusted data products? | 4 |
| How do we set up CI/CD quality gates? | 5 |
| How do Docker/Kubernetes/runtime concepts fit together? | 6 |
| How should observability and runbooks work? | 7 |
| How do we design secure systems? | 8 |
| How do we test enterprise systems properly? | 9 |
| How should Git and PR review work? | 10 |
| How do we design for performance and resilience? | 11 |
| How should documentation and KT be governed? | 12 |
| How do I operate as an engineering lead/architect? | 13 |
| How should AI/RAG/agents be governed? | 14 |
| What wealth-management technology domain concepts matter? | 15 |
| How should reporting/archive/evidence systems work? | 16 |
| What makes software bank-buyable? | 17 |
| How should migrations and cutovers be governed? | 18 |
| How should production support and incident management work? | 19 |

---

## Dependency View

Some packs are foundational for others:

```text
Pack 1: foundation
  -> Pack 2 backend architecture
  -> Pack 3 API governance
  -> Pack 4 data products
  -> Pack 5 CI/CD
  -> Pack 6 runtime
  -> Pack 7 observability
  -> Pack 8 security
  -> Pack 9 testing
  -> Pack 10 Git delivery

Packs 2-10
  -> Pack 11 performance/resilience
  -> Pack 12 documentation/knowledge governance
  -> Pack 13 engineering leadership
  -> Pack 14 AI governance

Packs 13-14
  -> Pack 17 productization
  -> Pack 18 migration
  -> Pack 19 production support

Pack 15 wealth domain and Pack 16 reporting
  -> Apply the technical practices to business-specific systems.
```

---

## Practical Usage Modes

| Usage Mode | How To Use |
|---|---|
| Personal study | Follow recommended sequence and role-based roadmaps. |
| Team KT | Use onboarding plan and role-based learning paths. |
| Repo standardization | Convert checklists into repo docs, templates, and CI gates. |
| Architecture review | Use relevant pack review questions before design approval. |
| PR review | Use API, testing, security, Git, and performance checklists. |
| Production readiness | Use runtime, observability, support, security, and DR packs. |
| Productization | Use productization, security, support, docs, and evidence packs. |
| Migration | Use migration, data, reporting, security, and operations packs. |
| AI adoption | Use AI governance, security, testing, and productization packs. |

## Applied Output Map

Use this map to convert reading into concrete engineering output.

| Learning focus | Read first | Produce |
|---|---|---|
| Service ownership | Backend service design, API contract engineering, documentation governance | Capability boundary note, dependency map, owner list and integration contract inventory. |
| API quality | API contract engineering, testing certification, Git workflow | OpenAPI review checklist, compatibility note, example payloads, error model and contract-test plan. |
| Data trust | Data-product engineering, security, observability | Data contract, source lineage map, freshness/quality rules, access policy and trust telemetry. |
| Release governance | CI/CD release evidence, Git workflow, testing certification | Local-check command, CI lane map, PR evidence rule, release checklist and rollback note. |
| Runtime readiness | Runtime infrastructure, observability, security, production support | Deployment model, configuration map, probes, dashboards, runbook and support route. |
| Resilience | Performance/resilience, observability, testing, production support | Timeout/retry policy, async recovery design, capacity assumptions and failure-mode tests. |
| Security posture | Security/cyber resilience, CI/CD, runtime, AI governance where relevant | Threat model, control owner map, secrets policy, sensitive-data rules and verification evidence. |
| Team KT | Technical learning path, documentation governance, leadership governance | Role curriculum, walkthrough exercise, teach-back checklist and follow-up improvement backlog. |
| Productization | Platform productization, security, production support, documentation governance | Buyer evidence pack, supported-feature matrix, implementation guide, support model and due-diligence answers. |

## Execution Matrix By Scenario

Use this matrix when the knowledge base needs to support a concrete delivery or review activity.

| Scenario | Primary guides | Expected output |
|---|---|---|
| New service or capability | Backend service design, API contract engineering, testing certification, observability supportability | Service boundary note, API contract, test plan, runbook stub and release evidence plan. |
| New data product | Data-product engineering, API/event contracts, security, observability, documentation governance | Producer contract, consumer contract, lineage note, quality rules, access policy and certification evidence. |
| Production release | CI/CD release evidence, Git release hygiene, testing certification, observability, security | PR evidence, release checklist, deployment plan, rollback note, known-risk register and validation results. |
| Architecture review | Architecture boundaries, engineering leadership, documentation governance, security, performance | Decision record, tradeoff analysis, risk list, follow-up actions and acceptance criteria. |
| Incident or support improvement | Production support, observability, performance/resilience, runtime infrastructure, documentation governance | Incident timeline, root-cause note, runbook update, dashboard or alert change and corrective-action tracker. |
| Migration or replatforming | Migration engineering, data products, reporting evidence, security, testing, production support | Inventory, mapping rules, reconciliation plan, mock-cutover evidence, rollback posture and hypercare plan. |
| AI-assisted feature | AI/RAG governance, security, data-product engineering, testing certification, observability | Use-case risk tier, source policy, grounding strategy, evaluation set, human-review rule and audit trail. |
| Enterprise buyer or due-diligence review | Platform productization, security, documentation governance, CI/CD evidence, production support | Readiness scorecard, supported-feature matrix, deployment model, support model and evidence pack. |

## Depth Progression

For each scenario, progress through four levels:

| Level | Focus | Reader should be able to |
|---:|---|---|
| 1 | Concept literacy | Explain the topic, why it matters and where it fits in an enterprise platform. |
| 2 | Implementation literacy | Identify the main files, contracts, runtime components, tests and operational artifacts involved. |
| 3 | Review literacy | Evaluate design quality, delivery risk, evidence quality, supportability and governance gaps. |
| 4 | Leadership literacy | Make tradeoffs, set standards, communicate risk and convert lessons into durable team practice. |

---

## Summary

The full series is a reusable engineering operating system.

Each pack is useful alone, but the full value comes when they are applied together across design, delivery, operations, and leadership.

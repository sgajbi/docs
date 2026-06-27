# Master Guide Index and Knowledge Map

## Purpose

This file provides the master knowledge map across the full enterprise technical guide series.

Use it to decide which guide to read or apply for a specific engineering problem.

---

## Curated Guide Key

The rest of this roadmap uses short guide numbers for compact cross-reference. Use this key to navigate to the curated repo guides.

| Guide | Curated guide |
|---:|---|
| 1 | [`Technical Knowledge Base`](../README.md) |
| 2 | [`Backend Architecture and Service Design`](../backend-service-design/README.md) |
| 3 | [`API Design, Governance and Contract Engineering`](../api-contract-engineering/README.md) |
| 4 | [`Data Products, Data Mesh, Lineage and Trust Engineering`](../data-product-engineering/README.md) |
| 5 | [`CI/CD, DevSecOps, Quality Gates and Release Evidence`](../cicd-devsecops-release-evidence/README.md) |
| 6 | [`Runtime Infrastructure, Containers and Kubernetes`](../runtime-infrastructure-kubernetes/README.md) |
| 7 | [`Observability, SRE, Runbooks and Supportability`](../observability-sre-supportability/README.md) |
| 8 | [`Security, Configuration, Secrets and Cyber Resilience`](../security-cyber-resilience/README.md) |
| 9 | [`Testing, Quality Engineering and Certification`](../testing-quality-certification/README.md) |
| 10 | [`Git Workflow, PR Review and Release Hygiene`](../git-workflow-release-hygiene/README.md) |
| 11 | [`Performance, Scalability, Resilience and Async Engineering`](../performance-scalability-resilience-async/README.md) |
| 12 | [`Documentation, Knowledge Management and Architecture Governance`](../documentation-knowledge-governance/README.md) |
| 13 | [`Engineering Leadership and Architecture Governance`](../engineering-leadership-architecture-governance/README.md) |
| 14 | [`AI, RAG, Copilot and Agentic Engineering Governance`](../ai-rag-copilot-agentic-governance/README.md) |
| 15 | [`Wealth Management Technology Domain Engineering`](../wealth-management-domain-engineering/README.md) |
| 16 | [`Reporting, Document Generation, Archive and Evidence Engineering`](../reporting-document-evidence-engineering/README.md) |
| 17 | [`Platform Productization and Bank-Buyable Software Readiness`](../platform-productization-bank-buyable-readiness/README.md) |
| 18 | [`Migration, Replatforming, Reconciliation and Cutover Engineering`](../migration-replatforming-cutover-engineering/README.md) |
| 19 | [`Production Support, Operations and Incident Excellence`](../production-support-operations-incident-excellence/README.md) |

---

## Knowledge Map By Theme

| Theme | Primary guides |
|---|---|
| Engineering foundations | Guides 1, 2, 3, 9, 10 |
| Backend architecture | Guides 2, 3, 8, 11 |
| API and contract governance | Guides 3, 4, 9, 11 |
| Data products and lineage | Guides 4, 12, 15, 18, 19 |
| CI/CD and DevSecOps | Guides 5, 8, 9, 10 |
| Runtime and Kubernetes | Guides 6, 7, 11, 19 |
| Observability and SRE | Guides 7, 11, 19 |
| Security and cyber resilience | Guides 8, 14, 17, 19 |
| Testing and certification | Guides 9, 16, 18 |
| Git and delivery discipline | Guides 10, 13 |
| Performance and async | Guides 11, 7, 19 |
| Documentation and KT | Guides 12, 13 |
| Engineering leadership | Guides 13, 12, 19 |
| AI and agentic engineering | Guides 14, 13, 17 |
| Wealth domain | Guides 15, 16, 18 |
| Reporting and archive | Guides 16, 15, 18 |
| Productization | Guides 17, 5, 8, 9, 12, 19 |
| Migration and cutover | Guides 18, 15, 16, 19 |
| Production support | Guides 19, 7, 11 |

---

## Knowledge Map By Engineering Question

| Question | Best guides |
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

Some guides are foundational for others:

```text
Guide 1: foundation
  -> Guide 2 backend architecture
  -> Guide 3 API governance
  -> Guide 4 data products
  -> Guide 5 CI/CD
  -> Guide 6 runtime
  -> Guide 7 observability
  -> Guide 8 security
  -> Guide 9 testing
  -> Guide 10 Git delivery

Guides 2-10
  -> Guide 11 performance/resilience
  -> Guide 12 documentation/knowledge governance
  -> Guide 13 engineering leadership
  -> Guide 14 AI governance

Guides 13-14
  -> Guide 17 productization
  -> Guide 18 migration
  -> Guide 19 production support

Guide 15 wealth domain and Guide 16 reporting
  -> Apply the technical practices to business-specific systems.
```

---

## Practical Usage Modes

| Usage Mode | How To Use |
|---|---|
| Personal study | Follow recommended sequence and role-based roadmaps. |
| Team KT | Use onboarding plan and role-based learning paths. |
| Repo standardization | Convert checklists into repo docs, templates, and CI gates. |
| Architecture review | Use relevant guide review questions before design approval. |
| PR review | Use API, testing, security, Git, and performance checklists. |
| Production readiness | Use runtime, observability, support, security, and DR guides. |
| Productization | Use productization, security, support, documentation and evidence packs. |
| Migration | Use migration, data, reporting, security and operations guides. |
| AI adoption | Use AI governance, security, testing and productization guides. |
| Source material intake | Use the roadmap, documentation governance and coverage matrix before adding or rewriting new technical material. |

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
| New technical source material | Technical roadmap, documentation governance, coverage matrix, role guide | Placement decision, duplicate check, sanitized additions, updated navigation and coverage note. |

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

Each guide is useful alone, but the full value comes when they are applied together across design, delivery, operations and leadership.

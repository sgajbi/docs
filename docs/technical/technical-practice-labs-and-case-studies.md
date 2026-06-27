# Technical Practice Labs And Case Studies

Use these labs to turn the technical knowledge base into practical engineering skill. Each lab is intentionally product-neutral and can be applied to any enterprise banking, wealth-management or regulated platform repository.

The purpose is not to memorize guidance. The purpose is to inspect a real or representative system, produce evidence, make a defensible engineering judgement and identify a small improvement.

## How To Use These Labs

For each lab:

1. read the listed guides,
2. inspect a real repository, service design, architecture note, API contract, runbook or delivery workflow,
3. produce the listed evidence artifacts,
4. answer the review questions,
5. identify one small improvement that could be delivered with tests, documentation and validation evidence.

Keep each lab output short. A useful lab output is normally one to three pages plus links to evidence.

## Lab Index

| Lab | Scenario | Primary guides | Evidence to produce |
|---:|---|---|---|
| 1 | Service boundary review | Architecture, backend service design, domain engineering | Boundary note, capability owner, dependency map and duplicated-rule risk list. |
| 2 | Backward-compatible API change | API contract engineering, testing, Git workflow | OpenAPI delta, consumer-impact note, compatibility tests and deprecation plan. |
| 3 | Data-product certification | Data-product engineering, security, observability | Data contract, lineage map, quality rules, access policy and certification evidence. |
| 4 | CI gate promotion | CI/CD release evidence, testing, Git workflow | Gate purpose, owner, lane placement, failure behavior, rollout plan and scorecard update. |
| 5 | Incident and observability review | Observability, SRE, production support | Alert-to-runbook map, severity decision, timeline, root cause and corrective actions. |
| 6 | Sensitive-data and access-control review | Security, API governance, testing | Threat statement, authorization matrix, sensitive-field inventory and negative tests. |
| 7 | Test strategy redesign | Testing certification, backend, API, data products | Test pyramid, golden cases, contract tests, integration tests and flakiness actions. |
| 8 | Async workflow resilience | Performance/async, observability, reporting | Job state model, idempotency rule, retry policy, dead-letter handling and recovery test. |
| 9 | Runtime deployment readiness | Runtime infrastructure, CI/CD, SRE | Container review, configuration map, probes, resources, rollback and smoke evidence. |
| 10 | Git, release and documentation truth | Git workflow, documentation governance | PR evidence checklist, release-note draft, decision record and stale-truth cleanup list. |
| 11 | AI/RAG or copilot governance | AI governance, data products, security, testing | Source policy, entitlement model, prompt/version record, evaluation set and safety tests. |
| 12 | Migration and cutover readiness | Migration engineering, data products, support | Inventory, mapping rules, reconciliation report, rollback tree and hypercare plan. |
| 13 | Reporting and archive evidence | Reporting/document evidence, performance, security | Snapshot rule, reproducibility proof, archive metadata, hash/integrity check and degraded-report behavior. |
| 14 | Platform productization readiness | Productization, security, CI/CD, documentation | Supported-feature matrix, buyer evidence pack, implementation guide and readiness scorecard. |

## 1. Service Boundary Review

Scenario:

A platform has a domain service, an experience API and a UI surface that all appear to calculate or classify the same business concept.

Read:

- [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md)
- [`backend-service-design/02-domain-boundaries-bounded-contexts-and-service-types.md`](backend-service-design/02-domain-boundaries-bounded-contexts-and-service-types.md)
- [`wealth-management-domain-engineering/11-api-bff-domain-service-and-data-product-boundaries.md`](wealth-management-domain-engineering/11-api-bff-domain-service-and-data-product-boundaries.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Boundary note | Names the capability, authoritative owner, composing layers and unsupported states. |
| Dependency map | Shows upstream sources, downstream consumers, synchronous calls, async feeds and cached views. |
| Duplicated-rule risk list | Identifies any rule repeated outside the owning service and the business impact if it diverges. |
| Improvement action | One small refactor, contract clarification, test addition or documentation update. |

Review questions:

1. Which service owns the business truth?
2. Which layers only compose or render the truth?
3. Is any business rule duplicated for UI convenience?
4. How are stale, partial, unavailable and permission-blocked states represented?
5. What evidence proves the boundary is implemented, not only documented?

## 2. Backward-Compatible API Change

Scenario:

An API response needs a new field and a legacy field should eventually be retired.

Read:

- [`api-contract-engineering/01-api-as-product-and-system-contract.md`](api-contract-engineering/01-api-as-product-and-system-contract.md)
- [`api-contract-engineering/10-versioning-compatibility-aliases-and-deprecation.md`](api-contract-engineering/10-versioning-compatibility-aliases-and-deprecation.md)
- [`testing-quality-certification/05-api-openapi-contract-and-error-behaviour-testing.md`](testing-quality-certification/05-api-openapi-contract-and-error-behaviour-testing.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Contract delta | Shows added field, unchanged fields, examples and error behavior. |
| Consumer-impact note | Lists known consumers, compatibility risk, rollout sequence and monitoring plan. |
| Deprecation plan | Defines warning period, replacement field, removal criteria and communication path. |
| Test evidence | Includes schema/contract tests, backward compatibility tests and negative examples. |

Review questions:

1. Is the change additive, breaking or behavior-changing?
2. Can old consumers continue to parse the response?
3. Are examples and generated docs updated?
4. Are error semantics unchanged or intentionally versioned?
5. Is the deprecation observable and governed?

## 3. Data-Product Certification

Scenario:

A reusable analytics dataset should be published as a trusted data product for downstream reports, dashboards or AI retrieval.

Read:

- [`data-product-engineering/README.md`](data-product-engineering/README.md)
- [`data-product-engineering/09-slo-access-evidence-and-policy-contracts.md`](data-product-engineering/09-slo-access-evidence-and-policy-contracts.md)
- [`observability-sre-supportability/11-data-product-and-trust-telemetry-observability.md`](observability-sre-supportability/11-data-product-and-trust-telemetry-observability.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Data contract | Names owner, schema, version, semantics, allowed consumers and lifecycle state. |
| Lineage map | Shows source system, transformations, quality gates and published output. |
| Trust rules | Defines freshness, completeness, reconciliation, quality thresholds and evidence retention. |
| Certification result | Shows pass/fail status, date, scope, exceptions and expiry or review cadence. |

Review questions:

1. Who owns the meaning of the data?
2. What happens when a source is stale or incomplete?
3. Which consumers are allowed to use the product?
4. What telemetry tells operators whether the product is trustworthy?
5. What would cause certification to be suspended?

## 4. CI Gate Promotion

Scenario:

A report-only quality check has become stable and the team wants to make it blocking in the pull-request lane.

Read:

- [`cicd-devsecops-release-evidence/01-cicd-as-evidence-production-system.md`](cicd-devsecops-release-evidence/01-cicd-as-evidence-production-system.md)
- [`cicd-devsecops-release-evidence/14-report-only-to-blocking-gate-promotion-and-maturity-roadmap.md`](cicd-devsecops-release-evidence/14-report-only-to-blocking-gate-promotion-and-maturity-roadmap.md)
- [`git-workflow-release-hygiene/09-main-branch-protection-status-checks-and-green-main-discipline.md`](git-workflow-release-hygiene/09-main-branch-protection-status-checks-and-green-main-discipline.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Gate purpose | Explains what risk the gate controls and why it belongs in the selected lane. |
| Baseline evidence | Shows recent pass/fail history, runtime cost, false positives and owner. |
| Failure behavior | Provides actionable messages, remediation guidance and exception process. |
| Rollout plan | Starts as report-only, promotes to warning if needed and then blocks with owner approval. |

Review questions:

1. Is the gate deterministic enough to block merges?
2. Does the output tell developers what to fix?
3. Is the runtime cost appropriate for the lane?
4. Who owns false-positive triage?
5. How will the gate be reviewed after promotion?

## 5. Incident And Observability Review

Scenario:

A critical report, calculation or client-facing workflow returned stale output because an upstream feed was delayed.

Read:

- [`observability-sre-supportability/README.md`](observability-sre-supportability/README.md)
- [`production-support-operations-incident-excellence/05-incident-management-severity-triage-and-command-center.md`](production-support-operations-incident-excellence/05-incident-management-severity-triage-and-command-center.md)
- [`production-support-operations-incident-excellence/09-problem-management-rca-postmortem-and-continuous-improvement.md`](production-support-operations-incident-excellence/09-problem-management-rca-postmortem-and-continuous-improvement.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Timeline | Captures detection, impact, containment, diagnosis, recovery and validation. |
| Alert-to-runbook map | Connects alert, dashboard, owner, runbook and escalation route. |
| Root-cause note | Separates triggering event, contributing controls and missing detection. |
| Corrective action list | Names owner, expected evidence, target date and prevention mechanism. |

Review questions:

1. Did the system detect the problem before users did?
2. Was the severity decision tied to business impact?
3. Could operators identify impacted workflows without exposing sensitive data?
4. Did dashboards show cause, not only symptoms?
5. Which test, metric, alert or runbook should change?

## 6. Sensitive-Data And Access-Control Review

Scenario:

A new capability exposes account, portfolio, document or advisory data through an API, UI panel, report or AI assistant.

Read:

- [`security-cyber-resilience/03-identity-authentication-authorization-rbac-abac-and-entitlements.md`](security-cyber-resilience/03-identity-authentication-authorization-rbac-abac-and-entitlements.md)
- [`security-cyber-resilience/06-sensitive-data-classification-privacy-masking-and-tokenization.md`](security-cyber-resilience/06-sensitive-data-classification-privacy-masking-and-tokenization.md)
- [`testing-quality-certification/11-security-entitlement-and-sensitive-content-testing.md`](testing-quality-certification/11-security-entitlement-and-sensitive-content-testing.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Authorization matrix | Maps actor, permission, object scope, allowed action and denial behavior. |
| Sensitive-field inventory | Lists restricted fields and where they must not appear. |
| Negative-test pack | Proves cross-client access, missing entitlement and restricted field leaks are blocked. |
| Evidence-safety note | Confirms logs, metrics, traces, screenshots and test artifacts are redacted. |

Review questions:

1. Is authorization object-level, not only role-level?
2. Are denied responses safe and non-enumerating?
3. Can support evidence be shared without restricted data?
4. Are AI prompts, report narratives or exports also governed?
5. What must be recertified when roles or entitlements change?

## 7. Test Strategy Redesign

Scenario:

A repository has many tests but incidents still escape because important behavior is only tested through fragile end-to-end flows.

Read:

- [`testing-quality-certification/02-test-pyramid-test-portfolio-and-risk-based-testing.md`](testing-quality-certification/02-test-pyramid-test-portfolio-and-risk-based-testing.md)
- [`testing-quality-certification/09-regression-golden-examples-and-certification-sweeps.md`](testing-quality-certification/09-regression-golden-examples-and-certification-sweeps.md)
- [`technical-master-index-study-roadmap/17-master-checklists-and-execution-playbook.md`](technical-master-index-study-roadmap/17-master-checklists-and-execution-playbook.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Behavior inventory | Lists critical rules, contracts, integrations and degraded states. |
| Test pyramid map | Places each behavior at the cheapest reliable level. |
| Golden-case catalog | Defines representative business examples with stable synthetic data. |
| Flakiness action list | Names flaky tests, cause, lane impact and remediation. |

Review questions:

1. Which failures would current tests miss?
2. Which tests are too high-level for the behavior they prove?
3. Are golden cases owned and reviewed?
4. Are degraded and permission-blocked states tested?
5. Which tests should move lanes or be split?

## 8. Async Workflow Resilience

Scenario:

A long-running report generation, data ingestion, calculation or AI workflow can be retried, replayed or resumed.

Read:

- [`performance-scalability-resilience-async/07-async-execution-job-state-polling-and-result-retrieval.md`](performance-scalability-resilience-async/07-async-execution-job-state-polling-and-result-retrieval.md)
- [`performance-scalability-resilience-async/08-workers-queues-leasing-dead-letter-replay-and-recovery.md`](performance-scalability-resilience-async/08-workers-queues-leasing-dead-letter-replay-and-recovery.md)
- [`observability-sre-supportability/10-async-worker-observability-replay-recovery-and-retention.md`](observability-sre-supportability/10-async-worker-observability-replay-recovery-and-retention.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Job state model | Defines accepted, running, succeeded, failed, retrying, cancelled and expired states. |
| Idempotency rule | Describes duplicate request handling, result reuse and conflict behavior. |
| Recovery playbook | Covers retry, replay, dead-letter inspection, cancellation and operator audit. |
| Resilience test | Proves worker restart, duplicate delivery, partial failure and replay behavior. |

Review questions:

1. Can duplicate submissions create duplicate business effects?
2. Can a job be resumed after worker restart?
3. Are failed jobs visible and actionable?
4. Is replay safe and auditable?
5. Does the user or caller see truthful progress and failure states?

## 9. Runtime Deployment Readiness

Scenario:

A service is ready for containerized deployment and needs production-readiness review.

Read:

- [`runtime-infrastructure-kubernetes/README.md`](runtime-infrastructure-kubernetes/README.md)
- [`10-infrastructure-containers-and-deployment.md`](10-infrastructure-containers-and-deployment.md)
- [`observability-sre-supportability/05-health-readiness-supportability-and-degraded-state-design.md`](observability-sre-supportability/05-health-readiness-supportability-and-degraded-state-design.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Container review | Confirms small image, non-root runtime, pinned dependencies and no secrets. |
| Configuration map | Lists required env vars, secret sources, defaults and validation behavior. |
| Probe review | Separates liveness, readiness and dependency supportability. |
| Rollback evidence | Shows previous version recovery path, migration posture and smoke checks. |

Review questions:

1. Does the container behave the same locally and in orchestration?
2. Can readiness block traffic when dependencies are unavailable?
3. Are resource requests, limits and autoscaling assumptions explicit?
4. Are migrations safe during rollout and rollback?
5. Is smoke evidence available after deployment?

## 10. Git, Release And Documentation Truth

Scenario:

A feature is implemented but docs, release evidence, wiki/source documentation or architecture notes do not fully match the code.

Read:

- [`git-workflow-release-hygiene/README.md`](git-workflow-release-hygiene/README.md)
- [`documentation-knowledge-governance/README.md`](documentation-knowledge-governance/README.md)
- [`17-implementation-evidence-reading-map.md`](17-implementation-evidence-reading-map.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Truth map | Connects code, tests, contracts, docs, runbooks and release notes. |
| PR evidence checklist | Lists commands, CI lanes, screenshots or runtime proof when relevant. |
| Stale-truth list | Names obsolete docs, comments, examples, diagrams or claims. |
| Decision record | Captures important tradeoffs, owner, date and expected review trigger. |

Review questions:

1. Can a new engineer understand current behavior from durable docs?
2. Do generated and authored docs agree?
3. Are planned and implemented states clearly separated?
4. Are release notes meaningful to operators and consumers?
5. Is stale truth removed rather than left for interpretation?

## 11. AI/RAG Or Copilot Governance

Scenario:

A team wants to add an AI assistant, retrieval workflow or agentic tool that uses internal documentation or operational evidence.

Read:

- [`ai-rag-copilot-agentic-governance/README.md`](ai-rag-copilot-agentic-governance/README.md)
- [`ai-rag-copilot-agentic-governance/03-knowledge-source-governance-and-entitlement-aware-retrieval.md`](ai-rag-copilot-agentic-governance/03-knowledge-source-governance-and-entitlement-aware-retrieval.md)
- [`ai-rag-copilot-agentic-governance/10-model-risk-evaluation-red-teaming-and-certification.md`](ai-rag-copilot-agentic-governance/10-model-risk-evaluation-red-teaming-and-certification.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Source policy | Lists approved sources, freshness rules, ownership, exclusions and retention. |
| Entitlement model | Explains which users can retrieve which content and why. |
| Evaluation set | Covers factuality, citation quality, refusal behavior, sensitive-data leakage and prompt injection. |
| Tool-safety note | Defines authorized tools, approval points, audit trail and blocked actions. |

Review questions:

1. Are source documents trustworthy and current?
2. Does retrieval enforce user entitlements before generation?
3. Are generated answers grounded with traceable evidence?
4. Can the assistant take unsafe actions without review?
5. What telemetry proves quality, safety and user value?

## 12. Migration And Cutover Readiness

Scenario:

A capability is moving from an older platform to a new service, data model, API or reporting workflow.

Read:

- [`migration-replatforming-cutover-engineering/README.md`](migration-replatforming-cutover-engineering/README.md)
- [`migration-replatforming-cutover-engineering/05-reconciliation-strategy-controls-tolerances-and-exceptions.md`](migration-replatforming-cutover-engineering/05-reconciliation-strategy-controls-tolerances-and-exceptions.md)
- [`migration-replatforming-cutover-engineering/07-cutover-strategy-command-center-and-go-no-go-governance.md`](migration-replatforming-cutover-engineering/07-cutover-strategy-command-center-and-go-no-go-governance.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Inventory | Covers systems, data, reports, interfaces, jobs, entitlements and operational owners. |
| Mapping rules | Define field semantics, transformations, defaults, exclusions and exception handling. |
| Reconciliation pack | Shows counts, balances, tolerances, breaks, sign-off and unresolved exceptions. |
| Cutover runbook | Names sequence, owner, go/no-go criteria, rollback tree, communication and hypercare. |

Review questions:

1. What must match exactly and what can use tolerance?
2. Which reports or client outputs must remain continuous?
3. What is the rollback decision point?
4. Which consumers need parallel-run evidence?
5. How will support teams detect post-cutover issues?

## 13. Reporting And Archive Evidence

Scenario:

A generated statement, review pack, regulatory report or client document must be reproducible and auditable.

Read:

- [`reporting-document-evidence-engineering/README.md`](reporting-document-evidence-engineering/README.md)
- [`reporting-document-evidence-engineering/03-report-data-snapshots-lineage-and-reproducibility.md`](reporting-document-evidence-engineering/03-report-data-snapshots-lineage-and-reproducibility.md)
- [`reporting-document-evidence-engineering/07-report-evidence-audit-trail-hash-and-integrity-controls.md`](reporting-document-evidence-engineering/07-report-evidence-audit-trail-hash-and-integrity-controls.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Snapshot rule | Defines as-of time, data versions, template version and calculation version. |
| Reproducibility proof | Shows the report can be regenerated or explained from captured inputs. |
| Archive metadata | Captures owner, retention, hash, retrieval policy, entitlement and delivery status. |
| Degraded-report behavior | Defines warnings, blocked sections, retry rules and client-safe messages. |

Review questions:

1. Can the same report be explained months later?
2. Are template, data and calculation versions recorded?
3. Can users access only documents they are entitled to view?
4. Does archive retrieval preserve integrity and audit trail?
5. What happens when a section is stale or unavailable?

## 14. Platform Productization Readiness

Scenario:

A platform capability is being positioned for enterprise buyer review, internal adoption or external due diligence.

Read:

- [`platform-productization-bank-buyable-readiness/README.md`](platform-productization-bank-buyable-readiness/README.md)
- [`platform-productization-bank-buyable-readiness/04-reference-architecture-deployment-and-integration-readiness.md`](platform-productization-bank-buyable-readiness/04-reference-architecture-deployment-and-integration-readiness.md)
- [`platform-productization-bank-buyable-readiness/16-readiness-scorecards-gap-analysis-and-improvement-roadmap.md`](platform-productization-bank-buyable-readiness/16-readiness-scorecards-gap-analysis-and-improvement-roadmap.md)

Produce:

| Artifact | What good looks like |
|---|---|
| Supported-feature matrix | Separates implemented, configurable, integration-dependent, planned and unsupported capabilities. |
| Buyer evidence pack | Includes architecture, security, deployment, data governance, support and test evidence. |
| Implementation guide | Explains environments, configuration, integration, roles, data setup and validation. |
| Readiness scorecard | Scores gaps by buyer risk, delivery effort, evidence strength and owner. |

Review questions:

1. Are capability claims backed by implemented evidence?
2. Can an enterprise buyer understand deployment and integration assumptions?
3. Are security, support and data-governance answers ready?
4. Are unsupported states explicit?
5. Which gap most blocks buyer confidence?

## Suggested 90-Day Practice Sequence

| Period | Labs | Output |
|---|---|---|
| Weeks 1-2 | Labs 1 and 2 | Boundary map and API compatibility review. |
| Weeks 3-4 | Labs 3 and 7 | Data-product certification note and redesigned test pyramid. |
| Weeks 5-6 | Labs 4 and 9 | CI gate promotion plan and runtime readiness review. |
| Weeks 7-8 | Labs 5 and 8 | Incident review and async recovery playbook. |
| Weeks 9-10 | Labs 6 and 11 | Access-control review and AI/RAG safety evaluation. |
| Weeks 11-12 | Labs 10, 12, 13 and 14 | Documentation truth map, migration readiness pack, reporting evidence review and productization scorecard. |

## Lab Output Template

Use this template for each lab:

```text
Lab:
System or artifact reviewed:
Guides used:
Evidence inspected:
Current state:
Risks or gaps:
Decision or recommendation:
Smallest useful improvement:
Owner:
Validation evidence expected:
Follow-up trigger:
```

## Curation Note

Curated as a practice layer for the technical knowledge base. It consolidates recurring architecture, API, data, CI/CD, runtime, observability, security, testing, async, AI, migration, reporting and productization scenarios into neutral labs that support self-learning, KT, architecture review and engineering-lead development.

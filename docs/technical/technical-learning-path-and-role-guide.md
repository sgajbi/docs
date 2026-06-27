# Technical Learning Path And Role Guide

This guide helps readers choose a practical path through the technical knowledge base by role, project type and engineering problem.

Use it when:

1. onboarding into an enterprise banking or wealth platform,
2. preparing for KT,
3. reviewing an architecture or pull request,
4. planning a delivery slice,
5. building broader technical leadership depth.

## Fast Orientation

Start with these files when time is limited:

| Need | Read first |
|---|---|
| Choose a structured technical study path | [`technical-master-index-study-roadmap/README.md`](technical-master-index-study-roadmap/README.md) |
| Turn study into repo evidence or KT actions | [`technical-master-index-study-roadmap/17-master-checklists-and-execution-playbook.md`](technical-master-index-study-roadmap/17-master-checklists-and-execution-playbook.md) |
| Understand the platform shape | [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md) |
| Learn wealth technology domain patterns | [`wealth-management-domain-engineering/README.md`](wealth-management-domain-engineering/README.md) |
| Understand backend service structure | [`backend-service-design/README.md`](backend-service-design/README.md) |
| Design or review APIs | [`api-contract-engineering/README.md`](api-contract-engineering/README.md) |
| Work with data products | [`data-product-engineering/README.md`](data-product-engineering/README.md) |
| Build or govern AI/RAG/copilot systems | [`ai-rag-copilot-agentic-governance/README.md`](ai-rag-copilot-agentic-governance/README.md) |
| Design reporting and document-evidence workflows | [`reporting-document-evidence-engineering/README.md`](reporting-document-evidence-engineering/README.md) |
| Plan migration, replatforming or cutover | [`migration-replatforming-cutover-engineering/README.md`](migration-replatforming-cutover-engineering/README.md) |
| Productize a platform for enterprise buyers | [`platform-productization-bank-buyable-readiness/README.md`](platform-productization-bank-buyable-readiness/README.md) |
| Prepare a PR or release | [`cicd-devsecops-release-evidence/README.md`](cicd-devsecops-release-evidence/README.md) |
| Support production | [`observability-sre-supportability/README.md`](observability-sre-supportability/README.md) |
| Run production support or incident response | [`production-support-operations-incident-excellence/README.md`](production-support-operations-incident-excellence/README.md) |
| Review security controls | [`security-cyber-resilience/README.md`](security-cyber-resilience/README.md) |
| Improve testing strategy | [`testing-quality-certification/README.md`](testing-quality-certification/README.md) |
| Review Git and PR hygiene | [`git-workflow-release-hygiene/README.md`](git-workflow-release-hygiene/README.md) |
| Handle performance or async work | [`performance-scalability-resilience-async/README.md`](performance-scalability-resilience-async/README.md) |
| Improve documentation and KT | [`documentation-knowledge-governance/README.md`](documentation-knowledge-governance/README.md) |
| Operate as an engineering lead or architect | [`engineering-leadership-architecture-governance/README.md`](engineering-leadership-architecture-governance/README.md) |

## Role-Based Reading Paths

### Backend Engineer

| Sequence | Topic |
|---:|---|
| 1 | [`02-backend-application-structure.md`](02-backend-application-structure.md) |
| 2 | [`backend-service-design/README.md`](backend-service-design/README.md) |
| 3 | [`api-contract-engineering/README.md`](api-contract-engineering/README.md) |
| 4 | [`security-cyber-resilience/03-identity-authentication-authorization-rbac-abac-and-entitlements.md`](security-cyber-resilience/03-identity-authentication-authorization-rbac-abac-and-entitlements.md) |
| 5 | [`testing-quality-certification/03-unit-domain-policy-calculation-and-lifecycle-testing.md`](testing-quality-certification/03-unit-domain-policy-calculation-and-lifecycle-testing.md) |
| 6 | [`performance-scalability-resilience-async/06-idempotency-concurrency-locking-and-duplicate-prevention.md`](performance-scalability-resilience-async/06-idempotency-concurrency-locking-and-duplicate-prevention.md) |

Primary outcome: write services with clear boundaries, testable domain rules, safe authorization, useful errors, observable behavior and release-ready tests.

### API Or Experience API Engineer

| Sequence | Topic |
|---:|---|
| 1 | [`03-api-contract-governance.md`](03-api-contract-governance.md) |
| 2 | [`api-contract-engineering/02-rest-resource-action-and-route-design.md`](api-contract-engineering/02-rest-resource-action-and-route-design.md) |
| 3 | [`api-contract-engineering/03-bff-experience-api-and-composition-patterns.md`](api-contract-engineering/03-bff-experience-api-and-composition-patterns.md) |
| 4 | [`api-contract-engineering/06-error-semantics-problem-details-and-supportability.md`](api-contract-engineering/06-error-semantics-problem-details-and-supportability.md) |
| 5 | [`api-contract-engineering/08-idempotency-concurrency-and-write-safety.md`](api-contract-engineering/08-idempotency-concurrency-and-write-safety.md) |
| 6 | [`api-contract-engineering/13-api-testing-contract-testing-and-certification.md`](api-contract-engineering/13-api-testing-contract-testing-and-certification.md) |

Primary outcome: design APIs that are stable, secure, supportable, version-aware and easy for clients to integrate.

### Frontend Engineer

| Sequence | Topic |
|---:|---|
| 1 | [`13-frontend-experience-api-and-ui-supportability.md`](13-frontend-experience-api-and-ui-supportability.md) |
| 2 | [`api-contract-engineering/03-bff-experience-api-and-composition-patterns.md`](api-contract-engineering/03-bff-experience-api-and-composition-patterns.md) |
| 3 | [`observability-sre-supportability/05-health-readiness-supportability-and-degraded-state-design.md`](observability-sre-supportability/05-health-readiness-supportability-and-degraded-state-design.md) |
| 4 | [`testing-quality-certification/08-e2e-browser-product-flow-and-canonical-runtime-testing.md`](testing-quality-certification/08-e2e-browser-product-flow-and-canonical-runtime-testing.md) |
| 5 | [`security-cyber-resilience/07-secure-api-bff-backend-and-ui-design.md`](security-cyber-resilience/07-secure-api-bff-backend-and-ui-design.md) |

Primary outcome: build UI flows that are gateway-backed, permission-aware, accessible, testable, degradation-aware and truthful about backend capability.

### QA Or Quality Engineer

| Sequence | Topic |
|---:|---|
| 1 | [`08-testing-performance-and-resilience.md`](08-testing-performance-and-resilience.md) |
| 2 | [`testing-quality-certification/README.md`](testing-quality-certification/README.md) |
| 3 | [`testing-quality-certification/09-regression-golden-examples-and-certification-sweeps.md`](testing-quality-certification/09-regression-golden-examples-and-certification-sweeps.md) |
| 4 | [`testing-quality-certification/10-test-data-synthetic-fixtures-seeding-and-sensitive-data-safety.md`](testing-quality-certification/10-test-data-synthetic-fixtures-seeding-and-sensitive-data-safety.md) |
| 5 | [`testing-quality-certification/16-ci-test-lane-placement-quality-metrics-and-flakiness-governance.md`](testing-quality-certification/16-ci-test-lane-placement-quality-metrics-and-flakiness-governance.md) |
| 6 | [`09-worked-examples-and-engineering-review-checklists.md`](09-worked-examples-and-engineering-review-checklists.md) |

Primary outcome: turn business behavior, contracts, degraded states, security boundaries and release claims into repeatable evidence.

### SRE, DevOps Or Platform Engineer

| Sequence | Topic |
|---:|---|
| 1 | [`10-infrastructure-containers-and-deployment.md`](10-infrastructure-containers-and-deployment.md) |
| 2 | [`runtime-infrastructure-kubernetes/README.md`](runtime-infrastructure-kubernetes/README.md) |
| 3 | [`observability-sre-supportability/README.md`](observability-sre-supportability/README.md) |
| 4 | [`cicd-devsecops-release-evidence/README.md`](cicd-devsecops-release-evidence/README.md) |
| 5 | [`performance-scalability-resilience-async/13-resource-limits-autoscaling-cost-and-capacity-planning.md`](performance-scalability-resilience-async/13-resource-limits-autoscaling-cost-and-capacity-planning.md) |
| 6 | [`security-cyber-resilience/10-runtime-kubernetes-network-and-workload-security.md`](security-cyber-resilience/10-runtime-kubernetes-network-and-workload-security.md) |

Primary outcome: make services deployable, observable, recoverable, secure, capacity-aware and operationally supportable.

### Architect

| Sequence | Topic |
|---:|---|
| 1 | [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md) |
| 2 | [`backend-service-design/02-domain-boundaries-bounded-contexts-and-service-types.md`](backend-service-design/02-domain-boundaries-bounded-contexts-and-service-types.md) |
| 3 | [`api-contract-engineering/01-api-as-product-and-system-contract.md`](api-contract-engineering/01-api-as-product-and-system-contract.md) |
| 4 | [`data-product-engineering/02-data-mesh-operating-model-and-domain-ownership.md`](data-product-engineering/02-data-mesh-operating-model-and-domain-ownership.md) |
| 5 | [`wealth-management-domain-engineering/11-api-bff-domain-service-and-data-product-boundaries.md`](wealth-management-domain-engineering/11-api-bff-domain-service-and-data-product-boundaries.md) |
| 6 | [`engineering-leadership-architecture-governance/05-architecture-review-process-and-design-quality.md`](engineering-leadership-architecture-governance/05-architecture-review-process-and-design-quality.md) |
| 7 | [`documentation-knowledge-governance/06-adr-rfc-decision-logs-and-architecture-governance.md`](documentation-knowledge-governance/06-adr-rfc-decision-logs-and-architecture-governance.md) |
| 8 | [`16-practitioner-review-prompts-and-checklists.md`](16-practitioner-review-prompts-and-checklists.md) |

Primary outcome: guide boundaries, contracts, ownership, decision records and delivery governance with evidence, not opinion alone.

### Engineering Lead

| Sequence | Topic |
|---:|---|
| 1 | [`14-engineering-leadership-learning-roadmap.md`](14-engineering-leadership-learning-roadmap.md) |
| 2 | [`engineering-leadership-architecture-governance/README.md`](engineering-leadership-architecture-governance/README.md) |
| 3 | [`technical-knowledge-coverage-matrix.md`](technical-knowledge-coverage-matrix.md) |
| 4 | [`cicd-devsecops-release-evidence/15-cicd-review-checklists-and-operating-playbook.md`](cicd-devsecops-release-evidence/15-cicd-review-checklists-and-operating-playbook.md) |
| 5 | [`git-workflow-release-hygiene/16-git-workflow-checklists-and-engineering-lead-playbook.md`](git-workflow-release-hygiene/16-git-workflow-checklists-and-engineering-lead-playbook.md) |
| 6 | [`documentation-knowledge-governance/16-documentation-checklists-and-engineering-lead-playbook.md`](documentation-knowledge-governance/16-documentation-checklists-and-engineering-lead-playbook.md) |
| 7 | [`platform-productization-bank-buyable-readiness/16-readiness-scorecards-gap-analysis-and-improvement-roadmap.md`](platform-productization-bank-buyable-readiness/16-readiness-scorecards-gap-analysis-and-improvement-roadmap.md) |
| 8 | [`17-implementation-evidence-reading-map.md`](17-implementation-evidence-reading-map.md) |

Primary outcome: improve team judgement, review quality, release discipline, KT, operational readiness and architecture governance.

### AI Engineer Or AI Product Technologist

| Sequence | Topic |
|---:|---|
| 1 | [`ai-rag-copilot-agentic-governance/README.md`](ai-rag-copilot-agentic-governance/README.md) |
| 2 | [`ai-rag-copilot-agentic-governance/03-knowledge-source-governance-and-entitlement-aware-retrieval.md`](ai-rag-copilot-agentic-governance/03-knowledge-source-governance-and-entitlement-aware-retrieval.md) |
| 3 | [`ai-rag-copilot-agentic-governance/08-agentic-workflows-tool-use-authorization-and-action-safety.md`](ai-rag-copilot-agentic-governance/08-agentic-workflows-tool-use-authorization-and-action-safety.md) |
| 4 | [`ai-rag-copilot-agentic-governance/10-model-risk-evaluation-red-teaming-and-certification.md`](ai-rag-copilot-agentic-governance/10-model-risk-evaluation-red-teaming-and-certification.md) |
| 5 | [`security-cyber-resilience/13-ai-rag-agent-and-model-risk-security-controls.md`](security-cyber-resilience/13-ai-rag-agent-and-model-risk-security-controls.md) |
| 6 | [`testing-quality-certification/15-ai-rag-agent-and-generated-output-testing.md`](testing-quality-certification/15-ai-rag-agent-and-generated-output-testing.md) |

Primary outcome: build AI-assisted capabilities that are grounded, permission-aware, evaluated, observable, secure and governed.

### Migration Or Transformation Engineer

| Sequence | Topic |
|---:|---|
| 1 | [`migration-replatforming-cutover-engineering/README.md`](migration-replatforming-cutover-engineering/README.md) |
| 2 | [`migration-replatforming-cutover-engineering/04-data-mapping-transformation-lineage-and-quality-rules.md`](migration-replatforming-cutover-engineering/04-data-mapping-transformation-lineage-and-quality-rules.md) |
| 3 | [`migration-replatforming-cutover-engineering/05-reconciliation-strategy-controls-tolerances-and-exceptions.md`](migration-replatforming-cutover-engineering/05-reconciliation-strategy-controls-tolerances-and-exceptions.md) |
| 4 | [`migration-replatforming-cutover-engineering/07-cutover-strategy-command-center-and-go-no-go-governance.md`](migration-replatforming-cutover-engineering/07-cutover-strategy-command-center-and-go-no-go-governance.md) |
| 5 | [`migration-replatforming-cutover-engineering/14-testing-certification-dress-rehearsal-and-mock-cutover.md`](migration-replatforming-cutover-engineering/14-testing-certification-dress-rehearsal-and-mock-cutover.md) |
| 6 | [`migration-replatforming-cutover-engineering/15-operational-readiness-hypercare-and-post-migration-stabilization.md`](migration-replatforming-cutover-engineering/15-operational-readiness-hypercare-and-post-migration-stabilization.md) |

Primary outcome: plan and execute migrations with controlled scope, reconciled data, tested cutover, rollback posture, business readiness and supportable hypercare.

### Production Support Or Incident Lead

| Sequence | Topic |
|---:|---|
| 1 | [`production-support-operations-incident-excellence/README.md`](production-support-operations-incident-excellence/README.md) |
| 2 | [`production-support-operations-incident-excellence/03-sla-slo-ola-error-budget-and-service-health.md`](production-support-operations-incident-excellence/03-sla-slo-ola-error-budget-and-service-health.md) |
| 3 | [`production-support-operations-incident-excellence/04-monitoring-alerting-dashboard-and-telemetry-governance.md`](production-support-operations-incident-excellence/04-monitoring-alerting-dashboard-and-telemetry-governance.md) |
| 4 | [`production-support-operations-incident-excellence/05-incident-management-severity-triage-and-command-center.md`](production-support-operations-incident-excellence/05-incident-management-severity-triage-and-command-center.md) |
| 5 | [`production-support-operations-incident-excellence/07-runbooks-playbooks-safe-operations-and-operator-tooling.md`](production-support-operations-incident-excellence/07-runbooks-playbooks-safe-operations-and-operator-tooling.md) |
| 6 | [`production-support-operations-incident-excellence/09-problem-management-rca-postmortem-and-continuous-improvement.md`](production-support-operations-incident-excellence/09-problem-management-rca-postmortem-and-continuous-improvement.md) |

Primary outcome: operate critical services with clear ownership, actionable alerts, safe runbooks, disciplined incident response, business communication and durable follow-through.

### Reporting Or Document Platform Engineer

| Sequence | Topic |
|---:|---|
| 1 | [`reporting-document-evidence-engineering/README.md`](reporting-document-evidence-engineering/README.md) |
| 2 | [`reporting-document-evidence-engineering/03-report-data-snapshots-lineage-and-reproducibility.md`](reporting-document-evidence-engineering/03-report-data-snapshots-lineage-and-reproducibility.md) |
| 3 | [`reporting-document-evidence-engineering/05-template-layout-rendering-and-document-generation.md`](reporting-document-evidence-engineering/05-template-layout-rendering-and-document-generation.md) |
| 4 | [`reporting-document-evidence-engineering/07-report-evidence-audit-trail-hash-and-integrity-controls.md`](reporting-document-evidence-engineering/07-report-evidence-audit-trail-hash-and-integrity-controls.md) |
| 5 | [`reporting-document-evidence-engineering/12-observability-sre-runbooks-and-report-operations.md`](reporting-document-evidence-engineering/12-observability-sre-runbooks-and-report-operations.md) |
| 6 | [`performance-scalability-resilience-async/08-workers-queues-leasing-dead-letter-replay-and-recovery.md`](performance-scalability-resilience-async/08-workers-queues-leasing-dead-letter-replay-and-recovery.md) |

Primary outcome: build reporting flows that are reproducible, entitlement-aware, evidence-backed, operationally visible and resilient under batch or large-report demand.

## Project-Based Reading Paths

| Project need | Recommended path |
|---|---|
| Add a new domain API | Architecture boundaries -> backend service design -> API contract engineering -> testing certification -> observability supportability. |
| Add an async calculation or report job | Backend service design -> performance/async engineering -> observability supportability -> testing certification -> CI/CD release evidence. |
| Build an AI/RAG/copilot capability | AI/RAG/copilot governance -> data-product engineering -> API contract engineering -> security/cyber resilience -> testing certification -> observability supportability. |
| Build a reporting or document-evidence workflow | Reporting/document evidence -> performance/async engineering -> security/cyber resilience -> observability supportability -> testing certification. |
| Migrate or replatform a capability | Migration/replatforming -> data-product engineering -> API/integration migration -> security entitlement migration -> testing certification -> observability and hypercare. |
| Prepare a platform for enterprise buyer review | Platform productization -> security/cyber resilience -> documentation governance -> CI/CD release evidence -> observability/SRE -> testing certification. |
| Improve production support and incident response | Production support/incident excellence -> observability supportability -> performance/resilience -> runtime infrastructure -> documentation governance. |
| Improve a slow or unstable workflow | Performance/async engineering -> observability supportability -> runtime infrastructure -> testing certification. |
| Harden access control | Security/cyber resilience -> API contract engineering -> backend service design -> testing certification. |
| Prepare a production release | CI/CD release evidence -> Git workflow release hygiene -> observability/SRE -> documentation governance. |
| Clean up repository docs | Documentation governance -> implementation evidence map -> Git workflow release hygiene -> technical coverage matrix. |
| Build a data product | Data-product engineering -> API/event contract engineering -> security/cyber resilience -> observability supportability -> testing certification. |
| Review an architecture proposal | Architecture boundaries -> backend/API/data-product deep dives -> documentation governance ADR/RFC guidance -> practitioner review prompts. |

## Review Cadence

Use this guide as a living route map:

1. update it when a new technical deep dive is added,
2. adjust role paths when coverage moves from broad guide to deep-dive guide,
3. keep project paths focused on work engineers actually perform,
4. avoid linking to local repository names or private product branding,
5. prefer neutral concepts and implementation evidence.

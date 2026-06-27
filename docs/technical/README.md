# Technical Knowledge Base

This section is a practical engineering knowledge base for enterprise-grade wealth and banking platforms.

It translates patterns observed across a mature multi-application platform into neutral, reusable guidance. It is intentionally not tied to any one product name or repository. Use it for self-learning, KT, architecture review, onboarding, engineering leadership development, and delivery planning.

## Reading Path

| File | Use |
|---|---|
| [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md) | Understand domain services, experience APIs, shared capabilities, platform governance, boundary ownership and integration rules. |
| [`wealth-management-domain-engineering/`](wealth-management-domain-engineering/README.md) | Deep-dive into wealth-management technology domain concepts, client/account/portfolio hierarchy, holdings, transactions, performance, risk, advisory, reporting, data lineage, API boundaries, migration and supportability. |
| [`02-backend-application-structure.md`](02-backend-application-structure.md) | Learn how backend services should be organized into routers, services, domain modules, repositories, adapters, contracts and runtime composition. |
| [`backend-service-design/`](backend-service-design/README.md) | Deep-dive into backend service mission, boundaries, clean architecture, domain models, use cases, ports/adapters, persistence, events, errors, security, observability, testing and refactoring. |
| [`03-api-contract-governance.md`](03-api-contract-governance.md) | Apply API design, OpenAPI, vocabulary, error semantics, pagination, versioning and contract governance practices. |
| [`api-contract-engineering/`](api-contract-engineering/README.md) | Deep-dive into API-as-contract design, REST routes, BFF composition, OpenAPI quality gates, DTO boundaries, errors, pagination, idempotency, caller context, versioning, security, observability, testing and event integration. |
| [`04-data-mesh-and-data-product-engineering.md`](04-data-mesh-and-data-product-engineering.md) | Learn domain-owned data products, trust telemetry, source ownership, access policy, evidence policy and data-product certification. |
| [`data-product-engineering/`](data-product-engineering/README.md) | Deep-dive into data-product mindset, data mesh operating model, producer/consumer contracts, canonical vocabulary, lineage, freshness, quality, catalog publication, SLO/access/evidence policy, trust telemetry, certification, security, observability, testing and maturity. |
| [`05-ci-cd-quality-gates-and-release-governance.md`](05-ci-cd-quality-gates-and-release-governance.md) | Understand repository-native commands, lane-based CI, quality gates, release evidence, branch hygiene and merge discipline. |
| [`cicd-devsecops-release-evidence/`](cicd-devsecops-release-evidence/README.md) | Deep-dive into CI/CD as evidence production, validation lanes, local checks, PR and main gates, quality gates, test gates, contract governance, DevSecOps, workflow security, container parity, release evidence, branch hygiene and gate maturity. |
| [`06-observability-sre-and-operational-support.md`](06-observability-sre-and-operational-support.md) | Learn logging, metrics, tracing, health checks, SLOs, incident response, runbooks, supportability and operational evidence. |
| [`observability-sre-supportability/`](observability-sre-supportability/README.md) | Deep-dive into structured logging, metrics, tracing, health/readiness, supportability states, dashboards, SLOs, alerting, runbooks, incidents, diagnostics, async worker observability, trust telemetry, sensitive-data boundaries, CI gates, on-call and production readiness. |
| [`07-security-configuration-and-sensitive-data.md`](07-security-configuration-and-sensitive-data.md) | Apply secure configuration, secrets handling, least privilege, sensitive-data controls, workflow security and evidence filtering. |
| [`security-cyber-resilience/`](security-cyber-resilience/README.md) | Deep-dive into threat modelling, IAM, entitlements, object-level authorization, sensitive-data handling, API/runtime controls, DevSecOps, supply chain, detection, incident response, AI security, security testing and worked examples. |
| [`08-testing-performance-and-resilience.md`](08-testing-performance-and-resilience.md) | Understand test pyramid, meaningful coverage, contract tests, integration tests, E2E tests, performance hygiene and resilience patterns. |
| [`testing-quality-certification/`](testing-quality-certification/README.md) | Deep-dive into quality engineering, test pyramid design, domain/API/data/integration/E2E testing, regression, certification sweeps, synthetic data, security testing, observability testing, migration/runtime tests, performance/resilience tests, AI testing, CI lane placement and flakiness governance. |
| [`09-worked-examples-and-engineering-review-checklists.md`](09-worked-examples-and-engineering-review-checklists.md) | Use practical examples and checklists for API changes, new services, data products, production incidents and PR reviews. |
| [`10-infrastructure-containers-and-deployment.md`](10-infrastructure-containers-and-deployment.md) | Learn container design, local compose, orchestration, deployment strategy, environment configuration, migrations and runtime evidence. |
| [`runtime-infrastructure-kubernetes/`](runtime-infrastructure-kubernetes/README.md) | Deep-dive into runtime architecture, container image design, local runtime parity, Kubernetes/OpenShift/AKS fundamentals, workloads, ingress, configuration, probes, resources, dependencies, deployment, runtime security, observability, resilience and platform readiness. |
| [`11-git-release-and-knowledge-management.md`](11-git-release-and-knowledge-management.md) | Apply Git hygiene, PR evidence, release discipline, documentation truth and engineering knowledge-management practices. |
| [`git-workflow-release-hygiene/`](git-workflow-release-hygiene/README.md) | Deep-dive into Git as delivery memory, branching, commits, change slicing, PR evidence, review depth, CODEOWNERS, merge strategy, branch protection, release notes, hotfixes, generated artifacts, documentation truth, branch cleanup and audit traceability. |
| [`12-technology-stack-and-engineering-patterns.md`](12-technology-stack-and-engineering-patterns.md) | Understand common technology choices, toolchain patterns, tradeoffs and how to read a new repository. |
| [`13-frontend-experience-api-and-ui-supportability.md`](13-frontend-experience-api-and-ui-supportability.md) | Learn gateway-first UI delivery, state handling, view models, browser validation, accessibility and frontend supportability. |
| [`technical-knowledge-coverage-matrix.md`](technical-knowledge-coverage-matrix.md) | Track current technical KB coverage, strongest areas and next enrichment slices. |
| [`technical-learning-path-and-role-guide.md`](technical-learning-path-and-role-guide.md) | Choose reading paths by role, project type and engineering problem across the technical deep-dive packs. |
| [`technical-master-index-study-roadmap/`](technical-master-index-study-roadmap/README.md) | Use the master technical study roadmap for learning sequence, role-based paths, KT, repository adoption, architecture review, PR standards, production readiness, AI adoption, specialization maps, scorecards, evidence cadence and quarterly maturity planning. |
| [`14-engineering-leadership-learning-roadmap.md`](14-engineering-leadership-learning-roadmap.md) | Follow a 30/60/90-day self-learning and KT path for enterprise platform engineering leadership. |
| [`15-performance-scalability-resilience-and-async-work.md`](15-performance-scalability-resilience-and-async-work.md) | Deepen performance, timeout, retry, back-pressure, idempotency, async execution, recovery, caching and large-read design. |
| [`performance-scalability-resilience-async/`](performance-scalability-resilience-async/README.md) | Deep-dive into performance mindset, latency, throughput, saturation, API performance, timeouts, retries, circuit breakers, back-pressure, idempotency, async jobs, workers, queues, caching, batching, database performance, graceful degradation, autoscaling, SLOs, performance testing and recovery runbooks. |
| [`16-practitioner-review-prompts-and-checklists.md`](16-practitioner-review-prompts-and-checklists.md) | Use practitioner prompts and checklists for architecture, API, data, CI/CD, testing, security, observability and readiness reviews. |
| [`17-implementation-evidence-reading-map.md`](17-implementation-evidence-reading-map.md) | Learn where to find implementation evidence while keeping reusable documentation neutral and source-safe. |
| [`documentation-knowledge-governance/`](documentation-knowledge-governance/README.md) | Deep-dive into documentation as engineering infrastructure, knowledge-base taxonomy, source-controlled durable truth, README standards, architecture docs, ADRs/RFCs, API/data-product docs, runbooks, capability-status docs, diagrams, onboarding, documentation gates, sensitive-data handling, AI/RAG readiness and knowledge-maintenance lifecycle. |
| [`engineering-leadership-architecture-governance/`](engineering-leadership-architecture-governance/README.md) | Deep-dive into engineering leadership, architecture governance, technical strategy, service ownership, quality maturity, technical debt, production readiness, cross-team dependency governance, metrics, coaching, incident learning and operating rhythm. |
| [`ai-rag-copilot-agentic-governance/`](ai-rag-copilot-agentic-governance/README.md) | Deep-dive into enterprise AI capability design, RAG, copilots, agents, entitlement-aware retrieval, prompt safety, grounding, tool authorization, model routing, evaluation, AI security, observability and AI operating model. |
| [`reporting-document-evidence-engineering/`](reporting-document-evidence-engineering/README.md) | Deep-dive into report request lifecycle, snapshots, reproducibility, templates, rendering, archive, evidence, entitlements, APIs, batch reporting, degraded reports, observability, testing, migration and AI-assisted narratives. |
| [`platform-productization-bank-buyable-readiness/`](platform-productization-bank-buyable-readiness/README.md) | Deep-dive into platform productization, bank-buyable readiness, buyer evidence, reference architecture, security, data governance, support, quality certification, onboarding, commercial readiness, pilots and due diligence. |
| [`migration-replatforming-cutover-engineering/`](migration-replatforming-cutover-engineering/README.md) | Deep-dive into migration strategy, current and target state, inventory, mapping, reconciliation, parallel run, cutover, rollback, integration migration, infrastructure migration, entitlement migration, report continuity, testing, hypercare and migration governance. |
| [`production-support-operations-incident-excellence/`](production-support-operations-incident-excellence/README.md) | Deep-dive into production support operating model, service ownership, SLAs/SLOs, monitoring, alerting, incidents, communications, runbooks, on-call, problem management, operational risk, data operations, batch/job support, DR/BCP, AI operations and maturity scorecards. |

## Mental Model

Enterprise platform engineering is not only code delivery. It is the disciplined alignment of:

1. clear ownership boundaries,
2. explicit contracts,
3. deterministic business behavior,
4. high-signal tests,
5. secure configuration,
6. observable runtime behavior,
7. controlled release evidence,
8. truthful documentation,
9. supportable operations.

The strongest engineering systems make the correct path easy: one local command for quality, explicit API contracts, bounded logs and metrics, clear runbooks, and documentation that matches implementation truth.

## How To Use This Section

| Role | Useful starting point |
|---|---|
| Backend engineer | Files 02, 03, 05, 08, 09, 13 and the backend/API deep-dive packs. |
| Architect | Files 01, 03, 04, 06, 09, 12, 13 and the backend/API/data-product deep-dive packs. |
| DevOps / platform engineer | Files 05, 06, 07, 08, 10, 11, 12 and the CI/CD/runtime/observability deep-dive packs. |
| QA engineer | Files 03, 05, 08, 09 and 13. |
| Frontend engineer | Files 03, 06, 08, 09, 12 and 13. |
| AI engineer / AI product technologist | The AI/RAG/copilot guide plus API, data-product, security, testing, observability and documentation-governance deep dives. |
| Migration / transformation engineer | The migration/replatforming guide plus data-product, API, reporting, security, runtime, CI/CD, observability, testing and Git deep dives. |
| Production support / SRE engineer | The production-support guide plus observability, performance, runtime, security, testing, CI/CD and documentation-governance deep dives. |
| Reporting / platform product engineer | The reporting/document-evidence and platform-productization guides plus migration, production support, performance, CI/CD, security, observability and testing deep dives. |
| Engineering lead | Files 01, 04, 05, 06, 09, 10, 11, 12, 13, 14, 16, 17, the technical learning path, the engineering-leadership guide, the wealth-domain guide, the AI guide, the reporting guide, the productization guide, the migration guide, the production-support guide, the backend/API/data-product/CI-CD/runtime/observability/security/testing/Git/documentation-governance deep-dive packs and the coverage matrix. |
| New joiner | Read files 01 through 14 in order, then use the master roadmap, files 15 through 17 and the coverage matrix to choose deeper slices. |

## Relationship To Reference Packs

This section is the learning and implementation layer. For deeper background, cross-check:

1. [`../reference/wealth-platform-architecture/`](../reference/wealth-platform-architecture/README.md),
2. [`../reference/canonical-data-dictionary-api-event-domain-model/`](../reference/canonical-data-dictionary-api-event-domain-model/README.md),
3. [`../reference/enterprise-delivery-sdlc-devsecops-operations/`](../reference/enterprise-delivery-sdlc-devsecops-operations/README.md),
4. [`../reference/enterprise-cloud-kubernetes-platform-engineering/`](../reference/enterprise-cloud-kubernetes-platform-engineering/README.md),
5. [`../reference/enterprise-security-cyber-resilience-controls/`](../reference/enterprise-security-cyber-resilience-controls/README.md),
6. [`../reference/ai-decision-intelligence-wealth-platform/`](../reference/ai-decision-intelligence-wealth-platform/README.md).

## Curation Note

Curated from implementation patterns, standards, CI structure, documentation conventions and operating controls observed across a multi-application wealth-platform ecosystem. The content has been normalized into neutral practitioner guidance and avoids local paths, product names, private repository labels and implementation claims that are not generally reusable.

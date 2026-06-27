# Technical Knowledge Base

This section is a practical engineering knowledge base for enterprise-grade wealth and banking platforms.

It translates patterns observed across a mature multi-application platform into neutral, reusable guidance. It is intentionally not tied to any one product name or repository. Use it for self-learning, KT, architecture review, onboarding, engineering leadership development, and delivery planning.

## Reading Path

| File | Use |
|---|---|
| [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md) | Understand domain services, experience APIs, shared capabilities, platform governance, boundary ownership and integration rules. |
| [`02-backend-application-structure.md`](02-backend-application-structure.md) | Learn how backend services should be organized into routers, services, domain modules, repositories, adapters, contracts and runtime composition. |
| [`backend-service-design/`](backend-service-design/README.md) | Deep-dive into backend service mission, boundaries, clean architecture, domain models, use cases, ports/adapters, persistence, events, errors, security, observability, testing and refactoring. |
| [`03-api-contract-governance.md`](03-api-contract-governance.md) | Apply API design, OpenAPI, vocabulary, error semantics, pagination, versioning and contract governance practices. |
| [`api-contract-engineering/`](api-contract-engineering/README.md) | Deep-dive into API-as-contract design, REST routes, BFF composition, OpenAPI quality gates, DTO boundaries, errors, pagination, idempotency, caller context, versioning, security, observability, testing and event integration. |
| [`04-data-mesh-and-data-product-engineering.md`](04-data-mesh-and-data-product-engineering.md) | Learn domain-owned data products, trust telemetry, source ownership, access policy, evidence policy and data-product certification. |
| [`data-product-engineering/`](data-product-engineering/README.md) | Deep-dive into data-product mindset, data mesh operating model, producer/consumer contracts, canonical vocabulary, lineage, freshness, quality, catalog publication, SLO/access/evidence policy, trust telemetry, certification, security, observability, testing and maturity. |
| [`05-ci-cd-quality-gates-and-release-governance.md`](05-ci-cd-quality-gates-and-release-governance.md) | Understand repository-native commands, lane-based CI, quality gates, release evidence, branch hygiene and merge discipline. |
| [`cicd-devsecops-release-evidence/`](cicd-devsecops-release-evidence/README.md) | Deep-dive into CI/CD as evidence production, validation lanes, local checks, PR and main gates, quality gates, test gates, contract governance, DevSecOps, workflow security, container parity, release evidence, branch hygiene and gate maturity. |
| [`06-observability-sre-and-operational-support.md`](06-observability-sre-and-operational-support.md) | Learn logging, metrics, tracing, health checks, SLOs, incident response, runbooks, supportability and operational evidence. |
| [`07-security-configuration-and-sensitive-data.md`](07-security-configuration-and-sensitive-data.md) | Apply secure configuration, secrets handling, least privilege, sensitive-data controls, workflow security and evidence filtering. |
| [`08-testing-performance-and-resilience.md`](08-testing-performance-and-resilience.md) | Understand test pyramid, meaningful coverage, contract tests, integration tests, E2E tests, performance hygiene and resilience patterns. |
| [`09-worked-examples-and-engineering-review-checklists.md`](09-worked-examples-and-engineering-review-checklists.md) | Use practical examples and checklists for API changes, new services, data products, production incidents and PR reviews. |
| [`10-infrastructure-containers-and-deployment.md`](10-infrastructure-containers-and-deployment.md) | Learn container design, local compose, orchestration, deployment strategy, environment configuration, migrations and runtime evidence. |
| [`runtime-infrastructure-kubernetes/`](runtime-infrastructure-kubernetes/README.md) | Deep-dive into runtime architecture, container image design, local runtime parity, Kubernetes/OpenShift/AKS fundamentals, workloads, ingress, configuration, probes, resources, dependencies, deployment, runtime security, observability, resilience and platform readiness. |
| [`11-git-release-and-knowledge-management.md`](11-git-release-and-knowledge-management.md) | Apply Git hygiene, PR evidence, release discipline, documentation truth and engineering knowledge-management practices. |
| [`12-technology-stack-and-engineering-patterns.md`](12-technology-stack-and-engineering-patterns.md) | Understand common technology choices, toolchain patterns, tradeoffs and how to read a new repository. |
| [`13-frontend-experience-api-and-ui-supportability.md`](13-frontend-experience-api-and-ui-supportability.md) | Learn gateway-first UI delivery, state handling, view models, browser validation, accessibility and frontend supportability. |
| [`technical-knowledge-coverage-matrix.md`](technical-knowledge-coverage-matrix.md) | Track current technical KB coverage, strongest areas and next enrichment slices. |
| [`14-engineering-leadership-learning-roadmap.md`](14-engineering-leadership-learning-roadmap.md) | Follow a 30/60/90-day self-learning and KT path for enterprise platform engineering leadership. |
| [`15-performance-scalability-resilience-and-async-work.md`](15-performance-scalability-resilience-and-async-work.md) | Deepen performance, timeout, retry, back-pressure, idempotency, async execution, recovery, caching and large-read design. |
| [`16-practitioner-review-prompts-and-checklists.md`](16-practitioner-review-prompts-and-checklists.md) | Use practitioner prompts and checklists for architecture, API, data, CI/CD, testing, security, observability and readiness reviews. |
| [`17-implementation-evidence-reading-map.md`](17-implementation-evidence-reading-map.md) | Learn where to find implementation evidence while keeping reusable documentation neutral and source-safe. |

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
| DevOps / platform engineer | Files 05, 06, 07, 08, 10, 11, 12 and the CI/CD/runtime deep-dive packs. |
| QA engineer | Files 03, 05, 08, 09 and 13. |
| Frontend engineer | Files 03, 06, 08, 09, 12 and 13. |
| Engineering lead | Files 01, 04, 05, 06, 09, 10, 11, 12, 13, 14, 16, 17, the backend/API/data-product/CI-CD/runtime deep-dive packs and the coverage matrix. |
| New joiner | Read files 01 through 14 in order, then use files 15 through 17 and the coverage matrix to choose deeper slices. |

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

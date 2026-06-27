# Technical Knowledge Base

This section is a practical engineering knowledge base for enterprise-grade wealth and banking platforms.

It translates patterns observed across a mature multi-application platform into neutral, reusable guidance. It is intentionally not tied to any one product name or repository. Use it for self-learning, KT, architecture review, onboarding, engineering leadership development, and delivery planning.

## Reading Path

| File | Use |
|---|---|
| [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md) | Understand domain services, experience APIs, shared capabilities, platform governance, boundary ownership and integration rules. |
| [`02-backend-application-structure.md`](02-backend-application-structure.md) | Learn how backend services should be organized into routers, services, domain modules, repositories, adapters, contracts and runtime composition. |
| [`03-api-contract-governance.md`](03-api-contract-governance.md) | Apply API design, OpenAPI, vocabulary, error semantics, pagination, versioning and contract governance practices. |
| [`04-data-mesh-and-data-product-engineering.md`](04-data-mesh-and-data-product-engineering.md) | Learn domain-owned data products, trust telemetry, source ownership, access policy, evidence policy and data-product certification. |
| [`05-ci-cd-quality-gates-and-release-governance.md`](05-ci-cd-quality-gates-and-release-governance.md) | Understand repository-native commands, lane-based CI, quality gates, release evidence, branch hygiene and merge discipline. |
| [`06-observability-sre-and-operational-support.md`](06-observability-sre-and-operational-support.md) | Learn logging, metrics, tracing, health checks, SLOs, incident response, runbooks, supportability and operational evidence. |
| [`07-security-configuration-and-sensitive-data.md`](07-security-configuration-and-sensitive-data.md) | Apply secure configuration, secrets handling, least privilege, sensitive-data controls, workflow security and evidence filtering. |
| [`08-testing-performance-and-resilience.md`](08-testing-performance-and-resilience.md) | Understand test pyramid, meaningful coverage, contract tests, integration tests, E2E tests, performance hygiene and resilience patterns. |
| [`09-worked-examples-and-engineering-review-checklists.md`](09-worked-examples-and-engineering-review-checklists.md) | Use practical examples and checklists for API changes, new services, data products, production incidents and PR reviews. |

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
| Backend engineer | Files 02, 03, 05, 08 and 09. |
| Architect | Files 01, 03, 04, 06 and 09. |
| DevOps / platform engineer | Files 05, 06, 07 and 08. |
| QA engineer | Files 03, 05, 08 and 09. |
| Engineering lead | Files 01, 04, 05, 06 and 09. |
| New joiner | Read files 01 through 09 in order. |

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


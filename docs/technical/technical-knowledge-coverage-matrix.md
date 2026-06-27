# Technical Knowledge Coverage Matrix

This matrix tracks the technical knowledge-base coverage and the next enrichment areas. It is the technical companion to the product coverage matrix.

Use it to decide:

1. what to read first,
2. where the knowledge base is already strong,
3. what should be enriched next,
4. which area supports a project, KT session, architecture review or engineering-leadership goal.

## Coverage Matrix

| Technical area | Primary guide | Current coverage | Next enrichment slice |
|---|---|---|---|
| Architecture boundaries | [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md) | Strong coverage of domain services, experience APIs, product UI, shared capabilities, platform governance, source ownership, composition boundaries and architecture smells. | Add deeper examples for event ownership, batch ownership, shared library boundaries and multi-region ownership decisions. |
| Backend application structure | [`02-backend-application-structure.md`](02-backend-application-structure.md) | Strong coverage of routers, services, domain modules, repositories, integrations, contracts, runtime composition, configuration and background work. | Add deeper examples for async workers, repository transaction boundaries, service protocols and refactoring large modules. |
| Backend service design deep dive | [`backend-service-design/`](backend-service-design/README.md) | Strong deep-dive coverage of service mission, capability ownership, bounded contexts, clean architecture, domain models, use cases, ports/adapters, persistence, transactions, outbox, idempotency, errors, caller context, observability, tests, refactoring and service-design checklists. | Add implementation-backed case studies for refactoring controller-heavy services, extracting ports, adding outbox processing and splitting overloaded service boundaries. |
| API contract governance | [`03-api-contract-governance.md`](03-api-contract-governance.md) | Strong coverage of endpoint design, request/response governance, supportability, error semantics, pagination, vocabulary, OpenAPI and aliases. | Add deeper examples for backward-compatible API change, consumer-driven contract testing and deprecation plans. |
| API contract engineering deep dive | [`api-contract-engineering/`](api-contract-engineering/README.md) | Strong deep-dive coverage of API-as-contract thinking, REST route design, BFF composition, OpenAPI quality gates, DTO/domain boundaries, problem details, pagination, idempotency, caller context, versioning, security, observability, contract testing, event integration and API governance playbooks. | Add worked examples for API lifecycle migration, deprecation windows, generated-client compatibility, BFF degraded response certification and AsyncAPI event contracts. |
| Data mesh and data products | [`04-data-mesh-and-data-product-engineering.md`](04-data-mesh-and-data-product-engineering.md) | Strong coverage of data products, certification, trust telemetry, access policy, evidence policy, producer workflow and consumer workflow. | Add deeper examples for data-product SLOs, dependency graphs, consumer impact analysis and source outage playbooks. |
| Data-product engineering deep dive | [`data-product-engineering/`](data-product-engineering/README.md) | Strong deep-dive coverage of data-product mindset, domain ownership, producer and consumer contracts, canonical vocabulary, lineage, freshness, trust metadata, quality states, API/event/batch contracts, catalog publication, SLO/access/evidence policies, runtime telemetry, certification, security, observability, testing, onboarding and maturity. | Add implementation-backed case studies for legacy-feed migration, consumer-impact analysis, certification expiry, source outage fallback and AI/RAG consumer trust boundaries. |
| CI/CD and quality gates | [`05-ci-cd-quality-gates-and-release-governance.md`](05-ci-cd-quality-gates-and-release-governance.md) | Strong coverage of repo-native commands, lane model, good gate design, release evidence, branch hygiene, merge governance and CI anti-patterns. | Add deeper examples for gate promotion intake, flaky-check demotion, quality scorecards and release artifact review. |
| Observability and SRE | [`06-observability-sre-and-operational-support.md`](06-observability-sre-and-operational-support.md) | Strong coverage of structured logs, metrics, traces, health checks, SLO thinking, incident workflow, runbooks and supportability states. | Add deeper examples for alert design, dashboard review, incident severity classification and error-budget operation. |
| Security and sensitive data | [`07-security-configuration-and-sensitive-data.md`](07-security-configuration-and-sensitive-data.md) | Strong coverage of secure configuration, secrets, authorization, sensitive observability, CI security controls, workflow security and evidence filtering. | Add deeper examples for threat modelling, key rotation, privileged access, secure evidence packs and AI/tool-call security. |
| Testing, performance and resilience | [`08-testing-performance-and-resilience.md`](08-testing-performance-and-resilience.md) | Strong coverage of test pyramid, meaningful coverage, contract tests, integration tests, E2E tests, performance hygiene, resilience patterns and migration smoke. | Add deeper examples for load-test design, resilience drills, benchmark baselines and test-data design. |
| Worked examples and review checklists | [`09-worked-examples-and-engineering-review-checklists.md`](09-worked-examples-and-engineering-review-checklists.md) | Strong examples for new domain APIs, gateway composition, data-product certification, CI gate promotion, incident review, PR review and new-service readiness. | Add more case studies for migration failure, security incident, performance regression, frontend panel drift and data-product stale state. |
| Infrastructure and deployment | [`10-infrastructure-containers-and-deployment.md`](10-infrastructure-containers-and-deployment.md) | Strong coverage of runtime environment layers, container design, Dockerfile review, compose, orchestration, deployment strategy, configuration, migrations and evidence. | Add deeper examples for Helm/Kustomize-style deployment review, blue/green rollout, canary monitoring and backup/restore drills. |
| Git and knowledge management | [`11-git-release-and-knowledge-management.md`](11-git-release-and-knowledge-management.md) | Strong coverage of Git working model, commit quality, branch hygiene, PR evidence, review discipline, release hygiene, documentation layers and decision records. | Add deeper examples for ADR templates, release notes, branch reconciliation, wiki publication and knowledge-base stewardship. |
| Technology stack and toolchain | [`12-technology-stack-and-engineering-patterns.md`](12-technology-stack-and-engineering-patterns.md) | Strong coverage of backend APIs, contracts, persistence, clients, frontend, observability, testing, static quality, AI automation, tradeoffs and repository reading. | Add deeper examples for dependency upgrade governance, runtime version policy, library selection criteria and toolchain drift review. |
| Frontend and experience API supportability | [`13-frontend-experience-api-and-ui-supportability.md`](13-frontend-experience-api-and-ui-supportability.md) | Strong coverage of gateway-first UI delivery, UI state taxonomy, dense banking UI, view models, response shape, component boundaries, browser validation, accessibility and frontend observability. | Add deeper examples for UI regression testing, design-system governance, accessibility audits and dashboard performance budgets. |
| Engineering leadership learning path | [`14-engineering-leadership-learning-roadmap.md`](14-engineering-leadership-learning-roadmap.md) | Structured roadmap for building technical fluency, delivery judgement, operational depth, architecture review and leadership habits. | Add role-specific curricula for backend leads, frontend leads, platform leads, QA leads and architecture owners. |
| Performance, scalability, resilience and async work | [`15-performance-scalability-resilience-and-async-work.md`](15-performance-scalability-resilience-and-async-work.md) | Strong coverage of latency budgets, timeouts, retries, back pressure, idempotency, durable async execution, recovery, replay, caching, batching, pagination, performance gates and anti-patterns. | Add worked examples for report generation, portfolio analytics jobs, data ingestion replay, outbox publishing and browser performance budgets. |
| Practitioner review prompts | [`16-practitioner-review-prompts-and-checklists.md`](16-practitioner-review-prompts-and-checklists.md) | Strong reusable prompts for architecture, API, data products, CI/CD, testing, security, observability, production readiness and engineering-lead review. | Add role-specific review packs for backend, frontend, platform, QA, SRE and architecture leadership. |
| Implementation evidence reading | [`17-implementation-evidence-reading-map.md`](17-implementation-evidence-reading-map.md) | Strong map from neutral concepts to repository evidence areas, file types, repository roles and source-backed implementation patterns. | Add examples of how to convert a repository observation into neutral knowledge-base content. |

## Technical Coverage Dimensions

Every technical guide should be reviewed against these dimensions:

| Dimension | Standard |
|---|---|
| Concept clarity | Explains what the topic is, why it matters and where it fits. |
| Implementation pattern | Shows a practical structure or workflow, not only theory. |
| Evidence | Names the tests, gates, artifacts or runtime proof that validate the pattern. |
| Failure behavior | Explains degraded, partial, blocked, stale or error states where relevant. |
| Security and privacy | Identifies sensitive-data and access-control concerns. |
| Operations | Covers logs, metrics, traces, runbooks, rollback or support ownership where relevant. |
| Review checklist | Gives engineers a way to inspect and improve real work. |
| Anti-patterns | Makes common failure modes explicit. |

## Current Technical Strengths

The technical knowledge base now has strong coverage for:

1. architecture and service ownership,
2. backend service structure,
3. backend service-design depth,
4. API contract governance,
5. API contract-engineering depth,
6. data products and data mesh,
7. data-product engineering depth,
8. CI/CD lane model and quality gates,
9. observability and SRE supportability,
10. security and sensitive-data handling,
11. testing, performance and resilience,
12. infrastructure, containers and deployment,
13. Git, release and documentation truth,
14. technology stack reasoning,
15. frontend and experience-API supportability,
16. performance, resilience and async execution depth,
17. practical engineering review checklists,
18. implementation evidence reading.

## Next High-Value Slices

Future technical enrichment should prioritize:

1. API backward-compatible change and deprecation case studies,
2. incident severity, alert design and dashboard review examples,
3. threat-model and sensitive-evidence-pack examples,
4. migration failure and rollback case studies,
5. frontend panel drift and browser-regression case studies,
6. dependency upgrade and runtime-version governance,
7. async report-generation and data-ingestion replay examples,
8. ADR, release-note and branch-reconciliation templates,
9. role-specific learning paths for backend, frontend, platform, QA and architecture leads.

## Maintenance Rule

Update this matrix when:

1. a new technical guide is added,
2. a technical guide gains material examples,
3. a gap becomes important for project delivery,
4. a repeated engineering pattern should become durable guidance,
5. a topic moves from "needs enrichment" to "strong coverage."

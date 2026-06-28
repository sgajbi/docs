# Technical Knowledge Coverage Matrix

This matrix tracks the technical knowledge-base coverage and the next enrichment areas. It is the technical companion to the product coverage matrix.

Use it to decide:

1. what to read first,
2. where the knowledge base is already strong,
3. what should be enriched next,
4. which area supports a project, KT session, architecture review or engineering-leadership goal.

For a shorter operating view, use [`technical-library-polish-dashboard.md`](technical-library-polish-dashboard.md). Keep this matrix as the deeper coverage and enrichment tracker.

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
| CI/CD, DevSecOps and release evidence deep dive | [`cicd-devsecops-release-evidence/`](cicd-devsecops-release-evidence/README.md) | Strong deep-dive coverage of CI/CD as evidence production, validation lanes, local checks, PR merge gates, main releasability, quality gates, test gates, contract governance, DevSecOps, workflow security, container/runtime gates, release evidence, branch hygiene, gate promotion and operating playbooks. | Add examples for flaky-check triage, report-only gate ownership, release-evidence manifests, workflow-permission hardening and incident-driven gate improvements. |
| Observability and SRE | [`06-observability-sre-and-operational-support.md`](06-observability-sre-and-operational-support.md) | Strong coverage of structured logs, metrics, traces, health checks, SLO thinking, incident workflow, runbooks and supportability states. | Add deeper examples for alert design, dashboard review, incident severity classification and error-budget operation. |
| Observability, SRE and supportability deep dive | [`observability-sre-supportability/`](observability-sre-supportability/README.md) | Strong deep-dive coverage of observability as an operator-facing feature, structured logs, event taxonomy, correlation, metrics and cardinality, tracing, health/readiness, supportability states, dashboards, SLOs, alerting, runbooks, incidents, diagnostics APIs, async worker telemetry, data-product trust telemetry, sensitive-data boundaries, observability tests, CI gates, on-call and maturity. | Add implementation-backed examples for incident command flow, alert route tuning, dashboard acceptance checks, no-sensitive-telemetry gates and supportability-state certification. |
| Security and sensitive data | [`07-security-configuration-and-sensitive-data.md`](07-security-configuration-and-sensitive-data.md) | Strong coverage of secure configuration, secrets, authorization, sensitive observability, CI security controls, workflow security and evidence filtering. | Add deeper examples for key rotation, production-access recertification, security scorecards and control-owner operating cadence. |
| Security and cyber resilience deep dive | [`security-cyber-resilience/`](security-cyber-resilience/README.md) | Strong deep-dive coverage of security control modelling, threat modelling, IAM, entitlements, object-level authorization, privileged access, sensitive-data handling, secrets, evidence packs, API/application/runtime controls, DevSecOps, supply chain, release gates, detection, incident response, BCP/DR, AI/RAG/tool security, security tests, review checklists and worked examples. | Add implementation-backed examples for key rotation, privileged-access review, cloud policy-as-code violations, ransomware recovery drills, security-scorecard reporting and access-recertification evidence. |
| Testing, performance and resilience | [`08-testing-performance-and-resilience.md`](08-testing-performance-and-resilience.md) | Strong coverage of test pyramid, meaningful coverage, contract tests, integration tests, E2E tests, performance hygiene, resilience patterns and migration smoke. | Add deeper examples for load-test design, resilience drills, benchmark baselines and test-data design. |
| Testing, quality engineering and certification deep dive | [`testing-quality-certification/`](testing-quality-certification/README.md) | Strong deep-dive coverage of testing as evidence, quality engineering mindset, test pyramid and portfolio design, risk-based testing, unit/domain/policy/calculation testing, service-layer testing, API/OpenAPI/error behavior testing, data-product and event testing, integration testing, E2E/browser/runtime testing, regression, golden examples, certification sweeps, synthetic fixtures, security testing, observability testing, migration/runtime/deployment testing, performance/resilience/chaos testing, AI/RAG/agent testing, CI lane placement, quality metrics and flakiness governance. | Add implementation-backed examples for certification evidence manifests, flaky-test remediation, golden-case catalogue ownership, browser evidence packs and escaped-defect learning loops. |
| Worked examples and review checklists | [`09-worked-examples-and-engineering-review-checklists.md`](09-worked-examples-and-engineering-review-checklists.md) | Strong examples for new domain APIs, gateway composition, data-product certification, CI gate promotion, incident review, PR review and new-service readiness. | Add more case studies for migration failure, security incident, performance regression, frontend panel drift and data-product stale state. |
| Technical practice labs and case studies | [`technical-practice-labs-and-case-studies.md`](technical-practice-labs-and-case-studies.md) and [`technical-practice-lab-sample-answers.md`](technical-practice-lab-sample-answers.md) | Strong applied lab coverage for service boundary review, backward-compatible API change, data-product certification, CI gate promotion, incident and observability review, sensitive-data and access-control review, test strategy redesign, async workflow resilience, runtime deployment readiness, Git/release/documentation truth, AI/RAG governance, migration and cutover readiness, reporting/archive evidence and platform productization readiness. Includes source-safe sample answers for all 14 labs, covering evidence inspection, current-state assessment, risks, recommendations, smallest useful improvement, validation evidence and follow-up triggers. | Add deeper implementation-shaped answer variants for frontend panel drift, source outage response, release rollback, buyer due-diligence simulation and post-incident corrective-action tracking. |
| Infrastructure and deployment | [`10-infrastructure-containers-and-deployment.md`](10-infrastructure-containers-and-deployment.md) | Strong coverage of runtime environment layers, container design, Dockerfile review, compose, orchestration, deployment strategy, configuration, migrations and evidence. | Add deeper examples for Helm/Kustomize-style deployment review, blue/green rollout, canary monitoring and backup/restore drills. |
| Runtime infrastructure and Kubernetes deep dive | [`runtime-infrastructure-kubernetes/`](runtime-infrastructure-kubernetes/README.md) | Strong deep-dive coverage of runtime architecture, container images, Docker Compose parity, Kubernetes/OpenShift/AKS concepts, workload types, service addressing, ingress, configuration, secrets, health probes, resources, autoscaling, stateful dependencies, deployment, rollback, runtime security, observability, resilience, DR, cost and platform readiness. | Add examples for Helm/Kustomize review, canary rollout monitoring, workload identity adoption, capacity incident response and backup/restore validation. |
| Git and knowledge management | [`11-git-release-and-knowledge-management.md`](11-git-release-and-knowledge-management.md) | Strong coverage of Git working model, commit quality, branch hygiene, PR evidence, review discipline, release hygiene, documentation layers and decision records. | Add deeper examples for ADR templates, release notes, branch reconciliation, wiki publication and knowledge-base stewardship. |
| Git workflow, PR review and release hygiene deep dive | [`git-workflow-release-hygiene/`](git-workflow-release-hygiene/README.md) | Strong deep-dive coverage of Git as delivery memory, branch strategy, commit discipline, change slicing, feature branch delivery, PR structure and evidence, code review responsibilities, ownership routing, CODEOWNERS, merge strategies, protected main, required checks, release tags, changelogs, hotfixes, generated artifacts, documentation truth, post-merge branch cleanup, release-line maintenance, audit traceability and engineering-lead playbooks. | Add implementation-backed examples for branch reconciliation, PR evidence scoring, release-note generation, hotfix postmortems, stale-branch cleanup and CODEOWNERS-sensitive-path reviews. |
| Technology stack and toolchain | [`12-technology-stack-and-engineering-patterns.md`](12-technology-stack-and-engineering-patterns.md) | Strong coverage of backend APIs, contracts, persistence, clients, frontend, observability, testing, static quality, AI automation, tradeoffs and repository reading. | Add deeper examples for dependency upgrade governance, runtime version policy, library selection criteria and toolchain drift review. |
| Frontend and experience API supportability | [`13-frontend-experience-api-and-ui-supportability.md`](13-frontend-experience-api-and-ui-supportability.md) | Strong coverage of gateway-first UI delivery, UI state taxonomy, dense banking UI, view models, response shape, component boundaries, browser validation, accessibility and frontend observability. | Add deeper examples for UI regression testing, design-system governance, accessibility audits and dashboard performance budgets. |
| Technical learning path and role guide | [`technical-learning-path-and-role-guide.md`](technical-learning-path-and-role-guide.md) | Strong role-based and project-based routing across backend, API, frontend, QA, SRE, DevOps, platform, architecture and engineering-lead learning paths. | Add team-specific curricula when recurring project roles require more granular onboarding plans. |
| Technical master index and study roadmap | [`technical-master-index-study-roadmap/`](technical-master-index-study-roadmap/README.md) | Strong coverage of master technical navigation, top-level fast-start routing, linked guide routing, scenario-based execution routing, recommended study sequence, role-based curricula, KT, repository adoption, repo maturity staging, durable truth artifacts, adoption PR patterns, architecture review, PR standards, production readiness, operational excellence, migration/reporting/domain/platform specialization, AI adoption, quick path selection, quarterly maturity checkpoints, personal learning scorecards, evidence cadence, study-to-evidence loops, knowledge-to-action conversion, roadmap operating model, practitioner readiness artifact matrices, maturity review rubric, practical learning outputs, curation guardrails, source-material intake, duplicate-avoidance workflow, evidence artifacts, teach-back prompts and execution playbooks. | Keep aligned with new technical deep dives and add implementation-backed maturity examples as the library grows. |
| Engineering leadership learning path | [`14-engineering-leadership-learning-roadmap.md`](14-engineering-leadership-learning-roadmap.md) | Structured roadmap for building technical fluency, delivery judgement, operational depth, architecture review and leadership habits. | Add role-specific curricula for backend leads, frontend leads, platform leads, QA leads and architecture owners. |
| Performance, scalability, resilience and async work | [`15-performance-scalability-resilience-and-async-work.md`](15-performance-scalability-resilience-and-async-work.md) | Strong coverage of latency budgets, timeouts, retries, back pressure, idempotency, durable async execution, recovery, replay, caching, batching, pagination, performance gates and anti-patterns. | Add worked examples for report generation, portfolio analytics jobs, data ingestion replay, outbox publishing and browser performance budgets. |
| Performance, scalability, resilience and async engineering deep dive | [`performance-scalability-resilience-async/`](performance-scalability-resilience-async/README.md) | Strong deep-dive coverage of performance mindset, latency, throughput, saturation, capacity, API performance, pagination, filtering, large result sets, timeouts, retries, backoff, jitter, circuit breakers, back-pressure, load shedding, rate limiting, bulkheads, idempotency, concurrency, duplicate prevention, async execution, job state, polling, workers, queues, leasing, dead-letter handling, replay, recovery, caching, materialized views, batching, streaming, database performance, graceful degradation, resource limits, autoscaling, SLOs, performance dashboards, load/stress/soak/chaos testing and recovery playbooks. | Add implementation-backed examples for portfolio analytics jobs, report-generation queues, source ingestion replay, database query tuning, cache invalidation incidents, autoscaling reviews and capacity planning scorecards. |
| Practitioner review prompts | [`16-practitioner-review-prompts-and-checklists.md`](16-practitioner-review-prompts-and-checklists.md) | Strong reusable prompts for architecture, API, data products, CI/CD, testing, security, observability, production readiness and engineering-lead review. | Add role-specific review packs for backend, frontend, platform, QA, SRE and architecture leadership. |
| Implementation evidence reading | [`17-implementation-evidence-reading-map.md`](17-implementation-evidence-reading-map.md) | Strong map from neutral concepts to repository evidence areas, file types, repository roles and source-backed implementation patterns. | Add examples of how to convert a repository observation into neutral knowledge-base content. |
| Documentation, knowledge management and architecture governance deep dive | [`documentation-knowledge-governance/`](documentation-knowledge-governance/README.md) | Strong deep-dive coverage of documentation as engineering infrastructure, knowledge-base taxonomy, information architecture, durable truth, source-controlled docs, wiki publication, README and repository navigation, architecture documentation, ADRs, RFCs, decision logs, architecture governance, API/data-product/contract documentation, runbooks, SRE documentation, supported-feature and capability-status maps, evidence maps, diagrams, KT, onboarding, learning paths, documentation quality gates, sensitive-data handling, AI-assisted documentation, RAG readiness, knowledge maintenance and engineering-lead playbooks. | Add implementation-backed examples for ADR lifecycle, stale-doc detection, wiki publication evidence, capability-status review, diagram review, onboarding certification and RAG-ready metadata. |
| Engineering leadership and architecture governance deep dive | [`engineering-leadership-architecture-governance/`](engineering-leadership-architecture-governance/README.md) | Strong deep-dive coverage of engineering leadership operating model, architect decision rights, technical strategy, service ownership, architecture review, ADR/RFC governance, quality maturity, technical debt, production readiness, dependency governance, stakeholder communication, metrics, coaching, incident learning, AI-assisted engineering and operating rhythm. | Add implementation-backed examples for quarterly architecture reviews, scorecard calibration, production-readiness sign-off, technical-debt tradeoff decisions and leadership communication packs. |
| AI, RAG, copilot and agentic engineering governance deep dive | [`ai-rag-copilot-agentic-governance/`](ai-rag-copilot-agentic-governance/README.md) | Strong deep-dive coverage of AI capability models, use-case risk tiering, RAG, knowledge-source governance, embeddings, prompt templates, grounding, copilot UX, agent tool use, model gateways, model-risk evaluation, AI security, entitlement-aware retrieval, observability, testing, AI-assisted engineering and AI operating model. | Add implementation-backed examples for retrieval evaluation datasets, prompt-injection regression, tool-call authorization, AI incident triage, model-router fallback and human-review workflow evidence. |
| Wealth management technology domain engineering deep dive | [`wealth-management-domain-engineering/`](wealth-management-domain-engineering/README.md) | Strong deep-dive coverage of wealth-platform domain concepts, client/account/portfolio hierarchy, holdings, transactions, valuation, market data, performance, risk, advisory, reporting, data sourcing, temporal patterns, API/service/data-product boundaries, integration, entitlements, operations, migration and golden scenarios. | Add implementation-backed examples for portfolio hierarchy migration, performance restatement, market-data outage handling, advisory suitability evidence and domain API boundary review. |
| Reporting, document generation, archive and evidence engineering deep dive | [`reporting-document-evidence-engineering/`](reporting-document-evidence-engineering/README.md) | Strong deep-dive coverage of reporting capability model, report jobs, idempotency, snapshots, lineage, reproducibility, report sections, templates, rendering, archive, retention, evidence, document integrity, entitlements, APIs, UI states, batch generation, degraded reports, operations, testing, migration and AI narratives. | Add implementation-backed examples for report-generation queues, golden report diffs, archive continuity migration, degraded-section warnings, document hash validation and support dashboard review. |
| Platform productization and bank-buyable software readiness deep dive | [`platform-productization-bank-buyable-readiness/`](platform-productization-bank-buyable-readiness/README.md) | Strong deep-dive coverage of productization mindset, enterprise buyer expectations, supported-feature evidence, reference architecture, deployment, integration, security, privacy, data governance, support, quality certification, documentation, onboarding, commercial readiness, customer implementation, roadmap, AI feature readiness, demos, POCs, third-party risk and readiness scorecards. | Add implementation-backed examples for buyer evidence packs, supported-feature matrices, procurement Q&A responses, synthetic demo environments, customer onboarding checklists and bank-buyable readiness scoring. |
| Migration, replatforming, reconciliation and cutover engineering deep dive | [`migration-replatforming-cutover-engineering/`](migration-replatforming-cutover-engineering/README.md) | Strong deep-dive coverage of migration mindset, current and target state, transition architecture, inventory, dependencies, wave planning, data mapping, reconciliation, parallel run, cutover, rollback, integration migration, infrastructure migration, entitlement migration, report/archive continuity, calculation continuity, testing, mock cutover, hypercare, stakeholder readiness and migration governance. | Add implementation-backed examples for reconciliation tolerances, parallel-run comparison packs, cutover command centers, rollback decision trees, entitlement migration tests, report archive continuity and post-migration stabilization dashboards. |
| Production support, operations and incident excellence deep dive | [`production-support-operations-incident-excellence/`](production-support-operations-incident-excellence/README.md) | Strong deep-dive coverage of production support operating model, service ownership, support boundaries, SLAs/SLOs/OLAs, monitoring, alerting, dashboards, incident command, communications, runbooks, on-call, problem management, release readiness, operational risk, data operations, batch/job operations, security/privacy incidents, DR/BCP, AI operations, metrics and maturity rhythm. | Add implementation-backed examples for severity triage, alert-to-runbook governance, incident communication, batch replay, data freshness incidents, DR drill evidence and post-incident corrective action tracking. |

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
9. CI/CD, DevSecOps and release-evidence depth,
10. observability and SRE supportability,
11. observability, SRE and operational-supportability depth,
12. security and sensitive-data handling,
13. security and cyber-resilience engineering depth,
14. testing, performance and resilience,
15. testing, quality engineering and certification depth,
16. infrastructure, containers and deployment,
17. runtime infrastructure and Kubernetes depth,
18. Git, release and documentation truth,
19. Git workflow, PR review and release hygiene depth,
20. technology stack reasoning,
21. frontend and experience-API supportability,
22. technical learning path and role-based navigation,
23. technical master index and study roadmap,
24. technical practice labs, case studies and sample answers,
25. performance, resilience and async execution depth,
26. performance, scalability, resilience and async engineering deep dive,
27. practical engineering review checklists,
28. implementation evidence reading,
29. documentation, knowledge management and architecture governance depth,
30. engineering leadership and architecture governance depth,
31. AI, RAG, copilot and agentic engineering governance depth,
32. wealth-management technology domain engineering depth,
33. reporting, document generation, archive and evidence engineering depth,
34. platform productization and bank-buyable software readiness depth,
35. migration, replatforming, reconciliation and cutover engineering depth,
36. production support, operations and incident excellence depth.

## Next High-Value Slices

Future technical enrichment should prioritize:

1. deeper implementation-shaped answer variants for frontend panel drift, source outage response, release rollback, buyer due-diligence simulation and post-incident corrective-action tracking,
2. API backward-compatible change and deprecation case studies,
3. incident severity, alert design and dashboard review examples,
4. key-rotation, privileged-access and security-scorecard examples,
5. migration failure and rollback case studies,
6. frontend panel drift and browser-regression case studies,
7. dependency upgrade and runtime-version governance,
8. implementation-backed async report-generation and data-ingestion replay examples,
9. implementation-backed documentation governance examples,
10. role-specific learning paths for backend, frontend, platform, QA and architecture leads,
11. AI evaluation, retrieval quality, prompt-injection and tool-authorization evidence examples,
12. reporting evidence, archive continuity, golden report and degraded-report case studies,
13. wealth-domain migration, restatement, suitability and supportability case studies,
14. platform productization scorecards, procurement evidence packs and bank-buyable readiness examples,
15. migration reconciliation, parallel-run, cutover, rollback and hypercare case studies,
16. incident command, production-support, DR drill, batch recovery and operational-risk evidence case studies.

## Maintenance Rule

Update this matrix when:

1. a new technical guide is added,
2. a technical guide gains material examples,
3. a gap becomes important for project delivery,
4. a repeated engineering pattern should become durable guidance,
5. a topic moves from "needs enrichment" to "strong coverage."

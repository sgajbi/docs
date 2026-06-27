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
| Understand the platform shape | [`01-architecture-and-service-boundaries.md`](01-architecture-and-service-boundaries.md) |
| Understand backend service structure | [`backend-service-design/README.md`](backend-service-design/README.md) |
| Design or review APIs | [`api-contract-engineering/README.md`](api-contract-engineering/README.md) |
| Work with data products | [`data-product-engineering/README.md`](data-product-engineering/README.md) |
| Prepare a PR or release | [`cicd-devsecops-release-evidence/README.md`](cicd-devsecops-release-evidence/README.md) |
| Support production | [`observability-sre-supportability/README.md`](observability-sre-supportability/README.md) |
| Review security controls | [`security-cyber-resilience/README.md`](security-cyber-resilience/README.md) |
| Improve testing strategy | [`testing-quality-certification/README.md`](testing-quality-certification/README.md) |
| Review Git and PR hygiene | [`git-workflow-release-hygiene/README.md`](git-workflow-release-hygiene/README.md) |
| Handle performance or async work | [`performance-scalability-resilience-async/README.md`](performance-scalability-resilience-async/README.md) |
| Improve documentation and KT | [`documentation-knowledge-governance/README.md`](documentation-knowledge-governance/README.md) |

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
| 5 | [`documentation-knowledge-governance/06-adr-rfc-decision-logs-and-architecture-governance.md`](documentation-knowledge-governance/06-adr-rfc-decision-logs-and-architecture-governance.md) |
| 6 | [`16-practitioner-review-prompts-and-checklists.md`](16-practitioner-review-prompts-and-checklists.md) |

Primary outcome: guide boundaries, contracts, ownership, decision records and delivery governance with evidence, not opinion alone.

### Engineering Lead

| Sequence | Topic |
|---:|---|
| 1 | [`14-engineering-leadership-learning-roadmap.md`](14-engineering-leadership-learning-roadmap.md) |
| 2 | [`technical-knowledge-coverage-matrix.md`](technical-knowledge-coverage-matrix.md) |
| 3 | [`cicd-devsecops-release-evidence/15-cicd-review-checklists-and-operating-playbook.md`](cicd-devsecops-release-evidence/15-cicd-review-checklists-and-operating-playbook.md) |
| 4 | [`git-workflow-release-hygiene/16-git-workflow-checklists-and-engineering-lead-playbook.md`](git-workflow-release-hygiene/16-git-workflow-checklists-and-engineering-lead-playbook.md) |
| 5 | [`documentation-knowledge-governance/16-documentation-checklists-and-engineering-lead-playbook.md`](documentation-knowledge-governance/16-documentation-checklists-and-engineering-lead-playbook.md) |
| 6 | [`17-implementation-evidence-reading-map.md`](17-implementation-evidence-reading-map.md) |

Primary outcome: improve team judgement, review quality, release discipline, KT, operational readiness and architecture governance.

## Project-Based Reading Paths

| Project need | Recommended path |
|---|---|
| Add a new domain API | Architecture boundaries -> backend service design -> API contract engineering -> testing certification -> observability supportability. |
| Add an async calculation or report job | Backend service design -> performance/async engineering -> observability supportability -> testing certification -> CI/CD release evidence. |
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

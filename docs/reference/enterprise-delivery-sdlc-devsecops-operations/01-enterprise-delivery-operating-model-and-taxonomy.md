# Enterprise Delivery Operating Model and Taxonomy

## 1. Executive summary

A wealth platform operating model defines how business capability moves from idea to production and then into controlled support.

In a private-banking or wealth-management environment, delivery must satisfy multiple goals at once:

| Goal | Meaning |
|---|---|
| Business value | The change must solve a real advisory, reporting, analytics, operations or client problem. |
| Domain correctness | Product, transaction, cashflow, valuation, performance and risk behaviour must be correct. |
| Control compliance | Suitability, entitlement, audit, data lineage, security and regulatory controls must hold. |
| Engineering quality | Code must be maintainable, tested, observable, secure and scalable. |
| Operational readiness | Support teams must know how to monitor, triage and recover the service. |
| Release safety | Deployment must be reversible, measurable and coordinated with upstream/downstream systems. |

The operating model should not be a bureaucracy that blocks delivery. It should be a system that makes delivery **repeatable, evidence-backed and safe**.

## 2. Wealth-platform delivery value chain

A practical value chain:

```text
Strategy / portfolio demand
  -> product discovery
  -> business analysis
  -> architecture and ADRs
  -> backlog refinement
  -> engineering implementation
  -> automated testing
  -> security and quality gates
  -> integration testing
  -> business validation
  -> release approval
  -> deployment
  -> monitoring
  -> incident/problem/change management
  -> continuous improvement
```

Each stage should create reusable evidence rather than one-off meeting notes.

## 3. Main operating layers

| Layer | Scope | Typical artefacts |
|---|---|---|
| Business capability | What business outcome is delivered | Business case, target state, capability map |
| Product management | What features and workflows are needed | Product backlog, roadmap, personas |
| Business analysis | What rules, scenarios and data are needed | User stories, acceptance criteria, decision tables |
| Architecture | How it fits the platform | ADRs, sequence diagrams, API/event contracts |
| Engineering | How it is built | Code, tests, CI results, PR reviews |
| Quality engineering | How confidence is built | Test strategy, automation, regression evidence |
| Security | How risk is controlled | threat model, SAST/DAST/SCA, secrets checks |
| Release management | How it safely reaches production | release note, deployment plan, rollback plan |
| Operations | How it runs safely | dashboards, alerts, runbooks, SLOs |
| Governance | How decisions are controlled | approvals, risk acceptance, audit evidence |

## 4. Delivery roles

| Role | Responsibilities |
|---|---|
| Product owner | Prioritizes business outcomes, validates feature value, owns product roadmap. |
| Business analyst / SME | Defines requirements, rules, examples, edge cases and business validation. |
| Architect | Owns target architecture, domain boundaries, integration patterns, ADRs and NFRs. |
| Engineering lead | Owns implementation design, code quality, CI/CD, technical delivery and engineering standards. |
| Developers | Build, test, document, review and support code. |
| QA / quality engineer | Designs test strategy, automation, regression and acceptance evidence. |
| DevOps / platform engineer | Owns pipelines, deployment, infrastructure, monitoring and operational tooling. |
| Security / risk | Reviews secure design, vulnerabilities, secrets, access, threat model and residual risk. |
| Operations / support | Monitors production, manages incidents, breaks and service restoration. |
| Release manager | Coordinates release readiness, dependencies, approvals and deployment windows. |
| Data owner / steward | Owns data meaning, quality, lineage and source-of-truth decisions. |
| Compliance / legal / risk governance | Reviews regulated workflows, disclosures, client impact and control obligations. |

## 5. Bank-grade delivery attributes

A release is bank-grade when it is:

| Attribute | Evidence |
|---|---|
| Traceable | Requirements link to code, tests, releases and production evidence. |
| Controlled | Approvals, exceptions and risk acceptances are recorded. |
| Testable | Acceptance criteria are automated where possible and manually evidenced where needed. |
| Secure | Secure design, dependency checks, secrets controls and vulnerability triage are complete. |
| Observable | Metrics, logs, traces, dashboards and alerts exist before go-live. |
| Reversible | Rollback, feature flag or compensating action exists. |
| Supportable | Runbooks, error codes, known issues and escalation paths are documented. |
| Explainable | Business users can understand what changed and why. |
| Auditable | The release can be reconstructed from evidence. |
| Sustainable | The design does not create unacceptable technical debt or operations load. |

## 6. Wealth-domain delivery examples

### Example 1: Portfolio transaction API performance improvement

A slow transaction API is not just a technical problem. It affects:

- portfolio review UI responsiveness
- advisor workflow
- client reporting generation
- performance calculation input
- auditability of transaction history
- pagination and filtering semantics
- database index and query design
- user expectation around completeness

Evidence should include API contract changes, pagination decision, database plan improvement, latency test, regression test, UI test and migration/backward-compatibility notes.

### Example 2: Structured note lifecycle support

A note lifecycle enhancement must coordinate:

- product master terms
- observation schedule
- coupon/redemption transaction types
- position lifecycle status
- valuation source
- reporting disclosure
- performance classification
- operations exception handling

This cannot be delivered safely by coding only the UI or only the transaction ingestion. It requires front-to-back lifecycle design.

### Example 3: AI-assisted portfolio-review explanation

An AI-generated explanation needs:

- approved knowledge source
- data permission check
- current portfolio snapshot
- calculation lineage
- prompt version
- answer citation or grounding
- human review workflow
- output archive
- prohibited advice guardrails

AI delivery needs SDLC, model-risk, data-governance and operational controls together.

## 7. Common anti-patterns

| Anti-pattern | Why it fails |
|---|---|
| Build-first delivery | Code is produced before rules, data ownership and test scenarios are understood. |
| UI-only definition of done | UI appears complete while API, reporting, reconciliation and operations are incomplete. |
| Manual evidence after the fact | Audit evidence is reconstructed late and becomes unreliable. |
| Over-centralized architecture review | Architects become bottlenecks instead of enabling decision clarity. |
| No source ownership | Data issues cannot be resolved because no team owns the field. |
| Release without runbook | Support cannot triage production issues quickly. |
| AI-generated code without review | Speed increases while control, security and correctness decrease. |
| "Temporary" workarounds | Exceptions become permanent because no expiry or remediation owner exists. |

## 8. Target maturity model

| Level | Description |
|---|---|
| 1. Ad hoc | Delivery depends on individuals and undocumented knowledge. |
| 2. Repeatable | Teams have templates, checklists and basic CI. |
| 3. Controlled | Quality gates, release evidence, monitoring and runbooks are standardized. |
| 4. Measurable | DORA, defect, incident and service metrics drive improvement. |
| 5. Intelligent | AI-assisted engineering, automated evidence, predictive risk and self-service controls improve delivery speed and quality. |

## 9. Practical operating model for your knowledge base

Each future product or platform pack should include:

- product/domain explanation
- lifecycle and transaction model
- position and data model
- valuation and risk treatment
- advisory and mandate treatment
- reporting implications
- source ownership
- QA scenarios
- controls
- implementation boundaries
- AI-assistance opportunities
- operating/runbook considerations

## 10. Practitioner explanation

A strong answer:

> "In wealth platforms, delivery excellence means more than Agile ceremonies or CI/CD. It means controlled delivery of business capability across product, data, calculation, reporting, advisory, compliance and operations. I look for traceability from requirement to code, tests, releases and production evidence; strong source ownership and lineage; secure DevSecOps; clear rollback and runbooks; and measurable improvement using service and delivery metrics. That is how we make complex wealth platforms safe to change."

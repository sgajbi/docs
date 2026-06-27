# Glossary, Practitioner Reference and Source Notes

## 1. Glossary

| Term | Meaning |
|---|---|
| SDLC | Software Development Lifecycle; the controlled process for planning, building, testing, releasing and operating software. |
| DevSecOps | Integration of development, security and operations into the delivery lifecycle. |
| CI | Continuous Integration; automated build/test checks after code changes. |
| CD | Continuous Delivery or Deployment; controlled movement of software toward production. |
| SAST | Static Application Security Testing. |
| DAST | Dynamic Application Security Testing. |
| SCA | Software Composition Analysis for dependency vulnerabilities. |
| SBOM | Software Bill of Materials; inventory of software components. |
| SLSA | Supply-chain Levels for Software Artifacts; guidance for software supply-chain security. |
| ADR | Architecture Decision Record. |
| NFR | Non-Functional Requirement. |
| SLO | Service Level Objective. |
| SLI | Service Level Indicator. |
| Error budget | Allowable unreliability under an SLO. |
| Runbook | Operational guide for support and recovery. |
| Playbook | Repeatable operational procedure, often broader than a runbook. |
| Postmortem | Incident review focused on learning and prevention. |
| Golden test | Deterministic test case with known expected result. |
| Feature flag | Runtime switch to enable/disable functionality. |
| Canary release | Progressive rollout to a small subset before wider release. |
| Blue-green deployment | Deployment pattern using parallel old/new environments. |
| Risk acceptance | Formal approval to proceed with residual risk. |
| Control evidence | Artefacts proving a control operated as expected. |
| Toil | Repetitive manual operational work that should be automated. |
| Prompt regression | Test set for validating AI prompt/model output stability. |
| Agent guardrail | Constraint controlling AI tool use, data access or action. |

## 2. practitioner questions and answers

### What does bank-grade delivery mean?

> Bank-grade delivery means controlled delivery of business capability, not just software deployment. It requires traceable requirements, secure SDLC, tested product behaviour, data lineage, release evidence, observability, rollback, runbooks, support ownership and auditability. In wealth platforms, this also means product, transaction, valuation, performance, suitability, mandate and reporting behaviour must be correct and explainable.

### How do you balance speed and control?

> I use automation and smaller changes to improve both speed and safety. CI/CD, automated tests, contract testing, feature flags, monitoring and repeatable release evidence reduce manual gates while preserving control. The aim is not to remove governance, but to make governance embedded and evidence-driven.

### How would you use AI in engineering delivery?

> I would use AI to accelerate documentation, test generation, refactoring, code explanation, runbooks and incident summaries. But AI-generated output must follow normal SDLC controls: feature branch, PR review, CI tests, security scans, domain validation and audit evidence. AI is an assistant, not the accountable decision maker.

### What are your release-readiness checks?

> I check scope, dependencies, API/event changes, data migration, test evidence, security scan status, performance results, rollback plan, feature flags, monitoring, runbooks, support readiness, known issues, stakeholder communications and post-deploy validation.

### What is production readiness?

> Production readiness means the team can operate the feature safely after deployment. It includes observability, alerts, runbooks, SLOs, support ownership, incident triage, data correction/replay procedures, rollback, stakeholder communication and post-incident learning.

### What makes testing different in wealth platforms?

> Wealth-platform testing must be domain-aware. We need golden scenarios for products, transactions, valuation, cashflows, corporate actions, performance, attribution, reporting, suitability and mandates. Generic code tests are not enough because the real risk is often business-rule or data-treatment error.

## 3. practitioner checklist

### Minimum release evidence

- story/requirement links
- design/ADR links
- code PRs
- CI results
- test report
- security scan report
- API/event contract validation
- data migration evidence
- performance evidence where relevant
- release note
- deployment plan
- rollback plan
- runbook update
- monitoring/dashboard link
- approval record

### Minimum production dashboard

- service availability
- request latency
- error rate
- dependency health
- job duration
- queue lag
- data freshness
- reconciliation breaks
- business exception count
- feature flag state
- deployment version

### Minimum AI-assisted engineering controls

- approved context sources
- no client secrets/PII in prompts
- feature branch only
- small changes
- tests required
- human review required
- security scans required
- generated dependencies justified
- documentation verified
- audit evidence retained where needed

## 4. Source notes and further reading

Use these sources as reference anchors when extending the pack:

| Source | Why it is useful |
|---|---|
| NIST SP 800-218 Secure Software Development Framework | Secure SDLC practices and common vocabulary for software producers and purchasers. |
| OWASP SAMM | Software assurance maturity model for evaluating and improving security posture. |
| DORA software delivery performance metrics | Delivery-flow and stability metrics for continuous improvement. |
| SLSA | Software supply-chain security, provenance and artifact-trust concepts. |
| CIS Controls | Prioritized cybersecurity controls and cyber-hygiene practices. |
| ISO/IEC 27001 | Information security management system standard. |
| OpenTelemetry | Telemetry standard for traces, metrics and logs. |
| ITIL 4 | Service-management practices and operating-model vocabulary. |
| NIST AI RMF and GenAI Profile | AI governance, risk and trustworthiness for AI-assisted delivery. |
| OWASP Top 10 for LLM Applications | LLM and agent security risks such as prompt injection and excessive agency. |

## 5. Suggested next packs

After this pack, useful next knowledge packs would be:

1. **Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems**
2. **Cybersecurity, IAM, Secrets, Data Protection and Threat Modelling for Wealth Platforms**
3. **Vendor, Procurement, Third-Party Risk and Bank-Buyable Software Controls**
4. **Wealth Platform Roadmap, Product Management and Commercialization**
5. **Office Knowledge Sharing, Training Curriculum and project preparation Master Pack**

## 6. Final mental model

```text
Domain knowledge tells us what is correct.
Architecture tells us how it should be structured.
SDLC tells us how it should be built.
DevSecOps tells us how it should be secured and released.
Operations tells us how it should run.
Controls tell us how it should be trusted.
AI tells us how this can be accelerated without losing accountability.
```

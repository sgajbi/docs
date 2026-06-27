# Wealth Platform Controls, Compliance, Audit and Risk Operations

## 1. Control mindset

Bank-grade software needs controls by design.

Controls should not be after-the-fact paperwork. They should be built into:

- requirements
- architecture
- data model
- APIs
- workflows
- CI/CD
- release management
- operations
- reporting
- audit evidence

## 2. Control categories

| Category | Examples |
|---|---|
| Access control | RBAC, ABAC, booking-centre scope, data entitlements. |
| Data control | validation, lineage, source ownership, reconciliation. |
| Security control | encryption, secrets, vulnerability management, secure coding. |
| Change control | approval, segregation, release evidence, rollback. |
| Operational control | monitoring, incident management, runbooks. |
| Advisory control | suitability, disclosures, product approval, consent. |
| Mandate control | investment restrictions, concentration, liquidity, leverage. |
| Reporting control | snapshot versioning, disclaimer, report archive. |
| Audit control | traceability, immutable evidence, reviewer records. |
| AI control | grounding, guardrails, human approval, prompt/model lineage. |

## 3. Control design pattern

A good control has:

- purpose
- owner
- trigger
- rule
- evidence
- exception process
- monitoring
- review frequency
- remediation owner
- audit trail

Example:

| Field | Example |
|---|---|
| Control | Suitability pre-trade check |
| Purpose | Prevent unsuitable product recommendation |
| Owner | Advisory controls |
| Trigger | Proposal creation/order submission |
| Rule | Product risk rating must not exceed client profile unless override permitted |
| Evidence | decision snapshot, rule version, product data, user, timestamp |
| Exception | supervisor approval with reason |
| Monitoring | override volume dashboard |
| Review | monthly |

## 4. Segregation of duties

Common SoD requirements:

| Activity | Should be separated from |
|---|---|
| Code authoring | Production approval |
| Business rule approval | Code implementation |
| Data correction request | Data correction approval |
| Product approval | Product sale/recommendation |
| Suitability override | Original recommendation |
| Production access | Change approval |
| Model/prompt design | Model/prompt independent validation where required |

## 5. Audit evidence

Evidence should be generated naturally by the workflow.

Examples:

- story and acceptance criteria
- ADR
- PR review
- CI logs
- security scan
- test report
- release approval
- deployment record
- config/version snapshot
- runbook version
- incident timeline
- user approval
- suitability decision
- report archive
- AI prompt/model/retrieval version

## 6. Risk acceptance

Not every risk can be eliminated. Risk acceptance should record:

- risk description
- impact
- likelihood
- mitigation
- residual risk
- owner
- approval
- expiry/review date
- remediation plan

Never leave risk acceptance open-ended.

## 7. Compliance-by-design in workflows

Examples:

| Workflow | Control points |
|---|---|
| Advisory proposal | product approval, target market, suitability, disclosure, consent. |
| DPM rebalance | mandate rules, drift, liquidity, concentration, pre-trade checks. |
| Report generation | data freshness, calculation lineage, disclosure, archive. |
| Data correction | request, approval, before/after, reconciliation, audit. |
| AI explanation | grounding, access control, output policy, human review. |
| Production hotfix | emergency approval, post-implementation review, regression. |

## 8. Data privacy

Privacy controls:

- data minimization
- role-based access
- purpose limitation
- masking/tokenization
- secure transport/storage
- retention and deletion policy
- cross-border transfer control
- audit access
- breach response
- privacy impact assessment for new workflows

## 9. Operational risk

Operational risk examples:

| Risk | Example control |
|---|---|
| Manual data correction error | four-eye approval and reconciliation. |
| Missed corporate action | event feed monitoring and exception queue. |
| Wrong client statement | report QA, source lineage and archive. |
| Unauthorized data access | ABAC and audit logs. |
| Failed release | rollback and post-deploy checks. |
| Calculation restatement | versioned results and restatement workflow. |
| AI hallucination | RAG grounding and human review. |

## 10. Compliance with architecture

Architecture controls:

- approved patterns
- ADR process
- API governance
- event schema governance
- data ownership
- non-functional requirements
- security architecture review
- cloud/Kubernetes standards
- observability requirements
- deprecation policy

## 11. Evidence repository pattern

A release evidence pack:

```text
release-2026-07/
  scope.md
  risk-assessment.md
  adr-links.md
  api-contracts/
  test-results/
  security-scans/
  migration-evidence/
  release-approval.md
  deployment-log.md
  rollback-plan.md
  post-deploy-checks.md
  known-issues.md
```

## 12. Control testing

Controls should be tested like features.

Examples:

- unauthorized advisor cannot view client from another booking centre
- suitability block cannot be bypassed by direct API call
- stale price creates report exception
- AI agent cannot call order-submission tool without approval
- data correction requires approval and creates audit trail
- release cannot proceed with critical vulnerability unless risk accepted

## 13. Audit project preparation

Strong audit response:

> "For this capability, requirement, design, implementation, test and release evidence are traceable. The control is enforced in the workflow and through API-level validation. Exceptions require approval and are logged with rule version and data snapshot. Production monitoring and reconciliation validate that the control remains effective after release."

## 14. Common control failures

| Failure | Consequence |
|---|---|
| Control only in UI | API bypass possible. |
| No rule version | Cannot explain historical decision. |
| No data snapshot | Decision cannot be reconstructed. |
| No exception expiry | Temporary waiver becomes permanent. |
| Manual spreadsheet evidence | Hard to trust and reproduce. |
| Missing owner | Breaks cannot be resolved. |
| No monitoring | Control failure discovered too late. |

## 15. Practitioner checklist

- [ ] Controls identified during requirements
- [ ] Control owner assigned
- [ ] Evidence generated automatically where possible
- [ ] Exceptions require approval
- [ ] Rule versions stored
- [ ] Data snapshots stored where needed
- [ ] API-level enforcement exists
- [ ] Monitoring exists
- [ ] Test scenarios include control failures
- [ ] Audit trail is searchable

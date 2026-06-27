# Change, Release, Deployment and Environment Management

## 1. Purpose

Change and release management ensure that production changes are delivered safely, with appropriate control, evidence, communication and recovery options.

In bank-grade systems, release governance must balance:

- speed
- stability
- regulatory/control needs
- dependency coordination
- operational readiness
- auditability

## 2. Change types

| Change type | Description | Example |
|---|---|---|
| Standard change | Pre-approved, low-risk, repeatable | routine certificate renewal. |
| Normal change | Planned release requiring approval | new portfolio report section. |
| Emergency change | Urgent fix for production issue | incorrect valuation fix. |
| Data change | Correction or migration of data | corporate action repair. |
| Configuration change | Runtime setting change | feature flag activation. |
| Infrastructure change | Platform or environment update | Kubernetes version upgrade. |
| Security change | Vulnerability remediation | patch critical library. |

## 3. Release types

| Release type | Example |
|---|---|
| Feature release | New advisory workflow. |
| Technical release | Refactoring or framework upgrade. |
| Regulatory release | Reporting disclosure change. |
| Migration release | Country onboarding or data cutover. |
| Hotfix | Production defect repair. |
| Data release | Historical data correction. |
| AI model/prompt release | Prompt, retrieval corpus or evaluator update. |

## 4. Release readiness artefacts

A release should include:

- release scope
- change list
- impacted services
- impacted countries/booking centres
- product/domain impact
- API/event changes
- data-model changes
- database migration plan
- feature flag plan
- test evidence
- security scan status
- performance evidence
- reconciliation evidence
- known issues
- rollback plan
- post-deploy validation
- support/runbook updates
- communication plan
- approval record

## 5. Deployment strategies

| Strategy | Use case | Notes |
|---|---|---|
| Rolling deployment | Stateless services | Needs backward compatibility. |
| Blue-green | Safer cutover | Requires duplicate environment capacity. |
| Canary | Progressive exposure | Good for controlled rollout. |
| Feature flag | Decouple deploy from release | Must manage flag lifecycle. |
| Shadow mode | Compare new logic without user impact | Useful for calculations. |
| Dark launch | Deploy hidden capability | Useful for performance validation. |
| Big-bang | Rare, high coordination | Sometimes unavoidable for migrations. |

## 6. Environment strategy

Typical environments:

| Environment | Purpose |
|---|---|
| Local/dev | Developer feedback. |
| CI | Automated test execution. |
| Integration | Service interaction testing. |
| System test | End-to-end functional testing. |
| Performance | Load and scalability testing. |
| UAT | Business validation. |
| Pre-prod | Production-like release rehearsal. |
| Production | Live clients/users. |
| DR | Disaster recovery validation. |

Environment controls:

- production-like data shape
- masked data where required
- controlled secrets
- environment parity
- version tracking
- test-data management
- access governance
- refresh schedule
- configuration drift detection

## 7. Configuration management

Configuration should be:

- externalized
- versioned
- environment-specific
- auditable
- validated at startup
- protected by access controls
- observable through safe endpoints
- not mixed with secrets

Examples:

- pricing source priority
- stale-price threshold
- report generation cut-off time
- feature flags
- region activation
- API timeout
- retry count
- circuit breaker settings

## 8. Rollback and roll-forward

Rollback plan should define:

- what can be rolled back
- what cannot be rolled back
- database migration rollback or compensation
- feature flag disablement
- data correction strategy
- expected downtime
- validation steps
- decision owner

Roll-forward is often safer when database/data transformations are involved, but it must still be controlled.

## 9. Data changes and backfills

Data changes need special controls:

- source of correction
- approval owner
- affected population
- before/after counts
- sample validation
- reconciliation
- rollback/compensation
- client/report impact
- audit record
- communication plan

Example: correcting historical transaction classification affects performance, realized P&L and client reporting. It cannot be treated like a simple technical patch.

## 10. Release calendar

A wealth platform release calendar should consider:

- market holidays
- month-end reporting
- quarter-end reporting
- tax reporting windows
- performance statement cycles
- client campaign windows
- trading freezes
- country rollout dates
- upstream/downstream releases
- regulatory deadlines

Avoid risky changes during critical reporting periods unless required.

## 11. Communication model

Release communication should include:

| Audience | Information needed |
|---|---|
| Advisors | workflow changes, known limitations, client impact. |
| Operations | controls, runbooks, exception handling. |
| Support | symptoms, dashboards, escalation paths. |
| Compliance/risk | control changes, disclosures, approvals. |
| Business sponsors | value delivered, risks, adoption plan. |
| Engineering | technical changes, dependencies, rollback. |

## 12. Change approval board

CAB should focus on risk and readiness, not ceremony.

Questions:

- What business process is impacted?
- What could fail?
- What is the client impact?
- How was it tested?
- How will we detect issues?
- How will we recover?
- What dependencies exist?
- Is risk accepted or mitigated?
- Is there an emergency contact?

## 13. AI release considerations

AI-related releases include:

- model version change
- prompt version change
- retrieval corpus change
- tool/action permission change
- evaluator change
- guardrail change
- policy update
- agent workflow update

These require:

- evaluation evidence
- regression prompt set
- safety testing
- access-control testing
- output lineage
- human-oversight workflow
- rollback to prior prompt/model/corpus

## 14. Release anti-patterns

| Anti-pattern | Risk |
|---|---|
| Deploying without business validation | Incorrect workflow reaches users. |
| No rollback plan | Incident recovery delayed. |
| Schema-breaking change | Rolling deployment fails. |
| Environment drift | UAT success does not predict production. |
| Uncontrolled feature flags | Behaviour changes without audit. |
| Manual database fix | Data lineage lost. |
| Releases during reporting freeze | Client reports impacted. |
| AI prompt change without evaluation | Output behaviour changes silently. |

## 15. Practitioner checklist

- [ ] Release scope is clear
- [ ] Impacted users and services identified
- [ ] Testing complete
- [ ] Security/vulnerability status accepted
- [ ] Database migration plan ready
- [ ] Feature flags documented
- [ ] Rollback/compensation plan ready
- [ ] Monitoring and alerts active
- [ ] Runbooks updated
- [ ] Support handover complete
- [ ] Communications sent
- [ ] Post-deploy checks defined
- [ ] Evidence archived

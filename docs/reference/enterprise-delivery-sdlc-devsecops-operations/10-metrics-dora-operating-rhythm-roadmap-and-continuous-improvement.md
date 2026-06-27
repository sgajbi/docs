# Metrics, DORA, Operating Rhythm, Roadmap and Continuous Improvement

## 1. Why metrics matter

Metrics help teams improve, but only when used carefully.

In enterprise wealth delivery, metrics should measure:

- delivery flow
- quality
- reliability
- security
- data health
- business impact
- operational load
- engineering sustainability

Metrics should not become targets that teams game.

## 2. Delivery metrics

| Metric | Meaning |
|---|---|
| Lead time for change | Time from code committed to production. |
| Deployment frequency | How often changes reach production. |
| Change fail rate | Share of deployments causing production intervention. |
| Failed deployment recovery time | Time to recover from failed deployment. |
| Deployment rework rate | Share of deployments that are unplanned rework/hotfix. |

These should be interpreted by service/context, not blindly compared across very different platforms.

## 3. Wealth-domain quality metrics

| Metric | Meaning |
|---|---|
| Escaped defects | Defects found after release. |
| Calculation breaks | Performance/valuation/accounting defects. |
| Reconciliation breaks | Cash/position/report mismatch. |
| Report exceptions | Client statements generated with degraded data. |
| Data-quality score | Completeness/validity/timeliness metrics. |
| UAT defect density | Business defects during acceptance. |
| Regression pass rate | Confidence in release. |
| Test flakiness | Trustworthiness of automation. |
| Vulnerability SLA | Time to fix CVEs. |
| Runbook coverage | Operational readiness. |

## 4. Reliability metrics

| Metric | Example |
|---|---|
| Availability | Portfolio API uptime. |
| Latency | p95/p99 report generation latency. |
| Error rate | failed valuation calls. |
| Batch completion | EOD batch completed before cut-off. |
| Queue lag | transaction ingestion lag. |
| Retry rate | downstream instability indicator. |
| Alert noise | alerts without action. |
| MTTA | mean time to acknowledge. |
| MTTR | mean time to restore. |
| Incident recurrence | problem-management effectiveness. |

## 5. Security and control metrics

| Metric | Purpose |
|---|---|
| Critical vulnerabilities open | Security exposure. |
| Mean time to remediate | Response effectiveness. |
| Secrets findings | Developer hygiene. |
| Privileged access exceptions | Access risk. |
| Failed access attempts | Possible abuse or configuration issue. |
| Suitability overrides | Advisory control monitoring. |
| Mandate breaches | Portfolio control health. |
| Risk acceptances past due | Governance discipline. |
| Audit evidence completeness | Release control maturity. |

## 6. Business-value metrics

Delivery should also measure business impact:

- advisor time saved
- report generation time reduced
- order rejection reduced
- manual breaks reduced
- data corrections reduced
- client complaints reduced
- onboarding time reduced
- migration sign-off faster
- production incidents reduced
- support toil reduced

## 7. Operating rhythm

| Cadence | Meeting / review | Purpose |
|---|---|---|
| Daily | Delivery sync | blockers, risk, dependency. |
| Weekly | Architecture/design sync | ADRs, cross-domain decisions. |
| Weekly | Quality review | defects, test gaps, data issues. |
| Fortnightly | Sprint review | business outcome validation. |
| Monthly | Service review | incidents, SLOs, capacity, risks. |
| Monthly | Product roadmap review | priorities and outcomes. |
| Quarterly | Architecture governance | platform direction and technical debt. |
| Quarterly | Control review | audit, risk, compliance, evidence. |

## 8. Roadmap structure

Roadmap should include:

| Lane | Example |
|---|---|
| Business features | portfolio review enhancements. |
| Product coverage | private markets lifecycle. |
| Analytics | attribution, benchmark, risk. |
| Platform | API performance, event architecture. |
| Data | source ownership, data-quality controls. |
| Security | secrets rotation, dependency upgrades. |
| Operations | runbooks, dashboards, incident reduction. |
| AI | advisor copilot, QA agent, documentation agent. |
| Migration | country rollout, legacy decommission. |

## 9. Technical debt management

Technical debt should be visible and prioritized.

Debt types:

- code complexity
- architecture boundary violation
- missing tests
- performance bottleneck
- weak observability
- manual operation
- undocumented business rule
- unsupported source data
- deprecated dependency
- stale feature flag
- missing lineage

Debt record fields:

- description
- impact
- affected service
- risk level
- remediation option
- owner
- target date
- linked incidents/defects

## 10. Continuous improvement loop

```text
Measure
  -> learn
  -> prioritize
  -> change
  -> validate
  -> standardize
```

Examples:

| Finding | Improvement |
|---|---|
| repeated stale-price incidents | add data freshness dashboard and report guardrail. |
| transaction API latency high | add keyset pagination and database indexes. |
| recurring release rollback | improve pre-prod rehearsal and feature flags. |
| AI documentation has false claims | require implementation-backed source verification. |
| support spends hours triaging | improve runbooks and error codes. |

## 11. Team health

Healthy delivery requires:

- manageable WIP
- clear ownership
- psychological safety
- learning culture
- sustainable on-call
- time for refactoring
- time for documentation
- good tooling
- low alert noise
- clear prioritization

## 12. Management reporting

Executive status should show:

- outcomes delivered
- risks and mitigations
- release health
- incident trend
- data-quality trend
- security posture
- roadmap confidence
- decisions needed

Avoid reporting only activity counts.

## 13. AI and metrics

AI can help:

- summarize metrics
- detect trend changes
- cluster incidents
- identify regression gaps
- draft release notes
- propose remediation backlog
- generate management status
- detect quality drift

But metric interpretation should remain human-owned.

## 14. Common metric mistakes

| Mistake | Better approach |
|---|---|
| Comparing all teams by same deployment frequency | Compare within context and over time. |
| Treating velocity as productivity | Measure outcomes and flow. |
| Rewarding low defect count | May discourage reporting. |
| Ignoring toil | Manual work silently consumes capacity. |
| Tracking too many metrics | Focus on decisions metrics support. |
| No action from metrics | Measurement without improvement wastes time. |

## 15. Practitioner explanation

> "I would use metrics as a continuous-improvement system, not a policing tool. DORA-style delivery metrics help measure flow and stability, but in wealth platforms I would add domain quality metrics such as reconciliation breaks, report exceptions, calculation defects, stale-data incidents, suitability overrides and production restatements. The goal is to connect engineering performance with business reliability and client trust."

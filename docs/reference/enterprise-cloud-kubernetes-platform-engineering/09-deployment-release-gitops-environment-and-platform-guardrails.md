<!--
Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems Reference Pack
Generated: 2026-06-27
Purpose: Professional study, architecture, platform design, project delivery, advisory/reporting/analytics integration, and future reference.
Disclaimer: Educational and architecture reference only. Not cloud, security, regulatory, legal, or investment advice.
-->

# 09 - Deployment, Release, GitOps, Environment and Platform Guardrails

## 1. Deployment principle

Production deployment should be boring, repeatable, auditable and reversible.

A good platform makes the safe path the easiest path.

## 2. Environment model

| Environment | Purpose |
|---|---|
| local | developer feedback |
| dev | integration of feature branches |
| test/SIT | system integration |
| UAT | business validation |
| pre-prod | production-like validation |
| prod | live service |
| DR | recovery capability |
| sandbox | controlled experimentation |
| AI eval | model/prompt/RAG evaluation |

## 3. Environment parity

Differences are unavoidable, but dangerous differences must be controlled:

- network topology
- IAM/entitlements
- market data access
- secrets
- database size
- source feed realism
- data masking
- time zone and business calendar
- cluster resource limits
- observability setup
- feature flags

## 4. CI/CD pipeline

Typical pipeline stages:

```text
commit
  -> lint
  -> unit tests
  -> security scan
  -> build
  -> image scan
  -> SBOM
  -> integration tests
  -> contract tests
  -> package/sign
  -> deploy to lower environment
  -> smoke tests
  -> regression tests
  -> approval
  -> production deployment
  -> post-deploy validation
```

## 5. GitOps

GitOps uses Git as the declarative source of truth for environment configuration.

Benefits:

- versioned deployment manifests
- peer review for environment changes
- drift detection
- rollback via Git history
- audit-friendly promotion
- standardization across applications

Controls:

- protected branches
- code owners
- environment overlays
- sealed secrets / external secrets
- policy checks
- automated sync with manual gates for prod
- drift alerts

## 6. Deployment strategies

| Strategy | Use |
|---|---|
| Rolling update | default stateless service deployment |
| Blue-green | low-risk switch between versions |
| Canary | release to small percentage first |
| Shadow | observe new version without serving user |
| Feature flags | decouple deploy and release |
| Dark launch | enable backend capability before UI |
| Batch cutover | scheduled switch for EOD/reporting |
| Emergency rollback | restore prior known-good version |

## 7. Wealth-specific deployment controls

| Capability | Release control |
|---|---|
| Performance engine | golden regression portfolios |
| Reporting | pixel/layout and data reconciliation tests |
| Buying power | scenario-based collateral/exposure checks |
| Advisory | suitability decision regression |
| Market data | price/FX freshness tests |
| AI copilot | groundedness, toxicity, leakage and tool tests |
| Transactions API | pagination and large-history performance tests |
| Portfolio API | holdings/cash/valuation reconciliation |

## 8. Release readiness

A production release should include:

- release notes
- business impact assessment
- deployment plan
- rollback plan
- database migration plan
- feature flags
- test evidence
- security scan results
- performance test results
- operational dashboard
- runbook updates
- support handover
- stakeholder communication
- post-deploy validation checklist

## 9. Database migrations

Rules:

- backward compatible where possible
- expand/contract pattern
- avoid long locks
- test on production-sized data
- include rollback or remediation plan
- version migrations
- never manually patch prod without audit
- validate row counts and checksums
- support dual-read/write if needed during migration

## 10. Feature flags

Use feature flags for:

- country rollout
- client segment rollout
- advisor group rollout
- new calculation method
- new report section
- new AI use case
- new data source
- new API response field

Controls:

- owner
- expiry date
- default state
- environment state
- audit log
- kill switch
- test coverage for both states

## 11. Platform guardrails

Enforce:

- approved base images
- required labels
- required resource requests/limits
- non-root containers
- no privileged pods
- required probes
- required network policy
- required observability
- no public endpoint unless approved
- no mutable image tags in prod
- external secret integration
- deployment window controls
- production change audit

## 12. Practitioner summary

A strong answer:

> In bank-grade Kubernetes platforms, release management must combine CI/CD, GitOps, policy-as-code, environment parity, progressive delivery, rollback readiness and post-deploy validation. For wealth systems, generic deployment success is not enough; the release must prove financial calculations, reports, entitlements, suitability and source-data behavior remain correct.

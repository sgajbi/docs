# Release Evidence, Production Readiness, and Change Control

## Purpose

This file explains what release evidence should contain and how production readiness should be assessed.

Release evidence helps teams make controlled deployment decisions and support production after deployment.

---

## Release Evidence Package

A release package should include:

- release scope
- changed services
- PRs and commits
- build artifact references
- image digests
- API/event/data-product changes
- database migrations
- configuration changes
- feature flags
- test summary
- security scan summary
- SBOM/provenance
- known issues
- rollback plan
- post-deploy verification
- monitoring links
- runbook links
- support contacts
- residual risks

---

## Production Readiness

Production readiness asks:

- Can it deploy?
- Can it start?
- Can it serve traffic?
- Can it fail safely?
- Can it be observed?
- Can it be rolled back?
- Can it be supported?
- Can it be audited?

---

## Change Control

Change control should capture:

- business impact
- technical impact
- risk level
- validation evidence
- implementation window
- rollback plan
- approvers
- communications
- post-change checks

Change control should be evidence-based, not purely form-based.

---

## Post-Deploy Verification

Verify:

- health/readiness
- key APIs
- error rate
- latency
- dependency health
- logs
- metrics
- dashboards
- alerts
- migration status
- feature flags
- critical workflows

---

## Rollback Plan

A rollback plan should define:

- artifact to roll back to
- database rollback/compensation
- configuration rollback
- feature flag rollback
- data correction implications
- communication path
- validation after rollback
- owner

---

## Release Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Release notes written from memory | Missing evidence. |
| No rollback plan | Incident duration increases. |
| Migrations not reversible or compensated | Deployment risk. |
| Security findings ignored | Risk accepted silently. |
| Monitoring not updated | Failure invisible. |
| Support team not briefed | Slow incident response. |
| Release artifact not tied to commit | Traceability gap. |

---

## Review Checklist

- Is release scope clear?
- Are commits and PRs listed?
- Are artifacts traceable?
- Are tests summarized?
- Are security scans triaged?
- Are migrations documented?
- Is rollback clear?
- Are dashboards/runbooks updated?
- Are support contacts defined?
- Are known risks recorded?
- Are post-deploy checks defined?

---

## Summary

Release evidence is the handoff from engineering to production operation.

A release is not complete until it can be deployed, verified, supported, and rolled back.

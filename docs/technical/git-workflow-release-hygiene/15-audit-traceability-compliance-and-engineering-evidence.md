# Audit, Traceability, Compliance, and Engineering Evidence

## Purpose

This file explains how Git workflow supports auditability and regulated SDLC evidence.

The goal is not paperwork. The goal is traceable, reviewable, evidence-backed delivery.

---

## Traceability Chain

A healthy traceability chain:

```text
requirement / issue / RFC
  -> branch
  -> commits
  -> pull request
  -> review
  -> CI checks
  -> artifacts
  -> release tag
  -> deployment
  -> runbook / support evidence
```

---

## Evidence Sources

Evidence can include:

- issue/story
- architecture decision
- PR description
- review approvals
- CI results
- test reports
- security scans
- dependency scans
- SBOM/provenance
- generated certification output
- release notes
- deployment record
- incident/problem review
- runbook update

---

## Compliance-Friendly Practices

- protected main
- required review
- required status checks
- signed tags/commits where required
- CODEOWNERS for sensitive areas
- release tagging
- artifact traceability
- documented hotfix workflow
- evidence retention
- access control on repositories
- secret scanning
- audit logs

---

## Audit Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Change merged with no PR | Review evidence missing. |
| PR lacks validation evidence | Audit gap. |
| Release tag missing | Deployment traceability weak. |
| CI artifacts not retained | Evidence unavailable. |
| Hotfix not documented | Control weakness. |
| Security exception not recorded | Hidden residual risk. |
| Manual evidence recreated later | Inaccurate evidence. |

---

## Review Checklist

- Is change linked to requirement/story?
- Are commits and PR traceable?
- Are reviewers recorded?
- Are CI checks retained?
- Are release artifacts tied to commit?
- Is deployment tied to release?
- Are exceptions recorded?
- Are hotfixes documented?
- Is evidence safe and current?

---

## Summary

Good Git workflow creates audit evidence as a natural byproduct of engineering, not as a separate scramble.

# CI/CD Review Checklists and Operating Playbook

## Purpose

This file provides reusable checklists for CI/CD, DevSecOps, quality gates, release evidence, and production-readiness reviews.

Use it during repository reviews, PR reviews, release planning, platform governance, and engineering maturity assessments.

---

## Repository CI Checklist

- [ ] Local install command exists.
- [ ] Local feature check command exists.
- [ ] PR-grade validation command exists.
- [ ] Main/release validation is defined.
- [ ] CI workflows are documented.
- [ ] Required checks are meaningful.
- [ ] Heavy checks are placed appropriately.
- [ ] Artifacts are retained.
- [ ] Generated artifacts are controlled.
- [ ] Failures are actionable.

---

## Feature Lane Checklist

- [ ] Lint runs.
- [ ] Format check runs.
- [ ] Typecheck runs where applicable.
- [ ] Unit/domain tests run.
- [ ] Fast contract checks run.
- [ ] Repository hygiene runs.
- [ ] Basic security check runs.
- [ ] Runtime is fast enough for iteration.

---

## PR Merge Gate Checklist

- [ ] Required checks protect main.
- [ ] Contract tests run.
- [ ] Integration tests run where needed.
- [ ] Migration smoke runs where needed.
- [ ] Docker build runs where needed.
- [ ] Security/dependency scan runs.
- [ ] Docs/README/wiki checks run where needed.
- [ ] PR template captures evidence.
- [ ] Reviewers can understand risk.

---

## Main Releasability Checklist

- [ ] Full validation suite defined.
- [ ] Release-grade artifacts generated.
- [ ] SBOM/provenance generated where required.
- [ ] Certification sweep runs where applicable.
- [ ] Docker/runtime smoke runs.
- [ ] Migration proof runs.
- [ ] Failure notification exists.
- [ ] Evidence retained.
- [ ] Release owner reviews output.

---

## DevSecOps Checklist

- [ ] Secrets scan exists.
- [ ] Dependency audit exists.
- [ ] Container scan exists.
- [ ] Workflow permissions are least privilege.
- [ ] Third-party actions are pinned.
- [ ] Fork PRs cannot access secrets.
- [ ] Vulnerability exceptions are time-bound.
- [ ] SBOM exists where required.
- [ ] Provenance captured where required.

---

## Contract Governance Checklist

- [ ] OpenAPI gate exists.
- [ ] Event schema gate exists where events exist.
- [ ] Data-product contract gate exists where data products exist.
- [ ] Vocabulary validation exists where shared enums exist.
- [ ] Documentation gates protect current truth.
- [ ] Supported-feature claims are evidence-backed.
- [ ] Contract changes are versioned.

---

## Release Evidence Checklist

- [ ] Release scope listed.
- [ ] Changed services listed.
- [ ] PRs and commits linked.
- [ ] Artifacts and image digests captured.
- [ ] Tests summarized.
- [ ] Security findings triaged.
- [ ] Migration plan included.
- [ ] Rollback plan included.
- [ ] Monitoring/runbooks linked.
- [ ] Known issues documented.
- [ ] Support contacts identified.

---

## Production Readiness Checklist

- [ ] Service starts in supported runtime.
- [ ] Health endpoint works.
- [ ] Readiness endpoint meaningful.
- [ ] Metrics available.
- [ ] Logs structured.
- [ ] Dashboards updated.
- [ ] Alerts actionable.
- [ ] Runbook current.
- [ ] Rollback tested or documented.
- [ ] Support team briefed.
- [ ] Residual risks accepted or tracked.

---

## Gate Promotion Checklist

- [ ] Gate purpose clear.
- [ ] Current mode defined: report-only, no-new-regression, blocking.
- [ ] Findings classified.
- [ ] Owner assigned.
- [ ] Threshold agreed.
- [ ] Exceptions time-bound.
- [ ] Runtime acceptable.
- [ ] Documentation available.
- [ ] Promotion plan defined.
- [ ] Trend reviewed.

---

## Operating Rhythm

Recommended rhythm:

| Cadence | Activity |
|---|---|
| Every PR | Review evidence, gate status, and contract impact. |
| Weekly | Review flaky checks, security findings, and quality trends. |
| Monthly | Review gate promotion candidates and CI runtime. |
| Before release | Review release evidence and production readiness. |
| After incident | Add or improve gates that would have detected/prevented issue. |
| Quarterly | Review CI/CD maturity roadmap and platform standards. |

---

## Summary

CI/CD governance is a living operating model.

Use these checklists to keep delivery fast, evidence-based, secure, and production-ready.

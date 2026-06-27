# Main Releasability, Release Gates, and Post-Merge Proof

## Purpose

This file explains how to validate the main branch after merge and before release.

PR gates prove a change is merge-ready. Main releasability gates prove the integrated main branch remains release-ready.

---

## Why Main Releasability Matters

A PR can pass alone but still create integrated risk.

Main releasability catches:

- cross-PR interactions
- environment drift
- heavier integration failures
- release packaging issues
- certification failures
- Docker/runtime issues
- performance regressions
- stale generated artifacts
- documentation publication gaps

---

## Main Releasability Gate

Typical checks:

- full test suite
- integration tests
- E2E tests
- Docker build and smoke
- release package generation
- migration verification
- security/dependency scan
- SBOM/provenance
- certification sweeps
- docs/wiki validation
- artifact upload

---

## Scheduled Versus Manual Gates

Use scheduled gates for:

- expensive tests
- nightly full regression
- dependency scans
- long-running performance checks
- certification evidence refresh

Use manual gates for:

- release candidate validation
- pre-demo certification
- production change approval
- hotfix validation
- cross-service product proof

---

## Post-Merge Proof

After merge:

- confirm main checks pass
- confirm generated evidence is current
- confirm artifacts uploaded
- confirm wiki/docs publication posture
- confirm no stranded release branch
- confirm follow-up backlog is captured
- monitor early signals if deployed

---

## Release Candidate Evidence

Release candidate evidence should include:

- commit SHA
- build artifact
- image digest
- dependency snapshot
- SBOM
- test summary
- security summary
- migration summary
- known issues
- rollback plan
- support contacts
- validation timestamp

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| PR gate treated as release gate | Integrated risk missed. |
| Main branch not regularly validated | Release surprises. |
| Scheduled checks fail unnoticed | False confidence. |
| Release artifacts not tied to commit | Weak provenance. |
| Certification evidence stale | Buyer/operator trust risk. |
| Docs merged but wiki not published | Knowledge drift. |
| Hotfix bypasses evidence | Audit and support gap. |

---

## Review Checklist

- Is main releasability defined?
- Are heavy checks scheduled or manual?
- Are failures monitored?
- Is release candidate tied to commit?
- Are artifacts retained?
- Is SBOM/provenance generated where needed?
- Is certification evidence current?
- Are docs/wiki publication steps known?
- Is rollback documented?
- Are support teams briefed?

---

## Summary

Main releasability protects the release line.

A healthy main branch is not accidental. It is continuously proved.

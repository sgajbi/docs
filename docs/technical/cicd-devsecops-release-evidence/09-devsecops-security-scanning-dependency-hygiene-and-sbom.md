# DevSecOps: Security Scanning, Dependency Hygiene, and SBOM

## Purpose

This file explains how security controls should be integrated into normal delivery.

DevSecOps means security is part of planning, coding, review, build, test, packaging, release, and operations.

---

## Security Checks

Common checks:

- SAST
- dependency vulnerability scan
- secrets scanning
- container image scan
- IaC scan
- workflow permission check
- license policy check
- banned package check
- sensitive-content scan
- API security tests
- dependency freshness review

---

## Dependency Hygiene

Good dependency hygiene:

- lockfiles
- pinned or controlled versions
- vulnerability triage
- no unused dependencies
- no unapproved packages
- license review where required
- transitive dependency awareness
- dependency update cadence
- exception expiry

---

## SBOM

A Software Bill of Materials records included components.

SBOM supports:

- vulnerability response
- audit evidence
- procurement review
- incident response
- dependency traceability

SBOM should be tied to:

- artifact
- commit
- build workflow
- timestamp
- dependency versions

---

## Provenance

Provenance explains how an artifact was built.

It answers:

- source repository
- commit SHA
- workflow run
- builder
- build inputs
- artifact digest
- approval path

---

## Vulnerability Triage

Triage should capture:

- package
- vulnerability id
- severity
- exploitability
- affected runtime path
- fix version
- mitigation
- owner
- due date
- exception expiry

Avoid blanket ignore lists.

---

## DevSecOps Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Security scan only before release | Late risk pressure. |
| Vulnerability ignores with no expiry | Permanent hidden risk. |
| No SBOM | Dependency response slow. |
| Secrets scan absent | Credential leakage. |
| Container scan ignored | Runtime risk. |
| Dev dependencies shipped to production image | Larger attack surface. |
| Workflow has write-all permissions | Supply-chain risk. |

---

## Review Checklist

- Are dependencies locked?
- Are vulnerability scans run?
- Are findings triaged?
- Are exceptions time-bound?
- Is SBOM generated where required?
- Is provenance captured?
- Are secrets scanned?
- Are containers scanned?
- Are licenses reviewed where needed?
- Are workflow permissions least privilege?

---

## Summary

DevSecOps shifts security into daily engineering.

Security evidence should be generated continuously, not assembled in panic before release.

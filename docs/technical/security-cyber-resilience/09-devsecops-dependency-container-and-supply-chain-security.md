# DevSecOps, Dependency, Container, and Supply-Chain Security

## Purpose

This file explains how security controls should be integrated into the software delivery lifecycle.

DevSecOps means security is part of normal engineering flow, not a separate final-stage activity.

---

## Secure SDLC Checks

Common controls:

- SAST
- dependency scan
- secrets scan
- container image scan
- IaC scan
- workflow permission scan
- license review
- OpenAPI security review
- sensitive-content scan
- SBOM generation
- provenance capture

---

## Dependency Security

Dependency controls:

- lockfiles
- pinned or controlled versions
- known vulnerability scan
- transitive dependency review
- unused dependency detection
- unapproved package blocking
- exception expiry
- patch cadence
- license policy

---

## Container Security

Container controls:

- minimal base image
- non-root runtime
- no secrets in image
- image vulnerability scan
- trusted registry
- image signing where required
- digest pinning where required
- SBOM/provenance
- no unnecessary packages

---

## Supply-Chain Security

Supply-chain controls:

- protected branches
- required review
- workflow least privilege
- pinned actions
- safe fork PR behavior
- signed commits/tags where required
- artifact provenance
- build isolation
- secure runners
- dependency governance

---

## Vulnerability Triage

Triage should record:

- vulnerability id
- affected package
- severity
- exploitability
- runtime exposure
- fix version
- mitigation
- owner
- due date
- exception expiry

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Security scan only before release | Late risk pressure. |
| Vulnerability ignore forever | Hidden risk. |
| Floating unpinned actions | Supply-chain risk. |
| CI token with write-all permissions | Privilege risk. |
| Secrets exposed to untrusted PR | Credential leak. |
| Dev dependencies in runtime image | Attack surface. |
| No SBOM | Slow vulnerability response. |

---

## Review Checklist

- Are dependencies scanned?
- Are secrets scanned?
- Are containers scanned?
- Are workflows least privilege?
- Are actions pinned?
- Is SBOM generated where needed?
- Is provenance captured?
- Are vulnerabilities triaged?
- Are exceptions time-bound?
- Are workflow changes reviewed?

---

## Summary

DevSecOps reduces risk by making security evidence continuous, repeatable, and visible.

# Workflow Security, Permissions, Secrets, Runners, and Supply Chain

## Purpose

This file explains how CI/CD workflows themselves should be secured.

The pipeline has power. If the pipeline is compromised, the product can be compromised.

---

## Workflow Permissions

Use least privilege.

Examples:

```yaml
permissions:
  contents: read
  pull-requests: read
```

Only grant write permissions when needed.

---

## Secrets Handling

CI secrets should be:

- stored in approved secret store
- scoped to environment/repository
- unavailable to untrusted fork workflows
- rotated
- masked in logs
- not echoed
- not written to artifacts
- not used in unnecessary jobs

---

## Pull Request Risks

Be careful with workflows triggered by external contributions.

Risks:

- untrusted code accessing secrets
- workflow injection
- unsafe `pull_request_target`
- script execution with elevated token
- artifact poisoning

---

## Action Pinning

Prefer pinned actions:

- version tags at minimum
- commit SHA pinning where policy requires
- trusted publishers
- minimal action dependencies

Avoid unreviewed third-party actions with broad permissions.

---

## Runner Security

Runner concerns:

- hosted vs self-hosted
- workspace cleanup
- secret exposure
- network access
- tool cache safety
- privileged Docker use
- artifact retention
- concurrency controls

Self-hosted runners require stronger governance.

---

## Supply Chain Controls

Controls include:

- signed commits/tags where required
- protected branches
- required reviews
- workflow change review
- dependency scanning
- SBOM
- provenance
- image signing
- digest pinning
- artifact retention

---

## Workflow Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| `permissions: write-all` | Excessive privilege. |
| Secrets exposed to untrusted PRs | Credential leak. |
| Unpinned third-party actions | Supply-chain risk. |
| Workflow changes not reviewed | Attack path. |
| Self-hosted runner not cleaned | Cross-job leakage. |
| Artifacts include secrets | Persistent leak. |
| Build and deploy use same broad token | Blast radius. |

---

## Review Checklist

- Are workflow permissions minimal?
- Are secrets scoped and protected?
- Are third-party actions pinned?
- Are workflow changes reviewed carefully?
- Are fork PRs safe?
- Are runners isolated?
- Are artifacts filtered?
- Is deployment permission separated?
- Is provenance captured?
- Are logs free of secrets?

---

## Summary

CI/CD security is product security.

A strong application can still be compromised by a weak workflow.

# Container, Runtime, Migration, and Docker Parity Gates

## Purpose

This file explains how CI/CD should validate runtime packaging, container startup, and migration safety.

A service that passes tests but cannot start in its supported runtime profile is not release-ready.

---

## Docker Build Gate

Docker build gate proves:

- Dockerfile is valid
- dependencies install
- app files are packaged
- image can be built reproducibly
- build context excludes local artifacts

---

## Docker Smoke Gate

Docker smoke gate proves:

- container starts
- app binds expected port
- health endpoint responds
- readiness behaves as expected
- required env vars are documented
- startup command works

---

## Runtime Parity

Runtime parity checks reduce "works locally" risk.

Validate:

- env var names
- service ports
- dependency URLs
- health/readiness
- logging output
- metrics endpoint
- migration command
- shutdown behavior

---

## Migration Gates

Migration gates validate database evolution.

Checks:

- migration files present
- migration applies
- rollback or compensating plan exists
- schema after migration expected
- migration idempotency where needed
- data checks before/after
- performance risk assessed

---

## Container Security

Runtime gates may include:

- non-root user
- minimal base image
- no secrets in image
- vulnerability scan
- image digest
- signed image where required
- no dev-only tools in production image

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Tests pass but Docker build not checked | Runtime failure late. |
| Health endpoint only checked locally | Container startup issues missed. |
| Migration not run in CI | Release failure. |
| Secrets baked into image | Security incident. |
| Docker image includes local caches | Large and non-reproducible. |
| Readiness always succeeds | Broken service receives traffic. |
| Migration rollback undocumented | Recovery risk. |

---

## Review Checklist

- Does image build in CI?
- Does container start?
- Does health endpoint respond?
- Is readiness meaningful?
- Are env vars documented?
- Are migrations tested?
- Is rollback/compensation documented?
- Is image scanned?
- Are secrets excluded?
- Is runtime evidence retained?

---

## Summary

Runtime gates prove that code can become a service.

Build success is not enough. The packaged service must start, expose health, support migrations, and be safe to deploy.

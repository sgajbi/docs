# Local Runtime, Docker Compose, and Developer Parity

## Purpose

This file explains how local runtime environments should support productive development while staying aligned with CI and platform runtime expectations.

Local runtime should help developers work quickly without creating misleading proof.

---

## Local Runtime Levels

| Level | Purpose |
|---|---|
| Local process | Fast development and debugging. |
| Local Docker | Prove container image and startup. |
| Docker Compose | Run service with local dependencies. |
| Platform-like local stack | Validate ingress and cross-service flows. |
| Canonical product runtime | Validate integrated user/product flows. |

Each level has a purpose. Do not confuse local process success with production readiness.

---

## Docker Compose Use Cases

Docker Compose is useful for:

- starting service dependencies
- local database testing
- local Kafka/Rabbit/Redis testing
- smoke testing service startup
- demo data seeding
- integration-lite validation
- developer onboarding
- Docker parity proof

Compose is not a substitute for production Kubernetes manifests.

---

## Local Parity Contract

Document:

```text
services:
ports:
hostnames:
required env vars:
optional env vars:
seed commands:
migration commands:
health checks:
shutdown/cleanup:
test commands:
known limitations:
```

---

## Local Hostnames

Canonical hostnames help avoid per-developer drift.

Example:

```text
api.dev.local
gateway.dev.local
app.dev.local
```

Rules:

- use canonical names for cross-service validation
- use localhost ports for direct debugging
- document hosts-file or DNS setup
- avoid random untracked hostnames

---

## Seed Data

Seed data should be:

- synthetic
- deterministic
- documented
- safe
- tied to validation scenarios
- resettable

Do not use real client or account data.

---

## Local Runtime Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Works only with developer's hidden `.env` | Onboarding and CI failures. |
| Compose starts but dependencies not healthy | False proof. |
| Local hostnames undocumented | Cross-service confusion. |
| Seed data not deterministic | Test instability. |
| Local stack differs from CI/runtime materially | Parity gap. |
| Compose treated as production deployment | Missing platform controls. |
| Cleanup deletes important developer files | Trust loss. |

---

## Review Checklist

- Is local startup documented?
- Is Docker Compose available where useful?
- Are ports and hostnames clear?
- Are migrations documented?
- Is seed data safe?
- Are health checks included?
- Are cleanup commands safe?
- Is CI parity explained?
- Are limitations documented?
- Can a new developer start the service without tribal knowledge?

---

## Summary

Local runtime should be fast, clear, and honest.

It helps developers build confidence, but it should not pretend to be full production proof.

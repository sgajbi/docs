# Migration, Runtime, Docker, and Deployment Testing

## Purpose

This file explains how to test runtime packaging, database migrations, Docker images, and deployment readiness.

A service that passes unit tests but fails to start, migrate, or deploy is not release-ready.

---

## Migration Tests

Test:

- migration applies
- rollback or compensation documented
- schema after migration expected
- idempotency where needed
- data backfill where applicable
- compatibility with old/new app versions
- migration performance for large tables where critical

---

## Docker Build Tests

Prove:

- image builds
- dependencies install
- build context clean
- no local caches included
- startup command exists
- image scan can run

---

## Docker Smoke Tests

Prove:

- container starts
- port binds
- health endpoint responds
- readiness responds
- required config validated
- metrics endpoint available
- logs emit structured startup event

---

## Deployment Tests

Test or validate:

- manifests render
- env vars present
- secrets referenced
- probes configured
- resources configured
- service/ingress configured
- rollback path
- feature flag defaults
- migration order

---

## Runtime Parity Tests

Compare:

- local config names
- CI config names
- container startup
- platform startup
- health/readiness behavior
- dependency addresses
- migration command

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Migration tested manually only | Release failure. |
| Docker image never built in PR | Runtime failure late. |
| Health smoke only, no readiness | Broken traffic routing. |
| Secrets required but not documented | Deployment failure. |
| Manifest changes not validated | Platform failure. |
| Rollback not tested or documented | Incident duration. |

---

## Review Checklist

- Are migrations tested?
- Are schema changes backward-compatible?
- Does Docker build?
- Does container start?
- Are health/readiness tested?
- Are required env vars documented?
- Are manifests validated?
- Are secrets referenced safely?
- Is rollback documented?
- Is runtime evidence retained?

---

## Summary

Runtime testing proves that code can become a deployable service.

It is a critical part of release confidence.

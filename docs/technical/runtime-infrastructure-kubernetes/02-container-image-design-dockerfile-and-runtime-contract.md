# Container Image Design, Dockerfile, and Runtime Contract

## Purpose

This file explains how to design container images for enterprise services.

A container image should be reproducible, minimal, secure, observable, and aligned with the service runtime contract.

---

## What A Container Image Should Do

A container image should package:

- application code
- runtime dependencies
- startup command
- expected working directory
- non-secret configuration defaults
- healthcheck support where appropriate

It should not package:

- secrets
- local `.env` files
- developer caches
- test artifacts unless required
- virtual environments from developer machines
- local databases
- raw logs
- unapproved tools
- unnecessary build dependencies in final image

---

## Dockerfile Principles

Good Dockerfile practice:

- use a minimal approved base image
- pin base image version where required
- separate dependency install from app copy
- use `.dockerignore`
- avoid root runtime user where possible
- avoid secrets in build args
- avoid downloading untrusted scripts at build time
- expose only required ports
- set explicit entrypoint/command
- support graceful shutdown
- keep final image small
- scan image for vulnerabilities

---

## Multi-Stage Builds

Use multi-stage builds when build-time dependencies are not needed at runtime.

Example pattern:

```dockerfile
FROM python:3.12-slim AS builder
# install/build dependencies

FROM python:3.12-slim AS runtime
# copy only runtime artifacts
```

Benefits:

- smaller image
- smaller attack surface
- fewer runtime dependencies
- cleaner provenance

---

## Runtime Contract

A container should make these expectations explicit:

```text
command:
port:
environment variables:
working directory:
health endpoint:
readiness endpoint:
metrics endpoint:
shutdown signal:
required filesystem paths:
non-root user:
```

---

## `.dockerignore`

Use `.dockerignore` to exclude:

```text
.git
.venv
__pycache__
.pytest_cache
.coverage
node_modules
dist
build
output
logs
*.db
.env
```

Adjust to the technology stack.

---

## Image Tagging

Use meaningful tags:

- commit SHA
- semantic version
- release candidate
- environment promotion tag
- immutable digest

Avoid relying only on `latest` for controlled environments.

---

## Container Security

Check:

- non-root user
- minimal base image
- no secrets
- patched dependencies
- image scan
- SBOM/provenance
- no unnecessary packages
- read-only filesystem where possible
- restricted Linux capabilities where platform supports it

---

## Container Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Build copies entire repo including caches | Large, noisy, unsafe image. |
| Secrets passed as build args | Secrets remain in image history. |
| Runs as root unnecessarily | Higher runtime risk. |
| Uses floating base image without control | Reproducibility risk. |
| Dev dependencies in production image | Larger attack surface. |
| No Docker smoke test | Runtime failures discovered late. |
| Entrypoint differs from documented command | Support confusion. |

---

## Review Checklist

- Is Dockerfile minimal?
- Is base image approved?
- Are dependencies pinned/controlled?
- Is `.dockerignore` complete?
- Are secrets excluded?
- Does container run as non-root where possible?
- Is startup command documented?
- Is health/readiness available?
- Is image scanned?
- Is image build reproducible?
- Is Docker smoke test present?

---

## Summary

A container image is a production artifact.

Treat it as carefully as source code: secure it, test it, scan it, version it, and document how it runs.

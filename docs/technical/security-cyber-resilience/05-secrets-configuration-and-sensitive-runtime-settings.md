# Secrets, Configuration, and Sensitive Runtime Settings

## Purpose

This file explains how to handle secrets and configuration safely.

Configuration controls runtime behavior. Secrets grant access. Both must be governed carefully.

---

## Secrets

Secrets include:

- passwords
- API keys
- tokens
- private keys
- signing keys
- database credentials
- connection strings with credentials
- cloud credentials
- webhook secrets

Secrets must not appear in:

- source code
- committed `.env`
- docs
- tests
- screenshots
- logs
- metrics
- traces
- container image layers
- CI artifacts
- prompts
- generated evidence

---

## Configuration

Configuration includes:

- hostnames
- ports
- feature flags
- timeouts
- dependency URLs
- runtime mode
- log level
- queue names
- limits
- profile selection

Configuration should be:

- typed
- validated
- documented
- environment-specific
- redacted where sensitive
- safe by default

---

## Secret Storage

Common patterns:

- cloud key vault
- Kubernetes secrets
- external secrets operator
- CI secret store
- workload identity fetching secrets
- mounted secret files

Use the institution-approved mechanism.

---

## Redaction

Redact values in diagnostics.

Example:

```text
DATABASE_URL=postgresql://***:***@db-host:5432/app
API_TOKEN=present
PRIVATE_KEY=not_displayed
```

---

## Rotation

Secret rotation should define:

- owner
- frequency
- emergency rotation process
- impacted services
- deployment dependency
- validation steps
- rollback plan
- evidence

---

## Configuration Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Secret in source | Immediate credential leak. |
| Secret in Docker build arg | Image history leak. |
| Full config printed at startup | Sensitive-data exposure. |
| Unsafe default in production | Misconfiguration risk. |
| Feature flag with no owner | Permanent complexity. |
| Environment variables read everywhere | Hard to audit and test. |
| Local `.env` copied into docs | Secret leakage. |

---

## Review Checklist

- Are secrets externalized?
- Are secrets absent from repo and artifacts?
- Is configuration typed?
- Are required values validated?
- Are defaults safe?
- Are values redacted?
- Is rotation defined?
- Are feature flags owned?
- Are config changes reviewed?
- Are missing/malformed settings tested?

---

## Summary

Secrets and configuration are production control surfaces.

Treat them with the same discipline as code and infrastructure.

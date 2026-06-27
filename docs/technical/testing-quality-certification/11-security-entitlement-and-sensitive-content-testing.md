# Security, Entitlement, and Sensitive-Content Testing

## Purpose

This file explains how to test security, authorization, entitlement, and sensitive-data controls.

Security tests should prove that unauthorized access is blocked and sensitive content does not leak through responses, logs, metrics, traces, diagnostics, or artifacts.

---

## Authorization Tests

Test:

- allowed read
- denied read
- allowed write
- denied write
- missing caller context
- malformed caller context
- wrong tenant/region
- wrong portfolio/client scope
- unsupported role/capability
- operator action without permission

---

## Permission-Blocked Tests

Permission-blocked tests should assert:

- correct status code or section state
- safe reason code
- no raw entitlement payload
- no restricted resource details
- audit event emitted where needed
- metric emitted with bounded labels
- UI can render safe blocked state where applicable

---

## Sensitive-Content Tests

Check that forbidden content is absent from:

- API responses
- error bodies
- logs
- metrics labels
- trace attributes
- diagnostic APIs
- screenshots
- generated evidence
- CI artifacts

---

## Secrets Tests

Scan for:

- private keys
- tokens
- passwords
- connection strings
- API keys
- unredacted secrets in logs/artifacts
- committed `.env` files

---

## Security Regression Tests

Add regression tests for:

- previous access-control bug
- raw error leakage
- sensitive metric label
- unsafe diagnostic output
- secrets accidentally committed
- prompt/context leakage
- unauthorized workflow action

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Only positive authorization test | Denial path unproved. |
| Permission denial returns empty data | User and audit confusion. |
| Logs not checked | Data leak persists. |
| Sensitive-content scan only manual | Leakage discovered late. |
| Security tests skipped as slow | Controls unproved. |
| AI prompts not tested | Sensitive context leakage. |

---

## Review Checklist

- Are allowed and denied paths tested?
- Are write permissions tested?
- Are operator actions tested?
- Are permission-blocked responses safe?
- Are logs/metrics/traces checked?
- Are diagnostics checked?
- Are secrets scanned?
- Are artifacts checked?
- Are tests in CI?
- Are security regressions captured?

---

## Summary

Security testing proves the system fails safely.

A secure system must deny correctly and avoid leaking through every output channel.

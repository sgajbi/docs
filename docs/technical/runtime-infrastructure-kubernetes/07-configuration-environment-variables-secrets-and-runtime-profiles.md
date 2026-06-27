# Configuration, Environment Variables, Secrets, and Runtime Profiles

## Purpose

This file explains how configuration and secrets should be handled across runtime environments.

Configuration is how the same artifact behaves correctly in different environments. Secrets are sensitive values that must be protected throughout the lifecycle.

---

## Configuration Principles

Good configuration is:

- externalized
- typed
- validated at startup
- environment-specific
- documented
- safe to inspect in redacted form
- testable
- separated from secrets
- consistent across local, CI, and platform runtime

---

## Environment Variables

Document every important environment variable:

```text
NAME:
Purpose:
Required:
Default:
Allowed values:
Secret:
Example:
Runtime profiles:
```

Avoid reading environment variables randomly across the codebase. Use typed settings or configuration modules.

---

## Runtime Profiles

Profiles may include:

- local
- test
- Docker local
- integration
- demo/certification
- staging
- production

Each profile should define:

- required config
- secrets source
- dependencies
- logging level
- feature flags
- database behavior
- external API behavior
- telemetry behavior

---

## Secrets

Secrets include:

- database passwords
- API keys
- tokens
- private keys
- connection strings
- signing secrets
- cloud credentials

Secrets must not be:

- committed
- baked into images
- printed in logs
- stored in test fixtures
- included in generated artifacts
- passed through unsafe build args

---

## Secret Delivery

Common secret delivery options:

- Kubernetes Secrets
- cloud key vault
- external secrets operator
- mounted secret files
- environment variables from secret store
- workload identity to fetch secrets

Choice depends on platform and policy.

---

## Configuration Diagnostics

A service may expose safe configuration diagnostics.

Show:

- config key present/missing
- active profile
- dependency configured/unconfigured
- redacted endpoint class
- feature flag state where safe

Do not show:

- secret values
- full connection strings
- tokens
- private URLs not approved for diagnostics
- credentials

---

## Configuration Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Hardcoded environment-specific values | Deployment drift. |
| Secret in source or image | Security incident. |
| Missing config defaults to unsafe behavior | Production risk. |
| Environment read in many modules | Hard to test and reason. |
| Docs list outdated env vars | Onboarding failure. |
| Diagnostic endpoint shows secrets | Data leakage. |
| Local and production config names differ | CI/runtime confusion. |

---

## Review Checklist

- Are settings typed?
- Are required vars documented?
- Are defaults safe?
- Are secrets externalized?
- Are values redacted in logs?
- Are profiles documented?
- Are feature flags owned?
- Are config changes reviewed?
- Are missing configs detected at startup?
- Are diagnostic views safe?

---

## Summary

Configuration should make runtime flexible without making behavior mysterious.

Secrets should be protected by design, not by developer memory.

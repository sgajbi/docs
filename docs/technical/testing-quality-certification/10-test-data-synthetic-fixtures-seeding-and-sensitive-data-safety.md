# Test Data, Synthetic Fixtures, Seeding, and Sensitive-Data Safety

## Purpose

This file explains how to design safe, deterministic, useful test data.

In financial systems, test data is a security and quality concern.

---

## Test Data Principles

Test data should be:

- synthetic
- deterministic
- realistic enough
- minimal for the test
- documented
- reusable where useful
- safe for CI artifacts
- safe for screenshots
- free of secrets
- free of real client/account data

---

## Synthetic Data

Synthetic data should avoid real:

- client names
- account numbers
- portfolio identifiers
- transaction references
- holdings details
- document identifiers
- advisor names
- credentials
- source file names if restricted

Use safe examples:

```text
CLIENT_SYNTHETIC_001
PORTFOLIO_SYNTHETIC_BALANCED_001
ACCOUNT_SYNTHETIC_001
TXN_SYNTHETIC_001
```

---

## Fixtures

Fixtures should be:

- small
- named by scenario
- versioned where important
- documented
- reused carefully
- easy to update intentionally

Avoid giant fixtures that no one understands.

---

## Seeding

Seed data supports integration, E2E, and certification tests.

Seeding should define:

- seed command
- dataset version
- expected identities
- reset behavior
- validation command
- safe data classification

---

## Snapshot Data

Snapshot data can help regression but should be managed.

Rules:

- snapshot only stable contract output
- review snapshot changes carefully
- avoid sensitive fields
- avoid huge opaque snapshots
- prefer semantic assertions for critical behavior

---

## Sensitive-Data Gates

Use checks to prevent:

- secrets in fixtures
- real-looking account data
- restricted names
- raw production exports
- internal file paths
- tokens
- private keys

---

## Test Data Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Production extract in tests | Privacy breach. |
| Random generated data without seed | Flaky tests. |
| Huge fixture with unclear meaning | Maintenance burden. |
| Snapshot includes sensitive data | Artifact leakage. |
| Seed data differs by machine | Non-reproducible. |
| Fixture tries to cover every case | Hard to understand. |

---

## Review Checklist

- Is data synthetic?
- Is data deterministic?
- Is fixture minimal?
- Is scenario documented?
- Are identifiers safe?
- Are secrets absent?
- Are snapshots reviewed?
- Is seeding repeatable?
- Is cleanup defined?
- Are sensitive-data gates present?

---

## Summary

Good test data makes tests trustworthy.

Unsafe test data can turn testing into a security incident.

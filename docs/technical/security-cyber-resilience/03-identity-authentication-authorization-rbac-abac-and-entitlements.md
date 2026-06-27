# Identity, Authentication, Authorization, RBAC, ABAC, and Entitlements

## Purpose

This file explains identity and access-control concepts used in enterprise systems.

The goal is to distinguish who the caller is, what they are allowed to do, and which data scope they can access.

---

## Authentication

Authentication answers:

```text
Who are you?
```

Examples:

- user login
- service identity
- token validation
- certificate-based identity
- workload identity
- SSO integration

---

## Authorization

Authorization answers:

```text
What are you allowed to do?
```

Examples:

- can read portfolio summary
- can submit proposal
- can request report
- can download document
- can replay failed job
- can run operator diagnostic

---

## Entitlements

Entitlements define scoped access.

Examples:

- allowed clients
- allowed portfolios
- allowed accounts
- allowed booking centers
- allowed regions
- allowed product types
- allowed document categories
- allowed workflow actions

---

## RBAC

Role-Based Access Control grants permissions based on roles.

Example roles:

- advisor
- portfolio manager
- operations user
- supervisor
- support engineer
- administrator

RBAC is simple but may be too coarse by itself.

---

## ABAC

Attribute-Based Access Control uses attributes.

Examples:

- user region
- account booking center
- client segment
- document classification
- transaction state
- workflow stage
- data sensitivity
- time or location

ABAC supports richer decisions.

---

## Permission-Blocked State

Permission denial should be explicit and safe.

Good behavior:

- return 403 if full operation blocked
- return section-level `PERMISSION_BLOCKED` if product response is partial
- avoid raw policy details
- log safe audit event
- emit bounded metric

---

## Access-Control Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| UI-only authorization | Backend bypass. |
| Service trusts all BFF calls blindly | Lateral access risk. |
| Raw entitlement errors returned | Policy leakage. |
| Empty result for permission denial | User confusion and audit gap. |
| Admin role used broadly | Excess privilege. |
| Authorization logic duplicated everywhere | Inconsistent decisions. |
| No audit for sensitive action | Traceability gap. |

---

## Review Checklist

- Is authentication model clear?
- Is authorization enforced server-side?
- Are roles/capabilities documented?
- Are entitlement scopes defined?
- Are permission-blocked states safe?
- Are sensitive reads audited where needed?
- Are writes and replay actions strongly authorized?
- Are access tests present?
- Are logs and metrics safe?
- Is least privilege applied?

---

## Summary

Identity tells who is calling. Authorization controls what they can do. Entitlements control what data scope they can access.

All three matter in financial platforms.

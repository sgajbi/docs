# Security, Identity, Entitlement, Secrets, and Access Migration

## Purpose

This file explains security and access-control migration concerns.

A migration is unsafe if users, services, and systems gain or lose access incorrectly.

---

## Security Migration Scope

Scope includes:

- user identities
- service identities
- roles
- groups
- entitlements
- permissions
- certificates
- secrets
- API keys
- tokens
- audit logs
- access reviews
- privileged access
- break-glass access

---

## Entitlement Mapping

Map:

- old roles to new roles
- old groups to new groups
- old portfolio/account scopes to new scopes
- old region/booking-center logic to new logic
- old document access to new document access
- old admin/support access to new support model

---

## Secrets Migration

Secrets migration should define:

- source secret store
- target secret store
- owner
- rotation plan
- cutover timing
- validation
- rollback
- audit

Never copy secrets through uncontrolled channels.

---

## Access Testing

Test:

- allowed user access
- denied user access
- advisor/team scope
- region/booking center
- support/admin access
- service-to-service access
- document/report download
- API access
- audit logging

---

## Security Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Entitlements tested only happy path | Data leakage. |
| Old admin access copied broadly | Excess privilege. |
| Secrets transferred manually in chat | Security incident. |
| Service identity shared | Poor traceability. |
| Access logs not migrated | Audit gap. |
| Denied access not tested | Control weakness. |
| Break-glass not defined | Incident risk. |

---

## Review Checklist

- Is identity migration in scope?
- Are roles mapped?
- Are entitlements mapped?
- Are secrets migrated securely?
- Are service accounts scoped?
- Is privileged access controlled?
- Are allowed/denied tests defined?
- Is audit logging ready?
- Is access review planned?
- Are rollback implications known?

---

## Summary

Access migration must prove both positive and negative paths.

The goal is not only that users can log in, but that they can access exactly what they should.

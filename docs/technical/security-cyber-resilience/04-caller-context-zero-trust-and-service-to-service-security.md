# Caller Context, Zero Trust, and Service-to-Service Security

## Purpose

This file explains caller context, zero-trust design, and service-to-service security.

Modern platforms should not trust calls only because they come from inside the network.

---

## Caller Context

Caller context is a typed representation of the caller and request scope.

Example fields:

```text
actor_id
caller_application
tenant_id
region
booking_center
role
capabilities
portfolio_scope
client_scope
correlation_id
trace_context
```

Do not pass raw headers everywhere. Convert them into typed context.

---

## Zero Trust

Zero trust means:

```text
Never trust implicitly. Verify explicitly.
```

In service design:

- authenticate callers
- authorize actions
- validate scopes
- minimize privileges
- propagate identity safely
- audit sensitive actions
- do not rely only on network location

---

## Service-to-Service Security

Controls may include:

- mTLS
- service identity
- signed tokens
- workload identity
- API gateway policies
- network policies
- short-lived credentials
- scoped service accounts
- request signing where needed

---

## Context Propagation

In a flow:

```text
UI -> BFF -> domain service -> shared capability
```

The platform should preserve:

- user/caller identity
- service identity
- tenant/region
- correlation id
- trace context
- entitlement scope where approved

Each service should still enforce its own responsibility.

---

## Trust Boundary Questions

- Where does identity originate?
- Which service validates it?
- Which fields are trusted?
- Which fields can be caller-provided?
- Which fields must be derived from token/policy?
- Which downstream services receive context?
- What is audited?
- What happens if context is missing or malformed?

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Internal network means trusted | Lateral movement risk. |
| Caller context accepted without validation | Spoofing risk. |
| UI can set privileged role header | Authorization bypass. |
| BFF strips context | Downstream cannot enforce. |
| Service identity shared by many apps | Poor traceability. |
| Long-lived service secrets | Credential risk. |
| No audit of privileged actions | Accountability gap. |

---

## Review Checklist

- Is caller context typed?
- Are trusted fields defined?
- Is context validated?
- Is service identity used?
- Are downstream calls authenticated?
- Are scopes preserved?
- Are privileged actions audited?
- Are missing/malformed context cases tested?
- Are network policies aligned?
- Is least privilege applied?

---

## Summary

Zero trust does not mean no trust. It means trust is explicit, scoped, validated, and evidenced.

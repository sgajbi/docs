# Secure API, BFF, Backend, and UI Design

## Purpose

This file explains how security responsibilities differ across UI, BFF, backend domain services, and shared capability services.

Good security requires each layer to enforce its responsibility without assuming another layer has solved everything.

---

## UI Security Responsibilities

UI should:

- render permission-blocked states safely
- hide unavailable actions
- avoid storing sensitive data unnecessarily
- avoid constructing privileged headers
- avoid exposing raw errors
- avoid embedding secrets
- avoid direct calls to internal services
- avoid treating AI output as official truth

UI should not be the only authorization control.

---

## BFF Security Responsibilities

BFF should:

- validate caller context
- propagate approved context
- enforce product-facing access where applicable
- avoid leaking raw upstream errors
- handle partial/permission-blocked states
- avoid recomputing domain truth
- avoid exposing internal service paths
- mediate document/AI/report access through controlled routes

---

## Domain Service Security Responsibilities

Domain service should:

- enforce server-side authorization
- validate scope
- protect source-owned data
- audit sensitive reads/writes
- reject unsupported actions
- return safe errors
- avoid leaking internal details
- protect write operations with idempotency

---

## Shared Capability Security Responsibilities

Shared services should:

- validate caller/service identity
- enforce capability-specific access
- protect generated artifacts
- control archive/document retrieval
- govern AI workflow execution
- maintain audit and retention
- avoid becoming business-domain authority

---

## Safe Error Design

Errors should not expose:

- stack traces
- SQL errors
- raw upstream payloads
- entitlement rules
- secrets
- internal paths
- prompts/model responses
- client/account details

---

## Secure Write Actions

Write actions should:

- require authentication
- enforce authorization
- require idempotency where needed
- validate lifecycle state
- audit action
- emit safe logs/metrics
- return safe conflict/denial states

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| UI-only authorization | Backend bypass. |
| BFF trusts user-provided role | Spoofing. |
| Domain service assumes gateway already checked | Lateral risk. |
| Raw upstream error returned | Information disclosure. |
| Direct document storage URL exposed | Data leakage. |
| AI workflow exposed without guardrails | Unsafe output/use. |
| Write action lacks idempotency | Duplicate side effects. |

---

## Review Checklist

- Is authorization enforced server-side?
- Does UI render blocked states safely?
- Does BFF propagate context safely?
- Does domain service validate scope?
- Are errors safe?
- Are write actions audited?
- Are document/AI/report routes controlled?
- Are internal paths hidden?
- Are tests present for denied access?

---

## Summary

Security is layered.

The UI improves user experience, the BFF protects product composition, and domain services enforce business authority.

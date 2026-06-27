# Deployment Strategies, Rollout, Rollback, and Release Activation

## Purpose

This file explains how to deploy services safely and how deployment differs from feature release.

A deployment moves code. A release activates capability.

---

## Deployment Strategies

| Strategy | Use |
|---|---|
| Rolling deployment | Default stateless compatible changes. |
| Blue/green | Safer full-environment switch. |
| Canary | Gradual traffic exposure. |
| Recreate | Simple but downtime-prone. |
| Shadow | Test behavior without user impact. |
| Feature flag | Decouple deploy from activation. |

---

## Rolling Deployment Requirements

Rolling deployment requires:

- backward-compatible API changes
- backward-compatible database changes
- readiness probes
- graceful shutdown
- no process-local critical state
- safe worker behavior
- version compatibility during overlap

---

## Feature Flags

Feature flags support:

- dark launch
- gradual activation
- tenant/region activation
- user group activation
- emergency rollback
- A/B experiments where allowed

Feature flags need:

- owner
- purpose
- expiry date
- audit trail
- test coverage
- cleanup plan

---

## Rollback

Rollback can include:

- previous image
- config rollback
- feature flag disable
- database compensation
- traffic routing switch
- worker pause
- queue replay decision
- data correction

Rollback must consider migrations and side effects.

---

## Deployment Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Deploy and release treated the same | No safe activation control. |
| Breaking DB change before app support | Runtime failure. |
| No rollback plan | Long incident. |
| Feature flags never removed | Complexity grows. |
| Canary without monitoring | Gradual failure unnoticed. |
| Worker and API version incompatible | Async failures. |
| No post-deploy checks | Broken release undetected. |

---

## Review Checklist

- What deployment strategy is used?
- Is change backward-compatible?
- Are migrations safe?
- Are feature flags needed?
- Is rollback possible?
- Are workers compatible?
- Are readiness probes meaningful?
- Are post-deploy checks defined?
- Is monitoring updated?
- Is release activation audited?

---

## Summary

Safe deployment is controlled change.

Good teams separate deploy, activate, observe, and rollback.

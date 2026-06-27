# Test Pyramid, Test Portfolio, and Risk-Based Testing

## Purpose

This file explains how to design a balanced test portfolio.

A good test strategy uses different test levels for different risks.

---

## Test Pyramid

A practical enterprise test pyramid:

```text
                 certification / live proof
              E2E / browser / product flow
          integration / contract / migration
      application use-case / workflow tests
  unit / domain / policy / calculation tests
```

The lower levels should be faster and more numerous. Higher levels should be fewer and focused on critical flows.

---

## Test Portfolio

| Test Level | Purpose |
|---|---|
| Unit | Prove small functions, mappers, validators. |
| Domain | Prove business rules, calculations, policies, lifecycle. |
| Application | Prove use-case orchestration with fake ports. |
| Contract | Prove API/event/data-product/schema compatibility. |
| Integration | Prove real adapters and dependency behavior. |
| E2E | Prove full system workflow. |
| Browser | Prove user-visible product behavior. |
| Certification | Prove supported claims with repeatable evidence. |
| Non-functional | Prove performance, resilience, security, runtime posture. |

---

## Risk-Based Testing

Focus tests where risk is highest.

Risk factors:

- money movement or calculation impact
- client-facing output
- regulatory/control impact
- security or entitlement boundary
- data migration
- external integration
- persistence change
- async workflow
- reporting/document generation
- AI-generated content
- high-volume API
- previous incident area

---

## Test Selection Guide

| Change Type | Minimum Test Focus |
|---|---|
| Domain rule | Unit/domain tests and golden examples. |
| API field change | Contract, OpenAPI, consumer tests. |
| New write workflow | Idempotency, authorization, lifecycle, integration. |
| Database migration | Migration apply/rollback, repository tests. |
| BFF composition | Contract, partial/degraded, caller-context forwarding. |
| UI panel | Browser/product flow, BFF contract, state rendering. |
| Worker/replay | Worker state, retry, idempotency, dead-letter. |
| Security change | Permission, sensitive-data, audit tests. |
| Observability change | Metric/log/trace validation and no-sensitive-content tests. |
| AI workflow | Grounding, injection, tool authorization, human review tests. |

---

## Test Economics

Cheap tests:

- unit
- domain
- use-case
- DTO validation

Medium-cost tests:

- contract
- integration
- migration
- Docker smoke

High-cost tests:

- E2E
- browser
- full stack
- performance
- certification
- cross-service live proof

Use high-cost tests where they add unique confidence.

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Inverted pyramid | Slow, flaky, hard-to-debug suite. |
| No contract tests | Consumer breakage. |
| Only mocked tests | Integration failures missed. |
| Only happy path | Real-world behavior unproved. |
| Every bug only tested E2E | Slow regression suite. |
| No risk-based prioritization | Effort spent on low-risk areas. |

---

## Review Checklist

- Is the test portfolio balanced?
- Are critical risks covered?
- Are domain rules tested low in pyramid?
- Are contracts protected?
- Are integration seams tested?
- Are product-critical flows covered E2E?
- Are non-functional risks tested?
- Are expensive tests justified?
- Are test lanes aligned with cost?

---

## Summary

A good test portfolio balances speed, confidence, and risk.

Do not test everything at the highest level. Test important behavior at the right level.

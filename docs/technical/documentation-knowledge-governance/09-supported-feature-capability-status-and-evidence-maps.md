# Supported Feature, Capability Status, and Evidence Maps

## Purpose

This file explains how to document what is implemented, partial, planned, unsupported, or unknown.

Supported-feature documentation protects teams from overclaiming and helps consumers understand current truth.

---

## Status Vocabulary

| Status | Meaning |
|---|---|
| Implemented | Code, tests, docs, and validation evidence exist. |
| Partially implemented | Some code exists, but gaps remain. |
| Proposed | Design exists, but implementation incomplete. |
| Planned | Future work. |
| Unsupported | Not supported by current system. |
| Deprecated | Supported temporarily but being replaced. |
| Unknown | Current support cannot be proven. |

---

## Supported Feature Entry

A feature entry should include:

```text
Feature:
Status:
Owner:
Description:
Supported inputs:
Unsupported inputs:
API/UI surfaces:
Data dependencies:
Tests:
Runtime evidence:
Known gaps:
Runbook:
Last reviewed:
```

---

## Evidence Map

An evidence map links claims to proof.

Examples:

| Claim | Evidence |
|---|---|
| API supports cursor pagination | OpenAPI contract, API tests, integration tests. |
| Report replay supported | use-case tests, worker tests, runbook, operator API. |
| Data product certified | producer contract, telemetry, certification output. |
| UI panel supported | browser test, BFF contract, screenshot evidence. |

---

## Safe Claiming

Avoid phrases such as:

- production-ready
- bank-buyable
- fully supported
- enterprise-grade
- certified

unless evidence is attached and current.

Use precise status language.

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Feature list without status | Consumers over-trust. |
| Planned capability written as supported | False claim. |
| Evidence missing | Review impossible. |
| Status never reviewed | Stale support matrix. |
| Unsupported limitations hidden | Product/support surprise. |
| Demo proof treated as production proof | Misleading readiness. |

---

## Review Checklist

- Is status explicit?
- Is owner listed?
- Is support scope clear?
- Are unsupported cases listed?
- Is evidence linked?
- Are tests current?
- Are known gaps documented?
- Is last reviewed date present?
- Is language precise?
- Is customer-facing claim safe?

---

## Summary

Supported-feature docs are truth tables.

They keep product, engineering, support, and leadership aligned on what can be safely claimed.

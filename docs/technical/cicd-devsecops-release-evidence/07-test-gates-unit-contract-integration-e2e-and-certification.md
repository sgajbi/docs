# Test Gates: Unit, Contract, Integration, E2E, and Certification

## Purpose

This file explains how to place automated tests across CI/CD lanes.

Testing should prove behavior at the right level and produce useful evidence for reviewers and release decisions.

---

## Test Gate Types

| Gate | Purpose |
|---|---|
| Unit tests | Fast behavior proof for small logic. |
| Domain tests | Business rules, calculations, policies, lifecycle. |
| Application tests | Use-case orchestration with fake ports. |
| Contract tests | API/event/data-product schema and compatibility. |
| Integration tests | Real adapters and dependency behavior. |
| E2E tests | Full workflow proof. |
| Browser tests | Product UI proof. |
| Migration tests | Database/schema evolution proof. |
| Docker smoke | Runtime packaging proof. |
| Certification tests | Repeatable evidence for supported claims. |

---

## Lane Placement

| Test Type | Feature Lane | PR Gate | Main Gate | Platform E2E |
|---|---:|---:|---:|---:|
| Unit | Yes | Yes | Optional | No |
| Domain | Yes | Yes | Optional | No |
| Contract | Yes | Yes | Yes | Conditional |
| Integration | Optional | Yes | Yes | Conditional |
| E2E | No | Conditional | Yes | Yes |
| Browser | No | Conditional | Yes | Yes |
| Migration | Optional | Yes | Yes | No |
| Docker smoke | Optional | Yes | Yes | Conditional |
| Certification | No | Optional | Yes | Yes |

---

## Certification Sweeps

A certification sweep should:

- use deterministic synthetic data
- call supported endpoints
- assert expected values
- validate supportability
- generate machine-readable output
- avoid sensitive data
- state what is not certified

Certification is useful for:

- demo readiness
- release proof
- product claim validation
- regression baseline
- support evidence

---

## Test Gate Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Only unit tests | Integration failures missed. |
| Only E2E tests | Slow, brittle, hard to diagnose. |
| Coverage without meaningful assertions | False confidence. |
| Certification uses unstable data | Evidence unreliable. |
| No contract tests | Consumers discover drift. |
| No migration tests | Release failures. |
| Browser screenshots without API proof | Decorative evidence. |

---

## Review Checklist

- Are changed behaviors tested?
- Are tests at the right level?
- Are contract changes protected?
- Are integration seams tested?
- Are degraded states tested?
- Are migration/runtime changes tested?
- Is certification evidence deterministic?
- Are test artifacts safe?
- Are tests placed in correct lane?
- Are flaky tests investigated rather than ignored?

---

## Summary

Good test gates create confidence by proving the right thing at the right level.

The goal is not more tests. The goal is better evidence.

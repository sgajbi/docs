# Backend Developer and Senior Engineer Learning Path

## Purpose

This file provides a learning path for backend developers and senior engineers.

The goal is to move from feature implementation to enterprise-grade service ownership.

---

## Core Packs

Read in this order:

1. Backend Architecture and Service Design
2. API Design, Governance, and Contract Engineering
3. Testing, Quality Engineering, and Certification Strategy
4. Git Workflow, PR Review, and Release Hygiene
5. CI/CD, DevSecOps, Quality Gates, and Release Evidence
6. Performance, Scalability, Resilience, and Async Engineering
7. Security, Configuration, Secrets, and Cyber Resilience Controls
8. Observability, SRE, Runbooks, and Operational Supportability

---

## Skills To Build

| Skill | Practice |
|---|---|
| Clean architecture | Separate controller, application, domain, ports, infrastructure. |
| API design | Define contract, errors, pagination, idempotency, OpenAPI. |
| Testing | Add domain, use-case, API, contract, integration, regression tests. |
| Async workflows | Implement durable jobs, idempotency, retries, replay, dead-letter. |
| Observability | Add metrics, logs, traces, supportability states. |
| Security | Avoid secrets, enforce authorization, protect sensitive outputs. |
| Delivery hygiene | Small commits, strong PR descriptions, evidence-backed changes. |
| Production thinking | Add runbooks, readiness checks, rollback notes. |

---

## Practical Exercises

1. Refactor a controller to call an application use case.
2. Add RFC 7807-style problem details to one API.
3. Add cursor/keyset pagination to a large list API.
4. Add idempotency to one write workflow.
5. Add structured logs and metrics for one use case.
6. Add integration tests for repository/database behavior.
7. Add an OpenAPI quality check.
8. Add a runbook for one failure scenario.

---

## Senior Engineer Growth

A senior engineer should start reviewing:

- service boundaries
- API compatibility
- data ownership
- failure modes
- performance risks
- security risks
- CI/CD gaps
- operational supportability
- documentation truth

---

## PR Review Questions

- Is the change in the right layer?
- Is the contract safe?
- Are error paths tested?
- Is idempotency needed?
- Are logs/metrics safe?
- Is data sensitive?
- Will this scale?
- Is documentation updated?
- Can support diagnose failure?

---

## Summary

A strong backend engineer builds services that are correct, testable, secure, observable, and supportable.

The goal is not only to make code work; it is to make services trustworthy.

# Application Use Case, Workflow, and Service Layer Testing

## Purpose

This file explains how to test application use cases and workflow orchestration.

The application layer coordinates domain logic, ports, transactions, idempotency, and external interactions. It should be testable without real infrastructure through fake ports.

---

## What To Test

Test use cases for:

- successful path
- invalid command
- permission blocked
- dependency unavailable
- stale source data
- partial source data
- idempotency duplicate
- idempotency conflict
- event/outbox creation
- transaction decision
- replay eligibility
- supportability result
- retry/dead-letter decision

---

## Fake Ports

Use fake implementations for:

- repository
- upstream client
- event publisher
- clock
- id generator
- object store
- cache
- authorization policy

Fakes should be simple and behavior-focused.

---

## Command and Query Testing

Command tests should assert:

- state changes
- domain policy invocation
- idempotency behavior
- emitted events
- audit intent
- result status

Query tests should assert:

- filtering
- pagination
- authorization scope
- supportability
- stale/partial data behavior

---

## Workflow Testing

Workflow tests should cover:

- start
- intermediate states
- completion
- retryable failure
- final failure
- replay
- cancellation
- retention
- recovery after restart where applicable

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Use case tested only through HTTP | Slow and harder to diagnose. |
| Use case directly mocks internal methods | Brittle tests. |
| External dependency used in use-case test | Flaky tests. |
| Idempotency untested | Duplicate side effects. |
| Replay not tested | Recovery fails. |
| Fakes too complex | Test behavior diverges from reality. |

---

## Review Checklist

- Are use cases directly tested?
- Are fake ports used?
- Are command and query paths covered?
- Is idempotency tested?
- Are failure modes tested?
- Are workflow states tested?
- Are events/outbox records asserted?
- Are supportability states asserted?
- Are tests deterministic?

---

## Summary

Application tests prove orchestration.

They give strong confidence without paying the full cost of infrastructure-heavy integration tests.

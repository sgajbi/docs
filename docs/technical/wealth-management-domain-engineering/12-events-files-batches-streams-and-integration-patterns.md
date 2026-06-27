# Events, Files, Batches, Streams, and Integration Patterns

## Purpose

This file explains integration patterns common in wealth platforms.

Wealth systems often integrate legacy sources, market data vendors, booking systems, analytics engines, reporting engines, and digital channels.

---

## Integration Patterns

| Pattern | Use |
|---|---|
| REST API | On-demand query or command. |
| Event stream | Change notification and near-real-time integration. |
| File/MFT | Legacy or bulk daily transfer. |
| Batch job | Scheduled processing. |
| Message queue | Async processing and decoupling. |
| Snapshot | Point-in-time state for reporting/analytics. |
| Data product | Governed reusable data contract. |

---

## File Integration

File integration should define:

- file name pattern
- schedule
- source owner
- schema
- encryption
- checksum
- record count
- retry/replay
- reconciliation
- archive/retention
- failure handling

---

## Event Integration

Events should define:

- event type
- producer
- version
- subject
- payload schema
- ordering expectation
- idempotency
- consumer group
- retry/dead-letter
- replay
- lineage

---

## Batch Integration

Batch design should include:

- batch id
- business date
- dependency readiness
- item states
- partial success
- reconciliation
- evidence
- rerun rules
- support runbook

---

## Integration Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| File arrives but no checksum validation | Corruption undetected. |
| Event has no version | Consumer breakage. |
| Batch has no item-level status | Recovery hard. |
| API used for huge extraction | Performance issue. |
| Retry not idempotent | Duplicate data. |
| No replay | Manual repair. |
| Integration failure not observable | Support gap. |

---

## Review Checklist

- Which integration pattern is appropriate?
- Is contract documented?
- Is schema versioned?
- Is encryption/access handled?
- Is idempotency defined?
- Is retry/replay defined?
- Is reconciliation defined?
- Is failure observable?
- Is lineage captured?
- Is runbook available?

---

## Summary

Integration is where wealth platforms meet reality: legacy systems, vendors, files, events, and operational timing.

Design integration as a governed contract.

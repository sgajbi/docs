# API, Event, File, Batch, and Integration Migration

## Purpose

This file explains migration patterns for integration surfaces.

Enterprise migrations often fail at interfaces: APIs, events, files, batch jobs, and downstream consumers.

---

## Integration Inventory

Inventory:

- API endpoints
- event topics
- file feeds
- batch schedules
- message queues
- downstream consumers
- upstream dependencies
- authentication methods
- schemas/contracts
- data volumes
- SLA/frequency
- owner

---

## API Migration

Plan:

- old API contract
- new API contract
- compatibility layer
- versioning
- consumer migration
- contract tests
- deprecation timeline
- traffic switch
- fallback

---

## Event Migration

Plan:

- topic/channel mapping
- schema versioning
- producer switch
- consumer groups
- replay
- dead-letter
- duplicate handling
- ordering requirements
- parallel publication if needed

---

## File and Batch Migration

Plan:

- file naming
- schema mapping
- encryption
- delivery location
- schedule
- checksum
- record count
- retry/replay
- consumer readiness
- decommission old feed

---

## Integration Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Downstream consumers not inventoried | Breakage after cutover. |
| API compatibility ignored | Consumer migration delayed. |
| Event schema changed silently | Consumer failures. |
| File checksum missing | Corruption undetected. |
| Batch schedule not aligned | Missed processing windows. |
| Auth method changed late | Integration blocker. |
| No replay plan | Recovery manual. |

---

## Review Checklist

- Are all interfaces inventoried?
- Are owners known?
- Are contracts versioned?
- Are consumers migrated?
- Are compatibility layers needed?
- Are schemas tested?
- Are schedules aligned?
- Is retry/replay defined?
- Is decommission plan clear?
- Is integration evidence captured?

---

## Summary

Integration migration is contract migration.

Treat every interface as a product with consumers, versioning, tests, and cutover evidence.

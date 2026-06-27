# API, Event, and Batch Contracts for Data Products

## Purpose

This file explains how data products can be served through APIs, events, batch files, snapshots, and catalogs.

A data product may have multiple serving surfaces, but all surfaces should preserve the same ownership, semantics, lineage, trust, and access model.

---

## Serving Patterns

| Pattern | Use |
|---|---|
| Query API | On-demand reads and UI/BFF integration. |
| Event | Lifecycle notification and downstream propagation. |
| Batch file | Large periodic transfer or legacy integration. |
| Snapshot | Point-in-time state for reporting or analytics. |
| Stream | Near-real-time changes. |
| Catalog entry | Discovery and metadata. |

---

## Query API Contract

A data-product API should define:

- product identity
- version
- request filters
- pagination
- response schema
- supportability
- lineage
- freshness
- access behavior
- error model
- examples

---

## Event Contract

A data-product event should define:

- event type
- version
- producer
- subject
- occurredAt
- payload schema
- lineage reference
- correlation id
- ordering expectations
- duplicate handling
- replay policy

---

## Batch Contract

A batch data product should define:

- file name pattern
- frequency
- schema
- header/trailer if any
- checksum
- record count
- encryption
- delivery location
- retention
- retry/replay
- reconciliation evidence
- access controls
- failure handling

---

## Snapshot Contract

A snapshot should define:

- snapshot id
- asOfDate/business date
- generatedAt
- source inputs
- schema version
- transformation version
- lineage bundle
- completeness status
- quality status
- retention

---

## API/Event Consistency

If the same product is exposed through API and event:

- field semantics should match
- product version should align
- quality states should align
- lineage should be comparable
- consumer contract should specify which surface is authoritative for which use

---

## Contract Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| API and file use same field name differently | Consumer confusion. |
| Event has no version | Compatibility risk. |
| Batch file lacks checksum/count | Reconciliation weak. |
| Snapshot lacks business date | Reporting ambiguity. |
| API exposes lineage but event does not | Inconsistent trust posture. |
| Catalog says product certified but API lacks evidence | False confidence. |
| Raw source file treated as product | No governed contract. |

---

## Review Checklist

- Which serving surfaces exist?
- Is product identity consistent?
- Is version clear?
- Are schemas aligned?
- Are lineage and freshness included?
- Are quality states included?
- Is access model consistent?
- Is pagination/batching defined?
- Are retries and replay defined?
- Are contracts tested?
- Are examples synthetic?

---

## Summary

A data product can be delivered through many technical surfaces. The governance contract must remain consistent across all of them.

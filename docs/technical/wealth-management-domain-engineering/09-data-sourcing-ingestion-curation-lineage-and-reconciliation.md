# Data Sourcing, Ingestion, Curation, Lineage, and Reconciliation

## Purpose

This file explains source-data engineering patterns in wealth platforms.

Source data must be ingested, curated, reconciled, and lineage-backed before downstream analytics and reports can trust it.

---

## Source Systems

Common source categories:

- client master
- account master
- portfolio master
- holdings/positions source
- transaction source
- instrument master
- market data
- price source
- FX source
- benchmark source
- advisory workflow source
- document/archive source

---

## Ingestion

Ingestion can use:

- API
- event stream
- file/MFT
- batch database extract
- message queue
- manual controlled upload
- vendor feed

Ingestion should define:

- source owner
- schedule/frequency
- schema
- validation
- retry/replay
- idempotency
- lineage
- error handling
- runbook

---

## Curation

Curation transforms raw source data into domain-ready data.

Curation may include:

- normalization
- enrichment
- validation
- deduplication
- effective dating
- classification
- currency normalization
- source-priority handling
- error quarantine

---

## Reconciliation

Reconciliation checks source and curated data agree.

Examples:

- record count
- total market value
- cash balance
- transaction count
- position quantity
- benchmark constituent total weight
- file checksum
- batch hash

---

## Lineage

Lineage should capture:

- source system
- source timestamp
- business date
- ingestion batch id
- transformation version
- reconciliation result
- evidence reference

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Raw source directly used by UI | Governance and quality risk. |
| No ingestion idempotency | Duplicate data. |
| Curation rules undocumented | Hard to explain. |
| Reconciliation only manual | Late breaks. |
| Backdated data not replayed | Historical analytics wrong. |
| Lineage missing | Reports cannot be defended. |
| Failed records silently dropped | Data loss. |

---

## Review Checklist

- Is source owner clear?
- Is ingestion method documented?
- Is schema validated?
- Is ingestion idempotent?
- Are curation rules documented?
- Is reconciliation defined?
- Are failures quarantined?
- Is lineage captured?
- Is replay supported?
- Are dashboards/runbooks available?

---

## Summary

Trusted analytics require trusted source-data engineering.

Ingestion, curation, reconciliation, and lineage are core platform capabilities.

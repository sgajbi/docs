# Report Data Snapshots, Lineage, and Reproducibility

## Purpose

This file explains how reports should capture data snapshots and lineage.

A report should be explainable after generation. This requires knowing exactly which data and methodology were used.

---

## Report Snapshot

A report snapshot captures the data context for generation.

It may include:

- report snapshot id
- business date
- generated timestamp
- portfolio/account/client scope
- source data versions
- analytics result ids
- benchmark version
- price/FX timestamp
- template version
- warnings and quality states

---

## Why Reproducibility Matters

Reproducibility supports:

- client queries
- audit review
- dispute handling
- migration validation
- regression testing
- report regeneration
- operational support
- archive continuity

---

## Lineage Fields

Report lineage should capture:

- source service
- source version
- source business date
- source generated timestamp
- calculation version
- input data references
- reconciliation status
- evidence references
- supportability states

---

## Business Date Versus Generated Timestamp

Keep these separate:

| Field | Meaning |
|---|---|
| Business date | Date the report data represents. |
| Generated timestamp | When the report was produced. |
| Source timestamp | When the source data was produced. |
| Valuation date | Date used for valuation. |
| Snapshot timestamp | When report snapshot was captured. |

---

## Immutable Report Package

A report package may include:

- content model
- snapshot metadata
- lineage
- warnings
- template version
- render options
- evidence references

Rendering from an immutable package improves reproducibility.

---

## Snapshot Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Report generated from live APIs without snapshot | Output cannot be reproduced. |
| Business date omitted | Report meaning unclear. |
| Template version omitted | Layout/content dispute hard to resolve. |
| Warnings not stored | Later explanation incomplete. |
| Price/FX version not captured | Valuation not explainable. |
| Snapshot includes sensitive data unnecessarily | Storage risk. |

---

## Review Checklist

- Is report snapshot required?
- Is business date captured?
- Are source versions captured?
- Are analytics versions captured?
- Are price/FX/benchmark references captured?
- Is template version captured?
- Are warnings stored?
- Can report be regenerated?
- Is snapshot access controlled?
- Is evidence retained?

---

## Summary

A report without snapshot and lineage is only an output.

A report with snapshot and lineage is evidence.

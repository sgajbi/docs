# Intraday, EOD, BOD, Snapshots, and Temporal Data Patterns

## Purpose

This file explains temporal data concepts that are critical in wealth platforms.

Many production issues come from confusing business date, source timestamp, valuation timestamp, generated timestamp, BOD, EOD, and intraday state.

---

## Key Temporal Terms

| Term | Meaning |
|---|---|
| Business date | Date for which data is valid in business process. |
| Generated timestamp | When the system generated output. |
| Source timestamp | When source produced or updated data. |
| Valuation date | Date/time used for portfolio valuation. |
| BOD | Beginning of day state. |
| EOD | End of day state. |
| Intraday | State updated during the business day. |
| Snapshot | Point-in-time captured state. |
| Restatement | Recomputed historical result due to correction. |

---

## BOD Versus EOD

BOD state may be used for:

- starting holdings
- portfolio opening value
- previous close
- benchmark opening weights
- risk starting point

EOD state may be used for:

- official valuations
- performance reporting
- reconciliation
- statement generation
- daily analytics

---

## Intraday Data

Intraday data may include:

- orders
- trades
- cash movements
- price updates
- exposure changes
- advisory workflow state
- buying power or availability
- risk alerts

Intraday data may be incomplete or provisional.

---

## Snapshot Design

Snapshots should capture:

- snapshot id
- business date
- generated timestamp
- source versions
- input data versions
- quality state
- lineage
- completeness
- supportability

---

## Temporal Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| GeneratedAt used as business date | Reporting error. |
| Intraday provisional data shown as official | Misleading output. |
| EOD and BOD mixed without label | Calculation inconsistency. |
| Snapshot not retained | Report not reproducible. |
| Backdated correction not restated | Historical result wrong. |
| Time zone ignored | Cross-region errors. |
| Stale state hidden | User over-trust. |

---

## Review Checklist

- Which date does this data represent?
- Is source timestamp captured?
- Is generated timestamp captured?
- Is BOD/EOD/intraday clearly labelled?
- Is snapshot needed?
- Is restatement supported?
- Are time zones defined?
- Are stale states visible?
- Is temporal logic tested?

---

## Summary

Temporal correctness is domain correctness.

Always distinguish business date, source timestamp, generated timestamp, and valuation time.

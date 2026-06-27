# Reporting, Document Generation, Archive, and Client Evidence

## Purpose

This file explains reporting and document-generation concepts in wealth platforms.

Reports are client-facing evidence. They must be reproducible, controlled, and archived where required.

---

## Reporting Lifecycle

```text
report request
  -> data snapshot selection
  -> analytics retrieval
  -> template selection
  -> rendering
  -> validation
  -> approval where required
  -> document generation
  -> archive
  -> delivery or retrieval
  -> audit/evidence
```

---

## Report Types

Examples:

- portfolio review
- performance report
- client statement
- holdings report
- transaction report
- income summary
- risk report
- proposal document
- mandate report
- factsheet

---

## Report Evidence

A report should capture:

- report id
- template version
- data snapshot
- source business date
- generated timestamp
- requested by
- approved by where needed
- analytics versions
- warnings
- supportability state
- document hash
- archive reference

---

## Archive

Archive concerns:

- document metadata
- retention
- access control
- immutable reference
- retrieval audit
- legal hold
- deletion/expiry
- versioning
- rendering evidence

---

## Report Supportability

A report should not be generated as if complete when:

- source data is stale
- required valuation missing
- performance calculation failed
- user lacks entitlement
- template unsupported
- document render failed
- data quality/reconciliation failed

---

## Reporting Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Report generated from live changing data without snapshot | Non-reproducible report. |
| Template version not captured | Hard to explain output. |
| Archive stores PDF only, no metadata | Weak retrieval/audit. |
| Report hides stale data | Client trust risk. |
| UI produces official PDF locally | Control and evidence risk. |
| No document hash | Integrity weak. |
| No runbook for failed report jobs | Support gap. |

---

## Review Checklist

- Is report snapshot defined?
- Is template version captured?
- Are analytics versions captured?
- Are stale/partial states handled?
- Is archive metadata complete?
- Is access control enforced?
- Is document hash stored?
- Is retention defined?
- Is report generation idempotent?
- Are golden reports tested?

---

## Summary

Reporting is evidence engineering.

A report must be explainable after it is generated, not only visually correct at generation time.

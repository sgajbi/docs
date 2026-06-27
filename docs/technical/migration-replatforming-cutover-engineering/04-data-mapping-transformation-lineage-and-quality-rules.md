# Data Mapping, Transformation, Lineage, and Quality Rules

## Purpose

This file explains how to define data mappings and transformation logic for migration.

Data migration must preserve business meaning, not just move columns.

---

## Data Mapping Catalogue

A mapping catalogue should include:

```text
source field
target field
business meaning
transformation rule
default rule
validation rule
owner
quality check
exception handling
lineage reference
```

---

## Mapping Types

| Mapping Type | Example |
|---|---|
| Direct | Source field copied to target field. |
| Transformed | Currency, format, or enum converted. |
| Derived | Target field calculated from multiple source fields. |
| Defaulted | Target field set by rule when source absent. |
| Split | One source field mapped to multiple target fields. |
| Merged | Multiple source fields merged into one target. |
| Dropped | Source field not migrated, with reason. |

---

## Transformation Rules

Rules should define:

- input
- output
- logic
- owner
- version
- examples
- edge cases
- invalid cases
- test cases

---

## Lineage

Capture:

- source system
- source record id
- target record id
- migration batch id
- transformation version
- migration timestamp
- validation result
- exception status

---

## Data Quality Rules

Quality rules may include:

- required fields
- type validation
- enum validation
- referential integrity
- duplicates
- balance/total reconciliation
- date validity
- currency validity
- entitlement mapping
- report continuity

---

## Mapping Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Mapping only in spreadsheet with no version control | Drift and audit risk. |
| Defaults undocumented | Business meaning unclear. |
| Dropped fields not explained | Later dispute. |
| Transformation rules not tested | Data corruption. |
| No lineage | Cannot trace migration. |
| Business meanings not mapped | Semantic errors. |

---

## Review Checklist

- Is mapping catalogue complete?
- Are transformation rules documented?
- Are defaults justified?
- Are dropped fields approved?
- Are examples included?
- Are validation rules defined?
- Is lineage captured?
- Are mappings versioned?
- Are tests tied to mappings?
- Are owners assigned?

---

## Summary

Data mapping is business translation.

If mapping rules are unclear, migrated data may be technically valid but business-wrong.

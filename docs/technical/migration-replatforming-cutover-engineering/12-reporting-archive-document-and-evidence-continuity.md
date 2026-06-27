# Reporting, Archive, Document, and Evidence Continuity

## Purpose

This file explains how to preserve reporting and archive continuity during migration.

Historical reports, documents, and evidence may remain legally, operationally, and commercially important after cutover.

---

## Continuity Scope

Include:

- generated reports
- report metadata
- document binaries
- archive references
- report templates
- evidence bundles
- document hashes
- retention classes
- legal holds
- access-control metadata
- download audit history
- old-to-new document id mapping

---

## Report Continuity

Validate:

- old reports retrievable
- new reports generated
- historical report metadata preserved
- template versions mapped
- report evidence preserved
- document hashes validated
- report access-control preserved

---

## Archive Migration

Archive migration should define:

- source archive
- target archive
- migration method
- metadata mapping
- binary validation
- hash verification
- retention/legal hold preservation
- access-control mapping
- fallback retrieval

---

## Evidence Continuity

Evidence should preserve:

- report job id
- source snapshot
- template version
- generated timestamp
- business date
- document hash
- approval/audit trail
- warnings
- supportability state

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Migrate PDFs but not metadata | Search/audit weak. |
| Legal hold ignored | Legal risk. |
| Document IDs not mapped | Broken links. |
| Historical evidence dropped | Reports not explainable. |
| Access model not migrated | Data leakage or blocked users. |
| Archive retrieval untested | Go-live surprise. |
| Retention reset incorrectly | Compliance concern. |

---

## Review Checklist

- Are historical reports in scope?
- Is archive metadata mapped?
- Are document hashes verified?
- Are retention/legal holds preserved?
- Is access-control migrated?
- Is evidence migrated?
- Is retrieval tested?
- Is fallback retrieval available?
- Is continuity sign-off defined?

---

## Summary

Archive continuity is not optional when historical documents remain business evidence.

Migration must preserve documents, metadata, access, retention, and explainability.

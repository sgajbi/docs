# Migration, Archive Continuity, and Historical Report Portability

## Purpose

This file explains migration and continuity concerns for reports and archives.

Historical reports often need to remain retrievable and explainable after platform migration or replatforming.

---

## Migration Concerns

Report/archive migration may include:

- document binaries
- archive metadata
- report job history
- templates
- evidence bundles
- document hashes
- access-control mappings
- client/account/portfolio identifiers
- retention/legal hold state
- download history
- storage location changes

---

## Continuity Requirements

Continuity should prove:

- old documents retrievable
- metadata preserved
- access control still works
- retention still valid
- legal holds preserved
- hashes/integrity preserved
- document ids mapped
- historical reports explainable
- user experience supported

---

## Migration Mapping

Define mapping for:

- old document id to new document id
- old client/account/portfolio id to new id
- old report type to new report type
- old template id to new template id
- old archive location to new location

---

## Parallel Validation

Validate:

- document count
- metadata count
- hash comparison
- random sample retrieval
- entitlement sample
- retention classes
- legal hold records
- broken document references
- report job history

---

## Archive Migration Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Migrate PDFs only, no metadata | Search/access/audit weak. |
| Lose legal hold state | Legal risk. |
| Document ids change without mapping | Broken links. |
| Access model not migrated | Data leakage or blocked users. |
| Hash not validated | Integrity weak. |
| Historical report evidence ignored | Client queries hard to answer. |
| Retention reset incorrectly | Compliance risk. |

---

## Review Checklist

- Is archive migration in scope?
- Are documents and metadata mapped?
- Are access rules migrated?
- Are retention/legal holds preserved?
- Are hashes validated?
- Are historical links preserved?
- Are samples tested?
- Is rollback defined?
- Is continuity evidence generated?
- Is support prepared?

---

## Summary

Archive continuity is business continuity.

A migrated system must preserve not only documents, but their meaning, access, retention, and evidence.

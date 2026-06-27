# Report Evidence, Audit Trail, Hash, and Integrity Controls

## Purpose

This file explains how to capture evidence for generated reports.

Evidence allows future reviewers to understand how a report was produced, from what data, by which process, and with what integrity controls.

---

## Evidence Bundle

A report evidence bundle may include:

- report job id
- request payload hash
- requested by
- caller context
- authorization result
- data snapshot id
- source versions
- analytics result ids
- template version
- renderer version
- warnings
- supportability states
- generated timestamp
- document hash
- archive id
- audit events
- validation results

---

## Audit Trail

Audit events may include:

- report requested
- report authorized
- data collected
- rendering started
- rendering completed
- archive stored
- report downloaded
- replay requested
- cancellation requested
- failure occurred

---

## Document Hash

A document hash supports integrity.

Capture:

- hash algorithm
- hash value
- generated timestamp
- file size
- document id
- archive reference

---

## Explainability

Report evidence should help answer:

- why does this number appear?
- which data was used?
- was data stale?
- which methodology/version was used?
- who generated the report?
- who downloaded it?
- was it modified after generation?

---

## Evidence Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Audit only in logs | Hard to retain/search. |
| No document hash | Integrity weak. |
| Evidence missing source versions | Report not explainable. |
| Request payload stored raw with sensitive data | Privacy risk. |
| Download not audited | Confidentiality gap. |
| Evidence not linked to archive | Traceability weak. |
| Replay overwrites old evidence | History loss. |

---

## Review Checklist

- Is evidence bundle defined?
- Are audit events captured?
- Is document hash generated?
- Are source versions captured?
- Are warnings captured?
- Is access/download audited?
- Is evidence access-controlled?
- Is evidence retained?
- Can report be explained later?
- Are evidence tests present?

---

## Summary

Report evidence is the memory of report generation.

Without evidence, a report is difficult to defend, reproduce, or investigate.

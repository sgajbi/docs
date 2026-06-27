# Document Storage, Archive Metadata, Retention, and Legal Hold

## Purpose

This file explains archive and document lifecycle patterns.

In regulated environments, generated reports and documents often require controlled storage, retrieval, retention, deletion, and audit.

---

## Archive Responsibilities

Archive capability should own:

- document metadata
- document binary/object reference
- storage location abstraction
- access control
- retrieval audit
- retention policy
- legal hold
- deletion/expiry
- integrity/hash
- archive status
- document versioning

---

## Archive Metadata

Metadata should include:

```text
document id
report job id
report type
client/account/portfolio scope
business date
generated timestamp
template version
content model version
document hash
file type
file size
retention class
legal hold state
created by
access classification
archive status
```

---

## Retention

Retention policy should define:

- retention duration
- retention class
- start date
- deletion process
- legal hold override
- audit requirements
- jurisdiction considerations
- archive owner

---

## Legal Hold

Legal hold prevents deletion even when normal retention expires.

Define:

- who can place hold
- who can remove hold
- audit evidence
- affected document scope
- notification
- review cadence

---

## Retrieval

Retrieval should be:

- authorized
- audited
- time-bound if using signed URLs
- source-safe
- logged without sensitive leakage
- monitored

---

## Archive Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Store PDF without metadata | Retrieval and audit weak. |
| Direct object-store link exposed | Access-control bypass. |
| No retention policy | Compliance and cost risk. |
| Legal hold not supported | Legal/audit risk. |
| Document hash missing | Integrity weak. |
| Archive deletion manual only | Operational risk. |
| Access logs missing | Investigation gap. |

---

## Review Checklist

- Is archive required?
- Is metadata complete?
- Is retention defined?
- Is legal hold needed?
- Is document hash generated?
- Is retrieval access-controlled?
- Is access audited?
- Is deletion/expiry governed?
- Is archive searchable?
- Are runbooks available?

---

## Summary

Archive is not just storage.

It is controlled evidence retention with access, integrity, lifecycle, and audit.

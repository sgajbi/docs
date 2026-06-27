# Data Protection, Encryption, Key Management, and Retention

## Purpose

This file explains data protection controls for enterprise platforms.

Data protection covers how data is encrypted, masked, retained, deleted, archived, and recovered.

---

## Encryption

Data should be protected:

- in transit
- at rest
- in backups
- in archives
- in object stores
- in database storage
- in message brokers where applicable

Transport encryption typically uses TLS. At-rest encryption depends on database, storage, and platform controls.

---

## Key Management

Key management should define:

- key owner
- key store
- rotation policy
- access policy
- audit trail
- backup/recovery
- emergency revocation
- separation of duties

Do not hardcode keys in source or config.

---

## Masking and Redaction

Use masking/redaction for:

- logs
- diagnostics
- screenshots
- support bundles
- customer evidence
- lower environments
- test fixtures

---

## Retention

Retention policy defines:

- what data is kept
- how long
- why
- where
- who can access
- deletion/expiry process
- legal hold behavior
- audit requirements

---

## Archive and Legal Hold

Archive systems may need:

- immutable records
- retention period
- access audit
- legal hold
- retrieval controls
- deletion restrictions
- evidence of generation and delivery

---

## Data Protection Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Encryption assumed but not evidenced | Control gap. |
| Keys stored with data | Weak protection. |
| No key rotation | Long-term exposure. |
| Retention undefined | Compliance and cost risk. |
| Support bundles include raw data | Data leakage. |
| Lower env has real data | Privacy risk. |
| Archive deletion not controlled | Legal/audit risk. |

---

## Review Checklist

- Is data classification defined?
- Is encryption in transit enabled?
- Is encryption at rest enabled?
- Are keys managed securely?
- Is rotation defined?
- Is masking/redaction used?
- Is retention defined?
- Is deletion process defined?
- Is archive/legal hold considered?
- Is evidence available?

---

## Summary

Data protection is a lifecycle discipline.

It covers data from creation to access, storage, observation, archive, and deletion.

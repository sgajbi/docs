# Sensitive Data Classification, Privacy, Masking, and Tokenization

## Purpose

This file explains how to classify and protect sensitive data.

Financial platforms handle client, account, portfolio, transaction, advisory, report, and operational data. Each class needs handling rules.

---

## Data Classification

| Class | Examples |
|---|---|
| Public | Public documentation or generic product information. |
| Internal | Non-sensitive architecture or process details. |
| Restricted | Internal source paths, operational evidence, config posture. |
| Confidential | Client, account, portfolio, transaction, holdings, advisory, report data. |
| Secret | Credentials, tokens, private keys, passwords. |

---

## Sensitive Data Locations

Sensitive data may appear in:

- database rows
- API responses
- event payloads
- logs
- metrics
- traces
- screenshots
- reports
- archives
- CI artifacts
- test fixtures
- AI prompts
- model outputs
- runbooks
- incident tickets

---

## Masking

Masking hides parts of a value.

Example:

```text
accountNumber: ****1234
email: s***@example.com
```

Use masking for display when partial identity is needed and approved.

---

## Tokenization

Tokenization replaces sensitive value with a token.

Use when systems need to reference sensitive data without storing the original value.

---

## Synthetic Data

Synthetic data should be used for:

- tests
- examples
- demos
- docs
- certification sweeps
- screenshots
- AI prompt examples

Synthetic data should be deterministic and realistic without being real.

---

## Privacy Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Real client data in tests | Privacy breach. |
| Portfolio id in metric label | Sensitive and high-cardinality. |
| Raw request body in logs | Confidential data exposure. |
| Screenshots contain real data | Distribution risk. |
| AI prompt includes restricted data | Data leakage and governance risk. |
| Evidence pack exposes internal source paths | Security risk. |
| Masking done only in UI | Backend artifacts may leak. |

---

## Review Checklist

- Is data classified?
- Are handling rules defined?
- Are logs safe?
- Are metrics safe?
- Are traces safe?
- Are screenshots safe?
- Are test fixtures synthetic?
- Are AI prompts governed?
- Are evidence packs filtered?
- Are retention and deletion rules defined?

---

## Summary

Sensitive data protection is not one feature.

It is a discipline across every place data can appear.

# Security, Privacy, and Sensitive Data in Documentation

## Purpose

This file explains how to keep documentation, examples, screenshots, diagrams, and evidence safe.

Documentation can leak sensitive information if not governed.

---

## Sensitive Documentation Content

Avoid exposing:

- client names
- account numbers
- portfolio identifiers
- transaction details
- holdings
- secrets
- tokens
- connection strings
- internal source paths
- raw entitlement rules
- raw logs
- prompts/model responses
- production screenshots
- restricted architecture details in public docs

---

## Safe Examples

Use:

- synthetic identifiers
- generic hostnames
- redacted values
- placeholder tokens
- simplified diagrams
- fake clients/accounts
- safe screenshots
- approved evidence summaries

---

## Internal Versus External Docs

Classify docs:

| Classification | Audience |
|---|---|
| Public | Safe for external publication. |
| Internal | General employee/internal audience. |
| Restricted | Engineering/support/security only. |
| Confidential | Controlled access, sensitive content. |

---

## Evidence Filtering

Evidence packs may need filtering before sharing.

Remove:

- secrets
- internal paths
- restricted identifiers
- raw logs
- raw payloads
- account/client data
- prompt/response text
- vulnerability details not intended for recipient

---

## Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Real data in screenshots | Privacy issue. |
| `.env` examples with real secrets | Credential leak. |
| Internal URLs in public docs | Reconnaissance risk. |
| Raw incident logs pasted into docs | Sensitive leakage. |
| AI prompt examples with restricted data | Data leak. |
| Public docs include architecture internals | Security exposure. |

---

## Review Checklist

- Is document classification clear?
- Are examples synthetic?
- Are screenshots safe?
- Are secrets absent?
- Are internal paths removed?
- Are raw logs removed?
- Are prompts/responses governed?
- Is evidence filtered?
- Is audience appropriate?
- Are sensitive-content checks run?

---

## Summary

Documentation security is part of platform security.

A document can leak as much as a log file or API response.

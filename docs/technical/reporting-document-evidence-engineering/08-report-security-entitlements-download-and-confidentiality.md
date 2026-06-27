# Report Security, Entitlements, Download, and Confidentiality

## Purpose

This file explains security controls for reports and documents.

Reports often contain highly confidential client, account, portfolio, holdings, performance, risk, and advisory data.

---

## Access Control

Report access should consider:

- user identity
- role
- client scope
- account scope
- portfolio scope
- relationship scope
- booking center
- jurisdiction
- report type
- document classification
- purpose
- workflow state

---

## Secure Download

Download should be:

- authorized
- audited
- mediated through service or controlled signed URL
- time-limited where applicable
- not expose raw storage paths
- log safe event
- emit bounded metrics

---

## Permission-Blocked State

If access is denied:

- return safe 403 or section-level blocked state
- do not reveal hidden client/document details
- do not expose raw entitlement rule
- log safe audit event
- show user-safe message

---

## Confidentiality

Protect:

- document binary
- archive metadata
- report evidence
- generated screenshots
- email attachments
- download URLs
- support bundles
- logs and traces

---

## Report Security Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Direct object-store URL exposed permanently | Data leakage. |
| Report download not audited | Investigation gap. |
| UI hides button but API allows download | Authorization bypass. |
| Report metadata leaks client names | Confidentiality breach. |
| Logs contain report payload | Sensitive-data leak. |
| Signed URL never expires | Access risk. |
| Support bundle includes PDF | Distribution risk. |

---

## Review Checklist

- Is access enforced server-side?
- Is document scope checked?
- Is download audited?
- Are URLs time-bound?
- Are raw storage paths hidden?
- Are logs/metrics safe?
- Are permission-denied responses safe?
- Are report metadata fields classified?
- Are security tests present?
- Are support bundles filtered?

---

## Summary

Report security is client confidentiality engineering.

Every generated document must be treated as sensitive unless proven otherwise.

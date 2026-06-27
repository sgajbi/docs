# Security, Privacy Incident, and Sensitive-Data Support Controls

## Purpose

This file explains how production support should handle security, privacy, and sensitive data.

Support teams often need diagnostics, but diagnostics must not become data leakage.

---

## Security Incident Types

Examples:

- unauthorized access
- suspicious login
- token/secret leakage
- data exposure in logs
- report/document access issue
- privilege escalation
- malware/compromise signal
- third-party breach notification
- AI prompt/context leakage
- privacy request mishandling

---

## Sensitive Support Data

Support must protect:

- client identifiers
- account numbers
- portfolio ids
- holdings
- transactions
- report documents
- entitlement details
- secrets
- tokens
- internal security rules
- raw logs with sensitive payloads
- AI prompts/responses

---

## Support Access Controls

Support access should be:

- least privilege
- time-bound where appropriate
- audited
- approved for privileged action
- scoped by environment
- scoped by customer/client where relevant
- reviewed regularly

---

## Safe Diagnostics

Use:

- correlation ids
- safe job ids
- redacted logs
- bounded metrics
- synthetic reproductions
- filtered support bundles
- diagnostic summaries
- controlled evidence export

Avoid copying raw production data into tickets or chats.

---

## Security Support Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Raw logs pasted into tickets | Data leakage. |
| Shared admin account | No accountability. |
| Support has broad permanent access | Excess privilege. |
| Security incident handled as normal defect | Escalation failure. |
| Secrets in runbook | Critical risk. |
| Report PDF attached to chat | Confidentiality breach. |
| AI prompt leakage ignored | Privacy/security risk. |

---

## Review Checklist

- Are security incident paths defined?
- Are privacy incident paths defined?
- Is support access least privilege?
- Is privileged access audited?
- Are diagnostics redacted?
- Are support bundles filtered?
- Are raw sensitive logs prohibited?
- Are runbooks secret-free?
- Are escalation criteria clear?
- Are support access reviews performed?

---

## Summary

Support must be empowered but controlled.

Safe diagnostics and least-privilege access protect client confidentiality while enabling recovery.

# Security as Architecture and Business Capability

## Purpose

This file explains why security should be treated as architecture and business capability, not as a final checklist.

In financial platforms, security protects client trust, financial data, advisory records, reports, operational evidence, AI context, and institutional accountability.

---

## Security Protects Business Trust

Security protects:

- client confidentiality
- account and portfolio data
- transaction and holdings data
- advisory suitability records
- mandates and workflow decisions
- reports and documents
- data-product trust evidence
- AI prompts and retrieved context
- operational diagnostics
- audit trails
- production systems
- release pipelines

Security failure is not only a technical failure. It can become business, regulatory, operational, legal, and reputational failure.

---

## Security Is Part Of Architecture

Architecture should define:

- trust boundaries
- identity model
- caller context
- authorization decisions
- sensitive data classes
- data flow
- secrets handling
- logging boundaries
- runtime permissions
- network access
- evidence and audit
- incident response

Security should be considered before implementation, not only before release.

---

## Security Across Layers

| Layer | Security Concern |
|---|---|
| UI | Permission-safe rendering, no hidden data leakage, no client-side-only authorization. |
| BFF / gateway | Caller context, upstream authorization propagation, safe composition. |
| Domain service | Server-side authorization, data ownership, business control enforcement. |
| Shared capability | Secure document, archive, AI, rendering, and workflow boundaries. |
| Data product | Access policy, field classification, evidence filtering. |
| CI/CD | Secrets, workflow permissions, dependency scans, artifact integrity. |
| Runtime | Workload identity, RBAC, network policy, pod security. |
| Observability | Source-safe logs, metrics, traces, diagnostics, and dashboards. |
| Operations | Incident response, cyber resilience, audit evidence. |

---

## Security By Design Questions

- Who can call this?
- What can they access?
- What can they change?
- What data is sensitive?
- What should never be logged?
- What secrets are needed?
- What happens if authorization fails?
- What is the audit trail?
- What is the abuse case?
- What evidence proves the control works?
- What happens during cyber incident?

---

## Common Security Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Security reviewed only before go-live | Late rework and risk acceptance pressure. |
| UI-only access control | Backend bypass possible. |
| Raw error details returned | Information disclosure. |
| Secrets in docs or examples | Credential leakage. |
| Logs contain request bodies | Sensitive-data exposure. |
| Broad CI permissions | Supply-chain risk. |
| AI output treated as official decision | Model-risk and governance issue. |
| Control exists only in policy document | No implementation evidence. |

---

## Review Checklist

- Are security assumptions documented?
- Are trust boundaries clear?
- Is caller identity defined?
- Is authorization enforced server-side?
- Are sensitive data classes known?
- Are logs/metrics/traces safe?
- Are secrets externalized?
- Are CI/CD workflows least privilege?
- Are runtime permissions scoped?
- Are controls testable and evidenced?

---

## Summary

Security is not a gate at the end. It is a design property of the whole platform.

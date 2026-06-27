# Security, Entitlements, Confidentiality, and Jurisdiction Controls

## Purpose

This file explains security and access-control concerns specific to wealth-management platforms.

Client confidentiality and jurisdiction-aware access are core domain requirements.

---

## Sensitive Data

Sensitive wealth data includes:

- client names
- account numbers
- portfolio identifiers
- holdings
- transactions
- balances
- performance
- risk profile
- suitability records
- advisory proposals
- reports
- documents
- relationship data
- entitlement policy details

---

## Entitlement Dimensions

Entitlements may depend on:

- advisor
- team
- client
- relationship
- account
- portfolio
- booking center
- jurisdiction
- region
- product type
- document type
- action type
- role

---

## Jurisdiction Controls

Jurisdiction affects:

- data residency
- reporting rules
- product eligibility
- advisory requirements
- tax/regulatory treatment
- document access
- cross-border restrictions
- retention rules

---

## Permission-Blocked State

A system should represent permission-blocked states safely.

Do not expose:

- hidden client existence
- raw entitlement rules
- internal policy names
- restricted account counts
- confidential identifiers

---

## Security Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Authorization only in UI | Backend bypass. |
| Portfolio id in logs/metrics | Data leakage. |
| Cross-jurisdiction data mixed | Regulatory risk. |
| Report download direct from storage | Access-control bypass. |
| Advisory notes exposed broadly | Confidentiality breach. |
| Empty data for denied access | User confusion and audit gap. |

---

## Review Checklist

- What data is sensitive?
- What entitlement dimension applies?
- Is authorization server-side?
- Is jurisdiction relevant?
- Are logs/metrics/traces safe?
- Are documents access-controlled?
- Are permission-blocked states safe?
- Are reports filtered correctly?
- Are security tests present?

---

## Summary

In wealth platforms, security is domain logic.

Entitlements, confidentiality, and jurisdiction rules must be designed into APIs, reports, analytics, and operations.

# Client, Account, Portfolio, Relationship, and Booking Hierarchy

## Purpose

This file explains core identity and hierarchy concepts in wealth-management platforms.

Understanding these structures is essential because entitlements, reporting, portfolio aggregation, advisory workflows, and regional controls depend on them.

---

## Common Hierarchy

```text
Client / party
  -> relationship / household / group
  -> legal entity / booking center / jurisdiction
  -> account
  -> portfolio
  -> sub-portfolio / sleeve / strategy
  -> holding / transaction / cashflow
```

Different institutions may model this differently, but the engineering concern is the same: scope and ownership must be explicit.

---

## Client

A client represents the person, entity, trust, family office, or corporate relationship.

Client data may include:

- client id
- name
- segment
- residency
- suitability profile
- risk profile
- relationship manager
- household/group membership
- confidentiality classification

Client data is highly sensitive.

---

## Relationship

A relationship groups clients/accounts for advisory, reporting, householding, or relationship-manager view.

Examples:

- family relationship
- private banking relationship
- household
- corporate group
- discretionary mandate relationship

---

## Account

An account is often a legal or booking structure.

It may have:

- account number
- booking center
- currency
- custody arrangement
- tax jurisdiction
- product permissions
- restrictions
- account status

Accounts may hold cash and securities.

---

## Portfolio

A portfolio is an investment grouping used for analysis, reporting, performance, risk, and advisory.

A portfolio may map to:

- one account
- many accounts
- sub-portfolio
- strategy sleeve
- mandate
- model portfolio
- reporting group

Do not assume account equals portfolio.

---

## Booking Center and Jurisdiction

Booking center and jurisdiction affect:

- data availability
- regulations
- reporting rules
- product eligibility
- entitlements
- data residency
- currency and tax treatment
- cutover and migration scope

---

## Entitlement Scope

Entitlements may apply at:

- client level
- relationship level
- account level
- portfolio level
- booking center
- region
- product type
- document type
- advisor/team

APIs and reports must enforce scope server-side.

---

## Hierarchy Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Account and portfolio treated as same | Wrong aggregation/reporting. |
| Entitlements checked only at client level | Portfolio/account leakage. |
| Booking center ignored | Regulatory and data residency risk. |
| Relationship grouping hardcoded in UI | Inconsistent views. |
| Portfolio scope missing from API | Ambiguous results. |
| Migration changes IDs without mapping | Continuity break. |

---

## Review Checklist

- What is the business scope: client, account, portfolio, or relationship?
- Is account-to-portfolio mapping explicit?
- Is booking center relevant?
- Are entitlements applied at correct level?
- Are identifiers stable?
- Is hierarchy lineage available?
- Are aggregation rules documented?
- Are migration mappings needed?
- Are examples synthetic?

---

## Summary

Wealth-platform engineering starts with hierarchy.

If client, account, portfolio, relationship, and booking scope are unclear, analytics, reporting, entitlements, and migration will be fragile.

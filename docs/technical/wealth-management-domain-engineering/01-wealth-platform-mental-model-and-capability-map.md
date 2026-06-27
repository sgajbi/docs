# Wealth Platform Mental Model and Capability Map

## Purpose

This file explains the end-to-end mental model for enterprise wealth-management technology platforms.

A wealth platform connects client data, account structures, portfolio data, instruments, market data, analytics, advisory workflows, reporting, and operations.

---

## Platform Mental Model

```text
Client / advisor / portfolio manager
  -> product channel / workbench / portal
  -> BFF / experience API
  -> portfolio source services
  -> analytics services
  -> advisory and mandate services
  -> reporting and archive services
  -> platform data products
  -> operational evidence and support
```

---

## Major Capability Areas

| Capability | Purpose |
|---|---|
| Client and relationship management | Represents client, household, relationship, advisor, segment, and suitability context. |
| Account and portfolio management | Represents legal accounts, portfolios, custody, booking center, and investment grouping. |
| Holdings and positions | Shows current or historical assets, quantities, valuation, and exposures. |
| Transactions and cashflows | Shows buys, sells, income, fees, transfers, subscriptions, redemptions, and cash movements. |
| Market and reference data | Provides instruments, prices, FX, ratings, sectors, classifications, and benchmark definitions. |
| Performance analytics | Measures returns, contribution, attribution, benchmark comparison, and composites. |
| Risk analytics | Measures concentration, exposure, volatility, drawdown, VaR, suitability, and scenario risk. |
| Advisory workflow | Supports proposal generation, recommendations, suitability checks, review, and approval. |
| Mandate and DPM | Supports discretionary mandates, model portfolios, rebalancing, restrictions, and oversight. |
| Reporting | Produces portfolio reviews, statements, factsheets, performance/risk reports, and evidence packs. |
| Archive | Stores generated documents, metadata, retention, retrieval, and audit evidence. |
| Operations and support | Supports data quality, reconciliation, incident diagnosis, runbooks, and controls. |

---

## Platform Layers

| Layer | Responsibility |
|---|---|
| Product UI | User journey, visualization, states, and interaction. |
| BFF / experience API | Product-ready composition and degraded-state mapping. |
| Domain service | Source-owned or methodology-owned business capability. |
| Data product | Governed reusable data contract with trust metadata. |
| Shared capability | Rendering, archive, AI, workflow, notification, or evidence service. |
| Platform governance | Standards, CI/CD, runtime, security, observability, and knowledge base. |

---

## Core Engineering Challenge

Wealth systems are difficult because they combine:

- complex financial data
- multiple source systems
- regional and booking-center differences
- client confidentiality
- entitlements
- historical corrections
- valuation timing
- performance methodology
- risk methodology
- reporting evidence
- migration continuity
- production support

---

## Capability Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| UI calculates official analytics | Methodology and audit risk. |
| BFF becomes source of domain truth | Ownership drift. |
| Analytics service owns source portfolio data | Source/analytics confusion. |
| Report engine recomputes business truth | Inconsistent outputs. |
| No lineage for report numbers | Client/audit challenge. |
| Market data treated as static afterthought | Incorrect valuation and analytics. |
| Entitlements applied only in UI | Data leakage risk. |

---

## Review Questions

- Which capability is being changed?
- Which service owns the truth?
- Which data sources are involved?
- Which methodology applies?
- What is visible to the user?
- What is stale, partial, or unsupported?
- What evidence proves the output?
- What operational support is needed?

---

## Summary

A wealth-management platform is a capability ecosystem.

Good architecture keeps domain truth, analytics, advisory workflow, reporting, and product experience connected but not confused.

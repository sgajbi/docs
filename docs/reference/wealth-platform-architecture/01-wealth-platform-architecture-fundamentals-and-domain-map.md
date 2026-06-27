# 01 - Wealth Platform Architecture Fundamentals and Domain Map

## 1. What a wealth platform is

A wealth platform is an integrated system of capabilities that allows a financial institution to onboard clients, maintain accounts and portfolios, offer products, capture advisory decisions, execute orders, settle transactions, value holdings, calculate analytics, monitor mandates, report to clients and operate safely under regulatory and operational controls.

It is not only a trading system, portfolio system, reporting system or CRM. A true wealth platform connects all of these domains through consistent data, contracts, workflows and controls.

## 2. Core architecture objective

The architecture objective is to create a platform that is:

| Quality | Meaning in wealth platform context |
|---|---|
| Correct | Holdings, cash, valuations, transactions, performance and reporting are accurate and explainable. |
| Consistent | The same client, portfolio, instrument, transaction and valuation mean the same thing across channels. |
| Reproducible | Reports and calculations can be regenerated with the same input version and calculation version. |
| Auditable | Every output has lineage to source data, transformations, business rules and user/system actions. |
| Resilient | Failures are isolated, recoverable and visible. |
| Extensible | New product types, countries, mandates and reporting formats can be added without rewriting the whole platform. |
| Secure | Access, data scope, client consent, privacy and audit are built in. |
| Operable | Production teams can monitor, reconcile, explain and recover the platform. |
| Advisory-ready | Product, risk, suitability and client context are available at the point of advice. |
| Reporting-ready | Data is snapshot-able, versioned and fit for statement/portfolio-review generation. |

## 3. Major wealth platform domains

```text
Client & Relationship Master
  |-- Party / CIF / BR / Household / Beneficial Owner
  |-- Account / Portfolio / Mandate / Booking Centre
  |-- Suitability / Risk Profile / Tax Residency
  `-- Entitlements / Advisor / Access Control

Product & Market Data
  |-- Instrument Master / Product Taxonomy
  |-- Issuer / Counterparty / Entity Master
  |-- Prices / FX / Curves / Benchmark Data
  |-- Corporate Actions / Income Events
  `-- Product Governance / APU / Target Market

Order & Investment Operations
  |-- Advisory Proposal / Consent / Pre-Trade Checks
  |-- Order Capture / Execution / Allocation
  |-- Confirmation / Settlement / Custody
  |-- Corporate Actions / Income Processing
  `-- Reconciliation / Exception Management

Investment Book of Record
  |-- Transactions / Transaction Legs
  |-- Cash Balances / Position Balances / Lots
  |-- Accruals / Income / Fees / Taxes
  |-- Realized and Unrealized P&L
  `-- Ledger / Audit / Source Lineage

Analytics & Controls
  |-- Valuation / Performance / Attribution
  |-- Risk / Exposure / Concentration
  |-- Benchmark / Composite / Mandate Analytics
  |-- Buying Power / Collateral / Credit Availability
  `-- Data Quality / Recalculation / Lineage

Reporting & Client Experience
  |-- Client Statements / Portfolio Review
  |-- Advisor Dashboard / Client 360
  |-- Digital Documents / Archive / Delivery
  |-- Exceptions / Disclosures / Degraded Data
  `-- Client Interaction History / Workflow
```

## 4. Capability-oriented decomposition

A wealth platform should be decomposed by capability, not by technical layer alone.

| Capability | Primary responsibility |
|---|---|
| Client master | Client, entity, relationship, household, role, beneficial owner, KYC, suitability, tax and entitlement context. |
| Product master | Instruments, products, identifiers, classifications, risk attributes, eligibility and lifecycle metadata. |
| Market data | Prices, FX, yield curves, rates, benchmark levels, index constituents and data-quality flags. |
| Order management | Proposal-to-order workflows, execution instructions, allocation and order state. |
| Transaction management | Economic bookings, settlement records, reversals, amendments and transaction lineage. |
| Position service | Current and historical holdings derived from transactions and corporate actions. |
| Cash/balance service | Settled cash, trade-date cash, pending cash, blocked cash and liquidity views. |
| Investment accounting | Cost, lots, accruals, income, fees, tax, realized/unrealized P&L and book-of-record outputs. |
| Performance engine | TWR, MWR, contribution, attribution, benchmark-relative return and composite analytics. |
| Risk engine | Exposure, concentration, volatility, drawdown, VaR/CVaR, stress and suitability risk indicators. |
| Mandate engine | DPM mandate rules, restrictions, asset allocation bands, drift, breach monitoring and pre/post-trade checks. |
| Reporting engine | Statement snapshots, portfolio review, charts, narratives, disclosures, PDFs and archives. |
| Entitlement/workflow service | Who can see/act, workflow tasks, approvals, digital consent, notes and audit trail. |

## 5. Logical architecture view

```text
Channels
  |-- Advisor Workbench
  |-- Client Portal / Mobile
  |-- Operations Console
  |-- Reporting Portal
  `-- API Consumers

Experience APIs / BFFs
  |-- Client 360 API
  |-- Portfolio Review API
  |-- Advisory Proposal API
  |-- Order Capture API
  `-- Reporting API

Domain Services
  |-- Client Master Service
  |-- Product Master Service
  |-- Market Data Service
  |-- Order Service
  |-- Transaction Service
  |-- Position Service
  |-- Cash Service
  |-- Accounting Service
  |-- Performance Service
  |-- Risk Service
  |-- Mandate Service
  |-- Reporting Service
  `-- Entitlement / Workflow Service

Integration & Data Platform
  |-- Event Bus / Streaming Platform
  |-- Batch/File Intake
  |-- API Gateway
  |-- Data Quality and Reconciliation
  |-- Snapshot Store
  |-- Audit and Lineage Store
  `-- Operational Monitoring

External / Enterprise Systems
  |-- Core Banking
  |-- Custodian
  |-- OMS / EMS
  |-- Market Data Vendor
  |-- Product Manufacturer / Issuer
  |-- Client Static / CRM
  |-- KYC / AML / Tax Systems
  |-- Document Archive
  `-- Regulatory Reporting Systems
```

## 6. Why architecture is harder in wealth than in simple trading platforms

Wealth platforms have special complexity:

| Complexity | Example |
|---|---|
| Multi-product | Bonds, notes, equities, funds, derivatives, private markets, lending, insurance, cash and structured products. |
| Multi-currency | Portfolio currency, instrument currency, settlement currency, income currency and reporting currency can differ. |
| Multi-entity | Client, account, portfolio, mandate, BR/CIF, household, trust, company, beneficial owner and advisor hierarchy. |
| Multi-source | Positions, transactions, prices, FX, product data and client data may come from different authoritative systems. |
| Lifecycle-rich | Corporate actions, coupons, maturities, redemptions, capital calls, derivatives expiry, fund dealing and structured-product observations. |
| Advisory controls | Suitability, target market, product governance, mandate rules, disclosures and consent must be captured. |
| Reporting sensitivity | Small differences in valuation, income, FX or cashflow classification can change client statements and advisor explanations. |
| Migration risk | Historical continuity, cost basis, performance history and source-system reconciliation are hard to preserve. |

## 7. Core design separations

### 7.1 Order vs trade vs transaction

| Concept | Meaning |
|---|---|
| Order | Client/advisor instruction to buy/sell/subscribe/redeem. |
| Trade/execution | Market or counterparty execution record. |
| Transaction | Economic posting into cash, position, lot, income, expense or ledger. |

Do not collapse all three into one object.

### 7.2 Legal holding vs analytical exposure

| Concept | Example |
|---|---|
| Legal holding | Structured note issued by Bank X. |
| Analytical exposure | Economic exposure to Apple, Nvidia, issuer risk, downside barrier and USD currency. |

Reports need legal holdings. Risk and mandate systems often need look-through or synthetic exposure.

### 7.3 Current position vs historical snapshot

A current position answers: "What does the client hold now?"

A reporting snapshot answers: "What did the report show as of a specific valuation date and data version?"

Never regenerate official reports from today's mutable state without controlling versioning.

### 7.4 Source event vs curated state

A source event is what arrived from a source system.

A curated state is the normalized, validated, enriched business representation used by consumers.

Both are needed: source event for audit, curated state for business use.

## 8. Architecture decision principles

For every domain, ask:

1. Who owns the data?
2. What is the source of truth?
3. Is the data event-sourced, snapshot-based, API-sourced or file-sourced?
4. What are the required identifiers?
5. What is the lifecycle state machine?
6. What are the calculation dependencies?
7. What is the reporting impact?
8. What are the degraded-data rules?
9. What are the reconciliation controls?
10. What audit evidence is required?

## 9. Example: Portfolio review dependency chain

```text
Portfolio Review
  depends on:
    Client / account / portfolio hierarchy
    Entitlements and advisor relationship
    Holdings and cash balances
    Transactions and cashflows
    Prices, FX and benchmark data
    Product classification and risk attributes
    Performance and attribution calculations
    Risk and concentration calculations
    Mandate status and restrictions
    Reporting snapshot and disclosure rules
```

If any upstream data is stale or missing, the report should not silently produce a misleading result. It should either block, degrade with explanation, or show a controlled exception.

## 10. Architecture north star

The north star is a platform where:

- every business output is traceable
- every domain has a clear owner
- every API and event has a contract
- every report has a snapshot and calculation lineage
- every exception has a workflow
- every client-facing number has an explanation path
- every migration has reconciliation evidence
- every product capability has an implementation-backed support boundary

# 03 - Account, Portfolio, Mandate, Booking Centre and Service Model Design

## 1. Overview

An account is the legal/accounting container. A portfolio is the investment/reporting container. A mandate defines the authority and investment rules. A booking centre determines legal, regulatory, tax, product and operational context. A service model determines how the client is advised and serviced.

These concepts are related but not identical.

## 2. Account model

An account is usually created by the core banking, custody, loan, fund platform, insurance or brokerage system.

### Account types

| Account type | Purpose |
|---|---|
| Cash account | Holds cash balances and settlement movements. |
| Custody account | Holds securities and corporate-action entitlements. |
| Trading account | Used for order execution and market access. |
| DPM managed account | Holds assets under discretionary authority. |
| Advisory account | Supports advised transactions. |
| Execution-only account | Client-initiated trades without advice. |
| Loan account | Borrowing and repayment. |
| Collateral account | Pledged or restricted assets. |
| Margin account | Securities-backed trading/lending with margin rules. |
| Insurance policy account | Policy contract and policy value. |
| Trust account | Account legally held by trustee. |
| Escrow / blocked account | Restricted account for specific purpose. |

### Account attributes

| Field | Meaning |
|---|---|
| account_id | Canonical account ID. |
| source_account_id | Core/custodian source ID. |
| account_type | Cash, custody, managed, loan, margin, etc. |
| legal_owner_party_id | Legal owner. |
| booking_entity | Bank/legal entity booking the account. |
| booking_centre | Singapore, Hong Kong, Switzerland, etc. |
| base_currency | Accounting/reporting base currency. |
| account_status | Active, blocked, closing, closed, dormant. |
| restriction_status | Hold-only, no-buy, no-sell, collateral blocked. |
| opening_date / closing_date | Lifecycle dates. |
| tax_lot_method | FIFO, average cost, specific ID, etc. |
| statement_preference | Frequency, language, recipients. |
| service_model | Advisory, DPM, execution-only, EAM, family office. |

## 3. Portfolio model

A portfolio is a logical grouping used for investment analysis, reporting, mandate management and portfolio review.

### Portfolio types

| Portfolio type | Example |
|---|---|
| Account portfolio | One portfolio maps to one custody account. |
| Consolidated portfolio | Multiple accounts grouped for reporting. |
| Managed portfolio | DPM portfolio under discretionary mandate. |
| Advisory portfolio | RM/advisor coverage portfolio. |
| Lending collateral portfolio | Pledged assets supporting credit line. |
| Proposal portfolio | Simulated portfolio used for investment proposal. |
| Household portfolio | Aggregated family/group view. |
| Sleeve portfolio | Equity sleeve, FI sleeve, alternatives sleeve. |
| Shadow portfolio | Migration, reconciliation or what-if portfolio. |

### Portfolio attributes

| Field | Meaning |
|---|---|
| portfolio_id | Canonical portfolio ID. |
| portfolio_type | Managed, advisory, consolidated, collateral, sleeve. |
| owner_party_id | Client/entity to whom portfolio belongs. |
| reporting_currency | Portfolio report currency. |
| inception_date | Start date for performance. |
| valuation_policy | Pricing and FX policy. |
| performance_policy | TWR/MWR/cashflow classification. |
| benchmark_id | Linked benchmark. |
| model_portfolio_id | Target allocation/model. |
| mandate_id | DPM/advisory mandate. |
| include_accounts | Linked accounts/positions. |
| exclusion_rules | Excluded assets/cash/loan accounts. |
| status | Active, archived, closed, simulation. |

## 4. Account-to-portfolio mapping

Mapping can be one-to-one, one-to-many or many-to-one.

| Pattern | Example | Key risk |
|---|---|---|
| One account to one portfolio | Simple custody account | Low complexity. |
| Many accounts to one portfolio | Cash + custody + loan account | Avoid double counting cash/loan. |
| One account to many portfolios | Account split into strategy sleeves | Requires allocation logic. |
| Household portfolio | Multiple legal owners | Must not imply common legal ownership. |
| Collateral portfolio | Pledged subset of holdings | Requires pledge/exclusion handling. |
| Proposal portfolio | Simulation from current holdings | Must be clearly non-book-of-record. |

## 5. Mandate model

A mandate defines investment authority, restrictions and objectives.

### Mandate types

| Mandate type | Meaning |
|---|---|
| Execution-only | Client decides; no recommendation. |
| Advisory | Advisor recommends; client approves. |
| Discretionary / DPM | Portfolio manager executes within agreed mandate. |
| Restricted advisory | Advisor can recommend only approved subset. |
| Model-based advisory | Portfolio compared against model. |
| External asset manager | EAM controls/advises under external authority. |
| Family-office advised | Family-office/investment committee has authority. |
| Trustee mandate | Trustee decision framework applies. |

### Mandate attributes

| Attribute | Examples |
|---|---|
| investment_objective | Income, growth, balanced, capital preservation. |
| risk_profile | Conservative, balanced, growth, aggressive. |
| target_allocation | Equity 40%, FI 45%, Alternatives 10%, Cash 5%. |
| benchmark | Composite benchmark. |
| permitted_products | Bonds, funds, equities, structured products. |
| restricted_products | No derivatives, no complex notes, no private markets. |
| concentration_limits | Max issuer 10%, product 20%, currency 30%. |
| liquidity_limits | Min cash 3%, max illiquid 15%. |
| leverage_policy | No leverage, max LTV, permitted overdraft. |
| ESG/preferences | Exclusions, sustainability preferences. |
| tax constraints | No US situs assets, avoid PFIC-like exposures. |
| review_frequency | Quarterly, semi-annual, annual. |
| authority model | Advisory approval, DPM discretion, investment committee. |

## 6. Booking centre

Booking centre is a critical attribute for legal/regulatory/product/data controls.

### Booking centre impacts

| Area | Impact |
|---|---|
| Legal terms | Account contract and applicable law. |
| Product eligibility | Which products can be sold/booked. |
| Tax reporting | FATCA/CRS/local tax statements. |
| Investor classification | Accredited/professional/retail frameworks. |
| Cross-border | What RM/advisor can discuss from which location. |
| Settlement | Local market/account structure and settlement cycles. |
| Custody | Local/global custodian chain. |
| Data privacy | Data storage, transfer and access controls. |
| Reporting disclosures | Statement language and regulatory disclosures. |
| Fees | Booking-specific fee schedule. |

### Booking-centre modelling principle

```text
Do not derive booking centre from client address alone.
Booking centre belongs to account/contract/legal entity context.
Client residence, RM location and booking centre can all differ.
```

## 7. Service model

The service model determines workflows, permissions, reporting and evidence.

| Service model | Platform implications |
|---|---|
| Advisory | Recommendation evidence, suitability, client consent, proposal history. |
| DPM | Mandate rules, model portfolio, rebalancing, PM authority, periodic review. |
| Execution-only | Client instruction capture, appropriateness where applicable, no advice labelling. |
| EAM | External advisor access, delegated authority, segregated reporting. |
| Family office | Multi-entity consolidation, investment committee, custom reports. |
| Lending-led | Collateral, pledge, LTV, availability and margin call reporting. |
| Insurance-led | Policy owner/insured/beneficiary and surrender value reporting. |

## 8. Status and restrictions

Account and portfolio status must be explicit.

| Status | Meaning |
|---|---|
| Active | Normal service. |
| Pending opening | Onboarding incomplete. |
| Blocked | Transactions restricted. |
| Restricted | Specific product/action restrictions. |
| Hold-only | Existing holdings can remain; new buys blocked. |
| Dormant | No activity for defined period. |
| Closing | Account in closure workflow. |
| Closed | No new activity; historical reporting only. |
| Deceased | Client death flagged; special estate workflow. |
| Under review | KYC/compliance/sanctions/manual review. |

## 9. Common design mistakes

| Mistake | Consequence |
|---|---|
| Booking centre stored only on client | Wrong for clients with multi-booking accounts. |
| Service model stored only on RM team | Fails account-level differences. |
| Mandate not effective-dated | Cannot reproduce historical breaches. |
| Portfolio closure deletes mappings | Historical reports fail. |
| Consolidated portfolio treated as legal account | Misleading client reporting and controls. |
| No explicit exclusions | Loans/collateral/cash can be double-counted. |
| No rule versioning | Suitability/rebalancing audit gaps. |

## 10. Practitioner summary

Account, portfolio, mandate, booking centre and service model are separate dimensions. A high-quality wealth platform must support multiple mappings, effective dating, source ownership, restriction states and audit lineage across all of them.

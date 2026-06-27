# 01 - Portfolio Reporting Fundamentals and Taxonomy

## 1. Reporting as a product capability

Portfolio reporting is a product capability, not merely a document-generation feature. It sits at the intersection of:

- client ownership and account hierarchy;
- instrument master and product taxonomy;
- transaction and cash ledger;
- position book;
- market data and pricing;
- performance and risk analytics;
- mandate and suitability controls;
- tax and regulatory classification;
- advisor commentary and client communication;
- archive, evidence and audit.

A report is a controlled view of the portfolio as of a reporting period and cut-off. It must be reproducible, explainable and traceable.

## 2. Core reporting questions

Every portfolio report should answer a consistent set of business questions.

| Question | Reporting section | Data dependency |
|---|---|---|
| What do I own? | Holdings / positions | Position book, instrument master |
| What is it worth? | Valuation / market value | Prices, FX, valuation hierarchy |
| What changed? | Activity / transactions | Transaction ledger, corporate actions |
| What income did I receive? | Income summary | Coupons, dividends, distributions, interest |
| What fees and taxes were charged? | Fees/tax summary | Fee engine, tax withholding, charges |
| How did I perform? | Performance | TWR/MWR, cashflow classification, valuations |
| What drove performance? | Contribution/attribution | Performance breakdown, classifications, benchmark |
| What risk do I carry? | Risk/exposure | Look-through, issuer, currency, sector, sensitivity |
| Am I within mandate? | Mandate/compliance | Restrictions, targets, thresholds, breaches |
| What needs attention? | Exceptions/alerts | Stale price, missing data, concentration, liquidity |

## 3. Reporting taxonomy

### 3.1 By audience

| Audience | Report need |
|---|---|
| Client | Clear statement of wealth, activity, income, performance and risk. |
| Advisor/RM | Explain performance, identify issues, prepare review conversation. |
| Portfolio manager | Mandate drift, trades, benchmarks, contribution, risk and constraints. |
| Operations | Reconciliation breaks, failed trades, pricing issues, corporate-action issues. |
| Compliance | Suitability evidence, disclosures, breaches, product eligibility. |
| Management | AUM, flows, revenue, risk concentrations and business metrics. |
| Auditor/regulator | Reproducibility, lineage, evidence, approvals and disclosures. |

### 3.2 By output type

| Output type | Example |
|---|---|
| Periodic statement | Monthly/quarterly client statement. |
| Portfolio review | Advisor-led review pack. |
| DPM report | Discretionary mandate report with benchmark and mandate details. |
| Risk report | Concentration, VaR, volatility, stress, exposure. |
| Performance report | TWR/MWR, benchmark comparison, contribution, attribution. |
| Activity report | Trades, transfers, income, fees, taxes, corporate actions. |
| Tax input report | Withholding, income classification, realized gains, tax lots. |
| Exception report | Missing price, stale NAV, failed trade, unconfirmed corporate action. |
| Regulatory disclosure | Costs/charges, product risk, suitability, relationship disclosures. |
| Archive copy | Final immutable report stored for evidence. |

### 3.3 By reporting basis

| Basis | Meaning |
|---|---|
| Trade-date basis | Recognizes trades on trade date. Useful for portfolio management and performance. |
| Settlement-date basis | Recognizes securities/cash after settlement. Useful for custody reconciliation. |
| Accounting basis | Follows accounting ledger rules. |
| Client statement basis | Client-facing basis, sometimes adjusted for clarity and local market conventions. |
| Performance basis | Uses performance book and cashflow classification. |
| Tax basis | Uses tax-lot and jurisdiction-specific classifications. |

A strong platform should not mix these implicitly. Reports should state or internally record the basis used.

## 4. Reporting design principles

1. **Client clarity first**: show what matters to the client without hiding important risk.
2. **Professional precision**: values must tie back to source systems and methodology.
3. **No false certainty**: label estimates, stale prices, provisional values and missing data.
4. **Consistent definitions**: same metric name should mean the same thing across screens, PDFs and APIs.
5. **Lineage by design**: report cells should be traceable to source, calculation version and run ID.
6. **Separation of data and presentation**: the reporting dataset should be independent of PDF layout.
7. **Reproducibility**: archived reports must be regenerated or replayed from stored snapshots.
8. **Product-aware reporting**: notes, derivatives, private markets and insurance need special handling.
9. **Mandate awareness**: DPM and advisory reports should show target, drift, breaches and actions.
10. **Exception transparency**: missing or bad data should become controlled explanations, not silent wrong numbers.

## 5. Good reporting versus weak reporting

| Weak reporting | Strong reporting |
|---|---|
| Shows market value without price source. | Shows valuation source and flags stale/estimated values. |
| Mixes trade-date and settlement-date values. | Explicitly controls reporting basis. |
| Shows performance without cashflow methodology. | Defines TWR/MWR, period, currency and cashflow treatment. |
| Hides missing data. | Shows controlled exceptions and impact. |
| Treats all products like equities. | Handles bonds, funds, notes, derivatives, private markets and insurance correctly. |
| Produces only PDF. | Produces reusable report dataset, PDF, APIs and archive evidence. |
| Cannot reproduce historical reports. | Stores report snapshot, version and calculation lineage. |

## 6. Relationship to previous knowledge packs

This pack consumes and presents outputs from:

- product taxonomy and instrument master;
- market data and pricing;
- investment accounting and ledger;
- portfolio performance, attribution and risk;
- trade lifecycle and operations;
- tax and regulatory reporting;
- product governance and suitability;
- advisory and rebalancing workflows.

In other words, reporting is the integration point where the platform becomes visible to the client.

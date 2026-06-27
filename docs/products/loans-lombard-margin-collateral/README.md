# Loans, Lombard Lending, Margin, Collateral and Credit Lines Reference Pack

## Purpose

This documentation pack explains lending and collateral products from a private-banking, wealth-management, portfolio-analytics and platform-implementation perspective.

It is designed for:

- reusable learning and practitioner review
- office knowledge sharing
- advisory and suitability discussions
- DPM / discretionary mandate design
- buying-power and credit-line systems
- portfolio analytics and reporting platforms
- transaction, position, collateral and exposure modelling
- QA, reconciliation, migration and production-support scenarios

**Curation note:** Curated from provided source material on 2026-06-27 and normalized for reusable practitioner reference.

**Last updated:** 2026-06-27

## Product family covered

This pack covers the lending and collateral side of wealth management:

- cash loans
- overdrafts
- revolving credit lines
- term loans
- Lombard lending / securities-based lending
- margin lending
- portfolio lending
- mortgage-backed / property-backed credit lines at a conceptual level
- collateral pledging
- one-way and mutual pledge structures
- cross-pledging between client entities and portfolios
- loan-to-value (LTV), haircut and eligibility rules
- available-to-invest / buying-power calculations
- margin calls, collateral shortfall and forced liquidation
- collateral release and substitution
- loan interest, commitment fees, arrangement fees and utilization fees
- collateral, exposure and credit-risk analytics

## Suggested reading order

1. `01-loans-lombard-margin-collateral-fundamentals-and-taxonomy.md`
2. `02-credit-lines-lombard-lending-ltv-haircuts-and-availability.md`
3. `03-loan-lifecycle-transactions-interest-fees-and-position-modelling.md`
4. `04-collateral-pledge-cross-pledging-and-buying-power-modelling.md`
5. `05-margin-lending-margin-calls-liquidation-and-securities-financing.md`
6. `06-data-model-valuation-income-accrual-performance-risk-and-analytics.md`
7. `07-advisory-mandate-suitability-reporting-and-client-experience.md`
8. `08-platform-implementation-controls-reconciliation-test-scenarios-and-glossary.md`
9. `09-source-notes-and-further-reading.md`
10. `10-worked-examples-and-implementation-patterns.md`

## Core mental model

A lending product creates **liability exposure** for the client and **credit exposure** for the bank.

A collateral product defines which assets support that exposure and how much lending value the bank assigns to those assets.

A buying-power system combines:

```text
Eligible collateral value
- Haircut / LTV adjustment
- Existing exposure
- Pending / in-flight exposure
- Reserved exposure
- Regulatory, suitability and mandate restrictions
= Available credit / buying power
```

In wealth platforms, lending is not only a product. It is also a control layer that affects trading, advisory, reporting, collateral monitoring, mandate compliance, risk analytics and client experience.

## Important design principle

Separate the following concepts clearly:

| Concept | Meaning |
|---|---|
| Loan / credit line | Facility that allows borrowing |
| Drawdown | Actual utilization of the facility |
| Exposure | Amount owed or economically at risk |
| Collateral | Assets pledged to secure exposure |
| Lending value | Collateral value after LTV/haircut rules |
| Availability | Remaining borrowing capacity |
| Buying power | Tradeable capacity after product, account and order controls |
| Margin requirement | Minimum collateral/equity required to support position |
| Margin call | Demand for more collateral, repayment or liquidation |
| Pledge | Legal/security relationship between collateral provider and credit beneficiary |
| In-flight order | Pending trade that should reserve buying power before settlement |

## Scope boundary

This pack focuses on wealth-management and portfolio-platform treatment. It is not a legal credit agreement template and does not replace local regulatory, legal, tax, credit-risk, or operations policies.

# Bonds Reference Pack

**Purpose:** A professional study and project reference for understanding, explaining, modelling, valuing, and processing bonds in a wealth-management / private-banking platform.

**Audience:** Product owners, business analysts, architects, engineers, QA, operations, advisors, portfolio analysts, and anyone who needs to understand bonds beyond surface-level product descriptions.

**Curation note:** Curated from provided source material on 2026-06-27 and normalized for reusable practitioner reference.

**Last updated:** 2026-06-27

---

## Reading Order

| File | Purpose |
|---|---|
| `01-bonds-fundamentals-and-product-taxonomy.md` | Explains what bonds are, how they differ from deposits/loans/notes/funds, key terms, major product variants, client objectives, and risks. |
| `02-bond-lifecycle-transactions-and-position-modelling.md` | Explains bond lifecycle events, transaction types, accrued interest handling, calls/puts/maturity/default/conversion, position modelling, and performance treatment. |
| `03-bond-data-model-valuation-yield-and-risk.md` | Gives a common data model for bonds, valuation hierarchy, pricing/yield concepts, duration/DV01/spread risk, clean/dirty price treatment, and look-through risk. |
| `04-platform-implementation-advisory-and-practitioner-reference.md` | Provides platform implementation checklists, advisory/suitability views, reporting requirements, operational controls, QA scenarios, glossary, and practitioner explanations and questions and answers. |
| `05-worked-examples-and-implementation-patterns.md` | Provides practical examples for clean/dirty price, accrued interest, coupon accrual, yield, duration, callable bonds, downgrades, maturity ladders, default/recovery and QA. |

---

## Core Mental Model

A bond is a **debt security**. The issuer borrows money from investors and promises to pay interest and repay principal according to the bond terms.

In simple form:

```text
Investor lends money to issuer.
Issuer pays coupons during the life of the bond.
Issuer repays principal at maturity, unless there is default, restructuring, conversion, call, or another contractual event.
```

Bonds are usually simpler than structured notes, but they still have many important variants: fixed-rate, floating-rate, zero-coupon, callable, putable, convertible, inflation-linked, perpetual, amortising, asset-backed, covered, subordinated, AT1, sovereign, corporate, high-yield, municipal, sukuk, and green bonds.

---

## Main Platform Design Principle

Model the **client accounting position** as the bond security, normally using **nominal amount** as the primary quantity.

Do not over-model every coupon payment, rating change, call schedule, or pricing source as a different position. These belong to the bond contract, schedule, lifecycle event, valuation, or risk model.

Recommended structure:

```text
Instrument
  +-- BondContract
        +-- Coupon schedule
        +-- Cashflow schedule
        +-- Day-count and business-day rules
        +-- Call / put / sinking fund schedules
        +-- Amortisation schedule
        +-- Rating and issuer data
        +-- Collateral / seniority / guarantee terms
        +-- Optional subtype extensions
        +-- Lifecycle events
        +-- Valuation records
```

Accounting transactions should be compact and economic:

```text
Subscription / Buy
Sell
Coupon payment
Accrued interest paid or received
Partial redemption / amortisation
Call redemption
Put redemption
Maturity redemption
Conversion
Tender / exchange offer
Default write-down
Recovery payment
Fees / taxes / correction reversal
```

---

## Scope Covered

This pack covers:

- Bond fundamentals and terminology
- Bond vs note vs deposit vs loan vs bond fund
- Government, sovereign, supranational, agency, municipal, and corporate bonds
- Investment-grade and high-yield bonds
- Fixed-rate, floating-rate, zero-coupon, inflation-linked, callable, putable, convertible, perpetual, amortising, covered, ABS/MBS, subordinated, AT1, sukuk, and ESG-labelled bonds
- Primary and secondary market lifecycle
- Trade, settlement, coupon, accrued interest, maturity, call, put, default, restructuring, recovery, and conversion events
- Position and transaction modelling
- Clean price, dirty price, accrued interest, yield, duration, DV01, convexity, spread, and valuation
- Listed vs OTC bond treatment
- Advisory and suitability considerations
- Platform implementation controls
- QA and regression test scenarios
- Practitioner explanations and questions and answers

---

## External Reference Sources

These sources were used as external validation and investor-education references:

- FINRA - Bonds: https://www.finra.org/investors/investing/investment-products/bonds
- FINRA - Fixed Income Data: https://www.finra.org/finra-data/fixed-income
- FINRA - Corporate and Agency Bond Data Glossary: https://www.finra.org/finra-data/fixed-income/corp-and-agency/glossary
- MoneySense Singapore - Understanding Bonds: https://www.moneysense.gov.sg/understanding-bonds/
- SEC - When Interest Rates Go Up, Prices of Fixed-Rate Bonds Fall: https://www.sec.gov/files/ib_interestraterisk.pdf
- Investor.gov - Callable or Redeemable Bonds: https://www.investor.gov/introduction-investing/investing-basics/glossary/callable-or-redeemable-bonds
- Investor.gov - Municipal Bonds, Asset Allocation, Diversification, and Risk: https://www.investor.gov/introduction-investing/general-resources/news-alerts/alerts-bulletins/investor-bulletins-35

---

## Important Disclaimer

This pack is for education, architecture, business analysis, and platform design. It is not investment advice, legal advice, tax advice, or regulatory advice. Real product treatment depends on jurisdiction, offering documents, custodian accounting, tax rules, market convention, issuer/custodian confirmation, regulatory classification, suitability policy, and the bank's internal product governance.

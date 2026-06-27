# Equities Reference Pack

## Purpose

This documentation pack is a professional, platform-oriented reference for **equities**. It is designed for:

- professional learning and practitioner refresh;
- office knowledge sharing;
- wealth-management product understanding;
- advisory, suitability and DPM discussions;
- portfolio analytics, performance, risk, concentration and client reporting;
- engineering design for product master, trade processing, positions, corporate actions, valuations, cost basis, P&L and reporting platforms.

**Curation note:** Curated from provided source material on 2026-06-27 and normalized for reusable practitioner reference.

The pack treats equities as more than a simple buy/sell product. In real wealth-management platforms, equities become complex because of **corporate actions, market data, multiple listings, custody, settlement cycles, tax, FX, ADR/GDR mechanics, cost-basis adjustments, performance classification and reconciliation**.

## Pack structure

| File | Purpose |
|---|---|
| `01-equities-fundamentals-and-product-taxonomy.md` | Explains what equities are, how they differ from bonds, funds and notes, and covers common shares, preferred shares, ADR/GDR, REITs, rights, warrants and key market concepts. |
| `02-equity-trade-lifecycle-transactions-and-position-modelling.md` | Covers order/trade/settlement lifecycle, transaction types, long/short positions, unsettled trades, lot and cost-basis modelling, P&L and performance treatment. |
| `03-equity-corporate-actions-dividends-and-cost-basis.md` | Detailed corporate-action guide covering dividends, stock splits, bonus issues, rights issues, DRIP/scrip, spin-offs, mergers, tender offers, buybacks, delisting, ADR events and cost-basis treatment. |
| `04-equity-data-model-valuation-pnl-performance-and-risk.md` | Covers data model, security master, listings, price and FX data, valuation, realized/unrealized P&L, total return, attribution inputs, risk analytics and data-quality controls. |
| `05-equity-platform-implementation-advisory-and-practitioner-reference.md` | Covers wealth-platform implementation, advisory/suitability, DPM use cases, controls, APIs/events, reconciliation, reporting and practitioner explanations. |
| `06-equity-glossary-control-checklists-and-test-scenarios.md` | Provides a quick reference glossary, transaction mapping, corporate-action mapping, controls and QA test scenarios for implementation. |
| `07-equity-advisory-mandate-analytics-reporting-deep-dive.md` | Dedicated advisory, DPM/mandate, model portfolio, health-check, attribution and reporting deep dive. |
| `08-equity-advanced-edge-cases-and-market-operations.md` | Advanced market-operations edge cases: ADR/GDR, odd lots, fractional shares, halts, IPOs, buybacks, securities lending, shorts, margin/collateral, restricted lists, migration and production triage. |
| `09-worked-examples-and-implementation-patterns.md` | Practical examples for trade settlement, dividends, splits, rights, ADRs, multi-currency P&L, contribution, delisting, corporate-action reconciliation and QA. |

## Recommended Learning and Project Sequence

1. Start with File 01 to understand the product and its variants.
2. Read File 02 to understand how equity trades become positions, cash movements, P&L and performance.
3. Read File 03 carefully, because corporate actions are the hardest part of equity platform processing.
4. Read File 04 to understand valuation, P&L, performance, risk and data modelling.
5. Use File 05 for platform design, advisory, suitability and practitioner review.
6. Use File 06 as an implementation checklist and QA reference.
7. Use File 07 for advisory, DPM, mandate, attribution and reporting work.
8. Use File 08 for advanced market operations and production edge cases.
9. Use File 09 for worked examples and implementation patterns.

## Core mental model

An equity is an ownership interest in an issuer. A direct equity position is usually represented by a **quantity of shares** held by a client portfolio. The return comes from:

```text
Equity Total Return = Price Return + Dividend Income + FX Return
```

From a platform perspective:

```text
Equity product complexity = trading + settlement + custody + market data + corporate actions + tax + FX + cost basis + performance + risk
```

Equities look simple at trade booking time, but become complex over time because issuer events can change share quantity, cash balances, cost basis, instrument identity, entitlements, withholding tax, voting rights and performance results.

## Design principle for wealth platforms

Use one core accounting position for the client's holding in the equity security, but maintain separate structures for:

- instrument and issuer master data;
- exchange/listing data;
- corporate action events and elections;
- transaction legs and cash legs;
- tax and fee legs;
- lot-level cost basis;
- valuation and market data;
- look-through and analytical exposure;
- performance classification;
- reconciliation and audit history.

Do not bury all equity complexity inside a single generic transaction record. Use economic transactions, transaction legs, corporate-action events and position lots so that each layer has a clear responsibility.

## Key external education and market-structure sources

The following public sources were used to validate core concepts:

- FINRA investor education on stocks: https://www.finra.org/investors/investing/investment-products/stocks
- FINRA settlement cycle education: https://www.finra.org/investors/insights/understanding-settlement-cycles
- Investor.gov ex-dividend date: https://www.investor.gov/introduction-investing/investing-basics/glossary/ex-dividend-dates-when-are-you-entitled-stock-and
- Investor.gov stock split glossary: https://www.investor.gov/introduction-investing/investing-basics/glossary/stock-split
- Investor.gov ADR glossary: https://www.investor.gov/introduction-investing/investing-basics/glossary/american-depositary-receipts-adrs
- SEC investor bulletin on ADRs: https://www.sec.gov/investor/alerts/adr-bulletin.pdf
- Investor.gov T+1 settlement bulletin: https://www.investor.gov/introduction-investing/general-resources/news-alerts/alerts-bulletins/new-t1-settlement-cycle-what-investors-need-know-investor-bulletin
- SGX Rulebook settlement chapter: https://rulebook.sgx.com/rulebook/chapter-9-settlement-1
- DBS corporate actions education: https://www.dbs.com.sg/treasures/investments/product-suite/equities/corporate-actions
- NYSE corporate actions data overview: https://www.nyse.com/market-data/corporate-actions
- DTCC corporate actions processing overview: https://dtcclearning.com/products-and-services/asset-services/1827-corporate-actions-processing.html

## Important note

This pack is for education, architecture, product understanding and platform design. It is not investment advice, legal advice, tax advice, accounting advice or a substitute for offering documents, exchange rules, custodian specifications, corporate-action notices or local regulatory requirements.

# Derivatives Reference Pack

## Purpose

This documentation pack is a professional, platform-oriented reference for **derivatives**. It is designed for:

- professional learning and practitioner refresh;
- office knowledge sharing;
- wealth-management product understanding;
- advisory, suitability, DPM and mandate discussions;
- portfolio analytics, risk, performance, concentration and client reporting;
- engineering design for product master, trade processing, lifecycle events, margin/collateral, valuation, Greeks, exposure, P&L and reconciliation platforms.

Derivatives are important because they sit behind many products already covered in the knowledge base:

- structured notes use embedded options, barriers, autocalls, FX options and credit derivatives;
- bond portfolios use interest-rate futures, swaps, options and CDS for hedging and relative-value strategies;
- equity portfolios use options, futures, forwards and total return swaps for overlays, protection, income generation and synthetic exposure;
- fund products may use derivatives for hedging, leverage, duration management, currency management or efficient portfolio management;
- DPM and advisory mandates often need to classify, constrain and report derivatives based on risk exposure, not just book cost.

This pack is written from the perspective of a wealth-management / private-banking platform architect working across advisory, mandates, analytics, reporting and implementation.

**Curation note:** Curated from provided source material on 2026-06-27 and normalized for reusable practitioner reference.

## Pack structure

| File | Purpose |
|---|---|
| `01-derivatives-fundamentals-and-product-taxonomy.md` | Explains what derivatives are, why they exist, exchange-traded vs OTC, use cases, product taxonomy and how derivatives combine with notes, bonds, funds and equities. |
| `02-options-lifecycle-transactions-and-position-modelling.md` | Covers listed and OTC options, calls, puts, exercise, assignment, expiry, premium, settlement, option positions, transaction types and option payoff modelling. |
| `03-forwards-futures-fx-and-commodity-derivatives.md` | Covers forwards, futures, FX forwards, NDFs, commodity futures, margin, variation margin, settlement and platform treatment. |
| `04-swaps-cds-and-otc-derivatives.md` | Covers interest-rate swaps, cross-currency swaps, total return swaps, equity swaps, inflation swaps and credit default swaps / CDS-like credit derivatives. |
| `05-derivatives-data-model-valuation-greeks-margin-collateral-risk.md` | Covers canonical data model, valuation, Greeks, DV01, CS01, margin, collateral, notional exposure, delta-equivalent exposure, VaR, stress and counterparty risk. |
| `06-derivatives-advisory-mandate-analytics-reporting-deep-dive.md` | Dedicated advisory, mandate, DPM, suitability, concentration, reporting, portfolio-review and client-communication deep dive. |
| `07-derivatives-platform-implementation-controls-reconciliation-test-scenarios.md` | Platform implementation guide: architecture, event model, transaction model, valuation controls, reconciliation, QA and production triage. |
| `08-derivatives-glossary-practitioner-reference-and-product-combinations.md` | Glossary, practitioner explanations, product-combination map, and reusable reference tables. |
| `09-worked-examples-and-implementation-patterns.md` | Worked examples for options, futures, FX forwards, NDFs, swaps, CDS, exposure lenses, margin/collateral treatment, SIMM-style margin inputs, volatility shocks, callable swaps, equity swap dividend adjustments, delivery notices, give-ups, early termination, close-out netting, wrong-way risk, collateral-interest settlement fails, fallback-rate transition, option amendments, cross-clearinghouse portability, model backtesting, hedge-accounting evidence, client-reporting explain packs, autocall observation corrections, commodity futures delivery alternatives, FX option premium settlement, swap novation waves, collateral substitution disputes, XVA sensitivity explanations, structured-product Greek restatements, intraday futures margin spikes, uncleared margin threshold resets, swap rate-fixing disputes, portfolio compression rejections, derivative tax-lot close-out reporting, collateral rehypothecation restrictions, FX barrier digital payout disputes, option auto-exercise thresholds, cleared margin model migrations, swaption exercise settlement, total return swap corporate-action basket adjustments, volatility-control overlays, knock-in recovery baskets, swap reset compounding disputes, futures exchange-for-physical processing, client-specific collateral segregation elections, derivative valuation reserve releases, option barrier dispute settlements, cleared swap compression fees, futures position-limit remediation, FX option fixing source overrides, collateral dispute interest claims, model reserve backtesting exceptions, cross-product delta-limit aggregation, client collateral concentration breaches, OTC confirmation mismatch ageing, option exercise funding shortfalls, clearing-house default fund assessments, structured payoff model recalibration, cross-currency option settlement netting, independent price verification breaches, collateral haircut schedule changes, swap unwind partial terminations, exchange outage trade busts, counterparty downgrade close-out triggers, option expiry cut-off disputes, cleared margin intraday add-ons, synthetic financing rate resets, collateral currency substitution FX haircuts, portfolio Greeks stale-source locks, derivative client consent remediation, give-up allocation rejects, option assignment notification delays, clearing-broker account transfer breaks, portfolio Greeks sign-convention disputes, collateral dispute partial settlements, variance-swap observation calendar corrections, futures average-price allocation breaks, option assignment buy-in funding, cleared swap account consolidation, cross-vendor vega bucket mapping, collateral dispute close-out settlements, variance-swap holiday-market disruption adjustments, futures delivery invoice corrections, option assignment tax-lot sourcing, cleared swap house transfer margin offsets, volatility surface vendor fallback mapping, collateral dispute settlement interest reversals, variance-swap extraordinary market closure determinations, futures delivery quality-grade appeals, option assignment lot-method correction reversals, cleared swap transferred collateral interest claims, volatility surface fallback expiry reviews, collateral dispute tax treatment reversals, variance-swap extraordinary closure settlement appeals, advisory controls, support boundaries and regression tests. |

## Recommended Learning and Project Sequence

1. Start with File 01 to understand derivative purpose, market structure and taxonomy.
2. Read File 02 carefully because options are the building block behind many structured notes and equity strategies.
3. Read File 03 to understand futures, forwards, FX and margin mechanics.
4. Read File 04 for OTC swaps and CDS, which are important for institutional portfolios and structured products.
5. Read File 05 for valuation, Greeks, exposure and risk modelling.
6. Read File 06 for advisory, mandate, DPM analytics and reporting.
7. Use File 07 as an implementation and QA reference.
8. Use File 08 for glossary, product combinations and practitioner reference.
9. Use File 09 for worked examples, QA scenarios, support-boundary decisions and implementation review.

## Core mental model

A derivative is a contract whose value depends on an underlying asset, rate, index, currency, commodity, credit event or benchmark.

```text
Derivative value = value of future contractual payoff linked to an underlying risk factor
```

A derivative may create exposure with little upfront cash. Therefore, for portfolio analytics and mandate control, book value is not enough.

```text
Derivative platform complexity = contract terms + lifecycle + valuation + margin/collateral + exposure + counterparty risk + suitability + reporting
```

## Design principle for wealth platforms

Model derivatives as contract positions with:

- product master / contract terms;
- underlyings and reference data;
- trade and lifecycle events;
- premium, settlement, margin and collateral cashflows;
- valuation and Greeks;
- exposure measures such as notional, delta-equivalent exposure, DV01 or CS01;
- counterparty, clearing broker and collateral relationships;
- mandate and suitability classification;
- performance, P&L and reporting treatment.

Do not rely only on market value. A derivative with small or zero market value may carry very large risk exposure.

## Key external education and market-structure sources

The following public sources were used to validate core concepts:

- Investor.gov introduction to options: https://www.investor.gov/introduction-investing/general-resources/news-alerts/alerts-bulletins/investor-bulletins-63
- Investor.gov options glossary: https://www.investor.gov/introduction-investing/investing-basics/glossary/options
- FINRA options education: https://www.finra.org/investors/investing/investment-products/options
- FINRA 0DTE options risk discussion: https://www.finra.org/investors/insights/zeroing-in-options-trading-strategy
- OCC Characteristics and Risks of Standardized Options: https://www.theocc.com/company-information/documents-and-archives/options-disclosure-document
- CFTC futures market basics: https://www.cftc.gov/LearnAndProtect/EducationCenter/FuturesMarketBasics/index2.htm
- CFTC Learn & Protect advisories: https://www.cftc.gov/LearnAndProtect/AdvisoriesAndArticles/learn_to_trade_without_scam.htm
- ISDA documentation and OTC derivatives market resources: https://www.isda.org/
- CME education on futures and options: https://www.cmegroup.com/education.html
- LCH / clearing house education resources: https://www.lch.com/

## Important note

This pack is for education, architecture, product understanding and platform design. It is not investment advice, trading advice, legal advice, tax advice, accounting advice or a substitute for offering documents, ISDA/CSA agreements, exchange rules, clearing broker specifications, custodian feeds, margin methodology, local regulatory requirements or approved product documentation.

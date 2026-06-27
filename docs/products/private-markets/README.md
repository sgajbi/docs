# Private Markets and Alternatives Reference Pack

## Purpose

This pack is part of the high-quality wealth product knowledge base being built for re-study, office knowledge sharing, advisory, discretionary mandate design, portfolio analytics, reporting, and wealth-platform implementation.

Private markets and alternatives are especially important in private banking and HNW/UHNW wealth because they introduce product behaviours that are very different from public equities, bonds, funds, derivatives and structured products:

- commitments rather than fully funded positions;
- capital calls rather than normal subscriptions only;
- unfunded commitment monitoring;
- distributions split into return of capital, income and realised gain;
- quarterly or lagged NAVs rather than daily exchange prices;
- long lock-ups and limited liquidity;
- J-curve behaviour;
- vintage-year and fund-life analysis;
- valuation uncertainty;
- complex fees, carried interest and waterfalls;
- side letters, gates, queues and transfer restrictions;
- suitability and mandate constraints around illiquidity, concentration and sophistication.

The pack is written for a wealth-management platform architect / portfolio analytics SME. It focuses not only on what the products are, but how they should be represented in a portfolio system: instrument master, position model, transaction model, lifecycle events, valuation, performance, risk, advisory controls, mandate rules and reporting.

**Curation note:** Curated from provided source material on 2026-06-27 and normalized for reusable practitioner reference.

**Last updated:** 2026-06-27

## Pack structure

1. `01-private-markets-fundamentals-and-taxonomy.md`
   Defines private markets and alternatives, compares them with public markets, and explains major product families.

2. `02-private-equity-venture-capital-growth-buyout-secondaries.md`
   Covers private equity, venture capital, growth equity, buyout, secondaries, co-investments and direct private company exposure.

3. `03-private-credit-direct-lending-distressed-mezzanine.md`
   Covers private credit, direct lending, mezzanine, distressed debt, special situations, asset-based lending and credit-risk analytics.

4. `04-real-assets-real-estate-infrastructure-natural-resources.md`
   Covers real estate, infrastructure, natural resources and real-asset fund structures.

5. `05-hedge-funds-and-liquid-alternatives.md`
   Covers hedge funds, liquid alternatives, strategies, lock-ups, gates, subscriptions/redemptions and risk analytics.

6. `06-lifecycle-transactions-position-commitment-and-cashflow-modelling.md`
   Gives a detailed platform model for commitments, capital calls, distributions, recallable capital, NAV updates, secondaries, redemptions, transfers and position accounting.

7. `07-data-model-valuation-nav-performance-risk-and-analytics.md`
   Covers data model, NAV lag, valuation hierarchy, IRR, TVPI, DPI, RVPI, PME, J-curve, risk analytics and portfolio reporting metrics.

8. `08-advisory-mandate-suitability-reporting-and-client-experience.md`
   Covers advisory suitability, mandate controls, DPM treatment, client reporting, portfolio review, liquidity planning, concentration and risk disclosure.

9. `09-platform-implementation-controls-reconciliation-test-scenarios-glossary.md`
   Covers implementation architecture, controls, reconciliation, data quality, QA scenarios, migration issues, production triage, glossary and practitioner-ready explanations.

10. `10-source-notes-and-further-reading.md`
    Captures source notes, regulatory nuance, and further learning topics.

11. `11-worked-examples-and-implementation-patterns.md`
    Provides worked examples for commitment ledgers, capital calls, distribution classification, NAV lag, restatements, partial-history IRR, private credit distributions, hedge fund gates, secondary sales, advisory controls, support boundaries and regression tests.

## Core modelling principle

Private markets should not be modelled like normal daily-priced securities only. They need a two-layer position model:

```text
Legal commitment layer
  - committed amount
  - called capital
  - unfunded commitment
  - recallable capital
  - commitment currency
  - commitment date and closing

Economic holding layer
  - contributed capital
  - remaining NAV / residual value
  - distributions received
  - realised and unrealised value
  - valuation date and lag
  - fund life, vintage and strategy exposure
```

In other words:

```text
Client exposure = funded NAV + unfunded obligation + expected future cashflows
```

A client may appear to hold only a small current NAV but still have a large future obligation through unfunded commitments. This is critical for buying power, suitability, liquidity planning, mandate monitoring and risk reporting.

## Relationship to earlier packs

- Funds pack: private funds are funds, but with commitment/capital-call mechanics and lower liquidity.
- Bonds pack: private credit resembles fixed income, but valuation, liquidity, covenants and repayment behaviour differ.
- Equities pack: private equity has equity-like return drivers but is not daily traded and often has fund-level reporting.
- Derivatives pack: hedge funds and alternative strategies may use derivatives, leverage, shorting and swaps.
- Structured products pack: some alternative exposure may be delivered via structured notes, certificates or feeder funds.
- Cross-product addenda: use the canonical position/transaction model and advisory/mandate framework consistently.

## Important note on regulation and sources

This pack is educational and implementation-oriented. It is not legal, tax, accounting, regulatory or investment advice. Private market rules differ by jurisdiction, product wrapper, client type, distribution channel and adviser status.

A regulatory nuance: the SEC adopted private fund adviser rules in 2023, but those rules were vacated by the U.S. Fifth Circuit in 2024, and the SEC announced that the newly adopted rules were vacated. Do not treat the 2023 U.S. private fund adviser reform package as an active universal requirement. Still, the concepts behind transparency around performance, fees, expenses, conflicts, valuation and audits remain useful as strong platform/reporting design principles.

## Sources for factual grounding

Key sources used for factual grounding and terminology include:

- SEC Investor.gov: private equity funds and hedge funds investor education.
- SEC resources for private funds and private fund adviser rule-status announcement.
- FINRA private placements guidance, including due diligence and suitability focus.
- Federal Reserve research on private credit characteristics and risks.
- MoneySense Singapore educational resources where relevant for investor risk framing.
- Common industry terminology used by ILPA, CFA/GIPS/private-market reporting practice and major private-market administrators, with implementation guidance adapted for wealth-platform design.

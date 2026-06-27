# Real Estate, REITs, Infrastructure and Real-Asset Funds Reference Pack

## Purpose

This pack is part of a high-quality wealth product knowledge base for re-study, office knowledge sharing, advisory, discretionary mandate design, portfolio analytics, reporting and wealth-platform implementation.

The product family covered here sits between public-market securities and private-market alternatives. It includes:

- listed REITs;
- non-traded / unlisted REITs;
- property companies and real estate operating companies;
- business trusts and stapled trusts;
- private real estate funds;
- direct property and fractional/property-linked structures;
- infrastructure funds and listed infrastructure securities;
- real-asset funds covering logistics, data centres, renewables, transport, utilities, towers, social infrastructure and other long-duration assets.

These products are important in private banking because they create income, diversification, inflation-sensitive exposure and alternative risk premia. They also create platform complexity because the same economic theme can appear through many wrappers: listed security, fund, private fund, trust, structured product, direct asset, loan-backed exposure or mandate overlay.

## Why this pack matters for advisory, mandates, analytics and reporting

Real estate and infrastructure are often presented as “income” or “real asset” allocations, but the product behaviour differs materially by wrapper.

A listed REIT behaves operationally like an equity, but economically it has property rent, leverage, interest-rate sensitivity, distribution policy and sector exposure.

A private real estate fund behaves like a private markets product with capital calls, distributions, NAV lag, appraisal uncertainty and long lock-up.

An infrastructure fund can behave like a hybrid of real asset, utility equity, private equity and inflation-linked cashflow exposure.

A wealth platform therefore needs to capture both:

```text
Accounting wrapper = equity / fund / private fund / trust / direct asset
Economic exposure = real estate / infrastructure / real asset sectors
```

This distinction is essential for:

- asset allocation;
- suitability;
- DPM mandate monitoring;
- portfolio review;
- performance contribution;
- income reporting;
- liquidity reporting;
- risk and concentration analytics;
- collateral and buying-power treatment;
- client and advisor explanations.

## Pack structure

1. `01-real-estate-reits-infrastructure-fundamentals-and-taxonomy.md`
   Explains the product family, wealth context, wrapper taxonomy and key economic drivers.

2. `02-listed-reits-business-trusts-and-property-securities.md`
   Covers listed REITs, S-REITs, REIT ETFs, property companies, business trusts, stapled securities, distributions and corporate actions.

3. `03-private-real-estate-funds-direct-property-and-property-linked-vehicles.md`
   Covers private real estate funds, core/core-plus/value-add/opportunistic strategies, direct property exposure, fractional structures and valuation/lifecycle differences.

4. `04-infrastructure-and-real-asset-funds.md`
   Covers infrastructure sectors, listed/private infrastructure, renewables, utilities, transport, social infrastructure, towers, data centres and inflation linkage.

5. `05-lifecycle-transactions-position-cashflow-and-event-modelling.md`
   Provides lifecycle, transaction types, position modelling, distributions, subscriptions/redemptions, capital calls, property income and event handling.

6. `06-data-model-valuation-nav-income-performance-risk-and-analytics.md`
   Covers common data model, valuation, NAV, appraisal, yield metrics, leverage, risk, income analytics, performance and look-through exposure.

7. `07-advisory-mandate-suitability-and-client-reporting-deep-dive.md`
   Covers advisory, suitability, DPM mandates, concentration, liquidity, income expectations, client reporting and portfolio review treatment.

8. `08-platform-implementation-controls-reconciliation-test-scenarios-and-glossary.md`
   Covers implementation, controls, reconciliations, QA scenarios, migration checks, production triage, glossary and practitioner-ready explanations.

9. `09-source-notes-and-further-reading.md`
   Provides factual grounding and source notes.

10. `10-worked-examples-and-implementation-patterns.md`
    Provides worked examples for listed REIT distributions, rights issues, private real estate capital calls, appraisal restatements, infrastructure fund distributions, redemption queues, direct property valuation, leverage analytics, advisory controls, support boundaries and regression tests.

## Core modelling principle

Do not model the theme only. Model both wrapper and economic exposure.

```text
Product wrapper:
  listed equity / REIT / trust / ETF / mutual fund / private fund / direct asset / structured product

Economic exposure:
  office / retail / industrial / logistics / data centre / residential / healthcare / hospitality / infrastructure / renewables / utilities

Lifecycle behaviour:
  exchange trade / NAV fund trade / capital call / distribution / appraisal update / redemption queue / corporate action
```

A listed REIT position can use equity-like trading and settlement, but reporting should understand property sector, geographic exposure, distribution yield, leverage and interest-rate sensitivity.

A private real estate fund position should use private-market commitment/NAV mechanics, but reporting should still understand property type, geography, leverage, occupancy, valuation lag and income distribution.

An infrastructure fund should understand contract duration, inflation linkage, regulatory exposure, concession risk and cashflow stability.

## Relationship to other product packs

- Equities pack: listed REITs and property companies trade like equities and share many corporate-action mechanics.
- Funds pack: REIT funds, infrastructure funds and real-asset funds follow fund subscription/redemption and NAV mechanics.
- Private Markets pack: private real estate and infrastructure funds follow commitment, capital-call, NAV-lag and distribution mechanics.
- Bonds pack: infrastructure debt and real estate debt behave like fixed income with collateral/covenant overlays.
- Loans/Collateral pack: real estate and REITs can be collateral with LTV/haircut rules; property loans and mortgage-linked credit lines affect buying power.
- Structured Products pack: property/REIT/index exposure can be packaged into structured notes/certificates.
- Cash/FX pack: distributions, cross-border property income and asset valuations often create FX exposure.

## Practitioner summary

Real estate and infrastructure are not one simple asset class. They are a set of product wrappers that deliver exposure to physical assets, property income, operating assets, concession cashflows, infrastructure services and inflation-sensitive assets. For wealth platforms, the challenge is to reconcile listed security behaviour, fund behaviour, private-market behaviour and real-asset analytics into a consistent portfolio model.

## Source provenance

Imported from C:\Users\Sandeep\Downloads\real_estate_reits_infrastructure_reference_pack.zip\real_estate_reits_infrastructure_reference_pack on 2026-06-27. The imported material was normalized for reusable practitioner framing.

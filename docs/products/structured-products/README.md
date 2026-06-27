# Structured Products Reference Pack

## Purpose

This pack is part of the high-quality wealth product knowledge base. It is designed for re-study, office knowledge sharing, advisory conversations, mandate design, portfolio analytics, reporting, and wealth-platform implementation.

Structured products connect the product families already covered in the knowledge base:

- bonds and notes provide the legal and funding wrapper;
- equities, indices, FX, rates, credit and commodities provide reference underlyings;
- derivatives create the payoff profile;
- funds and portfolios provide allocation, suitability, mandate and reporting context.

The pack intentionally goes beyond structured notes. Notes are one major wrapper, but structured exposure can also appear through deposits, certificates, warrants, structured funds, accumulators, decumulators, autocallables, participation products, buffer products, leveraged certificates and other market-linked investments.

**Curation note:** Curated from provided source material on 2026-06-27 and normalized for reusable practitioner reference.

**Last updated:** 2026-06-27

## Pack structure

1. `01-structured-products-fundamentals-and-taxonomy.md`  
   Defines structured products, common wrappers, product families, client objectives, risks and how structured products differ from bonds, notes, funds, equities and derivatives.

2. `02-payoff-building-blocks-product-combinations-and-examples.md`  
   Explains the payoff building blocks: debt legs, deposit legs, options, barriers, autocall, participation, caps, floors, buffers, leverage, coupon conditions, worst-of baskets and product combinations.

3. `03-lifecycle-transactions-position-and-event-modelling.md`  
   Describes lifecycle stages, transaction types, position modelling, event modelling, observation processing, settlement, early termination, maturity, physical delivery and corrections.

4. `04-data-model-valuation-risk-and-scenario-analytics.md`  
   Gives a platform-oriented data model for structured products, valuation approaches, model inputs, Greeks, sensitivities, stress testing, scenario analysis and controls.

5. `05-advisory-mandate-analytics-and-reporting-deep-dive.md`  
   Explains structured products in advisory, discretionary mandates, portfolio construction, concentration monitoring, suitability, risk disclosure, performance, contribution, attribution and client reporting.

6. `06-platform-implementation-controls-reconciliation-and-test-scenarios.md`  
   Provides implementation patterns, controls, reconciliation rules, data-quality checks, QA scenarios, migration considerations and production triage guidance.

7. `07-glossary-practitioner-reference-and-review-checklists.md`  
   Provides a glossary, practitioner-ready explanations, common mistakes, product comparison tables and reusable review checklists.

8. `08-worked-examples-and-implementation-patterns.md`
   Provides worked examples for autocallables, memory coupons, principal protection, buffers, dual currency investments, accumulators, decumulators, certificates, warrants, valuation controls, suitability, mandate treatment, support boundaries and regression tests.

## Core modelling principle

A structured product should be modelled as a legally held instrument or account product, not as every embedded component being separately held by the client.

```text
Client position = legal wrapper actually held by client
Embedded economics = payoff terms, reference assets, derivative legs and lifecycle rules
Look-through exposure = analytical exposure for risk, concentration and reporting
Actual new holding = created only if delivered or converted legally
```

For example, if a client holds an equity-linked structured product referencing Apple, the client does not normally hold Apple shares. Apple should be represented as an underlying and look-through exposure. Apple shares should become an actual position only if the structure physically settles into Apple shares.

## Relationship to other packs

- Structured notes pack: detailed treatment of issuer-note wrappers and note-specific modelling.
- Bonds pack: debt, coupon, yield, duration and credit concepts reused in structured products.
- Funds pack: wrappers, NAVs, look-through and fund-style structured strategies.
- Equities pack: equity underlyings, corporate actions and physical delivery.
- Derivatives pack: options, forwards, futures, swaps, CDS, Greeks and margin concepts used inside structured products.

## Sources used for factual grounding

This pack is written as an implementation-aware educational guide. It is not legal, tax, accounting or investment advice. Key investor-education and regulatory references include:

- Investor.gov / SEC investor bulletins on structured notes and principal protection.
- FINRA investor guidance on structured notes and higher-risk structured products.
- MoneySense Singapore guidance on structured notes, structured deposits and Specified Investment Products.
- MAS guidance and safeguards around Specified Investment Products / complex products.
- Product education materials from major wealth managers and exchanges where useful for terminology.

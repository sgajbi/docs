# Product Governance, Product Due Diligence, Approved Product Universe and Target Market Framework

## Purpose

This pack explains how a wealth-management organisation should govern investment products before they are recommended, offered, traded, held in mandates, reported to clients, or consumed by suitability and advisory engines.

It is intended for:

- wealth management technology architects,
- portfolio analytics and reporting teams,
- advisory platform owners,
- discretionary portfolio management teams,
- business analysts,
- product due-diligence teams,
- compliance and risk-control stakeholders,
- engineering teams building product-master, suitability, proposal, order and reporting platforms.

The pack is product-agnostic but uses examples from bonds, notes, funds, equities, derivatives, private markets, insurance-linked products, real assets and cash/FX products.

## Curation Note

Curated from provided source material on 2026-06-27 and normalized for reusable practitioner, product, platform and implementation reference.

## Core idea

A wealth platform should not treat product governance as static reference data. Product governance is a control framework that determines:

- which products are approved,
- for which client segments,
- in which booking centres and jurisdictions,
- through which channels,
- under which advisory or execution-only models,
- with which risk ratings, disclosures, restrictions and lifecycle review rules,
- and how the decision is evidenced for audit.

The Approved Product Universe (APU) is the executable version of product governance. It is the controlled product list and eligibility layer consumed by advisory, suitability, proposal, mandate, order, reporting and monitoring systems.

## Pack structure

1. `01-product-governance-fundamentals-and-operating-model.md`
2. `02-product-due-diligence-approval-and-apu-framework.md`
3. `03-target-market-client-segmentation-and-distribution-strategy.md`
4. `04-product-risk-rating-complexity-liquidity-and-value-assessment.md`
5. `05-product-lifecycle-review-suspension-exit-and-change-governance.md`
6. `06-data-model-apu-eligibility-rules-disclosures-and-lineage.md`
7. `07-advisory-suitability-mandate-and-pretrade-integration.md`
8. `08-platform-controls-reconciliation-qa-and-audit-scenarios.md`
9. `09-glossary-practitioner-reference-and-review-checklists.md`
10. `10-source-notes-and-further-reading.md`
11. `11-worked-examples-and-implementation-patterns.md`

## How this pack fits with the previous product packs

Previous packs explain the products and analytics. This pack explains whether those products should be available at all, and under what conditions.

| Existing knowledge area | Product-governance connection |
|---|---|
| Notes / structured products | complexity, payoff explainability, issuer risk, barrier risk, target market, disclosure, lifecycle review |
| Bonds | issuer/credit risk, rating, duration, liquidity, callable/subordinated/perpetual restrictions |
| Funds | share class selection, fees, liquidity gates, trailer fees, fund due diligence, target market |
| Equities | market, sector, concentration, restricted lists, sanctioned issuers, corporate-action risk |
| Derivatives | leverage, margin, appropriateness, client experience, pre-trade checks, mandate eligibility |
| Private markets | illiquidity, accredited/professional investor restrictions, capital calls, vintage risk |
| Cash/FX | deposit protection, FX suitability, settlement risk, FX leverage, DCI restrictions |
| Loans/lombard | collateral eligibility, LTV, leverage suitability, margin calls, pledged asset restrictions |
| Insurance/annuities | surrender value, policy complexity, rider disclosure, estate planning, premium financing |
| Tax/reporting | client documentation, withholding, cross-border restrictions, reportable attributes |
| Performance/risk | mandate suitability, ex-post monitoring, concentration, drawdown, stress and scenario outputs |

## Design principle

Product governance should be implemented as a versioned decision and rule framework, not as scattered flags across systems.

A good platform should be able to answer:

1. Why was this product approved?
2. Who approved it?
3. For whom is it intended?
4. For whom is it not intended?
5. Where can it be sold?
6. Through which channel can it be sold?
7. What disclosures are required?
8. What suitability checks are required?
9. What mandate restrictions apply?
10. What changed since the product was approved?
11. Why was this product available to this client at this time?
12. Why was this recommendation allowed, blocked or escalated?

## Important disclaimer

This pack is educational and platform-design oriented. It is not legal, regulatory, tax, compliance or investment advice. Jurisdiction-specific rules should be configured using official firm policies and legal/compliance interpretations.

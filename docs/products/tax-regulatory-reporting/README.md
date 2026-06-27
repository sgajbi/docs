# Tax-Aware Investment Reporting, Regulatory Reporting and Cross-Border Client Reporting Reference Pack

## Purpose

This reference pack explains how tax-aware investment reporting, regulatory reporting and cross-border client reporting should be understood and modelled in a private-banking / wealth-management platform.

It is designed for:

- product and domain learning;
- portfolio analytics and client reporting design;
- advisory and mandate management discussions;
- BA, QA, operations and production-support workflows;
- architecture and implementation of wealth platforms;
- office knowledge sharing and durable practitioner reference.

This pack is **not tax advice**. Tax outcomes are jurisdiction-specific and client-specific. The platform should support configurable tax classifications, withholding records, tax-lot logic, beneficial-owner documentation and reporting outputs, but final legal/tax interpretation should come from tax, legal, compliance or external advisers.

## Why this topic matters

All product families studied so far eventually flow into reporting and tax-aware analytics:

- equities create dividends, sales, corporate actions and tax-lot implications;
- bonds create coupons, accrued interest, amortisation/accretion and redemption events;
- funds create distributions, reinvestments, equalisation and look-through classifications;
- notes create coupons, redemptions, physical deliveries and issuer/reference events;
- derivatives create realised/unrealised P&L, margin, premiums, exercise/assignment and settlement outcomes;
- private markets create capital calls, distributions, return of capital, carried-interest-style economics and delayed NAVs;
- real estate/REITs create rental-income-like distributions, capital returns, withholding and leverage disclosures;
- insurance and annuities create premiums, surrender values, policy loans, claims and annuity income;
- trusts and family-office structures define who owns, controls, benefits from and reports the assets.

## Files included

1. `01-tax-aware-reporting-fundamentals-and-taxonomy.md`
2. `02-income-capital-gains-withholding-and-product-tax-treatment.md`
3. `03-tax-lots-cost-basis-realized-pnl-and-corporate-action-adjustments.md`
4. `04-cross-border-reporting-crs-fatca-qi-and-client-tax-documentation.md`
5. `05-regulatory-client-reporting-statements-and-disclosure.md`
6. `06-data-model-transactions-ledger-and-reporting-architecture.md`
7. `07-advisory-mandate-suitability-and-client-experience.md`
8. `08-platform-controls-reconciliation-test-scenarios-and-glossary.md`
9. `09-source-notes-and-further-reading.md`
10. `10-worked-examples-and-implementation-patterns.md`

## Core design principle

The platform should distinguish:

```text
Economic event -> Accounting transaction -> Tax classification -> Reporting output
```

The same economic event can have different tax treatment depending on jurisdiction, client tax residency, beneficial-owner status, account wrapper, treaty documentation, product type, source country, holding period and reporting regime.

Do not hardcode tax logic inside product transaction processing. Store clean product economics first; then enrich them with jurisdiction-specific tax classification, withholding, documentation and reportability rules.

## Recommended reading order

1. Start with fundamentals and taxonomy.
2. Study income, withholding and product tax-treatment patterns.
3. Study tax lots, cost basis and realised P&L.
4. Study CRS, FATCA, QI and client documentation.
5. Study reporting statements and disclosures.
6. Study data model and platform architecture.
7. Study advisory and mandate workflows.
8. Use the control and QA file for implementation validation.
9. Use the worked examples file for income, withholding, tax lots, corporate actions, fund distributions, structured-note delivery, documentation expiry, report corrections, wash-sale style controls, cross-border lot-method conflicts, mid-year residency changes, QI pool reporting, estate/trust statements, CRS/FATCA remediation, filing rejections, treaty-rate expiry, nominee withholding statements, tax voucher correction chains, portfolio transfer year-end statements, withholding-rate override governance, cross-border report suppression, self-certification conflicts, beneficial-owner look-through breaks, CRS cancellation workflows, withholding reclaim denials, instrument tax-classification overrides, tax-pack restatement notices, tax voucher duplicate suppression, cross-border reporting blackout windows, entity classification remediation, missing treaty-documentation ageing, withholding pool variance remediation, multi-jurisdiction tax-pack delivery controls, support boundaries and regression tests.

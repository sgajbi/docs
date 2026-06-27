# Investment Accounting, Ledger, Fees, P&L, Accruals and Book-of-Record Design Reference Pack

## Purpose

This pack documents the accounting-grade layer behind a private-banking / wealth-management platform. The earlier product packs explain products, payoffs, lifecycle events, advisory treatment and reporting needs. This pack explains how those events become a controlled **book of record**: cash balances, positions, lots, income, expenses, accruals, fees, realized P&L, unrealized P&L, FX effects, ledger postings, reconciliation records and reporting outputs.

It is written for product knowledge, architecture, business analysis, portfolio analytics, reporting, investment operations, QA, migration and production-support work. It is not accounting, audit, legal, tax or regulatory advice.

## Source Provenance

| Source | Details |
|---|---|
| Source pack | `C:\Users\Sandeep\Downloads\investment_accounting_ledger_reference_pack.zip\investment_accounting_ledger_reference_pack` |
| Import date | 2026-06-27 |
| Local treatment | Imported, normalized, enriched with worked examples, and indexed into the product knowledge library. |

## Recommended repo location

For your `sgajbi/docs` repo, the suggested target folder is:

```text
docs/products/investment-accounting-ledger/
```

This follows the repo pattern of putting product/domain knowledge packs under `docs/products/` and keeping numbered files when reading order matters.

## File map

| File | Purpose |
|---|---|
| `01-investment-accounting-fundamentals-and-book-of-record-design.md` | Core concepts, book-of-record types, accounting vs analytics, event-to-ledger model |
| `02-transaction-ledger-position-cash-and-lot-modelling.md` | Canonical data model for transactions, legs, cash, positions, lots and sub-ledgers |
| `03-trade-date-settlement-date-accruals-income-and-expense-accounting.md` | Trade-date vs settlement-date accounting, accruals, income entitlement, expenses and reversals |
| `04-cost-basis-realized-unrealized-pnl-fx-and-multi-currency.md` | Cost basis, lots, realized/unrealized P&L, FX translation and local/base/reporting currency |
| `05-fees-charges-tax-withholding-and-client-charging-models.md` | Fees, charges, rebates, trailer fees, withholding, gross/net reporting and fee allocation |
| `06-product-specific-accounting-treatment-across-asset-classes.md` | Product-specific treatment for cash, bonds, funds, equities, notes, derivatives, private markets, loans, insurance and real assets |
| `07-valuation-pricing-hierarchy-nav-amortization-and-fair-value-controls.md` | Valuation hierarchy, pricing sources, amortization, NAV, fair value controls and stale-price handling |
| `08-reporting-performance-reconciliation-and-audit-lineage.md` | How accounting feeds performance, reporting, reconciliation, audit, client statements and production support |
| `09-platform-implementation-controls-test-scenarios-and-glossary.md` | Architecture patterns, controls, data-quality rules, test scenarios, migration checks and glossary |
| `10-source-notes-and-further-reading.md` | Source notes, standards, reading list and implementation caveats |
| `11-worked-examples-and-implementation-patterns.md` | Practical examples for postings, trade-date and settlement-date views, lot P&L, FX translation, fee accruals, reconciliation breaks and correction lineage |

## Core design principle

A mature wealth platform should not store a transaction as a single flat row and then guess its impact downstream. It should record an auditable event chain:

```text
Source event -> normalized transaction -> accounting legs -> cash/position/lot movement -> ledger posting -> valuation -> analytics/reporting -> reconciliation evidence
```

The key design principle is **separation with traceability**:

- keep product lifecycle events separate from accounting postings;
- keep transaction facts separate from derived balances;
- keep trade date, settlement date, booking date and effective date explicit;
- keep local, settlement, portfolio-base and reporting currencies explicit;
- keep gross, fee, tax and net amounts explicit;
- keep valuation and accounting classifications explicit;
- keep every derived output traceable to source transactions, prices, FX rates and rules.

## Audience

This pack is intended for:

- wealth-platform architects;
- portfolio accounting and investment operations teams;
- portfolio analytics and performance teams;
- client-reporting teams;
- advisory and DPM platform teams;
- business analysts and product owners;
- QA, reconciliation and production-support teams;
- data migration and conversion teams;
- engineers designing transaction, position, ledger, reporting and accounting services.

## Important caveat

Accounting standards, tax treatment, client-reporting obligations and booking rules vary by jurisdiction, bank policy, custodian, accounting basis, client type and booking centre. The material here should be treated as product/platform knowledge and design guidance, not as a substitute for accounting-policy, tax, legal or regulatory sign-off.

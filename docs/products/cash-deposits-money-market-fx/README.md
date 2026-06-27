# Cash, Deposits, Money Market and FX Reference Pack

## Purpose

This pack is part of a broader wealth-management product knowledge base covering notes, bonds, funds, equities, derivatives, structured products and private markets. It focuses on the product family that sits underneath almost every portfolio process: **cash, deposits, money market instruments, money market funds and foreign exchange**.

These products look simple, but they drive many difficult platform, advisory and analytics questions:

- available cash versus ledger cash versus settled cash;
- order funding and buying power;
- multi-currency portfolios;
- settlement and value-date cash projections;
- interest accrual and deposit maturity;
- money market fund liquidity and stable/floating NAV behaviour;
- FX conversion, hedging and realised/unrealised FX P&L;
- overdraft, margin, collateral and credit-line usage;
- DPM mandate liquidity rules;
- portfolio performance treatment of cashflows, income and FX translation;
- client reporting of cash, yield, liquidity and currency exposure.

**Source provenance:** Imported from `C:\Users\Sandeep\Downloads\cash_deposits_money_market_fx_reference_pack.zip\cash_deposits_money_market_fx_reference_pack` into `docs/products/cash-deposits-money-market-fx/` on 2026-06-27.

**Last updated:** 2026-06-27

## Files

1. `01-cash-deposits-money-market-fx-fundamentals-and-taxonomy.md`
   Product taxonomy and business meaning.

2. `02-cash-accounts-balances-deposit-lifecycle-and-transactions.md`
   Cash account structures, balance types, deposit lifecycle and transaction modelling.

3. `03-money-market-instruments-funds-repos-and-liquidity-products.md`
   T-bills, commercial paper, certificates of deposit, repos, money market funds and liquidity products.

4. `04-fx-spot-forwards-swaps-ndf-lifecycle-and-position-modelling.md`
   FX spot, forwards, swaps, NDFs, settlement and FX transaction modelling.

5. `05-common-data-model-position-transaction-balance-and-ledger-design.md`
   Canonical product, position, transaction, balance, cash projection and ledger model.

6. `06-valuation-income-performance-risk-liquidity-and-buying-power.md`
   Valuation, accrued income, yield, performance, liquidity analytics, buying power and collateral.

7. `07-advisory-mandate-analytics-and-client-reporting-deep-dive.md`
   Advisory, mandates, portfolio analytics, reporting and client-experience treatment.

8. `08-platform-implementation-controls-reconciliation-test-scenarios-and-glossary.md`
   Implementation guidance, controls, QA scenarios, reconciliation and glossary.

9. `09-source-notes-and-further-reading.md`
   Source notes and suggested further reading.

## Core mental model

Cash is not a single number. A proper wealth platform should treat cash as a set of **currency-specific balances and projections**:

```text
Ledger cash
+ settled but not yet reflected transactions
+ pending settlements
+ income receivable
+ deposit maturities
- pending buys / withdrawals / fees / tax
= projected available cash
```

For advisory and portfolio analytics, the key is to separate:

- **accounting cash**: what is legally held in each account and currency;
- **available cash**: what can be used for an order after settlement and restrictions;
- **economic cash exposure**: currency and liquidity exposure;
- **performance cashflows**: external contributions/withdrawals versus internal investment activity;
- **funding/buying-power cash**: cash after credit-line, pledge, collateral and pending-order rules.

## Design principle

Model cash, deposits, money market instruments, money market funds and FX as related but distinct objects:

- cash balances are account/currency ledger positions;
- deposits are term products with maturity and interest terms;
- T-bills and money market instruments are securities or short-term debt instruments;
- money market funds are fund positions valued by NAV;
- FX spot and forward trades are multi-leg cash/derivative transactions;
- FX exposure is both a position-level and portfolio-level analytics dimension.

## Important Disclaimer

This pack is for education, product analysis, architecture, documentation, and platform design. It is not investment advice, legal advice, tax advice, accounting advice, regulatory advice, or client advice. Real treatment depends on jurisdiction, account terms, booking model, custodian records, settlement calendars, product documentation, and internal policy.

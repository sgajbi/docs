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

**Curation note:** Curated from provided source material on 2026-06-27 and normalized for reusable practitioner reference.

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

10. `10-worked-examples-and-implementation-patterns.md`
    Practical examples for available cash, deposit breakage, money market fund gates, FX spot settlement, FX forwards, NDFs, liquidity ladders, buying power, failed settlement, sweeps, overdrafts, negative interest, settlement holidays, stress liquidity, payment cut-offs, cash pooling, nostro reconciliation, deposit ladder repricing, payment recalls, booking-centre segmentation, intraday overdraft escalation, treasury sweep exceptions, trapped cash, blocked-market currency controls, pooled-client interest allocation, operational compensation, fraud recovery, emergency funding, cash haircut policy, liquidity transfer pricing, multi-entity treasury sweeps, payment prioritization, client-money segregation breaches, cross-border payment repair analytics, money-market fund liquidity fees, repo substitution failures, intraday nostro forecast variances, correspondent-bank fee disputes, account-level reserve optimization, FX settlement netting exceptions, FX swap rollover failures, deposit early-break approval disputes, virtual-account cash allocation, payment rail outage rerouting, treasury concentration-limit breaches, stale bank-balance certification, FX overlay funding waterfalls, liquidity stress communication, negative-rate client disclosure, multi-currency cash pledge disputes, payment screening queue ageing, treasury policy exception attestation, liquidity buffer model drift, payment beneficiary whitelist exceptions, deposit rollover suitability conflicts, intraday cash forecast confidence scoring, nostro overdraft fee allocation, FX conversion best-execution evidence, client cash yield tier changes, payment cut-off extension approvals, intraday liquidity stress-test backtesting, cross-currency liquidity buffer translation, returned-payment fee remediation, FX standing-instruction drift, dormant cash reactivation controls, same-currency cash pooling disputes, negative available cash from pending fees, payment purpose-code remediation, FX holiday mismatch repairs, treasury settlement prefunding exceptions, account closure residual balances, FX sweep rounding residuals, sanctions releases after cut-off, deposit break quote expiry, money-market fund redemption settlement delays, treasury counterparty settlement-limit exhaustion, backdated cash-rate corrections, intraday nostro cut-off breaches, payment beneficiary amendment disputes, liquidity forecast source outages, treasury rollover concentration exceptions and FX cash collateral substitution breaks.

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

# Cash, Deposits, Money Market And FX Knowledge Map

This page connects cash, deposits, money market products, and FX knowledge to reusable private-banking platform decisions.

Cash and FX sit underneath nearly every product workflow. They affect buying power, settlement, liquidity, performance, reporting currency, income, mandate rules, collateral, and client communication.

## Current Pack

| Product Area | Pack | Main Use |
|---|---|---|
| Cash, deposits, money market and FX | [`cash-deposits-money-market-fx/`](cash-deposits-money-market-fx/README.md) | Cash balances, deposits, money market instruments, money market funds, repos, FX spot, forwards, swaps, NDFs, liquidity, buying power, settlement, income, performance, advisory, reporting, controls, and QA reference. |

## Platform Design Distinctions

| Concept | Practical Treatment |
|---|---|
| Ledger cash | Accounting balance by account and currency. It is not automatically available for new orders. |
| Available cash | Cash that can be used after settlement, restrictions, pending orders, fees, taxes, overdraft rules, collateral, and credit-line policy. |
| Projected cash | Future dated view based on settlements, income receivables, deposit maturities, withdrawals, fees, tax, and known cashflows. |
| Deposit | Term product with principal, rate, maturity, accrual, rollover, breakage, and early-withdrawal treatment. |
| Money market instrument | Short-term debt/security exposure such as T-bill, commercial paper, certificate of deposit, or repo. |
| Money market fund | Fund position with NAV, liquidity terms, distribution/accrual behavior, and potential gate/suspension/stable-NAV nuance. |
| FX spot | Two-currency transaction with trade date, value date, dealt amount, counter amount, rate, spread, and settlement risk. |
| FX forward/swap/NDF | Forward-dated or derivative FX exposure with mark-to-market, fixing, settlement, hedge, and counterparty implications. |
| Multi-currency buying power | Indicative funding capacity after purchase-currency cash, source-currency cash, FX conversion, buffers, settlement timing, restrictions, pledge state and credit policy. |
| Cash sweep | Automated movement from operating cash into deposit, money market fund, sweep account or liquidity product; should preserve cash reservations, cut-offs, settlement and liquidity labels. |

## Funding And Liquidity Stress Cases

| Case | Platform Treatment |
|---|---|
| Approved overdraft | Separate settled cash, projected cash, overdraft utilization, facility limit, buffer and mandate permission. Do not show projected sale proceeds as available cash before settlement. |
| Negative interest | Accrue charges by currency, threshold, rate, period and booking centre. Report as expense/cash drag, not market loss. |
| Cross-currency settlement holiday | Calculate valid joint currency value date and identify security-versus-FX funding mismatches before approving orders. |
| Sweep unwind during stress | Treat money market sweep positions as liquidity products with NAV, cut-off, gate, settlement and redemption confirmation; do not equate them with bank cash. |
| Credit-line-funded purchase | Approve only after facility, collateral, haircut, currency, concentration, pledge and mandate checks pass; record drawdown and collateral reservation separately from cash. |

## Reusable Modelling Pattern

Use separate layers:

```text
Account And Currency Layer
  Account, currency, booking entity, cash ledger, restrictions

Balance Layer
  Ledger, settled, available, projected, reserved, collateral, overdraft, buying power

Instrument Layer
  Deposit terms, money market instrument terms, money market fund terms, FX contract terms

Transaction Layer
  Cash movement, interest, deposit placement/maturity, MMF order, FX spot, forward, swap, fee, tax

Projection Layer
  Settlement calendar, value date, income receivable, maturity cashflow, pending order, withdrawal

Analytics Layer
  Liquidity, yield, FX exposure, FX P&L, cash drag, contribution, funding gap, stress

Control Layer
  Reconciliation, overdraft checks, failed settlement, stale FX/rate, duplicate cashflow, audit trail
```

Do not collapse cash into one portfolio-level number. A platform should show at least currency, account, settlement status, restrictions, and projected availability when the workflow depends on funding or liquidity.

## Source Ownership Questions

Before building or changing a feature, identify the source of truth for:

| Data Area | Typical Source Candidates |
|---|---|
| Ledger cash | Custodian, core banking ledger, broker ledger, accounting system. |
| Available cash | Custodian availability feed, internal reservation engine, order-management rules, credit/collateral policy. |
| Pending settlements | Trade ledger, settlement system, custodian confirmations, payment queue. |
| Deposit terms | Product master, term sheet, core banking deposit system, treasury system. |
| Interest accrual | Deposit system, product terms, rate source, accrual engine. |
| Money market instrument price/yield | Market data, custodian, security master, valuation source, issuer/administrator. |
| Money market fund NAV/liquidity | Fund administrator, fund platform, custodian, market data vendor. |
| FX spot/forward rates | Dealing system, treasury, market data vendor, custodian confirmation. |
| FX settlement/fixing | Trade confirmation, settlement system, fixing source, custodian cash movement. |
| Reporting-currency translation | FX rate source, valuation policy, performance engine, reporting configuration. |

## API And UI Implications

APIs should expose:

1. currency-specific balances, not only aggregate cash,
2. ledger, settled, available, reserved, projected, and restricted balances separately,
3. value date, settlement date, and booking date,
4. pending cash movements with source and confidence,
5. deposit maturity, rate, accrual, rollover, and breakage terms,
6. money market NAV/yield/liquidity posture,
7. FX trade legs, rate, spread, value date, settlement status, and hedge link where applicable,
8. stale rate/NAV/rate-source flags,
9. buying-power basis and unsupported states,
10. linked FX funding requirement for cross-currency purchases,
11. cash sweep trigger, reservation, order, settlement and unwind state.
12. overdraft and credit-line funding basis where buying power includes borrowing capacity,
13. liquidity stress labels for gated funds, delayed FX, holidays, failed settlements and blocked source data.

UIs should make these states visible:

1. cash is not yet settled,
2. cash is reserved for pending orders or fees,
3. cash is in the wrong currency for a proposed purchase,
4. FX funding is required or blocked,
5. deposit maturity or rollover is pending,
6. money market fund liquidity is gated, suspended, or stale,
7. FX forward is hedge exposure rather than simple cash,
8. buying power includes or excludes credit/collateral.
9. FX-funded buying power is indicative until conversion and settlement policy are satisfied,
10. automatic sweeps can reduce operating cash and may create fund liquidity or settlement constraints.
11. negative interest, overdraft utilization, sweep gates and cross-currency settlement mismatch are present.

## QA Scenarios

High-value scenarios:

1. buy order reserves cash and later releases or consumes it on settlement,
2. sell order creates unsettled cash before availability,
3. dividend or coupon creates income receivable before cash receipt,
4. fixed deposit accrues interest and matures into cash,
5. early deposit break applies penalty and changes accrued interest,
6. money market fund redemption misses cut-off and settles next window,
7. FX spot converts one currency into another with correct value date,
8. FX forward marks to market without changing current cash until settlement,
9. NDF settles net cash after fixing,
10. projected cash blocks a purchase because pending settlement is late,
11. multi-currency purchase uses FX-funded buying power with haircut and value-date checks,
12. failed FX settlement restricts projected cash and opens exception workflow,
13. sweep order reserves excess cash and creates MMF units only after confirmed dealing NAV,
14. approved overdraft allows a trade only within facility, buffer and mandate limits,
15. negative interest accrues by threshold, rate period and currency,
16. cross-currency holiday creates a funding mismatch that is blocked, prefunded or bridged,
17. gated sweep unwind limits same-day withdrawal cash,
18. credit-line-funded purchase reserves collateral and records borrowing exposure.

## Useful Project Workflows

Use this pack when working on:

1. cash ledger and balance design,
2. buying-power and order-funding logic,
3. settlement and projected cash views,
4. deposit lifecycle and interest accrual,
5. money market instrument and MMF treatment,
6. FX spot/forward/swap/NDF modelling,
7. liquidity reporting and cash reserve monitoring,
8. multi-currency performance and FX P&L,
9. advisory funding checks and suitability notes,
10. DPM liquidity, drift, and rebalance funding workflows.

## Related Packs

| Pack | Relationship |
|---|---|
| [`funds/`](funds/README.md) | Money market funds follow fund order/NAV/liquidity patterns. |
| [`bonds/`](bonds/README.md) | T-bills, commercial paper, certificates of deposit, and repos reuse fixed-income yield, maturity, and credit concepts. |
| [`derivatives/`](derivatives/README.md) | FX forwards, swaps, NDFs, margin, collateral, and hedging share derivative concepts. |
| [`loans-lombard-margin-collateral/`](loans-lombard-margin-collateral/README.md) | Credit lines, pledged cash, collateral cash, overdrafts, availability, and buying-power rules. |
| [`private-markets/`](private-markets/README.md) | Capital calls, distributions, and liquidity planning depend on cash projection and available liquidity. |
| [`structured-products/`](structured-products/README.md) | Dual-currency investments, structured deposits, and FX-linked notes reuse structured payoff concepts. |

## Disclaimer

This map is for education, product analysis, architecture, documentation, and platform-design work only. It is not investment, legal, tax, accounting, regulatory, or client advice.

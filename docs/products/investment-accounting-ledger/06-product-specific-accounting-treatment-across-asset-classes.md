# 06 - Product-Specific Accounting Treatment Across Asset Classes

## 1. Why product-specific treatment matters

A common accounting model is needed, but product economics differ. The platform should use common transaction/leg/position structures with product-specific rules for lifecycle, valuation, income, cost and P&L.

## 2. Cash and deposits

| Topic | Treatment |
|---|---|
| Position | Cash balance by currency/account/value date |
| Income | Interest accrual and payment |
| Cost basis | Usually par/cash amount |
| Valuation | Nominal cash amount; FX translation for base/reporting currency |
| P&L | FX translation for foreign cash; interest income separately |
| Lifecycle | Deposit placement, rollover, maturity, early withdrawal, penalty |
| Controls | Value-date cash, overdraft, blocked cash, interest-rate changes |

## 3. Bonds and fixed income

| Topic | Treatment |
|---|---|
| Position | Nominal/face amount, quantity if needed |
| Income | Coupon accrual, coupon receipt, accrued interest paid/received on trade |
| Cost | Clean cost plus configured transaction costs; premium/discount amortization if applicable |
| Valuation | Clean/dirty price, accrued interest, yield, spread |
| P&L | Price P&L, accrued income, amortization, FX, credit impairment/write-down |
| Lifecycle | Buy/sell, coupon, call, put, maturity, default, restructuring |
| Controls | Day count, ex-coupon, schedule, stale price, issuer mapping |

## 4. Equities

| Topic | Treatment |
|---|---|
| Position | Shares/units; long/short if supported |
| Income | Dividends, withholding tax, stock dividends |
| Cost | Lot-based cost; adjusted for splits, spin-offs, return of capital |
| Valuation | Exchange close, bid/ask, corporate-action adjusted prices |
| P&L | Realized on sale, unrealized mark-to-market, FX translation |
| Lifecycle | Trade, settlement, dividend, split, rights, merger, tender, delisting |
| Controls | Corporate-action completeness, ex-date entitlement, tax-lot depletion |

## 5. Funds, ETFs and unit trusts

| Topic | Treatment |
|---|---|
| Position | Units/shares; fractional units common |
| Income | Distributions, reinvestment, equalization if supported |
| Cost | Average cost or lots based on jurisdiction/policy |
| Valuation | NAV for funds; exchange price/NAV for ETFs |
| P&L | NAV movement, distributions, FX |
| Lifecycle | Subscription, redemption, switch, transfer, distribution, reinvestment |
| Controls | Dealing date versus settlement date, NAV lag, gates, suspended funds |

## 6. Structured notes and structured products

| Topic | Treatment |
|---|---|
| Position | Note nominal/security position |
| Income | Fixed, floating, conditional or memory coupon when payable |
| Cost | Issue/purchase cost; not underlying asset cost unless physical delivery |
| Valuation | Issuer price, model value, fair value hierarchy |
| P&L | Mark-to-market, coupon, redemption, physical settlement loss/gain |
| Lifecycle | Observation, barrier, autocall, maturity, physical delivery, credit event |
| Controls | Lifecycle event separate from transaction, issuer risk, underlying links |

## 7. Derivatives

| Topic | Treatment |
|---|---|
| Position | Contract quantity, notional, exposure, long/short |
| Income | Premium, variation margin, swap cashflows, option exercise/expiry |
| Cost | Option premium, initial margin not cost, variation margin as settlement/P&L |
| Valuation | Fair value / mark-to-market; Greeks and exposure |
| P&L | Realized/unrealized, daily variation margin, FX, collateral interest |
| Lifecycle | Trade, margin, reset, exercise, expiry, closeout, novation |
| Controls | Notional sign, multiplier, contract specs, collateral, counterparty |

## 8. Private markets and alternatives

| Topic | Treatment |
|---|---|
| Position | Commitment, funded capital, unfunded commitment, NAV/capital account |
| Income | Distributions classified as income, return of capital, realized gain |
| Cost | Capital contributions adjusted for distributions/recallable amounts |
| Valuation | Manager NAV, lagged NAV, estimates, fair value hierarchy |
| P&L | NAV changes, realized distributions, FX |
| Lifecycle | Commitment, capital call, distribution, recallable distribution, transfer, secondary sale |
| Controls | NAV lag, notice matching, cashflow classification, vintage and strategy tags |

## 9. Loans, margin and collateral

| Topic | Treatment |
|---|---|
| Position | Liability balance, facility, utilized amount, available amount |
| Income/expense | Interest expense, facility fees, commitment fees |
| Cost | Principal liability at nominal/amortized basis |
| Valuation | Outstanding balance plus accrued interest; fair value if required |
| P&L | Financing cost; FX if loan currency differs from base |
| Lifecycle | Drawdown, repayment, interest, fee, margin call, liquidation |
| Controls | Pledged assets, LTV, haircut, exposure, one-way/cross pledge logic |

## 10. Insurance and annuities

| Topic | Treatment |
|---|---|
| Position | Policy contract, account value, cash value, surrender value |
| Income | Annuity payments, maturity benefit, claim proceeds where reportable |
| Cost | Premiums paid, policy charges, units for ILP sub-funds |
| Valuation | Insurer-provided policy value / surrender value / fund value |
| P&L | Often reporting-specific; separate protection benefit from investment value |
| Lifecycle | Premium, charge, switch, surrender, loan, claim, maturity |
| Controls | Beneficiary, policy owner, life assured, value date, surrender penalties |

## 11. Real estate, REITs and infrastructure

| Topic | Treatment |
|---|---|
| Position | Listed units/shares or private fund interest/direct property |
| Income | REIT distributions, rental income, fund distributions |
| Cost | Lot cost, acquisition costs, capital calls |
| Valuation | Exchange price, NAV, appraisal, manager valuation |
| P&L | Price/NAV movement, income, FX, leverage effect |
| Lifecycle | Trade, distribution, rights, capital call, appraisal, sale |
| Controls | Appraisal date, NAV lag, leverage, income sustainability |

## 12. Cross-product rule

Use common accounting objects, but attach product-specific rule sets:

```text
Generic transaction model + product-specific posting rules + jurisdiction/booking-centre policy = accounting output
```

This gives consistency without forcing all products into the same economics.

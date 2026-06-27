# 07 - Product-Specific Analytics Treatment Across Asset Classes

## 1. Why product-specific treatment matters

A common portfolio analytics engine should not treat every product exactly the same. Different products have different valuation, income, cashflow and risk behavior.

The objective is to use a common framework while allowing product-specific calculation rules.

## 2. Cash and deposits

| Analytics area | Treatment |
|---|---|
| Market value | Cash balance in currency translated to base currency |
| Performance | Interest income and FX movement; cash deposits/withdrawals external |
| Risk | FX risk, bank exposure, deposit insurance/credit risk, liquidity |
| Income | Accrued/paid interest |
| Mandate | Liquidity minimum/maximum, currency limits |
| Reporting | Cash by currency, available cash, pledged/restricted cash |

## 3. Money market funds and instruments

| Area | Treatment |
|---|---|
| Valuation | NAV or amortized cost depending product/accounting policy |
| Performance | NAV movement + income/distribution |
| Risk | Credit, liquidity, duration, liquidity gates, fund risk |
| Mandate | Cash-equivalent eligibility, liquidity bucket |
| Reporting | Yield, maturity profile, liquidity status |

## 4. Equities

| Area | Treatment |
|---|---|
| Valuation | Quantity x closing price x FX |
| Performance | Price return + dividends + FX |
| Contribution | Security/sector/country/currency contribution |
| Risk | Volatility, beta, drawdown, concentration |
| Corporate actions | Splits/spin-offs/mergers should avoid artificial returns |
| Mandate | Single stock, sector, country, restricted list limits |
| Reporting | Holdings, dividends, realized/unrealized P&L, allocation |

## 5. Bonds

| Area | Treatment |
|---|---|
| Valuation | Nominal x price % + accrued interest if dirty basis |
| Performance | Coupon/accrual + price movement + FX |
| Risk | Duration, DV01, credit spread, rating, maturity |
| Income | Coupon accrual and payment |
| Mandate | Credit quality, duration, issuer, maturity, currency |
| Reporting | Yield, duration, maturity ladder, rating distribution |

## 6. Structured notes

| Area | Treatment |
|---|---|
| Valuation | Issuer quote/evaluated/model price |
| Performance | Price movement + coupon + redemption/conversion + FX |
| Risk | Issuer risk, underlying risk, barrier/autocall risk, liquidity |
| Contribution | Note contribution; optional underlying scenario impact |
| Mandate | Complex product, issuer, underlying, concentration, barrier proximity |
| Reporting | Payoff summary, next observation, barrier distance, issuer |

## 7. Funds and ETFs

| Area | Treatment |
|---|---|
| Valuation | Units x NAV/market price |
| Performance | NAV movement + distributions + FX |
| Risk | Fund category, look-through, tracking error, liquidity |
| Income | Distribution or accumulation treatment |
| Mandate | Fund eligibility, asset-class look-through, concentration |
| Reporting | NAV, units, distributions, TER/fees, look-through if available |

## 8. Private markets

| Area | Treatment |
|---|---|
| Valuation | Latest reported NAV, estimates, capital account |
| Performance | IRR/MWR, TVPI, DPI, RVPI, NAV change, distributions |
| Risk | Illiquidity, vintage, manager, strategy, unfunded commitment |
| Income | Distributions split into income, gain, return of capital where available |
| Mandate | Commitment limits, unfunded exposure, liquidity planning |
| Reporting | Commitment, called capital, NAV, distributions, unfunded, multiples |

## 9. Hedge funds

| Area | Treatment |
|---|---|
| Valuation | Monthly NAV, sometimes estimated |
| Performance | NAV-based return, distributions if any |
| Risk | Strategy risk, leverage, gating, side pockets, valuation lag |
| Mandate | Alternative allocation, liquidity/lock-up, manager concentration |
| Reporting | NAV date, estimate/final flag, redemption terms, liquidity bucket |

## 10. Derivatives

| Area | Treatment |
|---|---|
| Valuation | Mark-to-market/model/exchange settlement |
| Performance | MTM change, premium, variation margin, exercise/expiry |
| Risk | Notional, Greeks, leverage, counterparty, margin |
| Contribution | Market value contribution plus notional/risk exposure explanation |
| Mandate | Derivative eligibility, leverage, hedging-only rules |
| Reporting | Notional, underlying, maturity, MTM, collateral/margin |

## 11. FX forwards and swaps

| Area | Treatment |
|---|---|
| Valuation | Forward MTM from spot, forward points and discount curves |
| Performance | FX movement and carry/forward points |
| Risk | Currency notional, counterparty, settlement risk |
| Mandate | Hedging ratio, currency exposure limits |
| Reporting | Buy/sell currencies, value date, rate, MTM, hedge designation |

## 12. Futures

| Area | Treatment |
|---|---|
| Valuation | Exchange settlement; daily variation margin |
| Performance | Realized daily P&L through variation margin |
| Risk | Notional exposure, margin, leverage, contract expiry/roll |
| Mandate | Leverage and derivative limits |
| Reporting | Contracts, notional, expiry, margin, roll schedule |

## 13. Options

| Area | Treatment |
|---|---|
| Valuation | Option premium/market price/model |
| Performance | Premium change, expiry, exercise, assignment |
| Risk | Delta/gamma/vega/theta, underlying exposure, max loss |
| Mandate | Option strategy approval, uncovered writing restrictions |
| Reporting | Strike, expiry, underlying, intrinsic/time value, Greeks |

## 14. Loans and liabilities

| Area | Treatment |
|---|---|
| Valuation | Outstanding principal + accrued interest |
| Performance | Usually financing cost/liability analytics, not asset return |
| Risk | Leverage, LTV, collateral coverage, interest-rate risk |
| Income | Interest expense, fees |
| Mandate | Borrowing permitted, max leverage, collateral requirements |
| Reporting | Loan balance, interest, LTV, collateral, availability |

## 15. Insurance and annuities

| Area | Treatment |
|---|---|
| Valuation | Cash value, surrender value, account value, benefit value |
| Performance | Often excluded or separately shown; ILPs can be unit/NAV based |
| Risk | Insurer risk, liquidity/surrender, premium obligation, market-linked funds |
| Income | Annuity payments, bonuses, policy values |
| Mandate | Estate planning/protection bucket, liquidity constraints |
| Reporting | Policy value, premium schedule, coverage, beneficiary, surrender charges |

## 16. Commodities and precious metals

| Area | Treatment |
|---|---|
| Valuation | Spot price x quantity; ETF/NAV/ETN price for wrappers |
| Performance | Price return + FX + roll yield for futures products |
| Risk | Commodity volatility, storage/custody, issuer/wrapper risk |
| Mandate | Commodity/precious metal allocation limits |
| Reporting | Physical/allocated/unallocated distinction, ETP type, collateral eligibility |

## 17. REITs and infrastructure

| Area | Treatment |
|---|---|
| Valuation | Listed price/NAV/appraisal depending vehicle |
| Performance | Price/NAV movement + distributions |
| Risk | Property sector, leverage, rates, occupancy, liquidity |
| Mandate | Real estate allocation, income target, concentration |
| Reporting | Yield, NAV, leverage, distribution, sector/geography exposure |

## 18. Trusts/family office structures

These are ownership/reporting structures, not asset classes.

Analytics impact:

- Aggregation hierarchy
- Beneficial ownership view
- Authorized viewer restrictions
- Entity-level mandates
- Tax/reporting flags
- Consolidated family reporting
- Inter-entity transfers
- Estate planning views

## 19. Product-specific cashflow treatment summary

| Product | Common income | Special cashflows |
|---|---|---|
| Equity | Dividend | Corporate actions, rights, spin-offs |
| Bond | Coupon | Accrued interest, maturity, call, default |
| Fund | Distribution | Subscription/redemption, reinvestment, switch |
| Note | Coupon | Autocall, barrier, physical delivery |
| Private fund | Distribution | Capital call, recallable distribution, commitment |
| Derivative | None/settlement P&L | Premium, margin, exercise, expiry |
| FX | None/carry | Settlement, rollover, swap points |
| Loan | Interest expense | Drawdown, repayment, margin call |
| Insurance | Annuity/bonus | Premium, surrender, claim, policy loan |
| Commodity | None unless wrapper | Roll, storage, delivery, ETP fees |

## 20. Expert summary

A common analytics model should use shared concepts: position, cashflow, income, valuation, exposure, risk, mandate and reporting. But product-specific extensions are essential. A bond needs duration and accrual logic. An option needs Greeks and expiry. A private fund needs commitment and IRR. A structured note needs payoff terms and issuer/underlying risk. This is how a platform achieves both standardization and correctness.

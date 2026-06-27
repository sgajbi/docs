# 02 — Cross-Product Advisory, Mandate, Analytics and Reporting Framework

## 1. Purpose

This file defines a common framework for connecting product knowledge to advisory, discretionary portfolio management, suitability, portfolio analytics and client reporting across notes, bonds, funds and equities.

## 2. Advisory lifecycle

```text
Client profile
  -> investment objective
  -> portfolio review
  -> idea / recommendation
  -> product and portfolio suitability
  -> proposal and disclosure
  -> client decision / mandate execution
  -> order and execution
  -> lifecycle monitoring
  -> performance/risk reporting
  -> review/rebalance/exit
```

## 3. Common suitability dimensions

| Dimension | Equity | Bond | Note | Fund |
|---|---|---|---|---|
| Risk profile | Volatility/downside | Credit/rate risk | Issuer + payoff risk | Strategy/asset risk |
| Complexity | Usually simple, except special cases | Moderate | Often complex | Varies |
| Liquidity | Exchange liquidity | Market/OTC liquidity | Often issuer-only liquidity | Dealing cycle/gates |
| Time horizon | Medium/long | Maturity/duration | Tenor/maturity | Investment horizon |
| Income | Dividends | Coupons | Coupons/conditional coupons | Distributions |
| Capital protection | None | Principal if no default and held to maturity | None/conditional/full at maturity | None usually |
| Currency | Trading/base currency mismatch | Bond currency | Note currency/underlying FX | Share class and underlying FX |
| Concentration | Single issuer/sector | Issuer/credit | Issuer + underlying | Fund/manager/look-through |
| Client knowledge | Equity investing | Fixed income/yield | Structured payoff understanding | Fund strategy/liquidity |

## 4. Mandate dimensions

| Dimension | Examples |
|---|---|
| Asset allocation | Equity, fixed income, cash, alternatives, structured products |
| Product eligibility | Direct equities allowed? Structured notes allowed? HY bonds allowed? Hedge funds allowed? |
| Risk budget | Volatility, VaR, drawdown, tracking error |
| Concentration | Issuer, sector, country, currency, manager, fund house |
| Credit quality | Minimum rating, HY limit, unrated limit |
| Liquidity | Daily/weekly/monthly/illiquid buckets |
| Tenor/duration | Bond/note maturity and duration limits |
| Complexity | SIP/EIP/complex product restrictions |
| Currency | Non-base currency limits and hedging rules |
| Income | Yield target and distribution preference |
| Tax constraints | Jurisdiction-specific |
| ESG/restrictions | Exclusions, sanctions and watchlists |
| Benchmark | Reference benchmark or custom composite |

## 5. Portfolio analytics dimensions

| Analytics | Equity | Bond | Note | Fund |
|---|---|---|---|---|
| Market value | Price × quantity | % par × nominal | % par/payoff valuation | NAV × units |
| Income | Dividends | Coupons/accrual | Coupons/conditional coupons | Distributions |
| Return | Price + income + FX | Price + coupon + FX | Mark-to-market + coupons + payoff | NAV + distributions |
| Risk | Vol/beta/drawdown | Duration/spread/default | Payoff/issuer/underlying | Strategy/look-through |
| Attribution | Sector/security/factor | Duration/curve/spread/issuer | Underlying/payoff/issuer | Asset allocation/manager |
| Liquidity | Volume/bid-ask | Bid/offer/market depth | Secondary issuer bid | Dealing frequency/gates |
| Concentration | Issuer/sector | Issuer/rating/sector | Issuer + underlyings | Fund/manager/look-through |

## 6. Reporting framework

A professional wealth report should answer:

1. What does the client own?
2. What is it worth?
3. What changed during the period?
4. What return did the portfolio generate?
5. What drove the return?
6. What income was generated?
7. What risks are present?
8. Is the portfolio within mandate?
9. What upcoming lifecycle events require action?
10. What should the advisor/client review next?

## 7. Product-specific upcoming event monitoring

| Product | Upcoming events |
|---|---|
| Equity | Dividends, rights, tender offers, mergers, earnings, delisting |
| Bond | Coupons, calls, puts, maturity, rating changes, default events |
| Note | Observation dates, coupon dates, barriers, autocall, maturity |
| Fund | Dealing cutoffs, distributions, gates, NAV delays, fund changes |

## 8. Unified health-check examples

| Health check | Products affected |
|---|---|
| Single issuer concentration | Equity, bonds, notes, fund look-through |
| Non-base currency exposure | All |
| Illiquid product concentration | Notes, funds, bonds, suspended equities |
| Income sustainability | Equity dividends, bond coupons, fund distributions |
| Credit quality deterioration | Bonds, note issuers/reference entities, FI funds |
| Product complexity | Notes, derivatives, hedge funds, complex ETFs |
| Mandate allocation drift | All |
| Corporate/lifecycle action due | Equity, bonds, notes, funds |

## 9. Implementation principle

Use product-specific models to capture economics, but publish normalized portfolio facts:

```text
Holding
Transaction
Valuation
Income
Risk Exposure
Lifecycle Event
Mandate Check
Report Section
```

This lets performance, risk, advisory and reporting engines handle products consistently while preserving product-specific accuracy.

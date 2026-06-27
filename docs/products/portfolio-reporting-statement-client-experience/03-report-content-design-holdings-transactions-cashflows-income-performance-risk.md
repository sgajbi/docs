# 03 - Report Content Design: Holdings, Transactions, Cashflows, Income, Performance and Risk

## 1. Portfolio summary design

The portfolio summary is the first page or first screen. It should give the client enough context to understand wealth, change, performance and risk without reading the full detail.

Recommended fields:

| Field | Notes |
|---|---|
| Total market value | Include valuation basis and base currency. |
| Cash and cash equivalents | Separate available cash, blocked cash and pledged cash if relevant. |
| Investment value | Securities and investment products excluding pure cash. |
| Liabilities | Loans, margin, overdraft, accrued interest payable. |
| Net portfolio value | Assets minus liabilities. |
| Period change | Market movement, net flows, income, fees and FX. |
| Performance return | TWR/MWR depending on context. |
| Income received | Gross and net income. |
| Top risk alerts | Concentration, leverage, liquidity, stale price, mandate breach. |

## 2. Holdings table

The holdings table should be product-aware but consistent.

Core columns:

| Column | Meaning |
|---|---|
| Product name | Client-friendly name, not only ticker/ISIN. |
| Identifier | ISIN, ticker, internal ID or contract ID. |
| Product type | Equity, bond, fund, note, derivative, private fund, etc. |
| Currency | Instrument/local currency. |
| Quantity/nominal | Shares/units/face value/contract amount. |
| Average cost | Cost basis in local and/or base currency. |
| Market price/NAV | Price with valuation date. |
| Market value | Local and base currency. |
| Accrued income | Bonds, loans, deposits, etc. |
| Unrealized P&L | Local and base currency. |
| Allocation % | Weight of portfolio. |
| Data flag | Stale, estimated, unavailable, provisional. |

Product-specific additions:

| Product | Additional fields |
|---|---|
| Bonds | Coupon, maturity, yield, rating, duration, accrued interest. |
| Structured notes | Issuer, underlying, barrier, maturity, next observation, valuation source. |
| Funds | NAV date, share class, distribution policy, redemption frequency. |
| Derivatives | Notional, contract multiplier, expiry, strike, delta, margin/collateral. |
| Private markets | Commitment, funded amount, unfunded amount, latest NAV date. |
| Insurance | Policy value, surrender value, sum assured, premium status. |
| Loans | Outstanding principal, interest rate, collateral value, LTV. |

## 3. Asset allocation section

Asset allocation should support multiple views:

- by product family;
- by asset class;
- by currency;
- by country/region;
- by sector;
- by issuer/counterparty;
- by liquidity bucket;
- by risk rating;
- by complexity;
- by mandate bucket.

A single legal holding may need multiple classifications. For example, a structured note is legally a note, but may have equity, issuer-credit and optionality exposure.

## 4. Transactions and activity section

Transactions should be grouped into client-understandable categories.

| Category | Examples |
|---|---|
| Purchases/subscriptions | Equity buy, bond buy, fund subscription, note subscription. |
| Sales/redemptions | Equity sell, bond sale, fund redemption, maturity redemption. |
| Income | Dividends, coupons, fund distributions, deposit interest. |
| Fees and charges | Advisory fee, custody fee, transaction fee, FX spread, financing charge. |
| Tax | Withholding tax, stamp duty, transaction tax. |
| Transfers | Cash/securities in/out, internal transfers. |
| Corporate actions | Splits, rights, mergers, spin-offs, tender offers. |
| Loan activity | Drawdown, repayment, interest, margin call collateral movement. |
| Private-market activity | Capital call, distribution, recallable distribution, equalization. |

## 5. Cashflow report

Cashflow reporting should separate investment activity from external client flows.

| Cashflow type | Performance treatment |
|---|---|
| Client contribution | External inflow. |
| Client withdrawal | External outflow. |
| Buy from existing cash | Internal investment activity. |
| Sell to existing cash | Internal investment activity. |
| Coupon/dividend retained | Investment income. |
| Coupon/dividend withdrawn | Income plus external outflow. |
| Fees paid from portfolio | Expense; may be external depending methodology. |
| Tax withheld | Tax expense/withholding. |
| Loan drawdown | Liability increase and cash increase; not investment performance. |
| Loan repayment | Liability decrease and cash decrease; not investment performance. |

Misclassifying cashflows is one of the most common causes of wrong performance.

## 6. Income report design

Income should be reported with gross/net clarity.

| Field | Meaning |
|---|---|
| Ex-date | Date entitlement arises for dividend-style events. |
| Record date | Date holder of record is determined. |
| Payment date | Date cash is paid. |
| Income type | Dividend, coupon, interest, distribution, rental-like distribution. |
| Gross amount | Before withholding and fees. |
| Tax withheld | Tax deducted. |
| Net amount | Cash received. |
| Currency | Income currency. |
| Reinvestment flag | Whether income was reinvested. |
| Recurring flag | Whether income is recurring/contractual/one-off. |

## 7. Performance section

Performance should be presented at multiple levels:

- portfolio total;
- account level;
- asset class;
- product type;
- currency;
- sector/country;
- security;
- benchmark relative;
- contribution/attribution.

Recommended labels:

| Metric | Notes |
|---|---|
| Period return | Clear start/end dates. |
| YTD return | Year-to-date in reporting currency. |
| Since inception return | Inception date must be shown. |
| Annualized return | Only where period length supports it. |
| Benchmark return | Benchmark identity and methodology shown. |
| Relative return | Portfolio return minus benchmark return. |
| Contribution | Additive return impact from each segment. |
| Attribution | Explanation of allocation/selection/currency effects. |

## 8. P&L section

P&L and performance are not the same. P&L is amount-based; performance is return-based.

P&L components:

- realized trading P&L;
- unrealized market P&L;
- FX translation P&L;
- accrued income movement;
- income received;
- fees and charges;
- taxes;
- financing cost;
- valuation adjustment;
- corporate-action adjustment.

A strong report separates local-currency P&L from base-currency FX translation.

## 9. Risk section

Risk should be layered.

| Risk type | Reporting view |
|---|---|
| Market risk | Volatility, drawdown, beta, VaR, stress. |
| Credit risk | Issuer, counterparty, rating, spread, exposure. |
| Liquidity risk | Liquidity bucket, notice period, lock-up, exit restrictions. |
| Concentration risk | Single name, issuer, sector, country, currency. |
| Product risk | Complex products, leverage, barriers, derivatives. |
| FX risk | Currency exposure before and after hedges. |
| Interest-rate risk | Duration, DV01, yield-curve exposure. |
| Leverage/collateral risk | LTV, margin utilization, headroom. |
| Operational data risk | Stale price, missing NAV, pending CA, estimated valuation. |

## 10. Report design checklist

Before publishing a reporting section, confirm:

- metric definition exists;
- source ownership is known;
- basis is known;
- currency is known;
- valuation date is known;
- exception behavior is defined;
- historical restatement behavior is defined;
- client label is business-approved;
- QA scenario exists;
- lineage can be traced.

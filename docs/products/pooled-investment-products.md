# Pooled Investment Products Knowledge Map

This page connects fund-related product knowledge to reusable private-banking platform decisions.

## Current Pack

| Product Area | Pack | Main Use |
|---|---|---|
| Funds | [`funds/`](funds/README.md) | Fund product taxonomy, share classes, subscriptions/redemptions, switches, transfers, distributions, NAV, fees, look-through exposure, risk, performance, and platform controls. |

## Product Families Covered

| Family | Platform Treatment |
|---|---|
| Mutual funds and unit trusts | Usually order-driven, NAV-priced, subscription/redemption lifecycle, share-class-specific dealing and fee terms. |
| ETFs | Exchange-traded security behavior with intraday price, but still fund look-through and expense/NAV concepts. |
| Money market funds | Liquidity and cash-like treatment require careful valuation, income, and risk classification. |
| Bond funds | Client owns fund units, not underlying bonds; duration, spread, and credit risk can be available through look-through analytics. |
| Equity funds | Client owns fund units, not each stock; allocation and risk views may use holdings or exposure files. |
| Multi-asset funds | Requires asset-class, currency, region, and risk decomposition when look-through is available. |
| Fund of funds | Nested exposure requires source quality controls and clear double-counting treatment. |
| Hedge funds | Often have lockups, gates, notice periods, side pockets, and less frequent NAV. |
| Private market funds | Commitment, capital call, distribution, unfunded commitment, NAV update, and vintage-year modelling matter. |

## Core Platform Distinctions

| Concept | Project Implication |
|---|---|
| Legal position | The client normally owns fund units or shares. |
| Look-through exposure | Analytical exposure to underlying assets, sectors, countries, currencies, strategies, duration, or credit quality. |
| Order lifecycle | Subscription, redemption, switch, transfer, cancellation, rejection, settlement, and confirmation often span multiple dates. |
| Valuation | NAV, bid/offer price, exchange price, stale NAV, estimated NAV, and price source hierarchy must be distinct. |
| Fees | Sales charge, trailer, management fee, performance fee, expense ratio, redemption fee, and swing pricing need clear modelling. |
| Income | Cash distribution, reinvested distribution, return of capital, accumulation share-class NAV growth. |
| Liquidity | Dealing frequency, cut-off, notice period, lockup, gates, suspension, and side pockets affect suitability and availability. |

## Questions To Ask Before Building A Fund Feature

1. Is this an order, transaction, position, NAV record, fee, distribution, lifecycle event, or look-through exposure?
2. Is the fund exchange-traded, NAV-traded, private, suspended, gated, or side-pocketed?
3. Which share class is held, and does it change currency, income treatment, hedging, or fees?
4. Is the requested value based on official NAV, estimated NAV, exchange price, custodian price, or manual override?
5. Does the client own the underlying securities directly, or is the platform showing analytical look-through?
6. Are subscriptions/redemptions amount-based, unit-based, or both?
7. Which dates drive the workflow: order date, cut-off, dealing date, NAV date, trade date, settlement date, booking date?
8. Are fees explicit transaction legs or embedded in net amount/NAV?
9. Are there liquidity constraints that should block or warn before order placement?
10. What should the system show when NAV, look-through, or fund status is stale or unavailable?

## API And UI Implications

Fund APIs and UI should make these states explicit:

1. order state and next expected date,
2. dealing terms and cut-off posture,
3. share-class identity and currency,
4. NAV date, price source, and stale/estimated status,
5. fee treatment,
6. distribution treatment,
7. available units after pending orders,
8. lockup, gate, suspension, or side-pocket posture,
9. look-through availability and source,
10. suitability and reporting restrictions.

## QA Scenarios To Preserve

1. amount-based subscription,
2. unit-based redemption,
3. switch between share classes or funds,
4. cash distribution,
5. reinvested distribution,
6. accumulating share class with no cash income,
7. ETF buy/sell through market transaction path,
8. stale NAV,
9. redemption gate,
10. private fund capital call and distribution,
11. fund split or merger,
12. look-through unavailable, partial, stale, and ready states.

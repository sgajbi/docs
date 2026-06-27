# 04 - Commodity Derivatives, ETPs, Structured Products and Product Combinations

## 1. Product wrapper matters

Commodity exposure is often delivered through a wrapper. The wrapper determines lifecycle, transaction type, valuation source, risk, reporting and suitability treatment.

## 2. Futures

A commodity futures contract is an agreement to buy or sell a commodity at a future date at a specified price. Most wealth clients do not intend physical delivery; they use futures exposure through managed products or professional accounts.

Key fields:

| Field | Example |
|---|---|
| contract_code | GCZ6 / CLF7 |
| underlying | Gold / WTI crude |
| exchange | CME / ICE / SGX / LME |
| contract_size | 100 troy oz / 1,000 barrels |
| tick_size | Minimum price increment |
| expiry_date | Contract expiry |
| delivery_type | Physical / cash settled |
| margin_requirement | Initial/maintenance margin |
| settlement_method | Daily mark-to-market |

Futures lifecycle:

| Event | Transaction/event |
|---|---|
| Open long/short | FUTURE_OPEN |
| Daily variation margin | FUTURE_VARIATION_MARGIN |
| Margin call | MARGIN_CALL / COLLATERAL_MOVEMENT |
| Roll | FUTURE_CLOSE + FUTURE_OPEN in next contract |
| Expiry cash settlement | FUTURE_CASH_SETTLEMENT |
| Physical delivery | DELIVERY_OUT/IN events, usually avoided in wealth portfolios |

## 3. Options on commodities/futures

Options give exposure to commodity price movement with nonlinear payoff.

| Option | Meaning |
|---|---|
| Call | Right to buy / benefit from price rise |
| Put | Right to sell / benefit from price fall |
| Long option | Pays premium, limited loss to premium |
| Short option | Receives premium, potentially large loss |
| Listed option | Exchange-traded, margin rules |
| OTC option | Bilateral contract, collateral/CSA may apply |

Commodity option valuation depends on:

- underlying futures/spot price;
- strike;
- time to expiry;
- volatility;
- rates;
- storage/carry assumptions;
- skew/smile;
- settlement and delivery details.

## 4. Forwards and swaps

Commodity forwards and swaps are common in institutional/private-bank structured solutions.

| Product | Description | Platform treatment |
|---|---|---|
| Forward | OTC agreement for future commodity price | OTC derivative position |
| Commodity swap | Exchange fixed price versus floating commodity index | OTC derivative with reset/payment schedule |
| Total return swap | Total return of commodity index exchanged for financing leg | OTC derivative exposure |
| Asian option/swap | Payoff based on average price | Path-dependent valuation |
| Quanto commodity product | Commodity payoff converted at fixed FX | Commodity + FX risk |

## 5. Commodity ETFs, ETCs and ETNs

| Wrapper | Legal/economic nature | Typical risk |
|---|---|---|
| Commodity ETF | Fund-like product; may hold physical metal or futures | Fund risk, tracking error, fees, liquidity |
| ETC | Exchange-traded commodity/certificate, often debt-like | Issuer/security structure, collateral, tracking |
| ETN | Unsecured debt note linked to commodity index | Issuer credit risk, call/early redemption, tracking |
| Leveraged/inverse ETP | Daily leveraged or inverse exposure | Compounding, volatility decay, suitability risk |

Important distinction:

- A physical gold ETF may hold gold bullion.
- A futures-based oil ETF may hold rolling futures contracts and collateral.
- A commodity ETN may hold no commodity; it is a promise from the issuer.
- A leveraged commodity ETP may reset daily and is not designed for long holding periods unless specifically understood.

## 6. Commodity-linked structured products

Common structures:

| Product | Underlying | Typical payoff |
|---|---|---|
| Gold-linked note | Gold price | Principal-protected or participation payoff |
| Oil-linked note | Brent/WTI | Coupon/participation/barrier payoff |
| Commodity autocall | Commodity index or basket | Coupon if above barrier, autocall if above call level |
| Range accrual note | Commodity stays within range | Coupon accrues for days within range |
| Dual asset note | Commodity + FX or equity | Hybrid payoff |
| Inflation/commodity basket note | Commodity basket | Participation in basket return |

Structured product modelling should reuse the structured product/note framework, with commodity-specific underlyings and valuation inputs.

## 7. Warrants, certificates and callable bull/bear contracts

Some markets offer listed commodity warrants/certificates/CBBC-style instruments.

| Product | Economic behavior | Risk |
|---|---|---|
| Warrant | Option-like payoff | Time decay, volatility, issuer risk if warrant issued by bank |
| Certificate | Tracker or payoff-linked security | Issuer/structure risk |
| Daily leveraged certificate | Daily leveraged return | Compounding and gap risk |
| Callable bull/bear contract | Leveraged directional product with mandatory call event | High loss risk if call level hit |

These are usually complex/speculative products and should be subject to strong suitability controls.

## 8. Product combinations

| Wealth product | Combined components |
|---|---|
| Physical gold holding | Metal + custody + storage/insurance |
| Unallocated gold account | Metal price exposure + provider credit claim |
| Gold ETF | Fund units + physical bullion basket + expense ratio |
| Oil ETF | Fund units + futures roll strategy + collateral |
| Commodity ETN | Issuer debt + commodity index payoff |
| Commodity-linked note | Note + commodity option/forward payoff + issuer risk |
| Gold-backed loan collateral | Metal holding + pledge + haircut/LTV |
| Commodity fund | Fund wrapper + manager strategy + underlying futures/equities |
| Managed futures fund | Fund wrapper + systematic futures strategy + margin/collateral |

## 9. Position modelling by wrapper

| Wrapper | Position type | Quantity basis | Valuation basis |
|---|---|---|---|
| Physical metal | Commodity holding | oz/grams/bars | spot/bid benchmark |
| Metal account | Commodity balance/account | oz/grams | bank/vendor price |
| ETF/ETC/ETN | Listed security | shares/units | exchange price/NAV/iNAV |
| Futures | Derivative contract | contracts × contract size | exchange settlement price |
| Option | Derivative contract | option contracts | market/model price |
| Swap | OTC derivative | notional | model fair value |
| Structured note | Security/note | nominal amount | issuer/evaluated/model price |
| Commodity equity | Equity | shares | exchange price |
| Commodity fund | Fund | units | NAV |

## 10. Advisory simplification

A good advisor explanation is:

“Commodity exposure can be accessed through physical holdings, funds, exchange-traded products, derivatives or structured products. The commodity view is only one part of the decision. The wrapper determines liquidity, costs, tax/accounting, issuer or counterparty risk, valuation transparency, leverage, margin and lifecycle treatment.”

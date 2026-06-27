# 10 - Worked Examples and Implementation Patterns

Use these examples when modelling commodities, precious metals, commodity-linked wrappers, and real-asset trading exposures for advisory, DPM, analytics, reporting, operations, QA, and implementation work.

## 1. Gold Exposure Across Wrappers

The same economic theme can appear through very different legal and operational wrappers.

| Wrapper | Legal position | Cashflow behavior | Valuation source | Main risk | Platform warning |
|---|---|---|---|---|---|
| Allocated gold bar | Specific physical bar or custody lot | Purchase, sale, custody/storage fees | Metal price plus custody source | custody, liquidity, storage, unit precision | Must preserve bar/lot identity where source supports it. |
| Unallocated metal account | Metal account claim on provider | metal debit/credit, fees | provider metal account valuation | provider credit, liquidity | Do not describe as specific allocated bullion. |
| Gold ETF | Listed security or fund unit | trade settlement, distributions if any | exchange price or NAV | market, tracking, fund/provider | Model like security/fund plus gold exposure. |
| Gold ETN/ETC | Listed note/certificate | trade settlement, possible issuer coupon/fees | exchange or issuer price | issuer credit, liquidity, tracking | Show issuer/provider risk separately from gold exposure. |
| Gold future | Derivative contract | margin, variation margin, roll, delivery risk | exchange settlement price | leverage, roll, margin, delivery | Market value is not exposure. |
| Gold-linked note | Structured product | coupon/redemption by payoff terms | issuer quote/model | issuer, payoff, barrier, liquidity | Underlying gold is analytical exposure, not a held asset. |

Design rule:

```text
Wrapper drives position and accounting.
Underlying commodity drives exposure, risk, and scenario analytics.
```

## 2. Allocated Gold Purchase And Sale

Scenario:

- Client buys 100 troy ounces of allocated gold.
- Purchase price: USD 2,000 per ounce.
- Custody/storage fee: USD 250 per quarter.
- Later sale price: USD 2,080 per ounce.

Purchase:

```text
cost = 100 x 2,000 = 200,000
```

Sale:

```text
sale proceeds = 100 x 2,080 = 208,000
gross gain = 8,000
```

Platform treatment:

- store unit of measure as troy ounces, not generic units;
- preserve custody/source reference for allocated holdings;
- book storage fees as expenses, not price movement;
- translate value using reporting FX rate when reporting currency differs;
- if bar identity is available, preserve bar/lot metadata for custody and audit.

Reporting:

| Item | Expected label |
|---|---|
| Holding quantity | 100 troy ounces |
| Market value | price x quantity, with source date |
| Fees | custody/storage fee |
| P&L | price movement plus realized sale result |
| Risk | commodity price, custody, liquidity, FX where applicable |

## 3. Allocated Versus Unallocated Metal Account

Scenario:

- Client A holds allocated gold.
- Client B holds unallocated gold account exposure.
- Both show 50 troy ounces of gold exposure.

Do not treat them as identical.

| Dimension | Allocated | Unallocated |
|---|---|---|
| Ownership/custody | specific custody entitlement where source supports it | claim on provider/account |
| Provider credit risk | lower but still operational/custody dependent | more important |
| Reporting label | allocated precious metal | unallocated precious metal account |
| Reconciliation | custody inventory/bar or account statement | provider balance statement |
| Collateral treatment | may receive different haircut | often provider-specific haircut |

QA assertion:

If a report says "allocated", source evidence must support allocated custody status. Otherwise show the position as unallocated, pooled, provider claim, or source-limited.

## 4. Commodity Futures Roll

Scenario:

- Portfolio holds 10 long crude oil futures.
- Contract size: 1,000 barrels.
- Front-month future: USD 80.
- Next-month future: USD 81.
- Roll from front month to next month.

Notional exposure:

```text
front-month notional = 10 x 1,000 x 80 = 800,000
next-month notional = 10 x 1,000 x 81 = 810,000
```

Roll workflow:

```text
close front-month contract -> realize futures P&L through variation margin -> open next-month contract -> update expiry and exposure
```

Implementation requirements:

- contract month and expiry must be explicit;
- contract multiplier must be source-owned;
- variation margin must reconcile to broker;
- roll trade should not be hidden as simple price movement;
- report roll yield or roll impact only when methodology is supported.

## 5. Commodity ETN Issuer Risk

Scenario:

- Client buys a commodity-linked ETN tracking an energy index.
- Index rises 6%.
- ETN issuer spread widens and secondary-market price rises only 3%.

Reporting distinction:

| Item | Treatment |
|---|---|
| Index return | reference performance, not client return |
| ETN price return | client investment return |
| Issuer credit | separate risk factor |
| Tracking/fees | explain difference where source data supports it |

Implementation boundary:

Generic listed-security support can store ETN position, price, market value, and transactions. Full commodity ETN support requires issuer risk, underlying index mapping, fee/tracking metadata, liquidity state, and source-backed exposure reporting.

## 6. Pledged Gold Collateral Haircut

Scenario:

- Gold collateral market value: USD 500,000.
- Eligible haircut: 25%.
- Existing loan exposure: USD 250,000.

Calculation:

```text
lending value = 500,000 x (1 - 25%) = 375,000
collateral headroom = 375,000 - 250,000 = 125,000
```

Controls:

- collateral value should use approved price source and FX rate;
- allocated/unallocated status may drive eligibility and haircut;
- stale commodity price should block or degrade buying-power calculation;
- pledge direction and legal entity must be explicit;
- commodity price stress should be part of margin-call monitoring.

## 7. Commodity-Linked Structured Note Observation

Scenario:

- Structured note references a gold index.
- Conditional coupon pays if index level >= 70% of initial.
- Initial level: 2,000.
- Observation level: 1,360.

Calculation:

```text
barrier level = 2,000 x 70% = 1,400
observation result = 1,360 < 1,400
coupon condition not met
```

Platform treatment:

- do not book coupon cashflow;
- store observation as lifecycle event;
- note remains legal holding;
- gold index is underlying exposure, not owned bullion;
- report missed coupon only if policy allows and source event is confirmed.

## 8. Commodity Fund Look-Through

Scenario:

- Client holds a commodity fund with 40% gold futures, 30% energy futures, 20% commodity equities, 10% cash.
- Fund NAV is daily.
- Look-through file is monthly.

Analytics treatment:

| View | Treatment |
|---|---|
| Legal holding | fund unit position |
| Market value | units x NAV |
| Exposure | use latest look-through with source date |
| Staleness | label exposure date separately from NAV date |
| Risk | separate commodity, equity, cash, FX, and derivative exposure where source supports it |

Do not imply daily look-through exposure if the source is monthly.

## 9. Advisory And Mandate Checklist

| Dimension | Required question |
|---|---|
| Objective | Diversification, inflation hedge, crisis hedge, income, speculation, collateral, or tactical trade? |
| Wrapper | Physical, account, ETF, ETC/ETN, derivative, fund, structured product, or equity proxy? |
| Liquidity | Can the position be sold, redeemed, delivered, rolled, or pledged? |
| Price source | Exchange, custodian, provider, issuer, model, or manual price? |
| Unit control | Are units, ounces, barrels, tonnes, contract sizes, and multipliers explicit? |
| Risk | Commodity price, issuer/provider, custody, margin, leverage, roll, FX, and concentration risk? |
| Mandate | Is commodity exposure allowed directly, by wrapper, by derivative, or only through funds? |
| Reporting | Should exposure appear under commodities, alternatives, real assets, equity proxy, or structured product? |

## 10. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Listed commodity wrapper | security/fund position, price/NAV, transactions, market value | tracking-error, roll-yield and issuer-spread decomposition |
| Physical/metal account | quantity, unit, provider/custodian, valuation source | bar-level inventory, storage-fee accrual and provider-risk workflow |
| Futures exposure | contract, expiry, multiplier, margin, variation margin | automated roll analytics and delivery-risk workflow |
| Collateral value | eligible quantity, price, FX, haircut, pledge state | stress-based collateral optimization |
| Structured commodity payoff | underlying reference, issuer quote, observation state | commodity payoff engine with observation replay |
| Look-through exposure | source file, exposure weights, source date | cross-wrapper commodity exposure engine |

## 11. Regression Test Pack

Minimum release-gate scenarios:

1. Allocated gold purchase stores troy ounces and custody metadata.
2. Unallocated metal account reports provider claim, not specific bars.
3. Gold sale realizes P&L and separates storage fees.
4. Commodity future posts variation margin and notional exposure.
5. Futures roll closes old contract and opens new contract.
6. Commodity ETN shows issuer risk and client return separately from index return.
7. Pledged gold haircut calculates lending value and stale-price degradation.
8. Commodity-linked structured note observation blocks coupon when condition fails.
9. Commodity fund NAV and look-through source dates are separately visible.
10. Missing unit of measure blocks client-ready commodity reporting.

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

## 9. Storage-Fee Accrual For Allocated Metal

Scenario:

- Client holds 100 troy ounces of allocated gold.
- Gold price: USD 2,000 per troy ounce.
- Annual storage fee: 0.20% of market value.
- Accrual period: 90 days on a 360-day basis.

Calculation:

```text
metal value = 100 x 2,000 = 200,000
storage fee accrual = 200,000 x 0.20% x 90 / 360 = 100
```

Treatment:

- book the fee as custody or storage expense;
- do not include the fee in commodity price return;
- reconcile accrual to the provider invoice when billed;
- reverse or true up the accrual when the actual invoice differs;
- preserve source date, fee basis and day-count convention.

Reporting control:

Storage fees should explain the difference between gross metal price return and net client return. They should not silently reduce the gold price or disappear into generic portfolio fees unless the reporting policy explicitly aggregates them.

## 10. Paid-In-Kind Metal Fee

Scenario:

- Client holds 50.000 kilograms of silver in a metal account.
- Provider deducts 0.100 kilograms as a storage or account fee.
- Silver price: USD 800 per kilogram.

Calculation:

```text
ending quantity = 50.000 - 0.100 = 49.900 kilograms
fee value = 0.100 x 800 = 80
```

Treatment:

| Item | Required handling |
|---|---|
| Quantity | Reduce metal quantity by the fee quantity. |
| Expense | Report an expense equal to the metal value deducted. |
| Sale treatment | Do not classify as client sale unless accounting or tax policy requires disposal treatment. |
| Cost basis | Adjust according to the supported cost-basis policy. |
| Reconciliation | Match provider statement quantity after deduction. |

QA assertion:

A paid-in-kind fee changes quantity and expense reporting. A cash-paid storage fee changes cash and expense reporting. These two fee types should not be conflated.

## 11. Commodity Option Exercise

Scenario:

- Client holds 5 long call options on crude oil futures.
- Strike price: USD 75.
- Futures settlement price at exercise: USD 82.
- Contract multiplier: 1,000 barrels.

Intrinsic value:

```text
intrinsic value = max(82 - 75, 0) x 1,000 x 5 = 35,000
```

Exercise workflow:

```text
confirm exercise terms -> determine cash or futures settlement -> close option state -> create settlement cashflow or resulting futures position -> update exposure and margin
```

Implementation requirements:

- option style, expiry, exercise window and settlement method must be explicit;
- cash-settled options create cash settlement, not physical commodity;
- futures-settled options create or adjust a futures position according to exchange rules;
- physical-delivery exposure requires separate delivery eligibility and operations workflow;
- exercise should preserve lineage from the original option contract.

## 12. Warehouse Receipt Backed Commodity Holding

Scenario:

- Client has exposure represented by a warehouse receipt.
- Receipt quantity: 1,000 units.
- Grade/location adjusted price: USD 52 per unit.
- Warehouse receipt has issuer, location, grade, lien and expiry fields.

Valuation:

```text
receipt value = 1,000 x 52 = 52,000
```

Required source checks:

| Check | Why it matters |
|---|---|
| Receipt identifier | Prevents duplicate or unsupported claims. |
| Quantity and unit | Drives valuation and delivery eligibility. |
| Grade and location | Determines price adjustment and deliverability. |
| Warehouse/provider | Drives custody and operational risk. |
| Liens or encumbrances | May block transfer, sale or pledge. |
| Expiry or inspection status | May degrade reporting or require renewal workflow. |

Reporting rule:

Show the position as a receipt-backed commodity claim, not generic spot commodity exposure. Legal ownership, transferability and storage state should remain visible.

## 13. Natural-Resource Fund Look-Through

Scenario:

- Client holds a natural-resource fund worth USD 500,000.
- Latest look-through file covers 70% of NAV.
- Covered exposures: 40% energy, 20% metals and mining, 10% agriculture.
- Unmapped residual: 30%.

Exposure treatment:

```text
energy exposure = 500,000 x 40% = 200,000
metals and mining exposure = 500,000 x 20% = 100,000
agriculture exposure = 500,000 x 10% = 50,000
unmapped residual = 500,000 x 30% = 150,000
```

Controls:

- report legal holding as fund units;
- report commodity or real-asset exposure as look-through analytics;
- show look-through source date and coverage percentage;
- keep unmapped exposure visible rather than forcing it into a false category;
- do not infer daily exposure changes from monthly or quarterly files.

## 14. Commodity Index Roll Yield

Scenario:

- Portfolio rolls 10 long commodity futures contracts.
- Old contract price: USD 80.
- New contract price: USD 82.
- Contract multiplier: 1,000.
- Market is in contango.

Roll impact:

```text
roll impact = (82 - 80) x 1,000 x 10 = 20,000 cost
```

Interpretation:

| Curve state | Long exposure roll effect | Reporting label |
|---|---|---|
| Contango | Usually negative roll impact when rolling to a higher-priced contract. | Roll cost or negative roll yield. |
| Backwardation | Usually positive roll impact when rolling to a lower-priced contract. | Roll benefit or positive roll yield. |

Performance reporting should separate:

1. spot or futures price movement,
2. roll impact,
3. collateral cash return,
4. fees and execution costs,
5. FX effect when reporting currency differs.

## 15. Physical Delivery-Risk Workflow

Scenario:

- Client account holds commodity futures approaching first notice date.
- Account mandate allows tactical commodity exposure but does not allow physical delivery.
- Broker calendar shows first notice date in two business days.

Workflow:

```text
detect notice window -> check account delivery eligibility -> block new delivery-risk positions -> generate roll or close-out task -> confirm execution -> verify no delivery obligation remains
```

Required controls:

- first notice date, last trade date and settlement method must come from source-owned contract data;
- non-delivery accounts should have pre-deadline close-out or roll controls;
- unresolved delivery risk should escalate before the account can enter delivery processing;
- reports should distinguish delivery-risk exceptions from ordinary unrealized P&L;
- manual overrides should require authority, reason and audit trail.

Negative QA case:

If the expiry calendar is missing or stale, the platform should fail closed for new delivery-risk exposure and mark existing positions for urgent review.

## 16. Advisory And Mandate Checklist

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

## 17. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Listed commodity wrapper | security/fund position, price/NAV, transactions, market value | tracking-error, roll-yield and issuer-spread decomposition |
| Physical/metal account | quantity, unit, provider/custodian, valuation source, source-backed fee events | bar-level inventory, provider-risk workflow and fee disputes |
| Futures exposure | contract, expiry, multiplier, margin, variation margin, roll impact and delivery-risk controls | automated roll optimization and delivery operations workflow |
| Collateral value | eligible quantity, price, FX, haircut, pledge state | stress-based collateral optimization |
| Structured commodity payoff | underlying reference, issuer quote, observation state | commodity payoff engine with observation replay |
| Look-through exposure | source file, exposure weights, coverage percentage, source date | cross-wrapper commodity exposure engine |
| Warehouse receipt | receipt identifier, quantity, grade, location, provider and encumbrance state | negotiable receipt transfer workflow and inspection lifecycle |

## 18. Regression Test Pack

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
10. Storage-fee accrual posts as expense and reconciles to provider invoice.
11. Paid-in-kind metal fee reduces quantity and reports expense without pretending it is ordinary price movement.
12. Commodity option exercise creates the correct cash settlement or resulting futures position.
13. Warehouse receipt value requires receipt identifier, quantity, grade, location and encumbrance checks.
14. Natural-resource fund look-through shows source date, coverage and unmapped residual exposure.
15. Commodity index roll impact is separated from spot return, collateral return, fees and FX.
16. Delivery-risk workflow blocks unintended physical delivery when contract calendar or mandate controls fail.
17. Missing unit of measure blocks client-ready commodity reporting.

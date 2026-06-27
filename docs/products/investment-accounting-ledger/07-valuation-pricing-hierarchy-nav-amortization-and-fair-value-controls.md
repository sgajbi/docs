# 07 - Valuation, Pricing Hierarchy, NAV, Amortization and Fair Value Controls

## 1. Purpose of valuation in the accounting layer

Valuation converts positions into monetary value for client reporting, performance, risk, collateral and accounting views. The platform must know not only the value, but also the source and quality of the value.

| Consumer | Valuation need |
|---|---|
| Client statement | Reportable market value |
| Performance | Daily/time-series value and cashflow timing |
| Risk | Exposure, sensitivities, stress inputs |
| Lending | Collateral value and haircut value |
| Accounting | Fair value, amortized cost, impairment/write-down views |
| Operations | Price reconciliation and exception handling |

## 2. Valuation hierarchy

| Level | Examples | Control implication |
|---|---|---|
| Level 1 style | Active exchange-traded equities/ETFs/futures | Use market close/official price; handle corporate actions |
| Level 2 style | Bonds with observable curves/spreads, OTC derivatives with observable inputs | Use evaluated/model price; validate inputs |
| Level 3 style | Private markets, illiquid notes, direct property, hard-to-value assets | Use manager/appraisal/model value; disclose lag/estimate |

This hierarchy should be implemented as source/quality metadata, not just a label in a document.

## 3. Fair value principle

For platform design, fair value should be treated as a controlled measurement process:

```text
Valuation = position x price/NAV/model value x FX + accrued components +/- adjustments
```

Key metadata:

| Field | Meaning |
|---|---|
| valuation_source | Exchange, custodian, issuer, vendor, manager, model, manual override |
| valuation_method | Market price, NAV, DCF, curve, model, appraisal, cost proxy |
| valuation_timestamp | When price/value was effective |
| price_date | Market date for price |
| fx_date | FX date used for conversion |
| quality_flag | Official, estimated, stale, missing, overridden, suspended |
| hierarchy_level | Observable/less observable/manager value/manual |
| override_reason | If manual override used |
| approval_status | Maker/checker if required |

## 4. Clean versus dirty values

Fixed income and notes may have clean and dirty values.

| Value | Meaning |
|---|---|
| Clean value | Price value excluding accrued interest |
| Accrued income | Coupon/interest earned but unpaid |
| Dirty value | Clean value + accrued income |

Reports must label whether market value includes accrued income. Performance and income reports should be consistent to avoid double-counting.

## 5. NAV-based valuation

Funds and private funds are often NAV-valued.

| Product | NAV characteristics |
|---|---|
| Mutual fund / unit trust | Daily/periodic official NAV |
| ETF | Exchange price plus NAV/iNAV analytics |
| Hedge fund | Monthly/quarterly NAV, possible estimates |
| Private equity fund | Quarterly lagged NAV, capital account statement |
| Real estate fund | Appraisal-driven NAV, lagged and smoothed |
| ILP sub-fund | Fund unit value from insurer/platform |

NAV lag must be visible. Do not treat a three-month-old NAV as equivalent to yesterday's exchange close.

## 6. Amortized cost

Amortized cost is relevant for some bonds, loans, deposits and accounting-policy views.

| Element | Meaning |
|---|---|
| Initial carrying amount | Purchase price plus/minus eligible transaction costs |
| Effective interest rate | Rate that discounts expected cashflows to carrying amount |
| Premium amortization | Reduces carrying amount toward redemption value |
| Discount accretion | Increases carrying amount toward redemption value |
| Impairment/write-down | Reduces carrying amount for credit loss or default |

Client market-value reporting and amortized-cost accounting can coexist as separate valuation bases.

## 7. Price source priority

Example configurable priority:

```text
Official exchange close -> primary vendor -> custodian price -> issuer quote -> internal model -> last price with stale flag -> manual override
```

Different products require different priorities. For a structured note, issuer quote may be primary. For listed equity, exchange close should usually dominate.

## 8. Stale/missing price controls

| Issue | Control |
|---|---|
| Missing price | Use previous price only with stale flag and aging rule |
| Stale price | Flag by product-specific threshold |
| Suspended security | Preserve last official price or fair-value committee price; disclose |
| Outlier price | Tolerance check versus prior price, benchmark, vendor comparison |
| Corporate action not reflected | Adjust price/quantity before valuation |
| FX missing | Use approved fallback or block reporting |
| Manual override | Require reason, approval, expiry and audit trail |

## 9. Valuation and P&L dependency

Unrealized P&L depends on valuation. If valuation is stale or estimated, P&L is also stale or estimated.

Recommended reporting labels:

- Official market value;
- Estimated market value;
- Last available value;
- Manager-reported NAV as of date;
- Suspended / no reliable valuation;
- Excluded from performance due to missing valuation.

## 10. Collateral valuation

Lending and buying power should not use simple market value alone.

```text
Collateral value = Market value x Eligibility x LTV / haircut rule x pledge availability
```

Important fields:

- market value;
- eligible collateral flag;
- LTV/haircut;
- concentration haircut;
- issuer/security restrictions;
- pledge status;
- stale-price adjustment;
- FX haircut;
- existing exposure allocation.

## 11. Valuation QA scenarios

| Scenario | Expected result |
|---|---|
| Equity price missing for one day | Prior price used only if stale policy allows and flag displayed |
| Bond accrued interest double-counted | Dirty value reconciliation catches mismatch |
| Fund NAV lagged three months | Valuation age shown; performance caveat applied |
| Structured note issuer quote unavailable | Fallback source used with lower quality flag |
| Manual price override | Approval and expiry required |
| Corporate action split without price adjustment | Outlier control flags large false P&L |

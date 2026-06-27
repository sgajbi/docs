# 11. Worked Examples and Implementation Patterns

## Purpose

This file turns market data, reference data, pricing and instrument-master concepts into practical examples for advisory, analytics, reporting, valuation, reconciliation, migration and QA.

## Example 1. Instrument master golden copy creation

### Scenario

Three source systems provide data for the same bond.

| Field | Vendor A | Custodian | Internal product master | Golden copy treatment |
|---|---|---|---|---|
| ISIN | XS123 | XS123 | XS123 | Match key |
| Currency | USD | USD | USD | Accepted |
| Coupon | 5.00% | 5.00% | 5.00% | Accepted |
| Maturity | 2030-06-30 | 2030-06-30 | 2030-06-30 | Accepted |
| Seniority | Senior unsecured | blank | Senior | Normalize with controlled vocabulary |
| Callable flag | Yes | No | Yes | Exception requiring source priority rule |

### Golden copy decision

The platform should not average or randomly choose conflicting reference data. It should apply source priority, validation rules and exception workflow.

### QA assertions

| Test | Expected result |
|---|---|
| Identifier matches but callable flag conflicts | Instrument enters exception workflow. |
| Vendor updates call schedule | Golden copy version changes with lineage. |
| Custodian has blank seniority | Blank does not overwrite trusted populated source. |
| Historical report is regenerated | Effective-dated instrument master version is used. |

## Example 2. Stale price and valuation hierarchy

### Scenario

A portfolio holds an OTC bond. Multiple price sources are available.

| Source | Price | Timestamp | Quality |
|---|---:|---|---|
| Executable quote | unavailable | n/a | Missing |
| Evaluated price vendor | 97.20 | Today 18:00 | Fresh |
| Custodian price | 96.80 | Yesterday 18:00 | Stale |
| Manual override | 97.00 | Today 19:00 | Approved override |

### Policy example

| Priority | Source | Rule |
|---|---|---|
| 1 | Approved manual override | Use only with reason and expiry. |
| 2 | Executable market quote | Use if fresh and valid. |
| 3 | Evaluated vendor price | Use if market quote unavailable. |
| 4 | Custodian price | Use as fallback with stale flag if outside threshold. |

### Reporting treatment

If the override is used, client and internal reports should preserve source lineage. If a stale fallback is used, report quality status should show degraded valuation.

### QA assertions

| Test | Expected result |
|---|---|
| Fresh vendor price exists and no override | Vendor price is selected. |
| Approved override exists | Override is selected with reason and expiry. |
| All sources are stale | Valuation is flagged source-limited or stale. |
| Price changes beyond threshold | Outlier workflow is triggered. |

## Example 3. FX rate selection for reporting

### Scenario

A SGD reporting portfolio holds USD and EUR assets.

| Currency pair | Source | Rate | Time |
|---|---|---:|---|
| USD/SGD | Official close | 1.3500 | 17:00 |
| EUR/SGD | Vendor close | 1.4600 | 17:00 |
| EUR/USD | Vendor close | 1.0815 | 17:00 |

### Implementation questions

| Question | Why it matters |
|---|---|
| Use direct EUR/SGD or triangulate through USD? | Avoid inconsistent cross rates. |
| Which timestamp defines report close? | Prevent mismatched prices and FX rates. |
| Which source owns official reporting FX? | Supports audit and reproducibility. |
| How are missing rates handled? | Avoid silent stale or synthetic conversion. |

### QA assertions

| Test | Expected result |
|---|---|
| Direct rate is missing | Triangulation is used only if policy allows it. |
| FX timestamp is after valuation cutoff | Rate is rejected or flagged. |
| Same report is regenerated later | Same effective FX rate version is used. |
| Currency pair is inverted | System applies reciprocal consistently and labels pair direction. |

## Example 4. Yield curve input and bond analytics

### Scenario

A fixed-income analytics engine needs a curve for USD bond duration and spread analytics.

| Input | Example |
|---|---|
| Curve family | USD Treasury |
| Tenors | 1Y, 2Y, 5Y, 10Y, 30Y |
| Rate type | Par, zero or discount curve |
| Timestamp | Close of business |
| Calendar | USD business calendar |

### Data-quality checks

| Check | Failure impact |
|---|---|
| Missing tenor | Duration and spread interpolation may fail. |
| Stale curve | Risk analytics may be misleading. |
| Wrong currency | Spread and valuation results are invalid. |
| Wrong curve type | Discounting and spread calculations are inconsistent. |

### QA assertions

| Test | Expected result |
|---|---|
| 5Y point is missing | Analytics degrades or interpolates according to explicit policy. |
| Curve date differs from price date | Report flags date mismatch. |
| Bond currency is USD but curve is EUR | Analytics validation fails. |
| Curve is restated | Downstream analytics are reproducible by curve version. |

## Example 5. Corporate action reference data revision

### Scenario

An issuer announces a stock split. A vendor later corrects the effective date.

| Field | Original | Revised |
|---|---|---|
| Event type | Stock split | Stock split |
| Ratio | 2-for-1 | 2-for-1 |
| Effective date | 2026-07-01 | 2026-07-02 |
| Source | Vendor feed | Vendor correction |

### Correct treatment

The platform should version the corporate action event and re-evaluate impacted positions, trades, valuations, reports and reconciliation results.

### QA assertions

| Test | Expected result |
|---|---|
| Effective date changes | Event version increments and impacted outputs are identified. |
| Report was already produced | Restatement workflow is triggered if material. |
| Split ratio is applied twice | Position validation catches duplicate processing. |
| Custodian event differs from vendor event | Exception workflow compares source authority and resolution. |

## Example 6. Product-specific minimum data requirements

| Product area | Required data examples | If missing |
|---|---|---|
| Bonds | Coupon, maturity, day count, issuer, rating, price, accrued interest | Yield, accrued income and duration degrade. |
| Funds | NAV, dealing calendar, share class, fees, distributions | Valuation and subscription/redemption workflow degrade. |
| Equities | Exchange, currency, corporate actions, close price, sector | Valuation, risk and event handling degrade. |
| Derivatives | Contract terms, underlier, notional, expiry, valuation inputs | Exposure, margin and P&L degrade. |
| Private markets | NAV date, capital calls, distributions, commitments | Performance and liquidity reporting degrade. |
| FX | Currency pair, fixing source, value date calendar, rate timestamp | Conversion, settlement and reporting degrade. |

### Design principle

Data completeness should be evaluated by product capability, not by a generic "instrument exists" flag. A product can be known to the instrument master while still being unsupported for valuation, advisory, reporting or analytics.

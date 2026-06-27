# 05. Worked Examples and Implementation Patterns

## Purpose

This file turns bond concepts into practical examples for advisory workflows, portfolio analytics, reporting, implementation and QA. It focuses on the calculations and lifecycle states that commonly create defects in wealth-management platforms.

## Example 1. Secondary bond buy with clean price and accrued interest

### Scenario

A client buys a fixed-rate corporate bond in the secondary market.

| Attribute | Value |
|---|---:|
| Nominal purchased | 200,000 |
| Clean price | 101.25 |
| Accrued interest | 1.35 per 100 nominal |
| Commission | 150 |
| Settlement currency | USD |

### Calculation

```text

clean consideration = nominal x clean price / 100
clean consideration = 200,000 x 101.25 / 100
clean consideration = 202,500

accrued interest paid = nominal x accrued interest / 100
accrued interest paid = 200,000 x 1.35 / 100
accrued interest paid = 2,700

settlement cash = clean consideration + accrued interest paid + commission
settlement cash = 202,500 + 2,700 + 150
settlement cash = 205,350

```

### Correct treatment

| Component | Treatment |
|---|---|
| Clean consideration | Security cost basis or investment purchase amount, depending policy. |
| Accrued interest paid | Accrual settlement, not price loss. |
| Commission | Fee or cost-basis adjustment, depending reporting policy. |
| Position quantity | Nominal amount of 200,000. |
| Statement value | Usually market value plus accrued interest when dirty value is shown. |

### QA assertions

| Test | Expected result |
|---|---|
| Trade is quoted clean | Settlement cash includes accrued interest separately. |
| Accrued interest is missing | Trade is incomplete for settlement and reporting. |
| Position quantity is stored as market value | Coupon, redemption and risk calculations fail. |
| Commission policy changes | Cost basis and performance treatment follow configured policy. |

## Example 2. Coupon accrual and coupon payment

### Scenario

A client holds a semi-annual fixed coupon bond.

| Attribute | Value |
|---|---:|
| Nominal | 500,000 |
| Annual coupon | 4.00% |
| Coupon frequency | Semi-annual |
| Coupon period days | 180 |
| Accrued days | 90 |
| Day-count basis | 360 |

### Accrued interest

```text

annual coupon cash = nominal x coupon rate
annual coupon cash = 500,000 x 4.00%
annual coupon cash = 20,000

period coupon = annual coupon cash / 2
period coupon = 10,000

accrued interest = nominal x coupon rate x accrued days / day-count basis
accrued interest = 500,000 x 4.00% x 90 / 360
accrued interest = 5,000

```

### Coupon payment

On coupon date, the platform should book coupon cash and reset the accrual for the next coupon period.

| Event | Position impact | Cash impact | Income impact |
|---|---|---:|---:|
| Daily accrual | No nominal change | No settled cash movement | Accrued income increases |
| Coupon payment | No nominal change | +10,000 | Realized income recognized |
| Accrual reset | No nominal change | No settled cash movement | Accrued income resets for new period |

### QA assertions

| Test | Expected result |
|---|---|
| Coupon date arrives | Cash income is booked and accrual resets. |
| Bond is bought mid-period | Accrued interest paid on purchase is not double counted as income. |
| Coupon is delayed by custodian | Receivable remains open until cash is received. |
| Withholding tax applies | Gross coupon, tax and net cash are separately reportable. |

## Example 3. Yield, price and premium bond explanation

### Scenario

A bond has a 5% coupon but trades above par because market yields have fallen.

| Attribute | Value |
|---|---:|
| Coupon | 5.00% |
| Clean price | 108.00 |
| Years to maturity | 4 |
| Redemption | 100.00 |

### Practitioner explanation

The client receives a 5% coupon on nominal, but pays 108 for a bond that redeems at 100. The premium is gradually offset by pull-to-par as maturity approaches. Therefore yield is lower than coupon.

### Approximate annualized pull-to-par effect

```text

premium over par = clean price - redemption value
premium over par = 108 - 100
premium over par = 8 points

annual premium amortization approximation = premium / years to maturity
annual premium amortization approximation = 8 / 4
annual premium amortization approximation = 2 points per year

rough yield intuition = coupon - annual premium amortization
rough yield intuition = 5% - 2%
rough yield intuition = about 3% before compounding and exact cashflow timing

```

This is not a formal yield calculation. It is a stakeholder explanation for why coupon and yield differ.

### QA assertions

| Test | Expected result |
|---|---|
| Bond is at premium | Report distinguishes coupon rate from yield. |
| Report says client earns 5% yield | Reporting QA fails unless exact yield supports that claim. |
| Price moves toward par near maturity | Pull-to-par effect is not treated as unexpected data error. |
| Yield source is unavailable | Report shows source-limited yield state rather than using coupon as a substitute. |

## Example 4. Duration, DV01 and rate shock

### Scenario

A portfolio holds a fixed-rate bond position.

| Attribute | Value |
|---|---:|
| Market value | 1,000,000 |
| Modified duration | 5.2 |
| Yield shock | +50 bp |

### Approximate price impact

```text

price impact percentage = -modified duration x yield change
price impact percentage = -5.2 x 0.50%
price impact percentage = -2.60%

market value impact = 1,000,000 x -2.60%
market value impact = -26,000

DV01 = market value x modified duration x 0.0001
DV01 = 1,000,000 x 5.2 x 0.0001
DV01 = 520 per 1 bp

50 bp impact = DV01 x 50
50 bp impact = 520 x 50
50 bp impact = 26,000

```

### Reporting treatment

| Metric | Use |
|---|---|
| Modified duration | Interest-rate sensitivity estimate. |
| DV01 / PV01 | Currency amount sensitivity for small yield moves. |
| Convexity | Improves estimate for larger moves. |
| Spread duration | Credit spread sensitivity, separate from risk-free rate duration. |

### QA assertions

| Test | Expected result |
|---|---|
| Yield rises | Fixed-rate bond market value generally falls. |
| Duration is missing | Rate-shock analytics should show degraded state. |
| Shock is expressed in bp | System converts bp correctly to decimal yield change. |
| Portfolio has floating-rate bonds | Duration is materially different from fixed-rate bonds. |

## Example 5. Callable bond yield-to-worst

### Scenario

A client considers a callable bond trading above par.

| Attribute | Value |
|---|---:|
| Clean price | 104.00 |
| Coupon | 5.00% |
| Maturity | 5 years |
| First call date | 2 years |
| Call price | 100.00 |

### Why yield-to-worst matters

If the issuer calls the bond after two years, the client receives fewer years of coupon income and loses the premium paid above par sooner. Yield to maturity may look attractive, but yield to call may be lower.

### Simplified intuition

```text

premium paid = 104 - 100
premium paid = 4 points

if called in 2 years:
annual premium loss approximation = 4 / 2 = 2 points per year
rough yield-to-call intuition = coupon - annual premium loss
rough yield-to-call intuition = 5% - 2% = about 3%

```

Formal yield-to-call and yield-to-maturity should be calculated using cashflow discounting. The key platform point is that yield-to-worst should compare all relevant redemption outcomes.

### QA assertions

| Test | Expected result |
|---|---|
| Callable bond trades above par | Yield-to-worst may be materially below yield-to-maturity. |
| Call schedule is missing | Yield-to-worst is source-limited and should not be shown as complete. |
| Issuer announces call | Lifecycle event creates redemption workflow and reporting note. |
| Advisor compares callable and non-callable bonds | Reinvestment and call risk are visible in advisory view. |

## Example 6. Rating downgrade, spread widening and collateral haircut

### Scenario

A bond held in a Lombard collateral portfolio is downgraded and credit spreads widen.

| Attribute | Before | After |
|---|---:|---:|
| Rating | A- | BBB- |
| Clean price | 99.50 | 94.00 |
| Nominal | 1,000,000 | 1,000,000 |
| Collateral haircut | 20% | 35% |

### Market value and collateral value

```text

before market value = 1,000,000 x 99.50 / 100 = 995,000
before collateral value = 995,000 x (1 - 20%) = 796,000

after market value = 1,000,000 x 94.00 / 100 = 940,000
after collateral value = 940,000 x (1 - 35%) = 611,000

collateral value reduction = 796,000 - 611,000
collateral value reduction = 185,000

```

### Platform impact

| Area | Impact |
|---|---|
| Performance | Price decline creates negative contribution. |
| Risk | Credit quality, spread exposure and concentration change. |
| Advisory | Client review may be required. |
| Lending | Collateral value falls and may trigger margin call. |
| Product governance | Product may move to watchlist or buy restriction. |
| Reporting | Explain downgrade, price impact and collateral implication if relevant. |

### QA assertions

| Test | Expected result |
|---|---|
| Rating changes | Risk and collateral haircut rules recompute. |
| Price changes but rating does not | Performance changes without rating event. |
| Collateral haircut changes but price does not | Lending availability changes even if performance does not. |
| Historical report is rerun before downgrade date | Old rating and old haircut version are used. |

## Example 7. Maturity ladder and reinvestment risk

### Scenario

A fixed-income portfolio has the following maturities.

| Bond | Market value | Maturity bucket |
|---|---:|---|
| Sovereign 2027 | 400,000 | 0 to 2 years |
| Corporate 2029 | 600,000 | 3 to 5 years |
| Subordinated bank 2034 | 500,000 | 6 to 10 years |
| Perpetual callable | 300,000 | No legal maturity / call-dependent |

### Reporting view

| Bucket | Market value | Weight |
|---|---:|---:|
| 0 to 2 years | 400,000 | 22.2% |
| 3 to 5 years | 600,000 | 33.3% |
| 6 to 10 years | 500,000 | 27.8% |
| Perpetual / call-dependent | 300,000 | 16.7% |

### Advisory interpretation

The portfolio is not simply "medium term" because the perpetual callable bond has no final legal maturity. A client liquidity discussion should separate legal maturity, expected call date and market liquidity.

### QA assertions

| Test | Expected result |
|---|---|
| Perpetual bond is present | It is not forced into the longest maturity bucket without label. |
| Callable bond has next call date | Report can show both legal maturity and next call date. |
| Maturity date is missing | Maturity ladder shows unknown bucket and data-quality issue. |
| Client has near-term cash need | Advisory workflow uses maturity ladder and liquidity classification. |

## Example 8. Default, write-down and recovery

### Scenario

A high-yield issuer defaults. The bond is written down and later receives a recovery payment.

| Attribute | Value |
|---|---:|
| Nominal | 500,000 |
| Pre-default clean price | 82.00 |
| Post-default valuation | 25.00 |
| Recovery cash received later | 130,000 |

### Valuation impact

```text

pre-default market value = 500,000 x 82.00 / 100 = 410,000
post-default market value = 500,000 x 25.00 / 100 = 125,000
valuation loss = 285,000

recovery cash received = 130,000

```

### Correct lifecycle treatment

| Event | Treatment |
|---|---|
| Default notice | Lifecycle event with source and effective date. |
| Price drop or write-down | Valuation and performance impact. |
| Coupon missed | Income receivable may be reversed or impaired. |
| Restructuring | May create new securities, cash or amended terms. |
| Recovery payment | Cash transaction linked to default/restructuring event. |

### QA assertions

| Test | Expected result |
|---|---|
| Bond defaults | Product status, valuation source and reporting label change. |
| Coupon is missed | Accrued income is reviewed, impaired or reversed by policy. |
| Recovery arrives later | Cash is linked to original default event, not treated as external contribution. |
| Position is exchanged for new notes | Old and new securities maintain lineage. |

## Implementation-backed capability lens

When reviewing whether a platform truly supports bonds, use these evidence questions:

| Capability | Evidence to look for |
|---|---|
| Nominal-based position model | Positions store nominal/current face, not only market value. |
| Clean/dirty price separation | Trades and valuations separate clean price, accrued interest and dirty value. |
| Coupon schedule support | Coupon accrual, payment, withholding and reset behavior are testable. |
| Lifecycle event support | Maturity, call, amortisation, default, recovery and conversion have explicit workflows. |
| Risk analytics | Yield, duration, spread, rating and concentration can be sourced and degraded safely. |
| Advisory and mandate controls | High-yield, subordinated, perpetual, callable and FX bonds can be governed differently. |
| Reporting explainability | Client reports can explain income, price movement, yield, maturity, credit and liquidity. |
| QA regression | Tests cover normal, degraded, corrected, restated and source-limited states. |

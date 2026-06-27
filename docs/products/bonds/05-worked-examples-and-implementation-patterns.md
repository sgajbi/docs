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

## Example 9. Inflation-linked bond principal uplift

### Scenario

A portfolio holds an inflation-linked government bond. The client statement must show the real coupon basis, the index uplift and the current market value without mixing them into one unexplained price move.

| Attribute | Value |
|---|---:|
| Original nominal | 100,000 |
| Base CPI index | 100.0000 |
| Current CPI index | 108.0000 |
| Real annual coupon | 1.50% |
| Clean real price | 102.00 |

### Calculation

```text

index ratio = current CPI / base CPI
index ratio = 108.0000 / 100.0000 = 1.0800

inflation-adjusted principal = original nominal x index ratio
inflation-adjusted principal = 100,000 x 1.0800 = 108,000

semi-annual coupon = inflation-adjusted principal x real coupon / 2
semi-annual coupon = 108,000 x 1.50% / 2 = 810

clean market value = inflation-adjusted principal x clean real price / 100
clean market value = 108,000 x 102.00 / 100 = 110,160

```

### Reporting interpretation

The client owns the bond nominal, but reporting should explain the inflation-adjusted principal used for valuation and coupon calculation. Performance should separate real price movement, inflation uplift and FX effect where applicable.

### QA assertions

| Test | Expected result |
|---|---|
| Current CPI is stale | Adjusted principal is labelled stale or blocked according to policy. |
| Deflation floor applies | Principal floor is applied only when the bond terms support it. |
| Coupon is projected | Coupon is labelled projected until the index ratio and payment are confirmed. |
| Performance is explained | Real return, inflation uplift and currency effect are separately attributable. |

## Example 10. Amortising bond principal schedule

### Scenario

A client holds an amortising bond. Principal is returned in instalments, so coupon income, outstanding exposure and maturity ladder reporting all change over time.

| Year | Opening principal | Coupon at 4.00% | Principal repayment | Closing principal |
|---:|---:|---:|---:|---:|
| 1 | 500,000 | 20,000 | 100,000 | 400,000 |
| 2 | 400,000 | 16,000 | 100,000 | 300,000 |
| 3 | 300,000 | 12,000 | 100,000 | 200,000 |
| 4 | 200,000 | 8,000 | 100,000 | 100,000 |
| 5 | 100,000 | 4,000 | 100,000 | 0 |

### Platform implication

The position model needs current face or outstanding principal, not only original nominal. Cashflow processing must split coupon from principal repayment so income, capital returned, realized performance and projected liquidity remain explainable.

### QA assertions

| Test | Expected result |
|---|---|
| Principal repayment posts | Outstanding principal falls and cash increases by the principal amount. |
| Coupon accrues after repayment | Accrual uses reduced principal from the effective schedule date. |
| Maturity ladder is shown | Ladder uses scheduled principal return dates, not only final legal maturity. |
| Schedule is missing | Bond is flagged as schedule-incomplete rather than treated as bullet maturity. |

## Example 11. Convertible bond conversion exposure

### Scenario

A client holds a convertible bond. The legal position is still a bond until conversion, but the advisory and risk views need to show equity optionality.

| Attribute | Value |
|---|---:|
| Bond nominal | 100,000 |
| Conversion price | 25.00 |
| Underlying share price | 28.00 |
| Bond floor valuation | 96,000 |

### Calculation

```text

shares on conversion = bond nominal / conversion price
shares on conversion = 100,000 / 25.00 = 4,000

conversion value = shares on conversion x underlying share price
conversion value = 4,000 x 28.00 = 112,000

equity option value indicator = conversion value - bond floor valuation
equity option value indicator = 112,000 - 96,000 = 16,000

```

### Correct holding treatment

The client should not see a direct equity position before conversion. Reporting can show analytical exposure, conversion value and downside bond floor, but the accounting position remains the convertible bond until a confirmed conversion event creates equity shares and closes or reduces the bond position.

### QA assertions

| Test | Expected result |
|---|---|
| Underlying share price is missing | Conversion value is unavailable and clearly labelled. |
| Conversion election is pending | Bond position remains open until conversion confirmation. |
| Conversion completes | Equity position is created from confirmed terms with lineage to the bond. |
| Mandate blocks direct equity | Workflow distinguishes current bond holding from potential equity delivery. |

## Example 12. Asset-backed security prepayment

### Scenario

An asset-backed security receives scheduled principal and unscheduled prepayment. Yield and weighted-average life must update because principal returns faster than expected.

| Attribute | Value |
|---|---:|
| Original principal | 1,000,000 |
| Opening current principal | 800,000 |
| Monthly coupon rate | 4.00% / 12 |
| Scheduled principal | 20,000 |
| Unscheduled prepayment | 80,000 |

### Calculation

```text

monthly interest = opening current principal x annual coupon / 12
monthly interest = 800,000 x 4.00% / 12 = 2,666.67

ending current principal = opening current principal - scheduled principal - unscheduled prepayment
ending current principal = 800,000 - 20,000 - 80,000 = 700,000

ending factor = ending current principal / original principal
ending factor = 700,000 / 1,000,000 = 0.7000

```

### Platform implication

ABS reporting needs factor, current principal, scheduled principal, unscheduled principal, interest and source remittance date. Prepayment changes reinvestment risk and yield; it should not be treated as a price gain.

### QA assertions

| Test | Expected result |
|---|---|
| Trustee remittance report arrives | Principal, interest and factor update from the same source period. |
| Unscheduled prepayment posts | Cashflow is classified as principal return, not income. |
| Factor is restated | Prior period valuation and performance can be corrected with audit trail. |
| Prepayment model is unavailable | Yield projection is labelled source-limited rather than over-precise. |

## Example 13. Multi-currency bond performance

### Scenario

A USD-reporting portfolio holds a EUR bond. The bond rises in local currency, but EUR weakens against USD.

| Attribute | Start | End |
|---|---:|---:|
| Local EUR value | 100,000 | 101,500 |
| EUR/USD FX rate | 1.1000 | 1.0800 |
| USD reporting value | 110,000 | 109,620 |

### Calculation

```text

local return = (101,500 - 100,000) / 100,000 = 1.50%

base-currency return = (109,620 - 110,000) / 110,000 = -0.35%

currency effect approximation = base-currency return - local return
currency effect approximation = -0.35% - 1.50% = -1.85%

```

### Reporting interpretation

The bond performed positively in EUR, but the USD statement shows a small loss because the currency move offset the local return. Reports should separate local security return, income, FX conversion effect and any hedge effect.

### QA assertions

| Test | Expected result |
|---|---|
| FX rate is stale | Reporting value and performance are labelled stale or blocked. |
| Coupon is paid in EUR | Income is converted using the policy rate for the cashflow date. |
| FX hedge exists | Hedged and unhedged performance views are separated. |
| Client asks why value fell | Explanation shows local bond return versus currency effect. |

## Example 14. Sinkable Bond Principal Reduction

### Scenario

A client holds a sinkable corporate bond with scheduled partial redemptions before final maturity. Unlike an ordinary bullet bond, the issuer repays part of the nominal amount each year.

| Attribute | Value |
|---|---:|
| Opening nominal | 1,000,000 |
| Annual sink amount | 100,000 |
| Coupon rate | 4.50% |
| Clean price before sink | 101.20 |
| Sink redemption price | 100.00 |

### Year-one event

```text

principal redeemed = 100,000 x 100.00 / 100 = 100,000
remaining nominal = 1,000,000 - 100,000 = 900,000
annual coupon after sink = 900,000 x 4.50% = 40,500

```

### Correct treatment

| Area | Treatment |
|---|---|
| Position | Reduce current face or nominal by the redeemed amount. |
| Cash | Book redemption cash separately from coupon income. |
| Accrual | Future coupon accrual uses remaining nominal. |
| Yield | Yield and weighted-average life update after the sink event. |
| Reporting | Maturity ladder shows scheduled principal return, not only final maturity. |

### QA assertions

| Test | Expected result |
|---|---|
| Sink schedule is present | Projected cashflows include partial redemptions. |
| Sink event posts | Position nominal falls and cash increases by principal amount. |
| Coupon accrues after sink | Accrual uses reduced nominal from the effective date. |
| Schedule is missing | Bond is flagged as schedule-incomplete rather than treated as a simple bullet bond. |

## Example 15. Make-Whole Call Premium

### Scenario

An issuer calls a corporate bond using a make-whole provision. The call price is determined from the present value of remaining cashflows using a benchmark rate plus a contractual spread.

| Attribute | Value |
|---|---:|
| Nominal | 500,000 |
| Par value | 100.00 |
| Make-whole call price | 103.75 |
| Accrued interest per 100 nominal | 1.20 |

### Settlement cash

```text

principal call proceeds = 500,000 x 103.75 / 100 = 518,750
accrued interest paid = 500,000 x 1.20 / 100 = 6,000
total redemption cash = 518,750 + 6,000 = 524,750

```

### Correct treatment

| Component | Treatment |
|---|---|
| Make-whole premium | Principal/redemption economics, not ordinary coupon income. |
| Accrued interest | Income accrual settlement through call date. |
| Position | Bond position closes or reduces according to called amount. |
| Performance | Redemption gain/loss compares proceeds with carrying value under policy. |
| Source | Call notice and calculation agent terms must support the price. |

### QA assertions

| Test | Expected result |
|---|---|
| Call notice includes make-whole price | Redemption uses sourced call price and accrued interest separately. |
| Call price is missing | Event remains pending or source-limited, not defaulted to par. |
| Partial call occurs | Position reduces only by called nominal. |
| Report classifies premium as coupon | Reporting QA fails because call premium and income are different components. |

## Example 16. Distressed Exchange Into New Notes

### Scenario

A distressed issuer offers to exchange old bonds for a package of new notes and cash.

| Attribute | Value |
|---|---:|
| Old bond nominal | 400,000 |
| Old bond carrying value | 220,000 |
| New note exchange ratio | 60% of old nominal |
| Cash consideration | 5% of old nominal |
| New note fair value | 72.00 |

### Exchange outcome

```text

new note nominal = 400,000 x 60% = 240,000
cash received = 400,000 x 5% = 20,000
new note fair value = 240,000 x 72.00 / 100 = 172,800
total package value = 172,800 + 20,000 = 192,800
economic change versus carrying value = 192,800 - 220,000 = -27,200

```

### Correct treatment

| Area | Treatment |
|---|---|
| Lifecycle | Old bond is closed or reduced through exchange event. |
| New holding | New note is created only after source-confirmed terms. |
| Cash | Cash consideration is linked to the exchange, not external contribution. |
| Performance | Economic loss or gain is traceable to the restructuring event. |
| Advisory | Distressed exchange status, liquidity and issuer risk remain visible. |

### QA assertions

| Test | Expected result |
|---|---|
| Exchange ratio is sourced | New nominal uses the confirmed ratio. |
| Cash component arrives late | Receivable remains linked to the exchange event. |
| New note identifier is missing | New position creation is blocked or support-limited. |
| Historical report is rerun | Old and new instruments retain event lineage and effective dates. |

## Example 17. Bond Tax-Lot Sale And Realized Result

### Scenario

A client sells part of a bond position accumulated through two purchases. The reporting policy uses FIFO lots.

| Lot | Nominal | Clean cost price |
|---|---:|---:|
| Lot A | 100,000 | 98.00 |
| Lot B | 150,000 | 101.00 |

Sale:

| Attribute | Value |
|---|---:|
| Nominal sold | 180,000 |
| Clean sale price | 102.00 |
| Commission | 120 |

### FIFO allocation

```text

sale proceeds before commission = 180,000 x 102.00 / 100 = 183,600
net sale proceeds = 183,600 - 120 = 183,480

cost from Lot A = 100,000 x 98.00 / 100 = 98,000
cost from Lot B = 80,000 x 101.00 / 100 = 80,800
allocated cost = 178,800

realized result = 183,480 - 178,800 = 4,680
remaining Lot B nominal = 150,000 - 80,000 = 70,000

```

### Correct treatment

The sale should reduce tax lots and position nominal independently from accrued interest settlement. Lot selection must be deterministic and auditable because realized gain, tax reporting, performance and remaining cost basis all depend on it.

### QA assertions

| Test | Expected result |
|---|---|
| FIFO policy applies | Sale consumes Lot A before Lot B. |
| Specific-lot election applies | Sale follows elected lots with approval evidence. |
| Accrued interest is present | Accrued interest settlement is reported separately from clean-price realized result. |
| Lot history is incomplete | Realized result is labelled partial or blocked according to reporting policy. |

## Example 18. Accrued-Interest Correction After Custodian Restatement

### Scenario

A custodian restates accrued interest for a settled bond trade because the original day-count convention was wrong.

| Attribute | Original | Corrected |
|---|---:|---:|
| Nominal | 300,000 | 300,000 |
| Accrued interest per 100 nominal | 0.90 | 1.05 |

### Correction

```text

original accrued interest = 300,000 x 0.90 / 100 = 2,700
corrected accrued interest = 300,000 x 1.05 / 100 = 3,150
cash and accrual correction = 3,150 - 2,700 = 450

```

### Correct treatment

| Area | Treatment |
|---|---|
| Trade record | Preserve original and corrected accrued-interest values. |
| Cash | Post correction cash or receivable/payable according to settlement state. |
| Income | Adjust accrued-income history without treating the correction as market price movement. |
| Reports | Restated statements carry correction lineage and reason code. |
| QA | Day-count convention and coupon schedule are revalidated. |

### QA assertions

| Test | Expected result |
|---|---|
| Restatement arrives | Adjustment is linked to original trade and source correction. |
| Correction changes cash | Cash ledger and accrued-income report reconcile. |
| Report was already published | Correction workflow records impacted report versions. |
| Day-count source remains missing | Future accrual remains support-limited until source terms are fixed. |

## Example 19. Cross-Currency Hedged Bond Attribution

### Scenario

A USD-reporting portfolio holds a EUR bond and an FX forward hedge. The bond gains locally, EUR weakens, and the hedge partly offsets the currency loss.

| Attribute | Value |
|---|---:|
| Starting EUR bond value | 1,000,000 |
| Local bond return | 2.00% |
| Starting EUR/USD | 1.1000 |
| Ending EUR/USD | 1.0700 |
| FX forward hedge P&L | 24,000 USD |

### Attribution

```text

starting USD value = 1,000,000 x 1.1000 = 1,100,000
ending EUR bond value = 1,000,000 x (1 + 2.00%) = 1,020,000
ending unhedged USD value = 1,020,000 x 1.0700 = 1,091,400

unhedged USD result = 1,091,400 - 1,100,000 = -8,600
local bond gain in USD at starting FX = 1,000,000 x 2.00% x 1.1000 = 22,000
currency effect approximation = -8,600 - 22,000 = -30,600
hedged result = -8,600 + 24,000 = 15,400

```

### Reporting interpretation

| Component | Meaning |
|---|---|
| Local bond return | Security return before reporting-currency translation. |
| Currency effect | Impact of EUR/USD movement on unhedged bond value. |
| Hedge P&L | FX forward contribution linked to the bond exposure or overlay strategy. |
| Residual | Basis difference, hedge ratio mismatch, timing and fees. |

### QA assertions

| Test | Expected result |
|---|---|
| Hedge link is present | Report can show hedged and unhedged views. |
| Hedge link is missing | FX forward remains separate derivative P&L, not forced into bond attribution. |
| FX rates are stale | Attribution is stale or blocked. |
| Hedge ratio is below 100% | Residual currency exposure remains visible. |

## Example 20. Source-Restated ABS Factor

### Scenario

A trustee restates the prior month factor for an asset-backed security after correcting principal collections. The original factor was already used in valuation and performance.

| Attribute | Original | Restated |
|---|---:|---:|
| Original principal | 1,000,000 | 1,000,000 |
| Ending current principal | 700,000 | 690,000 |
| Factor | 0.7000 | 0.6900 |

### Restatement impact

```text
principal delta = restated current principal - original current principal
principal delta = 690,000 - 700,000
principal delta = -10,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Position | Current face is restated from source, preserving original factor history. |
| Cashflow | Principal return or receivable/payable is adjusted with source period lineage. |
| Valuation | Market value, yield and weighted-average life are recomputed from restated factor. |
| Performance | Prior period impact is assessed for restatement or next-period correction. |
| Reporting | Reported factor carries source date, remittance period and restatement flag. |

### QA assertions

| Test | Expected result |
|---|---|
| Factor is restated after reporting | Impact assessment identifies affected valuation and performance outputs. |
| Cash principal does not match factor movement | Reconciliation break opens between trustee cash and factor data. |
| Restatement source date is missing | Output is labelled source-limited. |
| Historical report is rerun | Original and restated factor versions remain auditable. |

## Example 21. Bond Tender Offer With Partial Acceptance

### Scenario

An issuer offers to repurchase bonds at a premium. The client tenders 500,000 nominal, but the offer is oversubscribed and only 60% is accepted.

| Attribute | Value |
|---|---:|
| Tendered nominal | 500,000 |
| Accepted percentage | 60% |
| Tender price | 102.50 |
| Accrued interest per 100 nominal | 0.80 |

### Cash outcome

```text
accepted nominal = 500,000 x 60% = 300,000
tender principal proceeds = 300,000 x 102.50 / 100 = 307,500
accrued interest received = 300,000 x 0.80 / 100 = 2,400
total cash received = 307,500 + 2,400 = 309,900
remaining nominal = 500,000 - 300,000 = 200,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Election | Store tender instruction, deadline, accepted percentage and source notice. |
| Position | Reduce only accepted nominal, not full tendered nominal. |
| Cash | Split tender price economics from accrued interest. |
| Performance | Realized result compares accepted proceeds to the accepted lot carrying value. |
| Client reporting | Show remaining bond position and tender outcome clearly. |

### QA assertions

| Test | Expected result |
|---|---|
| Tender is only partially accepted | Position reduces by accepted nominal only. |
| Accrued interest is paid separately | Income/accrual reporting separates it from tender premium. |
| Acceptance percentage arrives late | Tender remains pending until source confirmation. |
| Client tenders after deadline | Election is rejected or labelled late according to source status. |

## Example 22. Consent Solicitation Fee And Covenant Change

### Scenario

An issuer asks bondholders to consent to a covenant amendment and pays a consent fee to participating holders.

| Attribute | Value |
|---|---:|
| Eligible nominal | 400,000 |
| Consent fee | 0.25 per 100 nominal |
| Consent fee cash | 1,000 |

### Calculation

```text
consent fee cash = eligible nominal x fee per 100 / 100
consent fee cash = 400,000 x 0.25 / 100
consent fee cash = 1,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Election | Store consent decision, authority, deadline and source acceptance status. |
| Cash | Book consent fee as event-linked cashflow according to income/fee policy. |
| Instrument terms | Update covenant terms only after amendment becomes effective. |
| Risk | Flag changed covenant protection, subordination or issuer flexibility. |
| Reporting | Explain fee and changed terms without treating the event as a sale. |

### QA assertions

| Test | Expected result |
|---|---|
| Consent submitted but not accepted | No fee is booked until source acceptance. |
| Amendment fails threshold | Instrument terms remain unchanged. |
| Fee is received without election record | Reconciliation exception opens. |
| Covenant change affects suitability | Product governance or advisory review is triggered. |

## Example 23. Contingent Convertible Trigger Event

### Scenario

A bank contingent convertible bond breaches a regulatory capital trigger. The instrument terms require a permanent write-down of 30% of nominal.

| Attribute | Value |
|---|---:|
| Opening nominal | 250,000 |
| Trigger write-down | 30% |
| Remaining nominal | 175,000 |

### Write-down

```text
write_down_nominal = 250,000 x 30% = 75,000
remaining_nominal = 250,000 - 75,000 = 175,000
```

### Correct treatment

| Area | Treatment |
|---|---|
| Event | Store regulatory trigger, issuer notice, effective date and term basis. |
| Position | Reduce nominal or create conversion shares according to confirmed instrument terms. |
| P&L | Recognize write-down or conversion economics under accounting/performance policy. |
| Income | Stop or amend coupon accrual if coupon cancellation or write-down terms require it. |
| Advisory | Flag complexity, capital-trigger risk, subordination and suitability implications. |

### QA assertions

| Test | Expected result |
|---|---|
| Trigger notice is confirmed | Position and valuation update from source terms. |
| Trigger is rumored but not confirmed | No accounting event posts; risk status may be flagged. |
| Coupon is cancelled | Accrual and projected income are reversed or stopped. |
| Terms require conversion instead of write-down | Equity position is created only after confirmed conversion terms. |

## Example 24. Perpetual Bond Coupon Reset

### Scenario

A perpetual subordinated bond has no final maturity. Its coupon resets on the first call date to the then-current benchmark rate plus a fixed reset spread.

| Attribute | Value |
|---|---:|
| Current nominal | 500,000 |
| Original coupon | 4.75% |
| Reset benchmark rate | 3.20% |
| Reset spread | 2.10% |
| Reset coupon | 5.30% |

### Reset calculation

```text
reset coupon = benchmark rate + reset spread
reset coupon = 3.20% + 2.10%
reset coupon = 5.30%

annual coupon cash after reset = 500,000 x 5.30% = 26,500
```

### Correct treatment

| Area | Treatment |
|---|---|
| Schedule | Store call date, reset date, benchmark, spread and coupon frequency. |
| Accrual | Start new accrual rate only from the effective reset date. |
| Yield | Use call and reset assumptions explicitly; do not force a legal maturity. |
| Liquidity | Treat perpetual maturity ladder separately from expected call scenarios. |
| Advisory | Explain extension risk if issuer does not call. |

### QA assertions

| Test | Expected result |
|---|---|
| Benchmark fixing is missing | Reset coupon is source-limited and accrual cannot finalize. |
| Issuer does not call | Position remains open and coupon resets according to terms. |
| System assumes maturity at first call | QA fails because call date is not legal maturity. |
| Reset spread is stale or wrong | Coupon and projected income are blocked or corrected. |

## Example 25. Hedged Bond Benchmark Attribution

### Scenario

A USD portfolio holds a EUR bond hedged with an FX forward. The performance report compares the hedged bond sleeve against a USD-hedged EUR bond benchmark.

| Component | Portfolio | Benchmark |
|---|---:|---:|
| Local bond return | 1.80% | 1.50% |
| Currency hedge result | 0.60% | 0.55% |
| Fees and residual | -0.10% | 0.00% |
| Total hedged return | 2.30% | 2.05% |

### Attribution

```text
excess return = portfolio hedged return - benchmark hedged return
excess return = 2.30% - 2.05%
excess return = 0.25%
```

### Correct treatment

| Area | Treatment |
|---|---|
| Benchmark | Use a benchmark that matches currency hedge policy and duration/spread exposure where available. |
| Hedge link | Attribute hedge P&L only when hedge relationship is source-backed or policy-defined. |
| Residual | Show timing, hedge ratio, roll cost, fees and basis differences separately where possible. |
| Reporting | Avoid comparing hedged portfolio return to an unhedged benchmark without disclosure. |

### QA assertions

| Test | Expected result |
|---|---|
| Benchmark hedge basis differs | Report labels benchmark mismatch or blocks precise attribution. |
| Hedge P&L is missing | Hedged attribution is source-limited. |
| Portfolio is partially hedged | Residual currency exposure remains visible. |
| Benchmark changes mid-period | Attribution stores effective-dated benchmark membership and policy. |

## Example 26. Multi-Book Bond Accounting

### Scenario

The same bond position is reported under different accounting views: investment performance uses fair value, while accounting ledger reporting uses amortized cost.

| Attribute | Value |
|---|---:|
| Nominal | 1,000,000 |
| Amortized cost carrying value | 985,000 |
| Fair market value | 972,000 |
| Accrued interest | 12,500 |

### View separation

| View | Value basis | Treatment |
|---|---|---|
| Accounting book | Amortized cost plus accrual | Supports ledger, income accrual and accounting reports. |
| Performance book | Fair value plus accrual | Supports market return, contribution and risk reports. |
| Tax book | Lot cost and realized result basis | Supports tax reporting and realized gain/loss policy. |

### Correct treatment

The platform should not overwrite one value basis with another. Each report must state which book, valuation basis, accrued-interest treatment and effective date it uses.

### QA assertions

| Test | Expected result |
|---|---|
| Accounting report is generated | Uses amortized cost basis and accrual policy. |
| Performance report is generated | Uses fair value basis and market return methodology. |
| Tax lot sale occurs | Uses tax book lot basis, not fair value. |
| One book is missing | Report labels unavailable basis instead of substituting another book silently. |

## Implementation-backed capability lens

When reviewing whether a platform truly supports bonds, use these evidence questions:

| Capability | Evidence to look for |
|---|---|
| Nominal-based position model | Positions store nominal/current face, not only market value. |
| Clean/dirty price separation | Trades and valuations separate clean price, accrued interest and dirty value. |
| Coupon schedule support | Coupon accrual, payment, withholding and reset behavior are testable. |
| Lifecycle event support | Maturity, call, sinkable schedules, amortisation, default, recovery, exchange and conversion have explicit workflows. |
| Risk analytics | Yield, duration, spread, rating and concentration can be sourced and degraded safely. |
| Advisory and mandate controls | High-yield, subordinated, perpetual, callable, distressed and FX bonds can be governed differently. |
| Reporting explainability | Client reports can explain income, price movement, yield, maturity, credit, liquidity, tax lots and hedge effects. |
| QA regression | Tests cover normal, degraded, corrected, restated and source-limited states. |

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

## Example 27. Callable Step-Up Note Extension Risk

### Scenario

A client holds a callable step-up note. The coupon increases if the issuer does not call, but the client faces extension risk and reinvestment uncertainty.

| Attribute | Value |
|---|---:|
| Nominal | 500,000 |
| Initial coupon | 3.50% |
| Coupon after first call date | 5.25% |
| First call date | Year 2 |
| Legal maturity | Year 7 |
| Current clean price | 101.00 |

### Call versus extension view

| Scenario | Coupon path | Principal timing | Advisory interpretation |
|---|---|---|---|
| Called at Year 2 | 3.50% until call | Principal returned early | Reinvestment risk if rates are lower. |
| Not called | 3.50%, then 5.25% | Principal remains until later call or maturity | Extension risk and longer issuer exposure. |

### Correct treatment

The next call date is not the legal maturity. Reporting should show legal maturity, next call date, call schedule, step-up coupon and yield basis separately. A yield-to-worst view should compare call and extension paths where source terms permit.

### QA assertions

| Test | Expected result |
|---|---|
| System treats first call date as maturity | QA fails because legal maturity and call option are different. |
| Step-up coupon is missing | Projected income after call date is source-limited. |
| Issuer does not call | Position remains open and coupon resets according to confirmed terms. |
| Advisor compares the note to a bullet bond | Extension risk, call risk and reinvestment risk are visible. |

## Example 28. Covered-Bond Pool Deterioration

### Scenario

A covered bond remains issuer-recourse, but the cover pool quality deteriorates because property collateral values fall and arrears rise.

| Attribute | Before | After |
|---|---:|---:|
| Bond nominal | 1,000,000 | 1,000,000 |
| Clean price | 100.20 | 97.40 |
| Cover pool overcollateralization | 18% | 9% |
| Mortgage arrears ratio | 1.2% | 3.8% |
| Rating outlook | Stable | Negative |

### Platform impact

| Area | Treatment |
|---|---|
| Valuation | Price and spread reflect changed cover-pool and issuer risk. |
| Risk | Track issuer rating, cover-pool metrics and concentration separately where sourced. |
| Advisory | Explain that covered bonds are not risk-free and depend on issuer and cover-pool quality. |
| Collateral | Lending haircut may change based on rating, liquidity or eligible-asset policy. |
| Reporting | Show rating outlook and risk note without overstating property-level analytics if unavailable. |

### QA assertions

| Test | Expected result |
|---|---|
| Cover-pool data is stale | Pool-quality analytics are stale or unavailable, not silently reused. |
| Rating outlook changes | Risk report and advisory watchlist update. |
| Haircut policy depends on rating only | Cover-pool deterioration is visible as a note but haircut follows policy. |
| Historical report predates deterioration | Old source values and rating outlook remain reproducible. |

## Example 29. Sukuk Profit Distribution And Principal Treatment

### Scenario

A client holds a sukuk that pays periodic profit distributions rather than conventional interest. The economic reporting resembles fixed income, but product taxonomy, client wording and compliance labels differ.

| Attribute | Value |
|---|---:|
| Face amount | 300,000 |
| Annual profit rate | 4.20% |
| Distribution frequency | Semi-annual |
| Distribution amount | 6,300 |
| Principal due at maturity | 300,000 |

### Calculation

```text
semi-annual profit distribution = face amount x annual profit rate / 2
semi-annual profit distribution = 300,000 x 4.20% / 2
semi-annual profit distribution = 6,300
```

### Correct treatment

| Area | Treatment |
|---|---|
| Taxonomy | Classify as sukuk or Islamic fixed-income instrument, not generic coupon bond only. |
| Cashflow | Book profit distribution using source terminology while preserving income analytics. |
| Reporting | Use client-appropriate labels such as profit distribution where required. |
| Advisory | Explain issuer/asset risk, structure, liquidity and suitability constraints. |
| Operations | Reconcile distribution notices and maturity proceeds to source terms. |

### QA assertions

| Test | Expected result |
|---|---|
| Report labels distribution as coupon where local policy disallows it | Reporting QA fails. |
| Distribution is delayed | Receivable remains open with source status. |
| Structure has asset-sale event | Lifecycle event is handled according to confirmed sukuk terms. |
| Client filters Shariah-compliant holdings | Sukuk taxonomy supports filter and reporting label. |

## Example 30. Municipal Tax-Equivalent Yield

### Scenario

A taxable investor compares a municipal bond with a corporate bond. The municipal bond has a lower stated yield, but tax treatment can make the after-tax comparison more attractive.

| Attribute | Value |
|---|---:|
| Municipal bond yield | 3.20% |
| Investor marginal tax rate | 35.00% |
| Comparable corporate yield | 4.60% |

### Calculation

```text
tax-equivalent yield = municipal yield / (1 - marginal tax rate)
tax-equivalent yield = 3.20% / (1 - 35.00%)
tax-equivalent yield = 4.92%
```

### Correct treatment

Tax-equivalent yield is an advisory comparison metric, not a contractual yield. It depends on jurisdiction, investor tax status and tax assumptions. Reports should not show it as universal product truth.

| Area | Treatment |
|---|---|
| Advisory | Explain assumptions and compare after-tax economics. |
| Suitability | Confirm investor tax profile and product eligibility. |
| Reporting | Label tax-equivalent yield as assumption-based. |
| QA | Do not calculate when tax status or jurisdiction is missing. |

### QA assertions

| Test | Expected result |
|---|---|
| Marginal tax rate is missing | Tax-equivalent yield is unavailable. |
| Investor is not taxable in relevant jurisdiction | Metric is hidden or labelled not applicable. |
| Tax assumptions change | Historical comparison preserves original assumptions. |
| Corporate yield is compared pre-tax | Report labels municipal tax-equivalent basis clearly. |

## Example 31. Green-Bond Use-Of-Proceeds Failure

### Scenario

A green bond issuer fails to allocate proceeds according to the stated framework within the expected reporting period. The financial bond remains outstanding, but ESG and advisory labels require review.

| Attribute | Value |
|---|---|
| Bond type | Green bond |
| Financial status | Performing |
| Use-of-proceeds report | Missing or adverse |
| ESG label status | Under review |
| Client restriction | Sustainable mandate requires eligible labelled instruments |

### Correct treatment

| Area | Treatment |
|---|---|
| Position | Bond remains a financial holding unless default or redemption event occurs. |
| ESG classification | Green label is suspended, degraded or under review according to source policy. |
| Mandate | Sustainable mandate compliance may breach even when credit status is unchanged. |
| Reporting | Explain ESG label review separately from price, income and credit performance. |
| Product governance | Trigger watchlist, restriction or reassessment workflow. |

### QA assertions

| Test | Expected result |
|---|---|
| Use-of-proceeds report is overdue | ESG status changes without closing the bond position. |
| Sustainable mandate requires eligible label | Mandate breach or review flag appears. |
| Credit rating is unchanged | Credit risk remains stable while ESG classification changes. |
| Historical report predates label issue | Original green-bond status remains auditable for that date. |

## Example 32. Bond Ladder Reinvestment Program

### Scenario

A client maintains a five-year bond ladder. One rung matures and proceeds should be reinvested into the longest target bucket while preserving credit quality, liquidity and concentration rules.

| Attribute | Value |
|---|---:|
| Maturing bond proceeds | 250,000 |
| Target ladder tenor | 5 years |
| Minimum rating | A- |
| Maximum issuer concentration | 10% |
| Minimum residual cash buffer | 50,000 |

### Reinvestment workflow

| Step | Treatment |
|---|---|
| Maturity | Principal proceeds become cash only after settlement confirmation. |
| Screening | Candidate bonds must pass rating, issuer, currency, liquidity and mandate filters. |
| Allocation | Reinvest into target ladder bucket while keeping residual buffer. |
| Execution | Trade order consumes confirmed maturity proceeds or approved bridge funding. |
| Reporting | Show ladder before and after reinvestment, including remaining cash and maturity profile. |

### QA assertions

| Test | Expected result |
|---|---|
| Maturity proceeds are pending | Reinvestment order is conditional or blocked until cash is available. |
| Candidate bond breaches issuer cap | Candidate is excluded or escalated. |
| No eligible five-year bond exists | Cash remains uninvested or shorter tenor is proposed with reason. |
| Client has withdrawal instruction | Reinvestment amount is reduced to preserve cash need. |

## Example 33. Benchmark-Duration Hedge With Futures

### Scenario

A discretionary portfolio is longer duration than its fixed-income benchmark. The portfolio manager uses bond futures to reduce duration exposure without selling cash bonds immediately.

| Attribute | Value |
|---|---:|
| Portfolio market value | 10,000,000 |
| Portfolio duration | 6.2 |
| Benchmark duration | 5.0 |
| Duration gap | 1.2 |
| Futures contract DV01 | 85 |
| Portfolio DV01 gap | 12,000 |

### Hedge sizing

```text
contracts needed = portfolio DV01 gap / futures contract DV01
contracts needed = 12,000 / 85
contracts needed = 141.18
```

Rounded hedge size and liquidity constraints should be reviewed before execution.

### Correct treatment

| Area | Treatment |
|---|---|
| Exposure | Futures reduce interest-rate duration exposure but create derivative exposure. |
| Performance | Hedge P&L should be attributed separately from cash bond return. |
| Mandate | Derivatives permission, leverage limits and collateral/margin rules apply. |
| Reporting | Show cash bond duration, hedge duration impact and residual duration gap. |
| QA | Futures DV01, contract multiplier and sign convention must be sourced and tested. |

### QA assertions

| Test | Expected result |
|---|---|
| Hedge direction is wrong | Duration gap increases and QA catches sign convention error. |
| Futures DV01 is stale | Hedge sizing is source-limited. |
| Mandate disallows derivatives | Hedge order is blocked even if analytics suggest benefit. |
| Benchmark changes | Target duration and hedge gap recalculate with effective-dated benchmark. |

## Example 34. Repo-funded bond position and financing carry

### Scenario

A client buys a liquid government bond and funds it with a short-term repo. The economic position is a bond holding plus secured financing, not a free cash purchase.

| Attribute | Value |
|---|---:|
| Bond nominal | 1,000,000 |
| Bond clean price | 99.20 |
| Accrued interest | 0.80 per 100 |
| Repo haircut | 3.00% |
| Repo rate | 4.80% |
| Repo tenor | 30 days |
| Day-count basis | 360 |

### Funding and carry calculation

```text
dirty bond value = nominal x (clean price + accrued interest) / 100
dirty bond value = 1,000,000 x (99.20 + 0.80) / 100
dirty bond value = 1,000,000

repo cash advanced = dirty bond value x (1 - haircut)
repo cash advanced = 1,000,000 x 97.00%
repo cash advanced = 970,000

client equity funding = dirty bond value - repo cash advanced
client equity funding = 30,000

repo interest = repo cash advanced x repo rate x tenor / day-count basis
repo interest = 970,000 x 4.80% x 30 / 360
repo interest = 3,880
```

### Correct treatment

| Area | Treatment |
|---|---|
| Position | Show bond nominal and market value separately from repo liability. |
| Cash | Repo cash advance is financing, not investment income. |
| Performance | Separate bond return, accrued interest, repo financing cost and any margin calls. |
| Risk | Include issuer risk, rate risk, repo counterparty risk, haircut and rollover risk. |
| Mandate | Check leverage, repo eligibility, collateral use and liquidity policy. |
| Reporting | Label funded exposure, net equity, financing cost and maturity/roll date. |

### QA assertions

| Test | Expected result |
|---|---|
| Repo-funded purchase is booked | Bond holding and repo liability are both created. |
| Repo haircut increases | Additional collateral or cash margin call is raised. |
| Repo matures before bond sale | Rollover, repayment or unwind workflow is required. |
| Mandate disallows leverage | Repo-funded purchase is blocked even if bond is eligible. |
| Report shows bond value only | QA fails because financing liability and net exposure are hidden. |

## Example 35. Covered-bond resolution event and cover-pool substitution

### Scenario

A covered-bond issuer enters resolution. The bond remains supported by the cover pool, but the administrator substitutes part of the pool and changes expected payment timing.

| Attribute | Before event | After event |
|---|---:|---:|
| Bond nominal | 750,000 | 750,000 |
| Clean price | 98.80 | 91.50 |
| Overcollateralization | 12% | 8% |
| Expected next coupon date | Original schedule | Delayed pending administrator notice |
| Rating | A | BBB watch |

### Correct treatment

| Area | Treatment |
|---|---|
| Lifecycle event | Store resolution notice, administrator, effective date and source. |
| Valuation | Use stressed price or independent valuation source; label source state. |
| Income | Review coupon receivable timing and impairment policy. |
| Risk | Track issuer resolution status, cover-pool metrics and rating watch separately. |
| Advisory | Explain that cover-pool support may preserve recoveries but does not remove timing and market risk. |
| Reporting | Show resolution state, delayed cashflow and valuation basis without implying default recovery certainty. |

### QA assertions

| Test | Expected result |
|---|---|
| Resolution notice arrives | Bond lifecycle status changes without deleting the position. |
| Coupon timing becomes uncertain | Projected coupon is labelled delayed or source-limited. |
| Cover-pool data is missing | Pool metrics are unavailable, not carried forward silently. |
| Rating watch affects mandate | Mandate breach or review workflow opens. |
| Historical report predates resolution | Prior schedule and rating remain reproducible. |

## Example 36. Sukuk asset-sale event and profit-distribution adjustment

### Scenario

A sukuk structure sells an underlying asset earlier than expected. Holders receive a partial principal repayment and a final adjusted profit distribution.

| Attribute | Value |
|---|---:|
| Opening face amount | 500,000 |
| Asset-sale principal repayment | 200,000 |
| Remaining face amount | 300,000 |
| Final profit distribution on sold portion | 4,200 |
| Source notice | Trustee asset-sale notice |

### Correct treatment

| Area | Treatment |
|---|---|
| Taxonomy | Preserve sukuk classification and client-facing terminology. |
| Position | Reduce face amount by confirmed principal repayment. |
| Cashflow | Split principal repayment from profit distribution. |
| Accrual | Stop accrual on repaid face from effective asset-sale date. |
| Advisory | Explain structural event, remaining exposure and liquidity implication. |
| Reporting | Show asset-sale event, principal returned, profit distribution and remaining face. |

### QA assertions

| Test | Expected result |
|---|---|
| Asset-sale notice arrives | Position face reduces only by confirmed repayment amount. |
| Profit distribution is received | Distribution is not treated as principal. |
| Source notice is missing effective date | Accrual and position update are source-limited. |
| Sukuk label is replaced by generic bond text | Reporting QA fails where terminology matters. |
| Remaining face is valued | Valuation uses remaining face and current price source. |

## Example 37. Callable step-up non-call campaign

### Scenario

An issuer signals that it may not call a step-up bond at the first call date. Advisors must review client expectations because the coupon steps up but principal remains invested for longer.

| Attribute | Value |
|---|---:|
| Nominal | 400,000 |
| Current coupon | 3.25% |
| Step-up coupon if not called | 5.00% |
| First call date | 2027-06-30 |
| Legal maturity | 2032-06-30 |
| Price before campaign | 101.40 |
| Price after non-call signal | 96.80 |

### Correct treatment

| Area | Treatment |
|---|---|
| Lifecycle | Non-call signal is advisory/risk information until issuer confirms call or non-call. |
| Valuation | Market price reflects extension risk and rate/spread repricing. |
| Income | Projected coupon after call date uses step-up terms only when terms and non-call state are confirmed or clearly projected. |
| Advisory | Explain extension risk, reinvestment assumptions, liquidity and issuer incentive. |
| DPM | Review duration, concentration and maturity-bucket drift after extension. |
| Reporting | Show legal maturity, next call date, expected scenario and source confidence separately. |

### QA assertions

| Test | Expected result |
|---|---|
| Issuer only signals possible non-call | No redemption event is booked. |
| Issuer confirms non-call | Position remains open and coupon schedule updates from effective date. |
| Report treats call date as maturity | QA fails because legal maturity remains later. |
| Price falls after non-call signal | Performance explains price effect separately from coupon income. |
| Mandate duration limit is breached | DPM or advisory review opens. |

## Example 38. Green-bond label remediation workflow

### Scenario

A green-bond issuer misses its use-of-proceeds reporting deadline. The sustainable mandate marks the bond under review, then the issuer later publishes a remediation report accepted by product governance.

| Attribute | Value |
|---|---|
| Bond type | Green bond |
| Financial status | Performing |
| Initial ESG label state | Under review |
| Remediation report | Published and accepted |
| Sustainable mandate | Label-eligible instruments required |

### Correct treatment

| Area | Treatment |
|---|---|
| Position | Bond remains a financial holding throughout the label review. |
| ESG label | Move from eligible to under review, then restored or rejected based on source and governance decision. |
| Mandate | Sustainable mandate breach/review state opens while label is under review. |
| Advisory | Explain distinction between credit performance and sustainability-label eligibility. |
| Reporting | Preserve timeline of label issue, remediation, acceptance and effective dates. |
| Product governance | Store reviewer, evidence, decision and next review date. |

### QA assertions

| Test | Expected result |
|---|---|
| Use-of-proceeds report is overdue | Label state moves to under review without closing position. |
| Remediation is accepted | Mandate review can close with source evidence and effective date. |
| Remediation is rejected | Label remains ineligible and mandate action remains open. |
| Historical report is rerun during under-review period | Under-review state remains visible for that date. |
| Credit status is unchanged | Credit risk is not incorrectly downgraded because ESG label changed. |

## Example 39. Duration-hedge roll management

### Scenario

A portfolio uses bond futures to hedge duration. The current futures contract approaches expiry and must be rolled into the next contract without creating a gap or doubling exposure.

| Attribute | Current contract | Next contract |
|---|---:|---:|
| Contracts | -140 | 0 |
| Contract DV01 | 85 | 88 |
| Target portfolio DV01 hedge | -11,900 | -11,900 |
| Days to expiry | 5 | 95 |

### Roll sizing

```text
next contracts needed = target hedge DV01 / next contract DV01
next contracts needed = -11,900 / 88
next contracts needed = -135.23
```

Rounded contract count, execution timing and residual DV01 should be recorded.

### Correct treatment

| Area | Treatment |
|---|---|
| Exposure | Close expiring contract and open next contract with controlled overlap window. |
| Performance | Separate old-contract close P&L, new-contract entry price and roll cost. |
| Risk | Monitor residual DV01 and temporary over/under-hedge during roll. |
| Operations | Confirm exchange, broker, margin and settlement events for both contracts. |
| DPM | Keep hedge within mandate derivative and leverage limits throughout roll. |
| Reporting | Show hedge roll state, residual duration gap and roll-date evidence. |

### QA assertions

| Test | Expected result |
|---|---|
| New contract opens before old contract closes | Temporary overlap is measured and within allowed tolerance. |
| Old contract expires without roll | Duration hedge drops and alert opens. |
| Next contract DV01 differs | Contract count recalculates from new DV01. |
| Margin increases during roll | Collateral and cash availability are updated. |
| Benchmark duration changes during roll | Target hedge amount recalculates before final execution. |

## Example 40. Bond repo margin call after haircut increase

### Scenario

A client funds a bond position through repo. The bond price falls and the repo counterparty increases the haircut, creating a cash margin call.

| Attribute | Before | After |
|---|---:|---:|
| Bond dirty value | 1,000,000 | 940,000 |
| Repo haircut | 5% | 8% |
| Repo cash advanced | 950,000 | 950,000 |

### Margin calculation

```text
new_max_repo_cash = 940,000 x (1 - 8%) = 864,800
margin_call = 950,000 - 864,800 = 85,200
```

### Correct treatment

| Area | Treatment |
|---|---|
| Cash | Margin call is a financing/collateral cash requirement, not bond investment loss by itself. |
| Position | Bond nominal remains unchanged unless collateral is liquidated or repo unwind occurs. |
| Risk | Track bond issuer risk, repo counterparty risk, haircut risk and liquidity risk separately. |
| Advisory | Explain leverage, forced-sale risk and cash buffer impact. |
| Reporting | Show bond value, repo liability, margin call, funding cost and net equity. |

### QA assertions

| Test | Expected result |
|---|---|
| Haircut increases | Repo availability recalculates and margin call opens. |
| Client has insufficient cash | Cure workflow, collateral substitution or unwind path opens. |
| Bond price recovers before cure | Margin call recalculates with source timestamp and counterparty rules. |
| Report shows only bond P&L | QA fails because financing margin call is hidden. |

## Example 41. Callable make-whole dispute

### Scenario

An issuer calls a bond using a make-whole formula. The custodian and issuer notice disagree on the make-whole spread input.

| Attribute | Issuer notice | Custodian calculation |
|---|---:|---:|
| Nominal | 500,000 | 500,000 |
| Make-whole price | 103.25 | 102.90 |
| Accrued interest per 100 | 1.10 | 1.10 |

### Cash difference

```text
issuer_total = 500,000 x (103.25 + 1.10) / 100 = 521,750
custodian_total = 500,000 x (102.90 + 1.10) / 100 = 520,000
dispute_amount = 1,750
```

### Correct treatment

- do not close the dispute by overwriting the make-whole input;
- preserve issuer notice, custodian calculation, source timestamp, curve/spread input and dispute owner;
- separate principal redemption, make-whole premium and accrued interest;
- label client-ready cash projection as disputed until authoritative confirmation arrives.

### QA assertions

| Test | Expected result |
|---|---|
| Make-whole sources disagree | Redemption cash is disputed or provisional. |
| Accrued interest agrees but premium differs | Accrued interest and make-whole premium remain separate. |
| Final cash settles at custodian amount | Difference is resolved with event lineage. |
| Historical projection is rerun | Uses the provisional/disputed state effective on that date. |

## Example 42. Benchmark-index bond removal

### Scenario

A bond is removed from a benchmark index after a downgrade below index eligibility criteria. A benchmark-aware mandate still holds the bond.

| Attribute | Value |
|---|---:|
| Portfolio bond market value | 750,000 |
| Portfolio market value | 15,000,000 |
| Prior benchmark weight | 1.20% |
| New benchmark weight | 0.00% |

### Active exposure

```text
portfolio_weight = 750,000 / 15,000,000 = 5.00%
active_weight_after_removal = 5.00% - 0.00% = 5.00%
```

### Correct treatment

- preserve benchmark effective date, removal reason, provider and source file;
- separate issuer downgrade, index removal and portfolio trading decision;
- recalculate active weight, tracking risk, mandate drift and turnover impact;
- avoid treating index removal as a bond sale or default event.

### QA assertions

| Test | Expected result |
|---|---|
| Benchmark provider removes bond | Benchmark constituent weight goes to zero from effective date. |
| Portfolio still holds bond | Active exposure increases and mandate review may open. |
| Index file is late | Benchmark-relative analytics are labelled stale/source-limited. |
| Historical attribution is rerun | Uses effective-dated benchmark membership. |

## Example 43. Post-default restructuring package

### Scenario

A defaulted bond restructures into a new note, cash recovery and equity warrants.

| Attribute | Value |
|---|---:|
| Old defaulted bond nominal | 1,000,000 |
| New note nominal received | 600,000 |
| New note clean price | 72.00 |
| Cash recovery | 80,000 |
| Warrant estimated value | 25,000 |

### Package value

```text
new_note_value = 600,000 x 72 / 100 = 432,000
package_value = 432,000 + 80,000 + 25,000 = 537,000
```

### Correct treatment

- close or reduce old defaulted bond only after restructuring effective date and source confirmation;
- create new instruments only after identifiers and terms are known;
- separate cash recovery, new debt value, equity/warrant value and realized loss treatment;
- preserve tax/accounting treatment and source lineage for each package component.

### QA assertions

| Test | Expected result |
|---|---|
| New note identifier is missing | New security position is not finalized. |
| Warrant value is estimated | Valuation is labelled estimated/source-limited. |
| Cash recovery settles later | Cash receivable remains pending until settlement confirmation. |
| Old bond remains in portfolio after effective restructuring | Residual defaulted position is flagged for reconciliation. |

## Example 44. Portfolio-level spread hedge overlay

### Scenario

A portfolio uses a credit index derivative to hedge spread exposure on a corporate bond sleeve.

| Attribute | Value |
|---|---:|
| Bond sleeve CS01 | 18,000 |
| Target hedge ratio | 60% |
| Credit index CS01 per contract | 450 |

### Hedge sizing

```text
target_hedge_cs01 = 18,000 x 60% = 10,800
contracts_needed = 10,800 / 450 = 24
```

### Correct treatment

- keep legal bond positions and derivative hedge positions separate;
- link hedge intent, hedge ratio, eligible sleeve and effective date;
- report gross bond spread exposure, hedge CS01 and residual CS01;
- separate bond carry and spread return from derivative hedge P&L.

### QA assertions

| Test | Expected result |
|---|---|
| Bond sleeve CS01 changes | Hedge target recalculates from current sensitivity. |
| Hedge link is missing | Derivative P&L remains separate and hedge report is incomplete. |
| Hedge exceeds mandate derivative limit | Order is blocked or escalated. |
| Credit index rolls | Old and new hedge exposure are measured through roll window. |

## Example 45. Odd-lot liquidity discount

### Scenario

A client holds a small odd-lot bond position. Market data provides an institutional round-lot price, but the executable quote for the small position is lower.

| Attribute | Value |
|---|---:|
| Nominal held | 75,000 |
| Round-lot mid price | 98.20 |
| Odd-lot executable bid | 97.10 |
| Price difference | 1.10 points |

### Valuation impact

```text
round_lot_value = 75,000 x 98.20 / 100 = 73,650
odd_lot_exit_value = 75,000 x 97.10 / 100 = 72,825
liquidity_discount = 825
```

### Correct treatment

- distinguish accounting valuation basis from executable liquidity estimate;
- label odd-lot liquidity discount as liquidity/execution analytics unless valuation policy adopts it;
- preserve quote size, source, timestamp and tradability state;
- avoid promising exit value from stale or non-firm quotes.

### QA assertions

| Test | Expected result |
|---|---|
| Position is below round-lot size | Liquidity analytics can apply odd-lot discount. |
| Executable quote is stale | Exit value is labelled stale or unavailable. |
| Client report uses mid price | Report should not imply same price is executable for odd lot. |
| Sale executes at odd-lot bid | Realized proceeds use actual execution, not prior mid valuation. |

## Example 46. Bond ETF creation/redemption look-through

### Scenario

A client holds a bond ETF that tracks an investment-grade credit index. The portfolio report needs to show fund-level valuation and liquidity, while risk analytics need issuer, duration and credit-spread look-through from the ETF basket.

| Attribute | Value |
|---|---:|
| ETF market value | 1,200,000 |
| Look-through coverage | 92% |
| ETF duration | 6.4 years |
| Basket-weighted spread duration | 6.1 years |
| Unlooked-through residual | 8% |

### Look-through exposure

```text
looked_through_value = 1,200,000 x 92% = 1,104,000
residual_unclassified_value = 1,200,000 x 8% = 96,000
```

### Correct treatment

- keep the legal holding as the ETF or fund position;
- use look-through only for analytics, concentration, credit quality, duration and stress reporting;
- preserve basket effective date, provider, coverage percentage and residual classification;
- do not create direct bond accounting positions from analytical look-through;
- label stale or partial baskets so risk users know the quality of the exposure view.

### QA assertions

| Test | Expected result |
|---|---|
| ETF basket coverage is below 100% | Residual unclassified exposure is visible. |
| Basket file is stale | Look-through analytics are labelled stale/source-limited. |
| ETF is redeemed in primary market | Accounting closes the ETF holding, not each constituent bond. |
| Issuer concentration report runs | Uses source-backed constituent weights and shows coverage percentage. |

## Example 47. Inflation-index restatement dispute

### Scenario

An inflation-linked bond uses a published index ratio. The index provider later restates the inflation index for an earlier valuation date, but the custodian has not yet corrected accrued principal and coupon calculations.

| Attribute | Original | Restated |
|---|---:|---:|
| Original nominal | 1,000,000 | 1,000,000 |
| Base index | 250.0000 | 250.0000 |
| Current index | 285.0000 | 284.2000 |
| Clean price | 101.20 | 101.20 |

### Principal impact

```text
original_adjusted_principal = 1,000,000 x 285.0000 / 250.0000 = 1,140,000
restated_adjusted_principal = 1,000,000 x 284.2000 / 250.0000 = 1,136,800
adjusted_principal_delta = -3,200
```

### Correct treatment

- preserve original and restated index values with provider, effective date and publication timestamp;
- keep valuation and income restatement provisional until custodian/accounting confirmation arrives;
- recalculate adjusted principal, real coupon base, clean/dirty value and inflation contribution;
- do not book a cash movement unless a coupon, redemption or correction cashflow is confirmed.

### QA assertions

| Test | Expected result |
|---|---|
| Index provider restates value | Historical adjusted principal is versioned with reason. |
| Custodian has not corrected accounting | Report labels valuation as restatement pending or source-limited. |
| Coupon date falls in affected period | Coupon accrual and received cash reconciliation are reviewed. |
| Performance attribution is rerun | Inflation contribution delta is separated from market-price movement. |

## Example 48. Sovereign sanctions restriction

### Scenario

A sovereign bond is still priced by market vendors, but new sanctions restrict trading, transfer and coupon processing for certain client types and booking centres.

| Attribute | Value |
|---|---:|
| Bond market value | 640,000 |
| Vendor price | 80.00 |
| Tradable amount today | 0 |
| Coupon due next month | 18,000 |
| Restriction status | Trading and transfer blocked |

### Correct treatment

| Area | Treatment |
|---|---|
| Valuation | Continue valuation only under approved policy and price hierarchy. |
| Trading | Block buys, sells, transfers or collateral use unless policy explicitly permits. |
| Income | Treat coupon projection as restricted or uncertain until paying-agent/custodian confirmation. |
| Advisory | Surface suitability, restriction and liquidity flags before any recommendation. |
| Reporting | Separate market value, restricted liquidity and blocked activity explanation. |
| Operations | Preserve sanctions source, effective date, client-scope rule and exception approvals. |

### QA assertions

| Test | Expected result |
|---|---|
| Vendor price exists but sanctions block trading | Position may be valued but is not tradable. |
| Coupon projection is generated | Coupon is labelled restricted/uncertain until processing is confirmed. |
| Advisor attempts sale order | Order is blocked before release. |
| Restriction changes by booking centre | Eligibility is evaluated by client, account, booking centre and effective date. |

## Example 49. Collateralized bond lending recall

### Scenario

A bond held in a discretionary portfolio is out on securities lending. The portfolio manager needs to sell the bond to reduce credit exposure, but the bond must be recalled before settlement.

| Attribute | Value |
|---|---:|
| Bond nominal held | 1,000,000 |
| Nominal on loan | 700,000 |
| Available nominal before recall | 300,000 |
| Sale order target nominal | 800,000 |
| Recall settlement estimate | T+2 |

### Sellable amount

```text
current_sellable_nominal = 1,000,000 - 700,000 = 300,000
recall_needed = 800,000 - 300,000 = 500,000
```

### Correct treatment

- distinguish held nominal, lent nominal, recalled nominal and sellable nominal;
- prevent sale settlement failure by linking order release to recall confirmation or partial execution plan;
- preserve lending counterparty, collateral, recall instruction, expected return date and fail state;
- report lending revenue separately from bond investment income and sale proceeds.

### QA assertions

| Test | Expected result |
|---|---|
| Sale target exceeds available nominal | Recall workflow opens before full order release. |
| Recall fails or returns late | Sale is resized, delayed or escalated for buy-in risk. |
| Only partial recall succeeds | Available sellable nominal updates and residual order remains controlled. |
| Client report is generated | Lending state and restricted sellability are explainable. |

## Example 50. Primary allocation scaling

### Scenario

A client subscribes for a new issue bond in the primary market. Demand is oversubscribed and the syndicate scales allocations. Cash reservation must be released for the unallocated amount.

| Attribute | Value |
|---|---:|
| Client order nominal | 1,000,000 |
| Issue price | 99.50 |
| Allocation percentage | 40% |
| Final allocated nominal | 400,000 |

### Cash reservation

```text
original_cash_reserved = 1,000,000 x 99.50 / 100 = 995,000
final_cash_needed = 400,000 x 99.50 / 100 = 398,000
cash_to_release = 995,000 - 398,000 = 597,000
```

### Correct treatment

- reserve cash or buying power for the submitted order according to policy;
- create final bond position only for allocated nominal after allocation confirmation;
- release excess reservation promptly with reason code and timestamp;
- preserve order, allocation file, price, settlement date, issuer, syndicate and suitability evidence;
- do not treat unallocated nominal as a cancellation failure or missed trade.

### QA assertions

| Test | Expected result |
|---|---|
| Allocation is scaled | Final position and settlement cash use allocated nominal only. |
| Cash reservation exceeds final need | Excess cash is released with allocation lineage. |
| Allocation file arrives late | Order remains pending/allocation-limited. |
| Multiple clients are allocated | Allocation and fairness evidence is retained by account/order. |

## Example 51. Distressed valuation committee override

### Scenario

A distressed bond has no reliable executable market quote. A valuation committee approves an override price for reporting while workout and restructuring negotiations continue.

| Attribute | Value |
|---|---:|
| Nominal held | 2,000,000 |
| Last vendor price | 38.00 |
| Approved committee price | 31.50 |
| Override expiry | 2026-07-31 |

### Valuation impact

```text
vendor_value = 2,000,000 x 38.00 / 100 = 760,000
committee_value = 2,000,000 x 31.50 / 100 = 630,000
override_valuation_delta = -130,000
```

### Correct treatment

- preserve vendor price, override price, committee approval, rationale, expiry and approvers;
- mark valuation as committee-approved, not market-executable;
- require review or expiry handling so stale overrides do not become permanent;
- separate valuation override from default, restructuring, write-down or realized loss events.

### QA assertions

| Test | Expected result |
|---|---|
| Approved override exists | Reporting valuation uses override with source and expiry. |
| Override expires | Valuation returns to policy fallback or blocks final reporting until reviewed. |
| Executable quote appears | Valuation hierarchy re-evaluates whether override remains appropriate. |
| Restructuring event later occurs | Accounting lifecycle event is processed separately from valuation override. |

## Example 52. Floating-rate fallback transition

### Scenario

A floating-rate note references a discontinued benchmark. The fallback waterfall moves the coupon index from the old benchmark to a replacement overnight rate plus a contractual spread adjustment.

| Attribute | Old basis | Fallback basis |
|---|---:|---:|
| Nominal | 1,000,000 | 1,000,000 |
| Old benchmark fixing | 4.80% | n/a |
| Replacement rate | n/a | 4.55% |
| Spread adjustment | n/a | 0.26% |
| Bond margin | 1.20% | 1.20% |
| Coupon period fraction | 0.25 | 0.25 |

### Coupon comparison

```text
old_coupon_rate = 4.80% + 1.20% = 6.00%
fallback_coupon_rate = 4.55% + 0.26% + 1.20% = 6.01%
fallback_coupon_amount = 1,000,000 x 6.01% x 0.25 = 15,025
```

### Correct treatment

- preserve the old benchmark, fallback trigger, replacement rate, spread adjustment and effective date;
- recalculate future coupons from the fallback basis without rewriting historical coupon events;
- keep client reporting explicit about benchmark transition and source evidence;
- do not apply fallback automatically when the instrument has bespoke fallback terms requiring issuer/custodian confirmation.

### QA assertions

| Test | Expected result |
|---|---|
| Benchmark discontinuation trigger is effective | Future coupon projections use fallback basis. |
| Spread adjustment is missing | Coupon projection is blocked or labelled incomplete. |
| Historical coupon period is before transition | Historical coupon remains on original fixing. |
| Custodian coupon differs from internal projection | Difference enters coupon reconciliation, not silent overwrite. |

## Example 53. Callable tender reoffer after issuer liability-management exercise

### Scenario

An issuer launches a tender offer for an existing callable bond and reoffers a new bond. The client tenders part of the position and retains the rest.

| Attribute | Value |
|---|---:|
| Existing nominal held | 1,500,000 |
| Tendered nominal | 1,000,000 |
| Accepted tender percentage | 65% |
| Tender price | 102.25 |
| New reoffer nominal subscribed | 500,000 |
| Reoffer price | 99.75 |

### Tender and reoffer cash

```text
accepted_tender_nominal = 1,000,000 x 65% = 650,000
tender_cash = 650,000 x 102.25 / 100 = 664,625
remaining_old_nominal = 1,500,000 - 650,000 = 850,000
new_bond_settlement = 500,000 x 99.75 / 100 = 498,750
net_cash_after_reoffer = 664,625 - 498,750 = 165,875
```

### Correct treatment

- process accepted tender, residual old bond and new reoffer bond as separate lifecycle events;
- preserve tender instruction, acceptance ratio, old identifier, new identifier and settlement dates;
- do not assume all tendered nominal is accepted;
- evaluate suitability and mandate controls for the new reoffer bond, not only the old holding.

### QA assertions

| Test | Expected result |
|---|---|
| Tender is partially accepted | Old position reduces only by accepted nominal. |
| Reoffer bond has a new identifier | New holding is created separately with its own terms. |
| Acceptance file arrives late | Tender result remains pending/source-limited. |
| Reoffer suitability differs | New subscription requires product and mandate checks. |

## Example 54. Cross-currency bond hedge effectiveness

### Scenario

A reporting-currency portfolio holds a EUR bond and uses an FX forward to hedge currency exposure. Portfolio reporting needs to separate local bond return, FX translation and hedge offset.

| Component | Amount |
|---|---:|
| Opening EUR bond value | 1,000,000 |
| Closing EUR bond value | 1,012,000 |
| Opening EUR/USD | 1.0800 |
| Closing EUR/USD | 1.0500 |
| FX forward hedge P&L in USD | 27,500 |

### Attribution

```text
opening_usd_value = 1,000,000 x 1.0800 = 1,080,000
closing_usd_unhedged_value = 1,012,000 x 1.0500 = 1,062,600
unhedged_usd_result = 1,062,600 - 1,080,000 = -17,400
hedged_usd_result = -17,400 + 27,500 = 10,100
```

### Correct treatment

- keep the legal bond holding and FX forward as separate positions;
- show local bond performance, FX translation and hedge P&L as explainable components;
- require source-backed hedge link before presenting a combined hedged result;
- preserve hedge ratio, hedge designation, notional, maturity and rollover assumptions.

### QA assertions

| Test | Expected result |
|---|---|
| Hedge link is source-backed | Hedged result can be shown with bond and forward components. |
| Hedge link is missing | Bond and FX forward P&L remain separate. |
| Forward notional differs from bond value | Residual unhedged FX exposure is visible. |
| Hedge rolls mid-period | Attribution splits by old and new forward periods. |

## Example 55. Collateral eligibility downgrade for pledged bonds

### Scenario

A Lombard facility accepts investment-grade bonds as collateral. A pledged bond is downgraded, reducing its collateral eligibility and triggering a facility headroom shortfall.

| Attribute | Before downgrade | After downgrade |
|---|---:|---:|
| Bond market value | 900,000 | 875,000 |
| Collateral haircut | 20% | 45% |
| Facility exposure | 600,000 | 600,000 |

### Headroom impact

```text
eligible_value_before = 900,000 x (1 - 20%) = 720,000
eligible_value_after = 875,000 x (1 - 45%) = 481,250
collateral_shortfall = 600,000 - 481,250 = 118,750
```

### Correct treatment

- update collateral eligibility from rating, product, issuer and facility policy evidence;
- separate bond valuation movement from collateral-policy haircut change;
- trigger margin call or cure workflow when facility exposure exceeds eligible collateral value;
- preserve rating source, effective date, haircut policy and client notification state.

### QA assertions

| Test | Expected result |
|---|---|
| Rating downgrade changes haircut | Eligible collateral value recalculates from effective date. |
| Market value also changes | Valuation and haircut effects are separately explainable. |
| Facility exposure exceeds eligible value | Margin call or cure workflow opens. |
| Rating source is stale | Collateral eligibility is labelled stale or blocked by policy. |

## Example 56. Bond option-adjusted spread model change

### Scenario

Risk analytics upgrades the callable-bond option-adjusted spread model. The market price is unchanged, but OAS, duration and scenario analytics shift because the model assumptions changed.

| Attribute | Old model | New model |
|---|---:|---:|
| Market price | 101.40 | 101.40 |
| OAS | 165 bps | 142 bps |
| Effective duration | 4.8 years | 4.3 years |
| Model version | v1 | v2 |

### Analytics change

```text
oas_delta = 142 - 165 = -23 bps
duration_delta = 4.3 - 4.8 = -0.5 years
```

### Correct treatment

- keep market valuation stable when price source is unchanged;
- version OAS, effective duration and scenario outputs by analytics model;
- explain model-driven analytics changes separately from market-price movement;
- require model approval, effective date and backtesting evidence for production reporting.

### QA assertions

| Test | Expected result |
|---|---|
| Model version changes but price does not | Valuation remains unchanged while analytics are versioned. |
| Client report includes OAS | Report carries model version or methodology date. |
| Old and new analytics are compared | OAS and duration deltas are explainable as model-driven. |
| Model approval is missing | Analytics remain draft or excluded from final reporting. |

## Example 57. Tax-lot portability after custody transfer

### Scenario

A bond position transfers from one custodian to another. The receiving platform has nominal and market value, but tax lots and accrued-interest history arrive later.

| Lot | Nominal | Clean cost | Accrued paid | Status |
|---|---:|---:|---:|---|
| Lot A | 400,000 | 98.50 | 4,200 | Received |
| Lot B | 600,000 | 101.20 | 6,100 | Pending |

### Portable cost basis

```text
received_clean_cost = 400,000 x 98.50 / 100 = 394,000
pending_lot_nominal = 600,000
known_accrued_paid = 4,200
pending_accrued_paid = 6,100
```

### Correct treatment

- transfer the accounting position only when custody position is confirmed;
- label tax-lot, accrued-interest and realized-gain reporting as incomplete until lot history is received;
- preserve old custodian lot ids, acquisition dates, cost basis, accrued paid and transfer date;
- do not collapse multiple lots into a single average-cost lot unless policy and jurisdiction permit.

### QA assertions

| Test | Expected result |
|---|---|
| Position transfer arrives before lots | Holding is visible but tax-lot reporting is incomplete. |
| Partial lot history is received | Known lots are versioned and pending lots remain exceptioned. |
| Bond is sold before lot history is complete | Realized gain/loss is provisional or blocked according to policy. |
| Final lot file arrives | Tax lots reconcile to transferred nominal and prior custodian evidence. |

## Example 58. Convertible hedge-accounting designation

### Scenario

A portfolio holds a convertible bond and uses an equity hedge to reduce embedded equity exposure. The accounting team wants to designate the hedge relationship from the start of the next reporting period. The platform must keep economic exposure, hedge attribution and hedge-accounting evidence separate.

| Attribute | Value |
|---|---:|
| Convertible nominal | 1,000,000 |
| Conversion price | 50.00 |
| Underlying share price | 62.00 |
| Convertible delta | 0.62 |
| Equity hedge shares | 12,000 |

### Hedge coverage

```text
conversion_shares = 1,000,000 / 50.00 = 20,000
delta_equivalent_shares = 20,000 x 0.62 = 12,400
hedge_coverage = 12,000 / 12,400 = 96.77%
```

### Correct treatment

- preserve convertible terms, conversion ratio, delta source, hedge instrument, hedge ratio and designation effective date;
- show economic hedge coverage even before formal hedge-accounting designation is approved;
- do not backdate hedge-accounting treatment before required documentation and effectiveness criteria are met;
- separate bond income, credit spread movement, embedded equity exposure and hedge P&L in reporting.

### QA assertions

| Test | Expected result |
|---|---|
| Hedge documentation is approved | Hedge-accounting designation becomes effective from approved date. |
| Hedge documentation is missing | Economic hedge analytics may show, but hedge-accounting label is blocked. |
| Convertible delta changes | Hedge coverage recalculates from the approved analytics source. |
| Hedge shares exceed policy tolerance | Exception workflow opens for rebalance or redesignation review. |

## Example 59. Municipal call refunding

### Scenario

A municipal issuer calls an outstanding bond after issuing a lower-coupon refunding bond. The client receives call proceeds and may reinvest, but the old bond's income, tax-equivalent yield and maturity profile must close from the call date.

| Attribute | Value |
|---|---:|
| Called nominal | 500,000 |
| Call price | 101.25 |
| Accrued interest | 4,800 |
| Old coupon | 4.50% |
| Replacement yield | 3.35% |

### Call proceeds

```text
principal_call_cash = 500,000 x 101.25 / 100 = 506,250
total_call_cash = 506,250 + 4,800 = 511,050
annual_coupon_income_lost = 500,000 x 4.50% = 22,500
```

### Correct treatment

- process the call as issuer redemption of the old identifier, not as a market sale;
- separate call premium, accrued interest and principal redemption in transactions and reporting;
- update maturity ladder, income projection and tax-equivalent yield from the call date;
- treat any replacement bond as a new suitability, tax and mandate decision.

### QA assertions

| Test | Expected result |
|---|---|
| Call notice is confirmed | Old bond position closes on call date with principal, premium and accrued interest. |
| Replacement bond is subscribed | New holding is created with its own identifier and terms. |
| Call notice is pending | Maturity ladder and income projection are labelled provisional. |
| Client report is generated | Report explains redemption and reinvestment separately. |

## Example 60. Sinking-fund lottery allocation

### Scenario

A sinking-fund bond redeems part of outstanding principal through a lottery. The client holds several lots, but only selected certificates are redeemed. The platform must reduce the selected nominal and keep unrecalled lots intact.

| Lot | Held nominal | Lottery selected | Redemption price |
|---|---:|---:|---:|
| Lot A | 300,000 | 100,000 | 100.50 |
| Lot B | 400,000 | 0 | 100.50 |
| Lot C | 300,000 | 150,000 | 100.50 |

### Redemption amount

```text
selected_nominal = 100,000 + 150,000 = 250,000
redemption_cash = 250,000 x 100.50 / 100 = 251,250
remaining_nominal = 1,000,000 - 250,000 = 750,000
```

### Correct treatment

- reduce only lottery-selected nominal and lots when source evidence identifies them;
- preserve lottery notice, selected certificates or lots, redemption price, accrued interest and effective date;
- do not apply a simple pro-rata reduction when the event is certificate-specific;
- update future coupon accrual and maturity ladder from remaining nominal.

### QA assertions

| Test | Expected result |
|---|---|
| Lottery-selected lots are supplied | Selected lots reduce by confirmed nominal only. |
| Selection file is missing | Redemption remains source-limited instead of estimated pro rata. |
| Coupon accrues after redemption | Accrual uses remaining nominal from redemption date. |
| Tax-lot report is generated | Redeemed and remaining lots remain traceable. |

## Example 61. Covered-bond rating watch trigger

### Scenario

A covered bond remains investment grade, but the cover pool is placed on negative watch. Collateral policy reduces eligibility until the watch is resolved. The bond valuation may stay stable while collateral and concentration treatment change.

| Attribute | Before watch | After watch |
|---|---:|---:|
| Market value | 1,200,000 | 1,195,000 |
| Collateral haircut | 18% | 30% |
| Facility exposure allocated | 850,000 | 850,000 |

### Eligible collateral value

```text
eligible_value_before = 1,200,000 x (1 - 18%) = 984,000
eligible_value_after = 1,195,000 x (1 - 30%) = 836,500
coverage_gap = 850,000 - 836,500 = 13,500
```

### Correct treatment

- preserve rating-watch notice, cover-pool source, haircut policy, facility id and effective date;
- separate market value movement from collateral eligibility change;
- trigger cure, collateral review or facility exception only when policy threshold is breached;
- keep client reporting explicit that rating watch is not the same as default or issuer call.

### QA assertions

| Test | Expected result |
|---|---|
| Rating watch changes collateral policy | Eligible collateral value recalculates from effective date. |
| Market price remains stable | Valuation stays source-priced while collateral treatment changes. |
| Facility exposure exceeds eligible value | Cure or exception workflow opens. |
| Watch is removed | Haircut treatment reverts only from source-confirmed effective date. |

## Example 62. Callable-bond scenario ladder

### Scenario

An advisor reviews a callable bond where the issuer may call in year 2, call in year 4 or leave the bond to maturity. Reporting should show scenario cashflows and yield-to-worst without implying that the worst case is a forecast.

| Scenario | Redemption date | Redemption price | Scenario yield |
|---|---|---:|---:|
| First call | Year 2 | 101.00 | 3.20% |
| Second call | Year 4 | 100.50 | 3.55% |
| Final maturity | Year 7 | 100.00 | 4.10% |

### Yield ladder

```text
yield_to_worst = min(3.20%, 3.55%, 4.10%) = 3.20%
yield_to_best = max(3.20%, 3.55%, 4.10%) = 4.10%
```

### Correct treatment

- calculate scenario yields from confirmed call schedule, price, coupon and settlement date;
- label yield-to-worst as scenario metric, not expected return;
- preserve the full scenario ladder for advisory explanation and suitability review;
- update projected cashflows when issuer notice or market price changes.

### QA assertions

| Test | Expected result |
|---|---|
| Call schedule is complete | Scenario ladder shows each valid call and maturity case. |
| Call schedule is missing | Yield-to-worst is blocked or labelled incomplete. |
| Issuer gives call notice | Projection switches from scenario to confirmed lifecycle event. |
| Client report is generated | Report distinguishes yield-to-worst, yield-to-maturity and confirmed call state. |

## Example 63. Bond fund in-kind redemption impact

### Scenario

A bond fund processes a large redemption partly in kind. The client gives up fund units and receives a basket of individual bonds plus residual cash. The legal holding, liquidity, valuation source and analytics treatment change after settlement.

| Component | Value |
|---|---:|
| Redeemed fund value | 2,000,000 |
| Bond basket value | 1,850,000 |
| Residual cash | 140,000 |
| Transaction costs | 10,000 |

### Settlement value

```text
net_received_value = 1,850,000 + 140,000 - 10,000 = 1,980,000
implementation_shortfall = 2,000,000 - 1,980,000 = 20,000
```

### Correct treatment

- close or reduce fund units based on confirmed redemption terms and settlement date;
- create individual bond positions only from confirmed in-kind basket identifiers, quantities and prices;
- separate residual cash, transaction costs and any implementation shortfall from ordinary fund performance;
- update look-through, issuer concentration, liquidity, yield, duration and tax-lot treatment after basket settlement.

### QA assertions

| Test | Expected result |
|---|---|
| In-kind basket is confirmed | Individual bond holdings are created with source identifiers and quantities. |
| Basket file is missing | Redemption remains pending or source-limited. |
| Basket contains restricted bonds | Advisory, mandate and reporting restrictions are evaluated after settlement. |
| Performance report is generated | Fund redemption, received bonds, residual cash and costs are explainable separately. |

## Example 64. Fallen-angel index migration

### Scenario

A corporate bond is downgraded from investment grade to high yield. The client still holds the bond, but the benchmark provider removes it from the investment-grade index after a transition window. Portfolio analytics must separate holding risk from benchmark migration.

| Attribute | Before migration | After migration |
|---|---:|---:|
| Portfolio market value | 1,000,000 | 960,000 |
| Portfolio weight | 2.00% | 1.92% |
| Benchmark weight | 1.50% | 0.00% |
| Active weight | 0.50% | 1.92% |

### Active weight impact

```text
active_weight_before = 2.00% - 1.50% = 0.50%
active_weight_after = 1.92% - 0.00% = 1.92%
active_weight_change = 1.92% - 0.50% = 1.42%
```

### Correct treatment

- retain the bond holding and tax lots until sold, matured, exchanged or otherwise processed;
- update benchmark-relative analytics from the benchmark provider effective date;
- route advisory and mandate checks when the bond becomes high-yield or outside approved universe;
- separate price loss, credit migration, benchmark removal and realized sale decision;
- preserve rating notice, index file, effective date and product-governance action.

### QA assertions

| Test | Expected result |
|---|---|
| Rating downgrade occurs | Credit classification and mandate controls update from effective source date. |
| Benchmark removes the bond | Benchmark weight becomes zero while client holding remains. |
| Portfolio report is generated | Active weight explains index migration separately from market value change. |
| Bond is sold after removal | Sale is processed as client transaction, not automatic index event. |

## Example 65. Callable notice timing dispute

### Scenario

An issuer call notice is received by the custodian after the platform's income projection cut-off. The issuer states the call is effective, while the custodian confirmation arrives late. The platform must avoid prematurely closing the position while still flagging the likely call.

| Attribute | Value |
|---|---|
| Held nominal | 750,000 |
| Call price | 100.75 |
| Issuer notice timestamp | 2026-05-02 17:20 |
| Custodian confirmation timestamp | 2026-05-03 09:15 |
| Projection cut-off | 2026-05-02 16:00 |

### Expected call cash

```text
expected_principal_cash = 750,000 x 100.75 / 100 = 755,625
```

### Correct treatment

- create a pending call event from issuer notice when policy allows, but do not close the position before required confirmation;
- label cashflow projection as pending, estimated or source-limited until confirmation arrives;
- preserve issuer notice, custodian confirmation, notice timestamp, cut-off and source priority;
- update maturity ladder, yield and income projection when the event becomes confirmed;
- track any client communication sent before final confirmation.

### QA assertions

| Test | Expected result |
|---|---|
| Issuer notice arrives after projection cut-off | Projection is flagged pending or updated in next cycle according to policy. |
| Custodian confirmation is missing | Position remains open and call cash is not booked as settled. |
| Custodian confirms next day | Lifecycle event is confirmed with source lineage. |
| Client report is generated during dispute | Report distinguishes expected call from confirmed redemption. |

## Example 66. Bond tender proration dispute

### Scenario

A client tenders 1,000,000 nominal into a liability-management offer. The issuer accepts only 62% because the offer is oversubscribed. The client disputes the accepted amount because an initial broker estimate showed 75%.

| Attribute | Value |
|---|---:|
| Tendered nominal | 1,000,000 |
| Broker estimated acceptance | 75.00% |
| Final accepted percentage | 62.00% |
| Tender price | 102.25 |
| Accrued interest on accepted amount | 8,500 |

### Accepted proceeds

```text
accepted_nominal = 1,000,000 x 62.00% = 620,000
principal_cash = 620,000 x 102.25 / 100 = 633,950
total_cash = 633,950 + 8,500 = 642,450
residual_nominal = 1,000,000 - 620,000 = 380,000
```

### Correct treatment

- reduce the position only by final accepted nominal, not tendered nominal or preliminary broker estimate;
- preserve tender instruction, preliminary estimate, final acceptance file and dispute state;
- keep residual nominal in the portfolio with unchanged terms unless the offer changes them;
- separate principal consideration, tender premium and accrued interest;
- reflect dispute as operations/client-service workflow, not as a price correction.

### QA assertions

| Test | Expected result |
|---|---|
| Final acceptance is lower than estimate | Position reduces by final accepted nominal only. |
| Estimate and final file conflict | Final issuer/custodian source wins according to source hierarchy. |
| Client disputes proration | Dispute workflow opens without changing accepted amount. |
| Report is generated | Accepted, residual and disputed quantities are explainable. |

## Example 67. Inflation-linked deflation floor

### Scenario

An inflation-linked bond has a principal floor at par. The index ratio falls below 1.00 during a deflation period. Valuation and projected redemption must apply the floor only if the bond terms support it.

| Attribute | Value |
|---|---:|
| Original nominal | 1,000,000 |
| Current index ratio | 0.9820 |
| Unfloored indexed principal | 982,000 |
| Principal floor | 1,000,000 |

### Floor calculation

```text
unfloored_principal = 1,000,000 x 0.9820 = 982,000
floor_protected_principal = max(982,000, 1,000,000) = 1,000,000
floor_benefit = 1,000,000 - 982,000 = 18,000
```

### Correct treatment

- apply the floor only when confirmed in bond terms and applicable to the relevant cashflow;
- keep market valuation, accrued inflation uplift and redemption projection separately explainable;
- source index ratios, base index, floor terms and publication timestamp;
- label analytics stale when index source is missing or restated;
- avoid applying a generic inflation-linked floor to instruments where terms allow negative indexation.

### QA assertions

| Test | Expected result |
|---|---|
| Index ratio falls below 1.00 and floor exists | Redemption projection uses floored principal. |
| Floor term is missing | Final projection is blocked or labelled source-limited. |
| Index is restated | Floor benefit and attribution are versioned from restated source. |
| Market price changes | Price performance remains separate from floor calculation. |

## Example 68. ESG covenant step-up

### Scenario

A sustainability-linked bond pays a coupon step-up if the issuer misses a verified ESG target. The target failure is confirmed after the coupon projection cycle, requiring future accruals and report labels to update.

| Attribute | Value |
|---|---:|
| Nominal | 2,000,000 |
| Base coupon | 3.10% |
| ESG step-up | 0.25% |
| Revised coupon | 3.35% |
| Annual coupon increase | 5,000 |

### Step-up impact

```text
revised_coupon = 3.10% + 0.25% = 3.35%
annual_coupon_increase = 2,000,000 x 0.25% = 5,000
```

### Correct treatment

- update coupon schedule only after source-confirmed target assessment and effective date;
- preserve verifier report, issuer notice, covenant terms and step-up calculation;
- label the bond as sustainability-linked only when source terms support the classification;
- separate coupon income change from credit spread or price movement;
- update advisory and product-governance views when ESG characteristics change or targets fail.

### QA assertions

| Test | Expected result |
|---|---|
| ESG target failure is confirmed | Future coupon accrual uses stepped-up coupon from effective date. |
| Verifier report is missing | Step-up is pending or source-limited. |
| Coupon projection was already published | Correction or next-cycle update is recorded with lineage. |
| Client report is generated | Report explains coupon step-up and ESG target failure separately from market return. |

## Example 69. Bond option exercise error

### Scenario

A putable bond allows holders to exercise a put at 100.00 on a specified exercise date. An operations user accidentally submits exercise for the full position even though the client approved only half. The platform must prevent or repair over-exercise before settlement.

| Attribute | Value |
|---|---:|
| Held nominal | 600,000 |
| Client-approved exercise nominal | 300,000 |
| Erroneous submitted nominal | 600,000 |
| Put price | 100.00 |
| Over-exercised nominal | 300,000 |

### Error amount

```text
over_exercised_nominal = submitted_nominal - approved_nominal
over_exercised_nominal = 600,000 - 300,000 = 300,000
overstated_put_cash = 300,000 x 100.00 / 100 = 300,000
```

### Correct treatment

- validate exercise instruction against client approval, eligible nominal, deadline and product terms;
- block or repair submitted nominal that exceeds approved nominal;
- keep approved exercise, erroneous instruction, cancellation/repair and final settlement linked;
- reduce position only by confirmed exercised nominal;
- preserve client approval, option notice, submission timestamp and custodian response.

### QA assertions

| Test | Expected result |
|---|---|
| Submitted exercise exceeds approval | Workflow blocks or routes to repair before settlement. |
| Deadline passes before repair | Exception state records missed correction and residual risk. |
| Custodian accepts corrected instruction | Position reduces only by corrected exercised nominal. |
| Client report is generated | Approved exercise, repair and final position are traceable. |

## Example 70. Callable put-window miss

### Scenario

A putable corporate bond allows investors to put the bond back to the issuer during a 10-business-day election window. The client intended to exercise the put, but the instruction was submitted after the custodian cut-off. The platform must preserve the missed-window evidence and keep the bond position open.

| Attribute | Value |
|---|---:|
| Held nominal | 850,000 |
| Put price | 100.00 |
| Client-approved exercise nominal | 850,000 |
| Custodian cut-off | 15:00 |
| Submitted time | 16:10 |

### Missed put cash

```text
expected_put_cash_if_accepted = 850,000 x 100.00 / 100 = 850,000
missed_cutoff_minutes = 70
```

### Correct treatment

- keep the bond position open because no accepted exercise occurred;
- preserve client approval, submission timestamp, custodian rejection and cut-off source;
- show missed optionality as operations/advisory impact, not as maturity or issuer default;
- update yield, maturity ladder and liquidity expectations to reflect continued holding;
- route any compensation or complaint workflow separately from bond lifecycle accounting.

### QA assertions

| Test | Expected result |
|---|---|
| Instruction is submitted after cut-off | Exercise is rejected or exceptioned with missed-window reason. |
| Client approved before cut-off but operations submitted late | Complaint or operational review workflow opens without closing the bond. |
| Report is generated | Bond remains held and put cash is not booked. |
| Future call/put schedule is displayed | Missed window is historical; future valid windows remain available if terms allow. |

## Example 71. Exchange-offer lock-up period

### Scenario

An issuer offers to exchange an old bond into a new longer-dated bond. Tendering holders are locked up after instruction submission and cannot sell the old bond while the exchange offer is pending.

| Attribute | Value |
|---|---:|
| Old bond nominal submitted | 1,200,000 |
| Exchange ratio | 0.92 |
| Expected new bond nominal | 1,104,000 |
| Lock-up start | Election submission |
| Final exchange confirmation | Pending |

### Expected exchange nominal

```text
expected_new_nominal = old_nominal_submitted x exchange_ratio
expected_new_nominal = 1,200,000 x 0.92 = 1,104,000
```

### Correct treatment

- restrict trading, lending recall release and collateral substitution for the submitted old nominal during lock-up;
- do not create the new bond position before final source confirmation and identifier availability;
- keep residual or rejected old nominal visible if the exchange is prorated or rejected;
- preserve offer circular, election instruction, lock-up period, final acceptance and new identifier lineage;
- label exposure as pending exchange, not as settled conversion.

### QA assertions

| Test | Expected result |
|---|---|
| Sell order is attempted during lock-up | Order is blocked or routed to exception. |
| Exchange confirmation is pending | New bond is not created as settled holding. |
| Final exchange ratio changes | Expected new nominal is recalculated with source lineage. |
| Offer is rejected or prorated | Old bond residual remains visible and tradability updates. |

## Example 72. Bond coupon claim dispute

### Scenario

A coupon is due on a bond held over record date, but the custodian does not credit the expected cash. Operations raises a coupon claim against the custodian or paying agent.

| Attribute | Value |
|---|---:|
| Eligible nominal | 2,500,000 |
| Annual coupon rate | 4.20% |
| Coupon frequency | Semi-annual |
| Expected coupon | 52,500 |
| Cash received | 0 |

### Claim amount

```text
expected_coupon = 2,500,000 x 4.20% / 2 = 52,500
coupon_claim_amount = 52,500 - 0 = 52,500
```

### Correct treatment

- create coupon receivable or claim workflow only when entitlement is source-supported;
- preserve record date, ex-date, pay date, position quantity, custodian statement and paying-agent status;
- do not mark the coupon as received until cash is credited or claim is settled;
- separate income accrual, receivable, claim state and actual cash;
- update client reports with pending income claim where material.

### QA assertions

| Test | Expected result |
|---|---|
| Coupon is expected but cash is missing | Claim workflow opens with entitlement evidence. |
| Custodian later credits cash | Receivable/claim closes against confirmed cash. |
| Entitlement is disputed | Income remains pending or source-limited. |
| Client report is generated | Coupon is labelled receivable or under claim, not paid cash. |

## Example 73. Index fast-entry inclusion

### Scenario

A newly issued bond is added to a benchmark through fast-entry rules before the normal monthly rebalance. Portfolio analytics must update benchmark exposure and active weights from the provider effective date.

| Attribute | Before fast entry | After fast entry |
|---|---:|---:|
| Portfolio market value | 0 | 0 |
| Benchmark weight | 0.00% | 0.65% |
| Portfolio weight | 0.00% | 0.00% |
| Active weight | 0.00% | -0.65% |

### Active weight

```text
active_weight_after = portfolio_weight_after - benchmark_weight_after
active_weight_after = 0.00% - 0.65% = -0.65%
```

### Correct treatment

- update benchmark composition from provider file and effective date;
- do not create a client holding merely because the bond enters the index;
- route portfolio manager/advisory review when benchmark-relative underweight becomes material;
- preserve issuer, identifier, index file, fast-entry rule and effective timestamp;
- explain active-risk change separately from market performance.

### QA assertions

| Test | Expected result |
|---|---|
| Benchmark provider adds bond by fast entry | Benchmark constituent appears from effective date. |
| Portfolio does not hold the bond | Active weight becomes negative without creating a position. |
| Identifier is missing or pending | Benchmark exposure is source-limited until instrument master resolves. |
| Report is generated | Underweight is explained as index inclusion, not sale activity. |

## Example 74. Inflation seasonality lag

### Scenario

An inflation-linked bond uses an indexation lag, so coupon and principal uplift use a prior inflation observation rather than the latest published CPI. A report must explain why current headline inflation does not immediately change the bond cashflow.

| Attribute | Value |
|---|---:|
| Original nominal | 1,000,000 |
| Lagged index ratio | 1.0825 |
| Latest index ratio | 1.0950 |
| Coupon rate | 1.20% |
| Coupon frequency | Semi-annual |

### Lagged coupon basis

```text
lagged_adjusted_principal = 1,000,000 x 1.0825 = 1,082,500
latest_adjusted_principal_for_analytics = 1,000,000 x 1.0950 = 1,095,000
semiannual_coupon_on_lagged_basis = 1,082,500 x 1.20% / 2 = 6,495
seasonality_lag_difference = 1,095,000 - 1,082,500 = 12,500
```

### Correct treatment

- apply the contract-specified lagged index for cashflow accrual and payment projection;
- optionally show latest-index analytics separately with clear label;
- preserve base index, lag rule, lagged observation, latest observation and publication date;
- do not restate coupon cashflow merely because a newer CPI value is available;
- label index seasonality or lag effects in performance attribution when material.

### QA assertions

| Test | Expected result |
|---|---|
| Latest CPI differs from lagged CPI | Cashflow uses lagged contract basis. |
| Lag rule is missing | Final cashflow projection is blocked or source-limited. |
| CPI is restated | Versioned index values update analytics according to policy. |
| Client report is generated | Lagged cashflow and latest-index analytics are not mixed. |

## Example 75. Covered-bond pool substitution

### Scenario

A covered-bond issuer substitutes part of the cover pool after a loan portfolio sale. The bond remains outstanding, but collateral-quality metrics and reporting labels must update from source evidence.

| Attribute | Before substitution | After substitution |
|---|---:|---:|
| Cover pool value | 1,250,000,000 | 1,230,000,000 |
| Bond outstanding | 1,000,000,000 | 1,000,000,000 |
| Overcollateralization | 25.00% | 23.00% |
| Ineligible assets | 15,000,000 | 22,000,000 |

### Coverage change

```text
overcollateralization_before = (1,250,000,000 - 1,000,000,000) / 1,000,000,000 = 25.00%
overcollateralization_after = (1,230,000,000 - 1,000,000,000) / 1,000,000,000 = 23.00%
oc_change = 23.00% - 25.00% = -2.00%
ineligible_asset_increase = 22,000,000 - 15,000,000 = 7,000,000
```

### Correct treatment

- keep client bond nominal unchanged unless a lifecycle event affects the bond itself;
- update cover-pool analytics, eligibility flags, overcollateralization and rating-watch inputs;
- preserve pool substitution notice, asset eligibility report, trustee/monitor confirmation and effective date;
- route collateral and advisory reviews when coverage deterioration breaches policy thresholds;
- separate collateral-quality analytics from market price and credit-spread movement.

### QA assertions

| Test | Expected result |
|---|---|
| Cover pool substitution file arrives | Cover-pool metrics update with effective date and source lineage. |
| Overcollateralization declines | Risk/advisory dashboard shows coverage change without changing nominal. |
| Ineligible assets breach policy | Exception or review workflow opens. |
| Client report is generated | Covered-bond exposure remains position-based while collateral analytics are updated. |

## Example 76. Callable make-whole tax allocation

### Scenario

A corporate bond is redeemed through a make-whole call. The custodian pays principal, accrued interest and a make-whole premium in one cash movement, but tax and performance reporting need separate components.

| Component | Amount |
|---|---:|
| Held nominal | 1,500,000 |
| Call price | 104.25 |
| Par principal component | 1,500,000 |
| Accrued interest | 18,750 |
| Make-whole premium | 63,750 |

### Tax allocation

```text
gross_redemption_cash = 1,500,000 x 104.25 / 100 + 18,750 = 1,582,500
make_whole_premium = 1,500,000 x (104.25 - 100.00) / 100 = 63,750
principal_cash = 1,500,000
```

### Correct treatment

- separate principal, accrued interest and make-whole premium even when custodian cash arrives as one posting;
- preserve issuer notice, call price, accrued-interest source, tax classification and withholding basis;
- route premium treatment to jurisdiction/product tax configuration rather than assuming capital gain or income;
- reduce nominal only after confirmed redemption;
- explain performance impact separately from coupon income and principal return.

### QA assertions

| Test | Expected result |
|---|---|
| Custodian sends one cash amount | Cash is split into principal, accrued interest and premium components. |
| Tax classification is missing | Tax report is source-limited or routed to tax review. |
| Call price differs from notice | Difference opens reconciliation or price challenge workflow. |
| Client report is generated | Premium is not merged into ordinary coupon income. |

## Example 77. Distressed consent fee clawback

### Scenario

A distressed issuer paid a consent fee for covenant amendments. The restructuring plan later includes a clawback of part of the consent fee from holders who participate in a new exchange package.

| Attribute | Value |
|---|---:|
| Eligible nominal | 2,000,000 |
| Original consent fee | 1.00 per 100 nominal |
| Clawback rate | 40% |
| Holders participating in exchange | Yes |

### Clawback amount

```text
original_consent_fee = 2,000,000 x 1.00 / 100 = 20,000
clawback_amount = 20,000 x 40% = 8,000
net_consent_fee_retained = 20,000 - 8,000 = 12,000
```

### Correct treatment

- preserve original consent acceptance, fee payment, restructuring terms, clawback trigger and exchange election;
- post the clawback as linked reversal/adjustment rather than rewriting the historical fee;
- distinguish consent fee income, clawback cash movement and exchange package value;
- update tax and client reporting according to source-backed classification;
- route disputed clawbacks to operations/tax review with original and adjusted values visible.

### QA assertions

| Test | Expected result |
|---|---|
| Clawback trigger applies | Adjustment is linked to original consent fee and exchange event. |
| Holder did not participate in exchange | Clawback does not apply unless terms say otherwise. |
| Source terms are missing | Clawback is blocked or source-limited. |
| Report is generated | Original fee, clawback and net retained amount are separately explainable. |

## Example 78. Sovereign CAC aggregation

### Scenario

A sovereign restructuring uses collective action clauses across multiple bond series. The client holds two affected series, and acceptance depends on both series-level and aggregated voting thresholds.

| Bond series | Held nominal | Series acceptance | Threshold |
|---|---:|---:|---:|
| Series A | 1,000,000 | 78% | 75% |
| Series B | 700,000 | 62% | 75% |
| Aggregated pool | 1,700,000 | 81% | 66.67% |

### CAC result

```text
series_a_passed = 78% >= 75% = true
series_b_passed = 62% >= 75% = false
aggregate_passed = 81% >= 66.67% = true
restructuring_binding = aggregate_passed and aggregation_terms_override_series_failure
```

### Correct treatment

- preserve CAC terms, series identifiers, voting record, aggregation method and final official notice;
- do not infer binding outcome from one series threshold alone;
- apply exchange, haircut or maturity extension only after official result and implementation date;
- explain affected positions by series because cashflows and new instruments may differ;
- keep advisory suitability and client communication separate from legal binding status.

### QA assertions

| Test | Expected result |
|---|---|
| One series fails but aggregate passes | Binding outcome follows the documented CAC aggregation terms. |
| Official notice is missing | Restructuring remains pending or source-limited. |
| Series terms differ | Each bond series keeps its own cashflow and exchange mapping. |
| Client report is generated | CAC aggregation is explained without collapsing all series into one generic event. |

## Example 79. Bond insurance wrap claim

### Scenario

A municipal bond misses an issuer coupon payment, but the bond has an insurance wrap. The insurer is expected to pay covered debt service after claim validation.

| Attribute | Value |
|---|---:|
| Insured nominal | 1,200,000 |
| Coupon rate | 3.50% |
| Coupon frequency | Semi-annual |
| Issuer cash received | 0 |
| Insurance coverage | Principal and interest |

### Insured claim

```text
expected_coupon = 1,200,000 x 3.50% / 2 = 21,000
insurance_claim_amount = expected_coupon - issuer_cash_received = 21,000
```

### Correct treatment

- preserve issuer default/missed-payment evidence, insurance policy, coverage scope, claim filing and insurer response;
- keep issuer credit event separate from insured recovery expectation;
- do not mark cash as received until insurer or paying agent pays;
- report claim state, expected insured amount and any coverage dispute separately;
- update risk analytics for both issuer risk and insurer counterparty/claims risk.

### QA assertions

| Test | Expected result |
|---|---|
| Issuer misses coupon but insurance applies | Claim workflow opens and cash remains receivable until paid. |
| Insurance coverage excludes disputed item | Claim is source-limited or partially eligible. |
| Insurer pays late | Receivable closes against insurer cash with delay evidence. |
| Client report is generated | Insured claim is not shown as ordinary paid coupon before receipt. |

## Example 80. MBS extension risk

### Scenario

A mortgage-backed security prepays slower than expected after rates rise. Weighted-average life extends, changing liquidity, duration and reinvestment assumptions.

| Attribute | Base case | Stress case |
|---|---:|---:|
| Current principal | 900,000 | 900,000 |
| Expected monthly prepayment | 45,000 | 18,000 |
| Weighted-average life | 3.2 years | 5.8 years |
| Effective duration | 2.7 | 4.9 |

### Extension impact

```text
prepayment_shortfall = 45,000 - 18,000 = 27,000 per month
wal_extension = 5.8 - 3.2 = 2.6 years
duration_extension = 4.9 - 2.7 = 2.2
```

### Correct treatment

- update analytics from prepayment model version, factor file, rate scenario and collateral assumptions;
- do not treat slower prepayment as default unless delinquency/default evidence supports it;
- show liquidity and duration impact separately from price movement;
- route mandate or advisory review when extended duration breaches policy;
- preserve base-case and stress-case assumptions for client and risk explanation.

### QA assertions

| Test | Expected result |
|---|---|
| Prepayment speed falls | WAL and duration extend with model/version evidence. |
| Factor file is stale | Extension analytics are blocked or labelled stale. |
| Mandate duration limit is breached | Review or exception workflow opens. |
| Client report is generated | Extension risk is explained separately from credit loss. |

## Example 81. Greenium performance attribution

### Scenario

A labelled green bond trades richer than a comparable conventional bond. Portfolio reporting needs to explain performance from spread movement, duration and the greenium without treating the label itself as a guaranteed return source.

| Attribute | Green bond | Conventional comparator |
|---|---:|---:|
| Starting spread | 85 bps | 105 bps |
| Ending spread | 78 bps | 101 bps |
| Duration | 6.0 | 6.0 |
| Starting greenium | 20 bps | n/a |
| Ending greenium | 23 bps | n/a |

### Attribution estimate

```text
green_bond_spread_tightening = 85 - 78 = 7 bps
comparator_spread_tightening = 105 - 101 = 4 bps
greenium_change = 23 - 20 = 3 bps
approx_greenium_return_effect = duration x greenium_change / 10000
approx_greenium_return_effect = 6.0 x 3 / 10000 = 0.18%
```

### Correct treatment

- preserve green-bond label source, taxonomy status, comparator selection, spread source and duration basis;
- explain greenium attribution as analytical estimate, not contractual payoff;
- keep ESG label failure/remediation workflow separate from market spread performance;
- route comparator methodology changes through model governance when used in client reporting;
- show residual spread movement that cannot be explained by the greenium estimate.

### QA assertions

| Test | Expected result |
|---|---|
| Comparator changes | Attribution is restated or versioned with methodology evidence. |
| Green label is under review | Greenium attribution is warning-labelled or suspended by policy. |
| Spread source is stale | Attribution is blocked or labelled stale. |
| Client report is generated | Greenium effect is shown as analytical attribution, not guaranteed income. |

## Example 82. Callable partial redemption lot selection

### Scenario

An issuer partially calls a bond issue. The custodian confirms that only part of the client's nominal is redeemed, and operations must select the impacted lots without reducing every lot proportionally unless the source event requires it.

| Lot | Nominal | Clean cost | Selected for redemption |
|---|---:|---:|---:|
| Lot A | 400,000 | 98.50 | 150,000 |
| Lot B | 350,000 | 101.20 | 0 |
| Lot C | 250,000 | 99.80 | 100,000 |

### Redemption cash and basis

```text
redeemed_nominal = 150,000 + 100,000 = 250,000
redemption_cash = redeemed_nominal x call_price / 100
selected_cost_basis = 150,000 x 98.50 / 100 + 100,000 x 99.80 / 100
```

### Correct treatment

- preserve issuer notice, custodian allocation file, selected certificate/lot ids, call price and accrued-interest treatment;
- reduce only source-selected lots when the allocation is lot-specific;
- avoid proportional lot depletion when final redemption allocation specifies certificates or custody lots;
- update remaining nominal, cost basis, accrued interest and tax-lot reporting from the selected-lot result;
- label pending or estimated allocations until final custodian confirmation arrives.

### QA assertions

| Test | Expected result |
|---|---|
| Final call allocation specifies lots | Only selected lots are reduced. |
| Allocation is preliminary | Position is projected or source-limited, not final. |
| Tax-lot basis is missing | Gain/loss reporting is blocked or labelled incomplete. |
| Client report is generated | Redeemed nominal, residual nominal and selected-lot basis are explainable. |

## Example 83. Distressed exchange consent hierarchy

### Scenario

A distressed issuer offers an exchange package that requires both exchange election and separate consent to covenant amendments. The client elects the exchange but does not provide the separate consent instruction before the consent deadline.

| Instruction | Status | Deadline |
|---|---|---|
| Exchange election | Submitted | Met |
| Covenant consent | Missing | Missed |
| Early consent fee | Conditional | Not earned |

### Eligibility result

```text
exchange_valid = exchange_election_submitted and exchange_deadline_met
consent_fee_eligible = covenant_consent_submitted and consent_deadline_met
```

### Correct treatment

- preserve offer circular, exchange election, consent terms, deadlines, fee conditions and final acceptance file;
- process the exchange only if the election is valid, but do not accrue consent fee when separate consent is missing;
- distinguish exchange package value, consent fee, clawback risk and old-bond restriction state;
- expose instruction hierarchy to advisors and operations before deadlines expire;
- keep rejected or missing consent evidence for client communication and audit.

### QA assertions

| Test | Expected result |
|---|---|
| Exchange election is valid but consent is missing | Exchange can proceed while consent fee remains ineligible. |
| Consent is submitted after deadline | Consent is rejected or source-limited according to terms. |
| Offer terms require consent for exchange eligibility | Exchange is blocked unless both instructions are valid. |
| Report is generated | Exchange outcome and consent-fee outcome are separate. |

## Example 84. Sovereign local-law redenomination

### Scenario

A sovereign bond governed by local law is redenominated from foreign currency into local currency under a statutory action. Portfolio reporting must preserve the original bond identity, new currency basis and any legal restriction state.

| Attribute | Before | After |
|---|---:|---:|
| Nominal | 1,000,000 USD | 920,000 local currency equivalent |
| Governing law | Local law | Local law |
| FX conversion rate | n/a | 0.92 |
| Trading status | Active | Restricted |

### Redenomination value

```text
redenominated_nominal_local = original_nominal_usd x statutory_conversion_rate
redenominated_nominal_local = 1,000,000 x 0.92 = 920,000
```

### Correct treatment

- preserve statutory notice, governing law, original identifier, replacement identifier if any, conversion rate, effective date and restriction status;
- treat redenomination as a lifecycle event, not a normal FX trade;
- update currency, nominal basis, cashflow schedule and reporting labels only after source-backed effective date;
- route valuation, suitability, restriction and tax reporting to review when market price or legality is uncertain;
- keep historical performance explainable in original and redenominated currency terms.

### QA assertions

| Test | Expected result |
|---|---|
| Redenomination notice is source-backed | Currency and nominal basis update from effective date. |
| Replacement identifier is missing | Holding remains source-limited until instrument mapping is complete. |
| Trading is restricted | Liquidity and advisory status update separately from valuation. |
| Client report spans event date | Pre-event and post-event currency bases are shown separately. |

## Example 85. Municipal insurance downgrade

### Scenario

A municipal bond is insured by a monoline insurer. The issuer rating is unchanged, but the insurer is downgraded, reducing the support value of the insurance wrap.

| Rating component | Before | After |
|---|---|---|
| Issuer rating | A | A |
| Insurer rating | AA | BBB |
| Portfolio rating basis | Insured rating | Review required |
| Market value | 1,020,000 | 995,000 |

### Support impact

```text
market_value_change = 995,000 - 1,020,000 = -25,000
insurance_support_downgraded = true
```

### Correct treatment

- preserve issuer rating, insurer rating, insurance policy, coverage scope, rating-agency notice and effective date;
- separate issuer credit quality from insurer support quality;
- update portfolio rating basis, collateral eligibility, concentration and advisory alerts according to policy;
- do not classify the issuer as downgraded when only insurer support changed;
- explain price/performance impact separately from any future claim expectation.

### QA assertions

| Test | Expected result |
|---|---|
| Insurer rating changes but issuer rating does not | Insurer support analytics update separately. |
| Policy uses lower-of issuer and insurer basis | Portfolio rating basis recalculates. |
| Insurance policy is missing | Insured-rating label is blocked or source-limited. |
| Client report is generated | Issuer risk and insurer-wrap risk are distinct. |

## Example 86. MBS factor restatement correction

### Scenario

An agency MBS factor file is restated after positions, income and paydown projections were already calculated. The platform must correct current face and cashflow projections without duplicating principal payments.

| Attribute | Original | Restated |
|---|---:|---:|
| Original face | 2,000,000 | 2,000,000 |
| Factor | 0.642500 | 0.638750 |
| Current face | 1,285,000 | 1,277,500 |
| Difference | n/a | -7,500 |

### Factor correction

```text
restated_current_face = original_face x restated_factor
restated_current_face = 2,000,000 x 0.638750 = 1,277,500
current_face_delta = 1,277,500 - 1,285,000 = -7,500
```

### Correct treatment

- preserve original factor file, restated factor file, publication timestamps, affected positions and prior report versions;
- restate current face, projected paydowns, amortization analytics and yield assumptions from the restated factor;
- avoid creating an extra principal cash event unless cash was actually received;
- trigger performance/report impact assessment when the restatement affects delivered statements;
- label stale or disputed factors until agency/custodian source is reconciled.

### QA assertions

| Test | Expected result |
|---|---|
| Factor is restated | Current face recalculates with versioned factor lineage. |
| Cash payment did not change | No duplicate principal payment is booked. |
| Report was delivered | Impact assessment identifies affected valuations and projections. |
| Custodian factor differs from agency file | Reconciliation exception opens. |

## Example 87. Green-bond taxonomy downgrade

### Scenario

A labelled green bond no longer meets the applicable taxonomy after a use-of-proceeds review. The bond remains a valid fixed-income holding, but green classification, reporting and mandate eligibility may change.

| Attribute | Before | After |
|---|---|---|
| Green label | Eligible | Downgraded |
| Market value | 1,100,000 | 1,075,000 |
| Green mandate eligibility | Eligible | Review required |
| Spread change | n/a | +18 bps |

### Classification impact

```text
green_eligible_exposure_after = 0 if taxonomy_status == downgraded else market_value
green_exposure_reduction = 1,100,000 - 0 = 1,100,000
```

### Correct treatment

- preserve taxonomy source, review date, downgrade reason, issuer response, prior label and effective date;
- remove or warning-label green-eligible exposure according to mandate policy without closing the bond position automatically;
- separate taxonomy downgrade from credit downgrade, market price movement and issuer default;
- route client reporting, mandate breach and product governance review when the downgrade is material;
- keep performance attribution from spread movement separate from ESG classification status.

### QA assertions

| Test | Expected result |
|---|---|
| Taxonomy downgrade is source-backed | Green eligibility updates from effective date. |
| Mandate requires green eligibility | Breach, review or divestment workflow opens according to policy. |
| Price changes after downgrade | Market performance and taxonomy status are reported separately. |
| Client report is generated | Green exposure excludes or labels downgraded bond exposure. |

## Example 88. Callable sinking-fund schedule conflict

### Scenario

A callable corporate bond has both scheduled sinking-fund reductions and an issuer call notice. The call notice appears to redeem more nominal than remains after the sinking-fund reduction.

| Attribute | Value |
|---|---:|
| Current nominal | 1,000,000 |
| Scheduled sinking-fund redemption | 150,000 |
| Issuer call notice nominal | 900,000 |
| Nominal available after sinking fund | 850,000 |
| Call price | 101.00 |

### Conflict check

```text
nominal_after_sinking_fund = current_nominal - sinking_fund_redemption
nominal_after_sinking_fund = 1,000,000 - 150,000 = 850,000
call_overage = issuer_call_notice_nominal - nominal_after_sinking_fund
call_overage = 900,000 - 850,000 = 50,000
```

### Correct treatment

- preserve sinking-fund schedule, issuer call notice, custodian confirmation, effective dates and nominal hierarchy;
- do not redeem more nominal than remains after confirmed prior lifecycle events;
- route conflict to asset-servicing repair before posting principal cash;
- keep projected cashflows source-limited while notice and schedule conflict;
- explain projected call proceeds separately from confirmed sinking-fund cash.

### QA assertions

| Test | Expected result |
|---|---|
| Call notice exceeds available nominal | Conflict exception opens. |
| Custodian confirms revised call amount | Call proceeds use revised source-backed nominal. |
| Sinking-fund event is cancelled | Available nominal recalculates before call proceeds. |
| Report is generated before repair | Projected call cash is labelled source-limited. |

## Example 89. Accrued-interest tax withholding mismatch

### Scenario

A bond sale includes accrued interest paid by the buyer. The custodian withholds tax on the full accrued amount, but tax rules require withholding only on the taxable portion.

| Attribute | Value |
|---|---:|
| Accrued interest received | 18,000 |
| Taxable accrued portion | 12,000 |
| Withholding rate | 15.00% |
| Custodian withholding | 2,700 |

### Expected withholding

```text
expected_withholding = taxable_accrued_interest x withholding_rate
expected_withholding = 12,000 x 15.00% = 1,800
withholding_mismatch = custodian_withholding - expected_withholding
withholding_mismatch = 2,700 - 1,800 = 900
```

### Correct treatment

- preserve sale proceeds, accrued interest, tax classification, withholding file, tax rule and custodian cash posting;
- separate accrued-interest economics from price gain/loss and from withholding cash;
- open tax/custody claim for excess withholding rather than silently adjusting income;
- keep statement labels clear: accrued interest received, withholding applied, reclaim or dispute state;
- route jurisdiction-specific tax handling through source-backed tax configuration.

### QA assertions

| Test | Expected result |
|---|---|
| Custodian withholding exceeds expected amount | Withholding mismatch is calculated and routed. |
| Taxable accrued portion is missing | Withholding expectation remains source-limited. |
| Reclaim is approved | Receivable or cash recovery links to original sale. |
| Client tax report is generated | Accrued interest and withholding remain separate. |

## Example 90. Sovereign payment moratorium

### Scenario

A sovereign announces a temporary payment moratorium. Coupon payment is deferred, not cancelled, and market prices decline because settlement timing and recovery assumptions changed.

| Attribute | Before moratorium | After moratorium |
|---|---:|---:|
| Expected coupon | 35,000 | Deferred |
| Market value | 960,000 | 810,000 |
| Payment status | Due | Moratorium |
| Moratorium period | n/a | 180 days |

### Deferred cash and price impact

```text
deferred_coupon_receivable = expected_coupon if moratorium_terms_preserve_claim else 0
market_value_change = 810,000 - 960,000 = -150,000
```

### Correct treatment

- preserve sovereign notice, moratorium terms, affected securities, payment dates, deferral period and legal status;
- distinguish deferred receivable, missed payment, default event and cancelled claim;
- update valuation, liquidity, credit watch and advisory restrictions from source-backed status;
- avoid posting coupon cash until paying-agent cash is received;
- explain market price loss separately from deferred income claim.

### QA assertions

| Test | Expected result |
|---|---|
| Moratorium preserves claim | Deferred coupon receivable is tracked, not settled cash. |
| Paying agent cash is absent | No coupon cash is posted. |
| Moratorium triggers credit policy | Restriction or review workflow opens. |
| Client report is generated | Deferred income and market-value loss are distinct. |

## Example 91. Bond quote stale-price challenge

### Scenario

A thinly traded bond is valued from a dealer quote that is five business days old. A newer executable quote is lower, but the pricing policy requires challenge evidence before overriding official price.

| Attribute | Official price | Challenge quote |
|---|---:|---:|
| Price | 96.40 | 94.75 |
| Quote age | 5 business days | Same day |
| Nominal | 1,000,000 | 1,000,000 |
| Price difference | n/a | -1.65 points |

### Valuation difference

```text
official_value = nominal x official_price / 100 = 964,000
challenge_value = nominal x challenge_price / 100 = 947,500
valuation_difference = challenge_value - official_value = -16,500
```

### Correct treatment

- preserve official price, quote timestamp, quote source, executable quote, challenge owner and pricing policy;
- label stale official price according to freshness policy while challenge is unresolved;
- do not overwrite official valuation until price challenge is approved;
- show advisory/reporting degraded state when stale price materially affects valuation, risk or suitability;
- keep price challenge outcome auditable for future valuation review.

### QA assertions

| Test | Expected result |
|---|---|
| Official quote breaches freshness threshold | Stale-price flag or price challenge workflow opens. |
| Executable quote is lower | Valuation difference is calculated but not automatically posted. |
| Pricing committee approves challenge | Official price updates with approval evidence. |
| Client report is generated before approval | Valuation is labelled stale or source-limited. |

## Example 92. Convertible call-forcing notice

### Scenario

An issuer calls a convertible bond to force conversion because the share price has traded above the call-forcing threshold. The investor must decide between conversion and call redemption before the deadline.

| Attribute | Value |
|---|---:|
| Bond nominal | 500,000 |
| Call redemption price | 102.00 |
| Conversion shares | 18,000 |
| Current share price | 31.00 |
| Conversion value | 558,000 |

### Conversion versus redemption

```text
call_redemption_cash = nominal x call_price / 100
call_redemption_cash = 500,000 x 102.00 / 100 = 510,000
conversion_value = conversion_shares x share_price = 18,000 x 31.00 = 558,000
conversion_advantage = 558,000 - 510,000 = 48,000
```

### Correct treatment

- preserve issuer notice, call-forcing threshold, observation evidence, conversion ratio, deadline and election options;
- separate economic comparison from client election authority and operational feasibility;
- block automatic conversion unless mandate, consent and custodian instructions permit it;
- update exposure, concentration, corporate-action and tax reporting after confirmed conversion or redemption;
- explain missed election, cash redemption and share delivery outcomes distinctly.

### QA assertions

| Test | Expected result |
|---|---|
| Call-forcing condition is source-backed | Election workflow opens with deadline and options. |
| Client authority is missing | Conversion instruction is blocked or escalated. |
| Conversion is elected | Bond closes and equity position opens only after confirmation. |
| Client report is generated | Redemption cash comparison and conversion outcome are traceable. |

## Example 93. Amortizing ABS cleanup call

### Scenario

An amortizing ABS reaches the cleanup call threshold. The sponsor may redeem remaining principal, but confirmation is required before projected principal is treated as payable.

| Attribute | Value |
|---|---:|
| Original pool balance | 50,000,000 |
| Current pool balance | 4,200,000 |
| Cleanup call threshold | 10.00% |
| Investor current face | 84,000 |
| Cleanup call price | 100.25 |

### Threshold and projected proceeds

```text
current_pool_percentage = current_pool_balance / original_pool_balance
current_pool_percentage = 4,200,000 / 50,000,000 = 8.40%
cleanup_call_eligible = current_pool_percentage <= 10.00%
projected_cleanup_cash = investor_current_face x cleanup_call_price / 100
projected_cleanup_cash = 84,000 x 100.25 / 100 = 84,210
```

### Correct treatment

- preserve ABS factor file, pool balance, cleanup threshold, sponsor notice, call price and effective date;
- treat threshold eligibility as a trigger condition, not confirmed redemption;
- keep current face, projected call cash and received principal cash separate;
- update prepayment, duration, yield and cashflow projections only with source-backed assumptions;
- label projected cleanup call cash as pending until sponsor/paying-agent confirmation arrives.

### QA assertions

| Test | Expected result |
|---|---|
| Pool falls below threshold | Cleanup call eligibility is flagged. |
| Sponsor notice is missing | Redemption cash remains projected, not confirmed. |
| Cleanup call is confirmed | Current face closes and principal cash posts on pay date. |
| Report is generated before confirmation | Cleanup call projection is labelled pending. |

## Example 94. Bond extension-option notice

### Scenario

An issuer sends notice that an extendible bond will be extended for two years. The platform must update maturity, projected coupon cashflows, duration and liquidity treatment without treating the event as a new purchase.

| Attribute | Before notice | After notice |
|---|---:|---:|
| Nominal | 1,000,000 | 1,000,000 |
| Original maturity | Year 3 | Year 5 |
| Coupon rate | 4.20% | 4.80% |
| Market price | 99.20 | 97.85 |
| Modified duration | 2.7 | 4.4 |

### Extension impact

```text
maturity_extension_years = extended_maturity_year - original_maturity_year
maturity_extension_years = 5 - 3 = 2
annual_coupon_increase = nominal x (new_coupon_rate - old_coupon_rate)
annual_coupon_increase = 1,000,000 x (4.80% - 4.20%) = 6,000
duration_extension = extended_duration - original_duration
duration_extension = 4.4 - 2.7 = 1.7
```

### Correct treatment

- preserve issuer extension notice, old and new maturity, coupon terms, election rights and effective date;
- update projected cashflows, maturity ladder, duration and liquidity horizon from source-backed terms;
- do not book a sale, purchase or realized gain/loss merely because maturity was extended;
- review mandate, concentration and DPM ladder breaches caused by longer tenor;
- explain coupon uplift separately from price and duration impact.

### QA assertions

| Test | Expected result |
|---|---|
| Extension notice is source-backed | Maturity, coupon schedule and duration projections update. |
| Election right is available | Workflow opens before terms are locked. |
| Mandate tenor limit is breached | Advisory or DPM exception is raised. |
| Client report is generated | Original maturity, extended maturity and income impact are traceable. |

## Example 95. Callable replacement reinvestment risk

### Scenario

A high-coupon bond is called in a lower-rate market. Principal cash is returned at call price, but replacement securities offer lower yield.

| Attribute | Called bond | Replacement option |
|---|---:|---:|
| Nominal / reinvestable cash | 1,000,000 | 1,020,000 |
| Coupon / available yield | 6.00% | 4.10% |
| Call price | 102.00 | n/a |
| Annual income | 60,000 | 41,820 |

### Reinvestment shortfall

```text
call_proceeds = nominal x call_price / 100
call_proceeds = 1,000,000 x 102.00 / 100 = 1,020,000
replacement_income = call_proceeds x replacement_yield
replacement_income = 1,020,000 x 4.10% = 41,820
annual_income_shortfall = old_coupon_income - replacement_income
annual_income_shortfall = 60,000 - 41,820 = 18,180
```

### Correct treatment

- preserve call notice, call price, accrued interest, proceeds, replacement yield assumptions and recommendation evidence;
- report call proceeds and reinvestment analytics separately from realized clean-price result;
- avoid implying the call premium fully offsets lower future income;
- evaluate replacement suitability, liquidity, credit quality, duration and concentration;
- update DPM income targets and maturity ladders when call proceeds are reinvested.

### QA assertions

| Test | Expected result |
|---|---|
| Bond is called | Principal proceeds post by call terms and current position closes. |
| Replacement yield is lower | Income shortfall is calculated as advisory analytics, not contractual cash. |
| No replacement is selected | Cash remains available with reinvestment-risk explanation. |
| Client report is generated | Call proceeds, call premium and future-income impact are distinct. |

## Example 96. Distressed interest capitalization

### Scenario

A distressed restructuring capitalizes missed coupon interest into new principal. The client receives no cash coupon, but the restructured note principal increases.

| Attribute | Value |
|---|---:|
| Old nominal | 800,000 |
| Missed coupon interest | 36,000 |
| Capitalization percentage | 100.00% |
| New nominal after capitalization | 836,000 |
| Market price after restructuring | 72.50 |

### Capitalized amount

```text
capitalized_interest = missed_coupon_interest x capitalization_percentage
capitalized_interest = 36,000 x 100.00% = 36,000
new_nominal = old_nominal + capitalized_interest
new_nominal = 800,000 + 36,000 = 836,000
post_restructuring_market_value = new_nominal x price / 100
post_restructuring_market_value = 836,000 x 72.50 / 100 = 606,100
```

### Correct treatment

- preserve restructuring terms, missed coupon, capitalization rate, new identifier and effective date;
- do not book capitalized interest as received cash;
- update nominal, accrual basis, cost basis and income classification according to source terms and tax policy;
- keep default, forbearance, capitalized interest and price impairment separate in reporting;
- review suitability and mandate impact for distressed/restructured exposure.

### QA assertions

| Test | Expected result |
|---|---|
| Coupon is capitalized | Principal increases and no cash coupon is posted. |
| Capitalization is partial | Residual missed interest remains separately tracked. |
| New identifier is issued | Position migration preserves lineage from old bond. |
| Report is generated | Capitalized interest is not presented as cash income. |

## Example 97. Sinkable tranche factor dispute

### Scenario

A sinkable asset-backed tranche receives two conflicting factor files from different sources. One factor would reduce current face more than the other.

| Attribute | Source A | Source B |
|---|---:|---:|
| Original face | 1,000,000 | 1,000,000 |
| Reported factor | 0.842500 | 0.836000 |
| Current face | 842,500 | 836,000 |
| Difference | n/a | -6,500 |

### Dispute amount

```text
current_face_a = original_face x factor_a = 1,000,000 x 0.842500 = 842,500
current_face_b = original_face x factor_b = 1,000,000 x 0.836000 = 836,000
current_face_dispute = current_face_b - current_face_a = -6,500
```

### Correct treatment

- preserve both factor files, timestamps, source hierarchy, trustee notice and current-face calculations;
- use approved source hierarchy or exception workflow before updating official current face;
- avoid booking duplicate principal reduction while factor dispute is unresolved;
- label projected principal, market value and yield as source-limited when current face is disputed;
- keep restatement capability for reports generated from superseded factors.

### QA assertions

| Test | Expected result |
|---|---|
| Two factor files conflict | Current-face dispute workflow opens. |
| Approved source hierarchy selects one factor | Official current face updates with evidence. |
| Factor is later restated | Affected valuation and report outputs are traceable. |
| Client report is generated during dispute | Current face and analytics are labelled source-limited. |

## Example 98. Sovereign sanctions coupon block

### Scenario

A sovereign bond coupon is due, but sanctions restrictions block payment routing through the paying agent. The entitlement exists, but cash cannot be received or distributed.

| Attribute | Value |
|---|---:|
| Coupon entitlement | 28,000 |
| Cash received | 0 |
| Blocked amount | 28,000 |
| Payment status | Sanctions blocked |
| Market value change | -90,000 |

### Blocked entitlement

```text
blocked_coupon_receivable = coupon_entitlement - cash_received
blocked_coupon_receivable = 28,000 - 0 = 28,000
```

### Correct treatment

- preserve sanctions source, paying-agent notice, entitlement amount, blocked-routing reason and legal status;
- distinguish blocked receivable, default, moratorium, cancelled claim and received cash;
- do not accrue unrestricted cash income when payment is blocked;
- update advisory restrictions, liquidity view and reporting labels from source-backed sanctions state;
- keep market-value loss separate from blocked coupon entitlement.

### QA assertions

| Test | Expected result |
|---|---|
| Coupon entitlement exists but cash is blocked | Receivable or blocked entitlement is tracked, not cash. |
| Sanctions status changes | Payment state recalculates from source-backed release or continued block. |
| Trading is restricted | Advisory and mandate controls reflect restricted status. |
| Client report is generated | Blocked coupon and price loss are distinct. |

## Example 99. Bond benchmark transition spread lock

### Scenario

A floating-rate note transitions from a discontinued benchmark to a replacement benchmark. The fallback spread is locked from the transition notice and affects future coupons.

| Attribute | Old benchmark | Replacement benchmark |
|---|---:|---:|
| Benchmark fixing | 5.20% | 5.05% |
| Original contractual spread | 1.10% | 1.10% |
| Fallback adjustment spread | n/a | 0.26% |
| Resulting coupon rate | 6.30% | 6.41% |

### Coupon comparison

```text
old_coupon_rate = old_benchmark + contractual_spread
old_coupon_rate = 5.20% + 1.10% = 6.30%
new_coupon_rate = replacement_benchmark + contractual_spread + fallback_adjustment_spread
new_coupon_rate = 5.05% + 1.10% + 0.26% = 6.41%
coupon_rate_change = 6.41% - 6.30% = 0.11%
```

### Correct treatment

- preserve fallback notice, old benchmark, replacement benchmark, contractual spread, adjustment spread and lock date;
- apply fallback spread only from the effective reset date;
- keep benchmark transition mechanics separate from discretionary coupon override;
- update accrual, yield, forecast cashflows, reporting labels and benchmark lineage;
- test historical coupons remain on original benchmark while future coupons use replacement terms.

### QA assertions

| Test | Expected result |
|---|---|
| Fallback spread is locked | Future reset coupon uses replacement benchmark plus contractual and adjustment spreads. |
| Effective date has not arrived | Historical accruals remain on old benchmark terms. |
| Replacement fixing is missing | Coupon projection is source-limited. |
| Client report is generated | Benchmark transition and spread adjustment are explainable. |

## Example 100. Bond tender withdrawal window

### Scenario

A client tenders a bond into an issuer offer but later asks to withdraw the instruction. The offer allows withdrawal only before a specified deadline. The platform must prevent false sale treatment while the tender remains withdrawable or pending final acceptance.

| Attribute | Value |
|---|---:|
| Held nominal | 1,000,000 |
| Tendered nominal | 600,000 |
| Tender price | 101.50 |
| Withdrawal deadline | 17:00 local time |
| Withdrawal request time | 16:35 local time |

### Withdrawable amount

```text
withdrawal_allowed = withdrawal_request_time <= withdrawal_deadline
cash_if_accepted = tendered_nominal x tender_price / 100
cash_if_accepted = 600,000 x 101.50 / 100 = 609,000
```

### Correct treatment

- preserve offer circular, tender instruction, withdrawal deadline, client request time, custodian acknowledgement and final acceptance file;
- restrict tendered nominal while the tender is pending, but do not reduce the position until accepted;
- restore sellable nominal when withdrawal is source-confirmed before the deadline;
- keep expected tender proceeds separate from confirmed settlement cash;
- show pending, withdrawn, accepted and rejected tender states distinctly in reporting.

### QA assertions

| Test | Expected result |
|---|---|
| Withdrawal arrives before deadline | Tender instruction is withdrawn and nominal becomes available after acknowledgement. |
| Withdrawal arrives after deadline | Tender remains pending or accepted according to offer terms. |
| Final acceptance is partial | Position reduces only by accepted nominal. |
| Client report is generated during pending state | Tendered nominal is restricted but not sold. |

## Example 101. Inflation-Linked Index Disruption

### Scenario

An inflation-linked bond requires a coupon calculation, but the scheduled inflation index publication is delayed. The platform must use the contractual fallback or label the cashflow as provisional.

| Attribute | Value |
|---|---:|
| Nominal | 1,000,000 |
| Base index | 250.000 |
| Latest confirmed index | 285.000 |
| Scheduled current index | Delayed |
| Coupon rate | 2.00% |

### Provisional coupon

```text
provisional_index_ratio = latest_confirmed_index / base_index
provisional_index_ratio = 285.000 / 250.000 = 1.1400
provisional_adjusted_principal = nominal x provisional_index_ratio
provisional_adjusted_principal = 1,000,000 x 1.1400 = 1,140,000
semiannual_coupon = provisional_adjusted_principal x coupon_rate / 2
semiannual_coupon = 1,140,000 x 2.00% / 2 = 11,400
```

### Correct treatment

- preserve index calendar, missing publication, fallback rule, latest confirmed index and later restatement source;
- label coupon, adjusted principal, yield and inflation attribution as provisional when the contractual current index is missing;
- avoid posting final cash income until paying-agent or index-source evidence confirms the applicable value;
- restate affected analytics when the delayed index is published;
- keep reporting clear on confirmed versus provisional inflation-linked values.

### QA assertions

| Test | Expected result |
|---|---|
| Current index is delayed | Coupon projection uses fallback or remains source-limited. |
| Index is later published | Adjusted principal and coupon projection restate with lineage. |
| Paying-agent cash differs from provisional estimate | Difference posts as adjustment, not price movement. |
| Report is generated before publication | Inflation-linked analytics are labelled provisional. |

## Example 102. Green-Bond KPI Step-Down Reversal

### Scenario

A sustainability-linked bond initially receives a coupon step-down because a KPI was reported as achieved. The verifier later corrects the result and reverses the step-down from the next coupon period.

| Attribute | Value |
|---|---:|
| Nominal | 2,000,000 |
| Base coupon | 4.00% |
| Step-down | 0.25% |
| Reversed coupon | 4.00% |
| Previously applied coupon | 3.75% |

### Coupon correction

```text
annual_coupon_delta = nominal x (reversed_coupon - previously_applied_coupon)
annual_coupon_delta = 2,000,000 x (4.00% - 3.75%) = 5,000
semiannual_coupon_delta = annual_coupon_delta / 2 = 2,500
```

### Correct treatment

- preserve KPI terms, verifier report, corrected verifier notice, effective date and coupon schedule version;
- reverse the coupon step-down only from the corrected effective period unless the notice requires historical restatement;
- separate green eligibility, KPI status, coupon economics and client sustainability reporting;
- update income forecasts, yield, suitability notes and mandate checks from the corrected source;
- keep the original KPI status and corrected status available for audit.

### QA assertions

| Test | Expected result |
|---|---|
| Verifier reverses KPI achievement | Future coupon schedule removes the step-down with evidence. |
| Historical restatement is required | Prior accrual correction posts with lineage. |
| Green eligibility remains valid | Coupon economics change without removing holding classification unless taxonomy source changes. |
| Client report is generated | KPI correction and coupon effect are explainable. |

## Example 103. Covered-Bond Overcollateralization Breach

### Scenario

A covered-bond monitor reports that the cover pool no longer meets the required overcollateralization level. The bond remains outstanding, but collateral quality and risk labels must change.

| Attribute | Value |
|---|---:|
| Cover pool value | 1,080,000,000 |
| Covered bonds outstanding | 1,030,000,000 |
| Required overcollateralization | 7.00% |
| Actual overcollateralization | 4.85% |
| Breach | 2.15 percentage points |

### Coverage test

```text
actual_oc = (cover_pool_value - bonds_outstanding) / bonds_outstanding
actual_oc = (1,080,000,000 - 1,030,000,000) / 1,030,000,000 = 4.85%
oc_shortfall = required_oc - actual_oc
oc_shortfall = 7.00% - 4.85% = 2.15%
```

### Correct treatment

- preserve cover-pool report, monitor notice, required OC covenant, actual OC, breach date and cure status;
- update risk labels, collateral eligibility and advisory review without closing the bond position;
- keep issuer credit risk, cover-pool support and bond market value as separate reporting dimensions;
- require source-backed cure notice before removing breach state;
- add the breach to credit, concentration and mandate exception review where relevant.

### QA assertions

| Test | Expected result |
|---|---|
| Actual OC falls below requirement | Breach state opens with calculated shortfall. |
| Cure report arrives | Breach state closes only with monitor evidence. |
| Bond price source is unchanged | Market value remains separate from cover-pool covenant status. |
| Collateral report is generated | Eligibility reflects current breach and haircut policy. |

## Example 104. MBS Servicer Advance Recovery

### Scenario

An MBS servicer advances payments during borrower delinquency and later recovers the advance from pool collections. The recovery affects remittance classification and projected cashflows.

| Attribute | Value |
|---|---:|
| Servicer advance outstanding | 120,000 |
| Pool collections this period | 500,000 |
| Advance recovery | 80,000 |
| Net distributable collections | 420,000 |
| Investor share | 10.00% |

### Investor cash impact

```text
net_distributable_collections = pool_collections - advance_recovery
net_distributable_collections = 500,000 - 80,000 = 420,000
investor_distribution = net_distributable_collections x investor_share
investor_distribution = 420,000 x 10.00% = 42,000
```

### Correct treatment

- preserve servicer report, delinquency status, advance balance, recovery amount, pool collections and investor share;
- separate servicer advance recovery from scheduled interest, scheduled principal and prepayment;
- update cashflow projections, yield and delinquency analytics from the remittance source;
- avoid treating advance recovery as a new client fee or market loss;
- keep servicer advance data available for risk, extension and liquidity analysis.

### QA assertions

| Test | Expected result |
|---|---|
| Advance recovery is reported | Distributable cash is reduced with remittance evidence. |
| Recovery exceeds collections | Cashflow is source-limited or exceptioned by remittance rules. |
| Delinquency later cures | Advance balance and cashflow projection update with source lineage. |
| Client report is generated | Distribution components separate advances, interest and principal. |

## Example 105. Subordinated Note Write-Up Event

### Scenario

A subordinated note was previously written down after a regulatory trigger. Later, the issuer receives approval to write up part of the principal. The platform must increase current nominal without treating it as a cash purchase.

| Attribute | Value |
|---|---:|
| Original nominal | 1,000,000 |
| Nominal after write-down | 650,000 |
| Approved write-up amount | 150,000 |
| New current nominal | 800,000 |
| Cash received | 0 |

### Nominal restoration

```text
new_current_nominal = nominal_after_write_down + approved_write_up_amount
new_current_nominal = 650,000 + 150,000 = 800,000
restored_percentage_of_original = new_current_nominal / original_nominal
restored_percentage_of_original = 800,000 / 1,000,000 = 80.00%
```

### Correct treatment

- preserve original write-down notice, write-up approval, effective date, current nominal and regulatory source;
- increase current nominal through lifecycle adjustment, not purchase, transfer or income cashflow;
- update coupon basis, market value, yield, loss-recovery analytics and reporting labels from the effective date;
- keep prior write-down, partial write-up and remaining impairment visible;
- require source-backed confirmation before reversing impairment state.

### QA assertions

| Test | Expected result |
|---|---|
| Write-up notice is confirmed | Current nominal increases without purchase cash. |
| Approval is partial | Remaining impairment remains visible. |
| Coupon basis changes | Future accrual uses restored current nominal from effective date. |
| Client report is generated | Write-down history and write-up recovery are explainable. |

## Example 106. Coupon Deferral Election Reversal

### Scenario

An issuer of a hybrid bond elects to defer a discretionary coupon. The issuer later reverses the election before the contractual cut-off and confirms that the coupon will be paid.

| Attribute | Value |
|---|---:|
| Nominal | 1,500,000 |
| Coupon rate | 5.20% |
| Coupon frequency | Semiannual |
| Deferred coupon status | Reversed before cut-off |

### Coupon reinstatement

```text
semiannual_coupon = nominal x coupon_rate / 2
semiannual_coupon = 1,500,000 x 5.20% / 2 = 39,000
```

### Correct treatment

- preserve original deferral notice, reversal notice, contractual cut-off, paying-agent confirmation and coupon schedule version;
- move coupon state from deferred to payable only when reversal is source-backed and before the permitted cut-off;
- restore accrued income and expected cashflow from the effective source date;
- keep the deferral history visible for credit, income reliability and suitability review;
- avoid duplicating coupon accrual if provisional deferral and reversal both touched the same coupon period.

### QA assertions

| Test | Expected result |
|---|---|
| Deferral reversal is accepted before cut-off | Coupon status changes to payable with source lineage. |
| Reversal arrives after cut-off | Coupon remains deferred unless paying-agent evidence overrides. |
| Accrual was already stopped | Accrual resumes without duplicate coupon income. |
| Client report is generated | Deferral history and reinstated coupon are explainable. |

## Example 107. Make-Whole Notice Rescission

### Scenario

A corporate issuer announces a make-whole call, then rescinds the notice before the redemption date due to a financing delay.

| Attribute | Value |
|---|---:|
| Nominal | 2,000,000 |
| Announced make-whole price | 103.25 |
| Expected redemption cash | 2,065,000 |
| Rescission status | Confirmed |

### Reversed redemption cash

```text
expected_redemption_cash = nominal x make_whole_price / 100
expected_redemption_cash = 2,000,000 x 103.25 / 100 = 2,065,000
reversed_projected_cash = 2,065,000
```

### Correct treatment

- preserve original call notice, rescission notice, redemption date, paying-agent source and notice validity terms;
- reverse projected redemption cash and restore ongoing position state when rescission is confirmed;
- keep accrued interest, yield-to-call, yield-to-maturity and reinvestment assumptions versioned;
- prevent premature position closure or realized gain/loss booking;
- show the rescinded call as a lifecycle event for audit and advisor explanation.

### QA assertions

| Test | Expected result |
|---|---|
| Make-whole call is rescinded | Position remains open and projected redemption cash is reversed. |
| Position was pre-closed | Closure is reversed with lifecycle lineage, not treated as new purchase. |
| Yield analytics are recalculated | Yield-to-call is removed or source-limited while maturity yield remains. |
| Client report is generated | Rescinded notice is visible as lifecycle history. |

## Example 108. Inflation Floor Restatement Dispute

### Scenario

An inflation-linked bond has a deflation floor. The calculation agent restates an index period, and investors dispute whether the floor should apply before or after restatement.

| Attribute | Value |
|---|---:|
| Principal before restatement | 1,020,000 |
| Restated indexed principal | 990,000 |
| Floor principal | 1,000,000 |
| Disputed amount | 10,000 |

### Floor impact

```text
floored_principal = max(restated_indexed_principal, floor_principal)
floored_principal = max(990,000, 1,000,000) = 1,000,000
disputed_floor_adjustment = floored_principal - restated_indexed_principal
disputed_floor_adjustment = 10,000
```

### Correct treatment

- preserve index restatement, bond terms, floor clause, calculation-agent notice, dispute state and final determination;
- separate provisional indexed principal, floored principal, disputed floor adjustment and final paying-agent cash;
- avoid treating the disputed floor amount as settled cash until determination is final;
- keep performance and income analytics source-limited while the principal basis is disputed;
- update client reporting with restatement lineage and dispute status.

### QA assertions

| Test | Expected result |
|---|---|
| Restated index breaches floor | Floored and unfloored principal are both visible. |
| Floor treatment is disputed | Disputed adjustment remains provisional. |
| Final determination arrives | Principal basis and cashflow restate with source lineage. |
| Performance report is generated | Inflation contribution is labelled provisional or final as appropriate. |

## Example 109. Covered-Bond Issuer Substitution

### Scenario

A covered-bond program substitutes the issuer after a legal reorganization. The cover pool remains in place, but issuer identity, rating and reporting lineage change.

| Attribute | Before | After |
|---|---|---|
| Issuer | Bank A |
| Substitute issuer | - | Bank B |
| Cover pool | Program 2028 Pool | Program 2028 Pool |
| Rating | A | A- |
| ISIN | Unchanged | Unchanged |

### Rating-notch movement

```text
issuer_identity_changed = true
isin_changed = false
rating_changed = true
```

### Correct treatment

- preserve issuer substitution notice, legal opinion, rating notice, cover-pool continuity evidence and effective date;
- update issuer, obligor and concentration analytics from the effective date without duplicating the position;
- retain cover-pool lineage and historical issuer identity for audit and reporting;
- refresh eligibility, collateral, mandate and concentration tests where issuer identity matters;
- avoid treating issuer substitution as a sale, transfer or new issue unless the source terms require it.

### QA assertions

| Test | Expected result |
|---|---|
| Issuer is substituted and ISIN is unchanged | Position continues with updated issuer lineage. |
| Rating changes | Credit and mandate analytics refresh from effective date. |
| Cover pool remains same | Collateral lineage carries forward with substitution evidence. |
| Client report is generated | Prior issuer, substitute issuer and effective date are explainable. |

## Example 110. MBS Delinquency Severity Shock

### Scenario

An MBS pool reports a sharp increase in serious delinquencies and loss severity. Cashflow projections must update without treating projected losses as realized principal losses before remittance evidence.

| Attribute | Prior | Revised |
|---|---:|---:|
| Pool balance | 50,000,000 | 50,000,000 |
| Serious delinquency rate | 3.00% | 8.00% |
| Loss severity assumption | 25.00% | 40.00% |
| Investor share | 2.00% | 2.00% |

### Expected loss shock

```text
prior_expected_loss = pool_balance x prior_delinquency_rate x prior_loss_severity
prior_expected_loss = 50,000,000 x 3.00% x 25.00% = 375,000
revised_expected_loss = pool_balance x revised_delinquency_rate x revised_loss_severity
revised_expected_loss = 50,000,000 x 8.00% x 40.00% = 1,600,000
investor_expected_loss_delta = (1,600,000 - 375,000) x 2.00% = 24,500
```

### Correct treatment

- preserve servicer delinquency report, severity model version, pool balance, investor share and remittance evidence;
- update projected cashflows, yield, extension risk and credit analytics from the revised assumptions;
- keep modelled expected loss separate from realized principal write-down or cash shortfall;
- require remittance or trustee evidence before booking realized principal loss;
- label client reporting as scenario, modelled or realized according to source state.

### QA assertions

| Test | Expected result |
|---|---|
| Delinquency and severity assumptions rise | Expected loss and cashflow projections update. |
| No realized remittance loss exists | Principal write-down is not booked. |
| Remittance confirms loss | Realized loss posts with trustee or servicer lineage. |
| Risk report is generated | Scenario loss and realized loss are distinct. |

## Example 111. Subordinated Call-Skip Event

### Scenario

An issuer has the right to call a subordinated note at the first call date but chooses not to call. The bond remains outstanding and resets to a higher coupon margin.

| Attribute | Value |
|---|---:|
| Nominal | 1,000,000 |
| Pre-call coupon | 4.75% |
| Reset coupon after call skip | 6.10% |
| Expected call redemption | 1,000,000 |
| Call outcome | Not called |

### Coupon reset impact

```text
annual_coupon_increase = nominal x (reset_coupon - pre_call_coupon)
annual_coupon_increase = 1,000,000 x (6.10% - 4.75%) = 13,500
projected_redemption_reversed = 1,000,000
```

### Correct treatment

- preserve call notice window, non-call announcement, reset terms, coupon fixing and effective date;
- reverse projected call redemption and keep the position open;
- update coupon schedule, yield, duration, extension risk and reinvestment assumptions;
- distinguish issuer non-call risk from payment default or missed coupon;
- refresh advisory, suitability and mandate review for extended subordinated exposure.

### QA assertions

| Test | Expected result |
|---|---|
| Issuer skips call | Position remains open and projected redemption is reversed. |
| Coupon resets higher | Future accrual uses reset coupon from effective date. |
| Client expected call liquidity | Liquidity forecast removes expected redemption and flags extension risk. |
| Report is generated | Non-call, reset coupon and extension risk are visible. |

## Example 112. Callable Consent-Revocation Window

### Scenario

A callable bond consent campaign allows investors to support a covenant amendment and related call-price adjustment. The investor submits consent, then revokes during the permitted revocation window before the tabulation date.

| Attribute | Value |
|---|---:|
| Nominal consented | 2,500,000 |
| Consent fee | 0.25 points |
| Revoked nominal | 1,000,000 |
| Remaining valid consent | 1,500,000 |

### Valid consent fee

```text
valid_consented_nominal = consented_nominal - revoked_nominal
valid_consented_nominal = 2,500,000 - 1,000,000 = 1,500,000

valid_consent_fee = valid_consented_nominal x consent_fee / 100
valid_consent_fee = 1,500,000 x 0.25 / 100 = 3,750
```

### Correct treatment

- preserve consent instruction, revocation request, revocation deadline, tabulation notice, custodian acknowledgement and final acceptance;
- remove revoked nominal from consented holdings while keeping original instruction lineage;
- accrue or project consent fee only on valid accepted nominal;
- keep callable terms, revised covenants and call-price assumptions provisional until campaign outcome is final;
- report revoked consent as lifecycle instruction history, not as trade activity.

### QA assertions

| Test | Expected result |
|---|---|
| Revocation arrives before deadline | Valid consented nominal and fee reduce. |
| Revocation arrives after deadline | Consent remains pending or accepted unless source terms allow late revocation. |
| Campaign fails threshold | Consent fee and covenant changes are reversed or cancelled. |
| Client report is generated | Original consent, revocation and final valid instruction are explainable. |

## Example 113. Municipal Tax-Status Reclassification

### Scenario

A municipal bond coupon was reported as tax-exempt. The issuer later reclassifies part of the income as taxable due to a private-use or project-status change.

| Attribute | Value |
|---|---:|
| Coupon received | 45,000 |
| Taxable reclassification percentage | 35% |
| Reclassified taxable income | 15,750 |
| Remaining tax-exempt income | 29,250 |

### Reclassified income

```text
taxable_income = coupon_received x taxable_reclassification_percentage
taxable_income = 45,000 x 35% = 15,750

tax_exempt_income = coupon_received - taxable_income
tax_exempt_income = 45,000 - 15,750 = 29,250
```

### Correct treatment

- preserve issuer tax notice, original coupon classification, revised classification, tax year, statement version and correction evidence;
- restate income character without changing the original cash receipt amount;
- update tax reporting, performance income buckets and client statement labels from the effective tax period;
- retain prior statement lineage and issue correction notices where client reporting was already delivered;
- avoid treating tax-status reclassification as bond default, price loss or new cashflow.

### QA assertions

| Test | Expected result |
|---|---|
| Tax notice reclassifies coupon | Taxable and tax-exempt income buckets update. |
| Cash receipt is unchanged | No duplicate cash transaction is booked. |
| Prior statement was delivered | Correction workflow preserves prior and revised versions. |
| Tax report is generated | Income character and source notice are visible. |

## Example 114. ABS Servicer Replacement

### Scenario

An ABS trust replaces its servicer after servicing-quality failures. The pool continues, but payment files, delinquency reporting and advance assumptions move to the successor servicer.

| Attribute | Before | After |
|---|---|---|
| Servicer | Servicer A | Servicer B |
| Pool balance | 80,000,000 | 80,000,000 |
| Investor share | 1.50% | 1.50% |
| Transition reserve | - | 600,000 |

### Investor reserve exposure

```text
investor_reserve_exposure = transition_reserve x investor_share
investor_reserve_exposure = 600,000 x 1.50% = 9,000
```

### Correct treatment

- preserve trustee notice, old servicer, successor servicer, transition date, payment file format, reserve terms and remittance calendar;
- update servicer source ownership without duplicating or closing the ABS position;
- label cashflow projections as transition-sensitive until successor files are certified;
- separate transition reserve exposure from realized principal loss or coupon shortfall;
- refresh operational reconciliation and data-quality checks for the new servicer feed.

### QA assertions

| Test | Expected result |
|---|---|
| Successor servicer is effective | Source owner updates from transition date. |
| First successor file is missing | Position remains open but cashflow state is source-limited. |
| Transition reserve is created | Reserve exposure is reported separately from realized loss. |
| Remittance is received | Payment is reconciled to successor servicer evidence. |

## Example 115. Sovereign GDP-Warrant Attachment

### Scenario

A sovereign restructuring package includes new bonds plus GDP-linked warrants. The warrant entitlement is attached to the original eligible nominal and should not be treated as ordinary coupon income.

| Attribute | Value |
|---|---:|
| Eligible restructured nominal | 3,000,000 |
| GDP warrant ratio | 0.08 warrants per 100 nominal |
| Warrant units received | 2,400 |
| Initial fair value per warrant | 12.50 |

### Warrant fair value

```text
warrant_units = eligible_nominal / 100 x warrant_ratio
warrant_units = 3,000,000 / 100 x 0.08 = 2,400

initial_warrant_value = warrant_units x fair_value_per_warrant
initial_warrant_value = 2,400 x 12.50 = 30,000
```

### Correct treatment

- preserve restructuring terms, eligible nominal, exchange allocation, warrant identifier, valuation source and custody receipt;
- create or update the GDP warrant as a linked lifecycle entitlement, not as coupon income;
- keep bond recovery, new-bond position and warrant exposure separately visible;
- include GDP trigger, valuation uncertainty, liquidity and suitability considerations in advisory and reporting views;
- avoid booking warrant value before custody or source-backed entitlement confirmation.

### QA assertions

| Test | Expected result |
|---|---|
| Warrant entitlement is confirmed | Linked warrant position or entitlement is created with lineage. |
| Warrant valuation is unavailable | Exposure is source-limited and not priced with bond clean price. |
| Restructured bond is reported | New bond recovery and warrant upside are distinct. |
| Client report is generated | GDP warrant trigger and valuation state are explainable. |

## Example 116. Covered-Bond Liquidity-Reserve Drawdown

### Scenario

A covered-bond program draws on its liquidity reserve to cover temporary cashflow timing stress. The drawdown supports coupon payment but weakens cover-pool support metrics until replenished.

| Attribute | Value |
|---|---:|
| Liquidity reserve before draw | 18,000,000 |
| Reserve drawdown | 4,500,000 |
| Required reserve level | 15,000,000 |
| Reserve after drawdown | 13,500,000 |

### Reserve shortfall

```text
reserve_after_drawdown = liquidity_reserve_before_draw - reserve_drawdown
reserve_after_drawdown = 18,000,000 - 4,500,000 = 13,500,000

reserve_shortfall = required_reserve_level - reserve_after_drawdown
reserve_shortfall = 15,000,000 - 13,500,000 = 1,500,000
```

### Correct treatment

- preserve cover-pool monitor report, reserve draw notice, coupon payment evidence, reserve requirement, cure plan and replenishment status;
- keep coupon payment status separate from reserve-strength and cover-pool deterioration analytics;
- update eligibility, collateral, rating-watch and concentration labels where reserve shortfall matters;
- avoid treating reserve drawdown as principal loss or bond redemption;
- show replenishment progress and breach cure status in operational reporting.

### QA assertions

| Test | Expected result |
|---|---|
| Reserve falls below required level | Reserve shortfall and cure workflow open. |
| Coupon is paid using reserve | Coupon status is paid while support metric weakens. |
| Reserve is replenished | Breach or watch state clears only from source-backed replenishment. |
| Risk report is generated | Coupon payment, reserve draw and reserve shortfall are distinct. |

## Example 117. Private-Placement Transfer Restriction Waiver

### Scenario

A private-placement bond has transfer restrictions. A client requests a transfer to an approved buyer, and the issuer grants a one-time waiver after suitability, KYC and investor-qualification checks.

| Attribute | Value |
|---|---:|
| Nominal to transfer | 1,200,000 |
| Transfer price | 98.40 |
| Gross transfer consideration | 1,180,800 |
| Waiver status | Approved for one buyer |

### Transfer consideration

```text
gross_transfer_consideration = nominal_to_transfer x transfer_price / 100
gross_transfer_consideration = 1,200,000 x 98.40 / 100 = 1,180,800
```

### Correct treatment

- preserve private-placement memorandum, transfer restriction, waiver approval, approved buyer, investor qualification, settlement instruction and expiry;
- allow transfer only within the source-backed waiver scope;
- keep waiver evidence, buyer eligibility, settlement status and any price discount visible;
- prevent reuse of the waiver for a different buyer, amount, account or expiry period;
- treat the event as restricted transfer or secondary sale, not as redemption or issuer call.

### QA assertions

| Test | Expected result |
|---|---|
| Waiver approves one buyer | Transfer is allowed only for that buyer and nominal scope. |
| Transfer request exceeds approved nominal | Excess amount is blocked or requires new waiver. |
| Waiver expires before settlement | Transfer is blocked or re-approved. |
| Client report is generated | Restriction, waiver, transfer price and settlement state are visible. |

## Example 118. Callable Call-Price Amendment Dispute

### Scenario

An issuer amends a callable bond notice and changes the call price after the custodian has already projected redemption proceeds. The desk disputes whether the amended price applies to the client position because the amendment arrived close to the notice deadline.

| Attribute | Value |
|---|---:|
| Called nominal | 2,000,000 |
| Original call price | 102.00 |
| Amended call price | 101.50 |
| Price dispute impact | 10,000 |

### Dispute amount

```text
call_price_dispute_amount = called_nominal x (original_call_price - amended_call_price) / 100
call_price_dispute_amount = 2,000,000 x (102.00 - 101.50) / 100 = 10,000
```

### Correct treatment

- preserve original call notice, amended notice, receipt timestamp, custodian projection, paying-agent confirmation and dispute owner;
- keep projected redemption cash provisional until the valid call price is source-confirmed;
- separate call premium, accrued interest and disputed price delta;
- avoid closing the position at the amended price until notice validity and effective terms are resolved;
- report the event as a lifecycle dispute, not as ordinary market price movement.

### QA assertions

| Test | Expected result |
|---|---|
| Amended price arrives after projection | Projected cash is versioned and dispute state opens. |
| Amendment is validated | Redemption proceeds update from source-backed amended terms. |
| Amendment is rejected | Original projection remains with dispute closure evidence. |
| Client report is generated | Original price, amended price and disputed delta are explainable. |

## Example 119. Municipal Escrow Defeasance Release

### Scenario

A municipal bond was defeased through an escrow portfolio. After final defeased liabilities are covered, excess escrow assets are released. The release must be distinguished from coupon income, call premium and principal redemption.

| Attribute | Value |
|---|---:|
| Escrow asset value | 5,000,000 |
| Remaining defeased liabilities | 4,850,000 |
| Releaseable escrow excess | 150,000 |
| Investor participation | 2.00% |

### Investor release amount

```text
releaseable_escrow_excess = escrow_asset_value - remaining_defeased_liabilities
releaseable_escrow_excess = 5,000,000 - 4,850,000 = 150,000

investor_release = releaseable_escrow_excess x investor_participation
investor_release = 150,000 x 2.00% = 3,000
```

### Correct treatment

- preserve defeasance notice, escrow report, liability schedule, trustee release notice, investor share and payment evidence;
- classify escrow release according to source terms and tax treatment rather than assuming ordinary coupon income;
- keep defeased bond status, escrow asset coverage and released excess separately visible;
- update yield, maturity and cashflow projections without changing historical coupon receipts;
- retain statement correction lineage if prior reporting treated escrow coverage differently.

### QA assertions

| Test | Expected result |
|---|---|
| Escrow excess is released | Investor release is calculated from participation share. |
| Release classification is unknown | Cash is source-limited until trustee or tax classification is confirmed. |
| Defeased liabilities remain | Only excess after liabilities is releaseable. |
| Report is generated | Defeasance, liability coverage and released excess are labelled separately. |

## Example 120. ABS Trustee Reporting Restatement

### Scenario

An ABS trustee restates the monthly report after discovering a prior factor error. The restatement changes current principal, prepayment attribution and yield assumptions but does not create a new trade.

| Attribute | Original | Restated |
|---|---:|---:|
| Original client nominal | 4,000,000 | 4,000,000 |
| Reported factor | 0.8420 | 0.8300 |
| Current principal | 3,368,000 | 3,320,000 |
| Principal delta | - | -48,000 |

### Principal restatement

```text
original_current_principal = original_nominal x original_factor
original_current_principal = 4,000,000 x 0.8420 = 3,368,000

restated_current_principal = original_nominal x restated_factor
restated_current_principal = 4,000,000 x 0.8300 = 3,320,000

principal_restatement_delta = restated_current_principal - original_current_principal
principal_restatement_delta = 3,320,000 - 3,368,000 = -48,000
```

### Correct treatment

- preserve original trustee report, restated report, factor version, remittance date, restatement reason and impacted cashflows;
- update current principal, factor history, WAL/yield assumptions and prepayment attribution from the restated report;
- avoid booking a sale, redemption or realized loss solely because the trustee restated a factor;
- keep prior client statements and revised statement evidence linked;
- recalculate downstream risk and performance using effective restatement policy.

### QA assertions

| Test | Expected result |
|---|---|
| Trustee factor is restated | Current principal and factor history update with version lineage. |
| Prior statement was delivered | Correction or restatement workflow preserves prior values. |
| Cash receipt is unchanged | No duplicate cash transaction is created. |
| Analytics are recalculated | Yield, WAL and attribution use the restated factor from effective date. |

## Example 121. Sovereign Warrant Trigger Dispute

### Scenario

A sovereign GDP-linked warrant may pay out when reported GDP growth exceeds a threshold. The issuer and investors dispute the official statistic used for the trigger determination.

| Attribute | Value |
|---|---:|
| Warrant units held | 2,400 |
| Trigger payout per warrant | 14.00 |
| Issuer accepted payout | 8.50 |
| Disputed payout delta | 13,200 |

### Disputed payout

```text
investor_expected_payout = warrant_units x trigger_payout_per_warrant
investor_expected_payout = 2,400 x 14.00 = 33,600

issuer_accepted_payout = warrant_units x issuer_accepted_payout_per_warrant
issuer_accepted_payout = 2,400 x 8.50 = 20,400

disputed_payout_delta = investor_expected_payout - issuer_accepted_payout
disputed_payout_delta = 33,600 - 20,400 = 13,200
```

### Correct treatment

- preserve warrant terms, trigger statistic, calculation-agent notice, issuer position, investor claim and dispute status;
- keep expected payout, accepted payout and disputed delta separately visible;
- avoid recognizing disputed payout as received cash until settlement is confirmed;
- update valuation, recovery and reporting labels as provisional where trigger validity is unresolved;
- link the warrant dispute to the restructuring package without changing the new bond position.

### QA assertions

| Test | Expected result |
|---|---|
| Trigger statistic is disputed | Payout state becomes provisional or disputed. |
| Issuer pays lower amount | Accepted cash and disputed receivable remain separate. |
| Dispute is settled | Payout and receivable clear with source evidence. |
| Client report is generated | Trigger basis, accepted amount and disputed amount are explainable. |

## Example 122. Covered-Bond Reserve Replenishment Cure

### Scenario

A covered-bond program previously breached its liquidity-reserve requirement. The issuer replenishes part of the reserve, but the cure is incomplete until the reserve reaches the required level.

| Attribute | Value |
|---|---:|
| Required reserve level | 15,000,000 |
| Reserve before replenishment | 13,500,000 |
| Replenishment received | 1,100,000 |
| Reserve after replenishment | 14,600,000 |

### Remaining shortfall

```text
reserve_after_replenishment = reserve_before_replenishment + replenishment_received
reserve_after_replenishment = 13,500,000 + 1,100,000 = 14,600,000

remaining_reserve_shortfall = required_reserve_level - reserve_after_replenishment
remaining_reserve_shortfall = 15,000,000 - 14,600,000 = 400,000
```

### Correct treatment

- preserve cover-pool monitor report, replenishment notice, cash evidence, required reserve level, cure deadline and rating/watch state;
- mark the breach as partially cured until the remaining shortfall is zero or formally waived;
- update collateral eligibility, rating-watch labels and concentration analytics from source-backed cure status;
- avoid clearing a breach only because a replenishment payment was received;
- keep reserve drawdown, replenishment and remaining shortfall as separate support metrics.

### QA assertions

| Test | Expected result |
|---|---|
| Partial replenishment arrives | Remaining shortfall is calculated and breach remains open. |
| Full replenishment arrives | Cure state clears only with source-backed reserve level. |
| Cure deadline passes | Escalation or rating-watch workflow remains active. |
| Risk report is generated | Reserve before, replenishment and remaining shortfall are visible. |

## Example 123. Private-Placement Buyer Qualification Failure

### Scenario

A private-placement bond transfer waiver is granted subject to buyer qualification. Settlement must be blocked when the buyer fails qualified-investor or jurisdiction eligibility checks before transfer date.

| Attribute | Value |
|---|---:|
| Proposed transfer nominal | 1,500,000 |
| Transfer price | 97.25 |
| Gross transfer consideration | 1,458,750 |
| Buyer qualification status | Failed |

### Blocked transfer value

```text
blocked_transfer_value = proposed_transfer_nominal x transfer_price / 100
blocked_transfer_value = 1,500,000 x 97.25 / 100 = 1,458,750
```

### Correct treatment

- preserve private-placement memorandum, transfer waiver, buyer qualification evidence, failed eligibility reason, settlement instruction and cancellation evidence;
- block settlement and retain the holding when buyer qualification fails;
- release or reclassify any reserved settlement cash according to cancellation policy;
- keep restriction waiver and buyer qualification as separate controls;
- avoid treating failed transfer as redemption, default or completed sale.

### QA assertions

| Test | Expected result |
|---|---|
| Buyer qualification fails | Transfer settlement is blocked before position movement. |
| Waiver exists but buyer fails | Waiver alone is insufficient to complete transfer. |
| Cash was reserved | Reservation is released or held according to cancellation evidence. |
| Client report is generated | Failed qualification, blocked transfer and continuing holding are visible. |

## Example 124. Call Notice Amendment Cut-Off Conflict

### Scenario

An issuer sends an amended call notice after the custodian's corporate-action processing cut-off. The paying agent confirms the amended call price, but the custodian rejects the late amendment for that processing cycle. The platform must show the at-risk redemption cash without closing the position until the valid processing instruction is confirmed.

| Attribute | Value |
|---|---:|
| Affected nominal | 2,500,000 |
| Amended call price | 101.20 |
| Original call price | 100.75 |
| Custodian cut-off status | Missed |

### At-risk redemption cash

```text
at_risk_redemption_cash = affected_nominal x amended_call_price / 100
at_risk_redemption_cash = 2,500,000 x 101.20 / 100 = 2,530,000
```

### Correct treatment

- preserve original call notice, amended notice, paying-agent confirmation, custodian cut-off evidence and final processing instruction;
- show redemption cash as provisional or disputed when the amendment is economically valid but operationally rejected;
- do not close the position, recognize realized proceeds or update performance until the custodian confirms processing;
- keep notice validity, operational cut-off and cash projection as separate states;
- avoid overwriting the original notice without an audit trail.

### QA assertions

| Test | Expected result |
|---|---|
| Amendment arrives after cut-off | Position remains open and projected proceeds are flagged as at risk. |
| Paying agent confirms the amendment | Economic evidence is retained but does not override custodian processing state. |
| Custodian later accepts the amendment | Redemption proceeds and position closure use accepted processing evidence. |
| Client report is generated | Original price, amended price, cut-off miss and provisional cash state are visible. |

## Example 125. Escrow Reinvestment Shortfall

### Scenario

A defeasance escrow portfolio is reinvested after securities mature earlier than expected. The reinvestment yield is lower than the defeased bond liability yield, creating a projected shortfall against future principal and coupon obligations.

| Attribute | Value |
|---|---:|
| Required future liabilities | 10,000,000 |
| Projected escrow value after reinvestment | 9,875,000 |
| Reinvestment date | 2026-08-14 |
| Escrow status | Coverage shortfall |

### Escrow reinvestment shortfall

```text
escrow_reinvestment_shortfall = required_future_liabilities - projected_escrow_value_after_reinvestment
escrow_reinvestment_shortfall = 10,000,000 - 9,875,000 = 125,000
```

### Correct treatment

- retain escrow statement, reinvestment trade evidence, liability schedule, projected income assumptions and trustee shortfall notice;
- classify the issue as escrow coverage risk, not a current principal loss, until legal or trustee treatment confirms impact;
- surface the shortfall in risk and reporting while keeping the bond position and defeased liability schedule intact;
- require source-owner approval before using alternative reinvestment assumptions;
- avoid blending escrow income, coupon income and shortfall remediation into one cashflow type.

### QA assertions

| Test | Expected result |
|---|---|
| Reinvestment yield falls below assumption | Escrow coverage shortfall is calculated from source schedules. |
| Trustee provides updated projection | Shortfall uses trustee-backed projection rather than manual estimate. |
| Shortfall is not yet crystallized | Position is not written down solely from projected shortfall. |
| Risk report is generated | Required liabilities, projected escrow value and shortfall are visible. |

## Example 126. ABS Waterfall Restatement Dispute

### Scenario

An ABS trustee restates the monthly waterfall allocation after a servicer correction. Principal and interest distributions both change, and investors dispute whether the restated allocation should be applied to previously reported cashflows.

| Distribution component | Original | Restated |
|---|---:|---:|
| Principal distribution | 180,000 | 166,500 |
| Interest distribution | 42,000 | 47,250 |

### Restatement deltas

```text
principal_delta = restated_principal_distribution - original_principal_distribution
principal_delta = 166,500 - 180,000 = -13,500

interest_delta = restated_interest_distribution - original_interest_distribution
interest_delta = 47,250 - 42,000 = 5,250

cashflow_restatement_delta = principal_delta + interest_delta
cashflow_restatement_delta = -13,500 + 5,250 = -8,250
```

### Correct treatment

- preserve original trustee report, restated trustee report, servicer correction evidence, waterfall rule version and investor dispute status;
- restate principal and interest components separately because they affect position factor, income, yield and tax treatment differently;
- keep disputed restatement amounts visible until trustee finality and custodian booking agree;
- update analytics only from the accepted restatement basis;
- avoid treating the net delta as a simple fee, price movement or trade correction.

### QA assertions

| Test | Expected result |
|---|---|
| Restatement changes principal and interest | Separate deltas are calculated and retained. |
| Investor dispute remains open | Reporting marks cashflow restatement as disputed or provisional. |
| Custodian booking differs from trustee file | Break is routed to reconciliation rather than silently accepted. |
| Performance is calculated | Income, principal return and valuation effects remain separated. |

## Example 127. Sovereign Warrant Valuation Source Override

### Scenario

A sovereign GDP-linked warrant is thinly traded and the vendor price is stale. The valuation committee approves a temporary override based on a documented methodology and market inputs. Reporting must show the override delta and retain the approved valuation source.

| Attribute | Value |
|---|---:|
| Warrant units | 12,000 |
| Vendor price | 18.40 |
| Committee-approved price | 21.75 |
| Override status | Approved |

### Valuation override delta

```text
valuation_override_delta = warrant_units x (committee_price - vendor_price)
valuation_override_delta = 12,000 x (21.75 - 18.40) = 40,200
```

### Correct treatment

- retain vendor price, stale-price evidence, committee approval, methodology note, market inputs and override expiry;
- classify the valuation as committee-approved rather than market-executable when liquidity is weak;
- include the override delta in valuation control reporting and portfolio valuation audit trails;
- require expiry, renewal or reversion logic for temporary overrides;
- avoid using the committee price as implied execution liquidity without separate trading evidence.

### QA assertions

| Test | Expected result |
|---|---|
| Vendor price is stale | Override requires approval and methodology evidence. |
| Committee price is active | Portfolio valuation uses approved source with source flag. |
| Override expires | Valuation reverts or requires renewed approval. |
| Client report is generated | Valuation basis and override status are available for disclosure or advisor explanation. |

## Example 128. Covered-Bond Cure Deadline Waiver

### Scenario

A covered-bond program misses an overcollateralization cure deadline. The trustee grants a limited waiver and extends the deadline while the breach remains unresolved. The platform must not mark the breach as cured only because the deadline changed.

| Attribute | Value |
|---|---:|
| Original cure deadline | 2026-09-30 |
| Revised cure deadline | 2026-10-31 |
| Required collateral top-up | 18,000,000 |
| Confirmed collateral top-up | 6,500,000 |

### Deadline extension and remaining top-up

```text
days_extended = revised_cure_deadline - original_cure_deadline
days_extended = 31 days

remaining_top_up = required_collateral_top_up - confirmed_collateral_top_up
remaining_top_up = 18,000,000 - 6,500,000 = 11,500,000
```

### Correct treatment

- preserve original breach notice, waiver approval, revised deadline, collateral top-up evidence and rating-agency or trustee commentary;
- keep breach status, waiver status and cure completion as separate fields;
- continue risk monitoring until the remaining top-up is fully confirmed;
- show waiver expiry and escalation trigger in operations and risk reporting;
- avoid resetting breach age or hiding the original missed deadline.

### QA assertions

| Test | Expected result |
|---|---|
| Waiver extends deadline | Breach remains open with revised deadline and waiver evidence. |
| Partial top-up arrives | Remaining top-up is calculated and monitoring continues. |
| Revised deadline expires | Escalation triggers if cure evidence is incomplete. |
| Risk report is generated | Original deadline, revised deadline, waiver status and remaining top-up are visible. |

## Example 129. Private-Placement Resale Legend Removal

### Scenario

A restricted private-placement bond becomes eligible for resale after the holding period and issuer counsel opinion are complete. The resale legend is removed for part of the holding. The event changes transferability but is not itself a sale, redemption or price change.

| Attribute | Value |
|---|---:|
| Total restricted nominal | 3,000,000 |
| Nominal released from legend | 1,200,000 |
| Remaining restricted nominal | 1,800,000 |
| Legal opinion status | Received |

### Transferable percentage

```text
transferable_percentage = released_nominal / total_restricted_nominal
transferable_percentage = 1,200,000 / 3,000,000 = 40.00%
```

### Correct treatment

- retain issuer notice, counsel opinion, holding-period evidence, legend removal instruction and custodian eligibility confirmation;
- update transferability and restriction status without creating a sale or redemption transaction;
- keep released and restricted nominal visible when only part of the holding is released;
- ensure suitability, advisory and reporting controls can distinguish market liquidity from legal transferability;
- avoid using legend removal as proof of executable liquidity or client consent to sell.

### QA assertions

| Test | Expected result |
|---|---|
| Legal opinion is received | Transferability can be updated only with source-backed evidence. |
| Partial nominal is released | Released and remaining restricted nominal are both visible. |
| No trade is instructed | Position quantity and cost basis remain unchanged. |
| Client report is generated | Restriction status and eligible transferable nominal are available. |

## Example 130. Call Event Beneficial-Owner Election Conflict

### Scenario

A callable bond event allows beneficial owners to elect whether holdings should be called, retained or tendered through an intermediary. Two beneficial-owner instruction files conflict for the same omnibus position. The platform must preserve election conflict state and avoid processing the call as if all beneficial-owner instructions were aligned.

| Attribute | Value |
|---|---:|
| Omnibus nominal | 8,000,000 |
| Election file A call nominal | 5,000,000 |
| Election file B call nominal | 4,250,000 |
| Overlapping disputed nominal | 1,250,000 |

### Conflict amount

```text
unresolved_call_election_nominal = overlapping_disputed_nominal
confirmed_call_nominal = min(election_file_a_call_nominal, election_file_b_call_nominal) - unresolved_call_election_nominal
confirmed_call_nominal = 4,250,000 - 1,250,000 = 3,000,000
```

### Correct treatment

- preserve call notice, intermediary election files, beneficial-owner mapping, conflict reason and resolution owner;
- process only confirmed election nominal while disputed nominal remains blocked or pending;
- avoid applying one beneficial owner's instruction across another beneficial owner's allocation;
- show disputed call election state in operations and client reporting where material;
- retain original and corrected instruction versions for audit.

### QA assertions

| Test | Expected result |
|---|---|
| Conflicting election files arrive | Disputed nominal is identified and blocked from automatic call processing. |
| One file is corrected | Confirmed and disputed nominal are recalculated from latest source evidence. |
| Omnibus position is reported | Beneficial-owner election status is not collapsed into a single account-level flag. |
| Event closes | Resolution evidence is required for all disputed nominal. |

## Example 131. Escrow Asset Substitution Dispute

### Scenario

A municipal escrow defeasance portfolio substitutes one escrow security for another. The trustee accepts the substitution, but the analytics team disputes whether the replacement assets fully cover scheduled debt-service cashflows.

| Attribute | Value |
|---|---:|
| Required escrow coverage value | 12,600,000 |
| Replacement escrow asset value | 12,240,000 |
| Disputed shortfall | 360,000 |
| Next debt-service date | 90 days |

### Escrow substitution shortfall

```text
escrow_substitution_shortfall = required_escrow_coverage_value - replacement_escrow_asset_value
escrow_substitution_shortfall = 12,600,000 - 12,240,000 = 360,000
```

### Correct treatment

- preserve trustee substitution notice, asset list, valuation date, coverage test, disputed assumption and reviewer;
- update escrow asset composition only from trustee-approved source evidence;
- keep coverage dispute separate from bond default, call or redemption state;
- show shortfall and dispute state in risk and operations dashboards until resolved;
- avoid using stale escrow values for client reporting after substitution.

### QA assertions

| Test | Expected result |
|---|---|
| Replacement value is below required coverage | Shortfall is calculated and dispute remains open. |
| Trustee accepts substitution | Asset composition updates but coverage dispute remains separately visible. |
| Valuation date changes | Coverage test recalculates from effective-dated source values. |
| Report is generated | Escrow substitution and coverage dispute are source-labelled. |

## Example 132. ABS Residual Certificate Cashflow Correction

### Scenario

An ABS residual certificate receives a corrected trustee report after the original waterfall allocated too much residual cashflow. The correction reduces income previously reported and requires a restatement path.

| Attribute | Value |
|---|---:|
| Original residual cashflow | 420,000 |
| Corrected residual cashflow | 315,000 |
| Prior period reported income | 420,000 |
| Correction amount | 105,000 |

### Residual cashflow correction

```text
residual_cashflow_correction = original_residual_cashflow - corrected_residual_cashflow
residual_cashflow_correction = 420,000 - 315,000 = 105,000
```

### Correct treatment

- preserve original trustee report, corrected trustee report, waterfall version, affected period and approval state;
- restate income, cashflow and performance treatment according to correction policy;
- keep correction separate from current-period residual cashflow;
- explain corrected residual income in client and management reporting where prior reports are affected;
- reconcile cash received, income recognized and residual certificate carrying value.

### QA assertions

| Test | Expected result |
|---|---|
| Corrected trustee report arrives | Prior residual cashflow is adjusted through correction workflow. |
| Cash already settled | Ledger cash and income restatement are reconciled separately. |
| Performance report is regenerated | Prior-period contribution reflects approved correction handling. |
| Correction is unapproved | Client-facing restatement is blocked. |

## Example 133. Sovereign Warrant Expiry Acceleration

### Scenario

A sovereign restructuring warrant has an expiry acceleration clause that is triggered by a settlement amendment. The accelerated expiry reduces optionality and requires valuation and reporting updates.

| Attribute | Value |
|---|---:|
| Original days to expiry | 420 |
| Accelerated days to expiry | 180 |
| Days lost | 240 |
| Warrant notional | 6,000,000 |

### Expiry acceleration

```text
days_lost_to_acceleration = original_days_to_expiry - accelerated_days_to_expiry
days_lost_to_acceleration = 420 - 180 = 240
```

### Correct treatment

- preserve amendment notice, trigger evidence, original expiry, accelerated expiry, valuation source and custodian confirmation;
- update warrant lifecycle state and valuation assumptions from effective date;
- keep expiry acceleration separate from warrant exercise or expiry event;
- explain optionality reduction in risk and client reporting where material;
- block stale valuations that still use original expiry after acceleration becomes effective.

### QA assertions

| Test | Expected result |
|---|---|
| Acceleration trigger is confirmed | Effective-dated expiry is updated. |
| Valuation source still uses old expiry | Stale valuation warning or block is raised. |
| Warrant expires early | Expiry processing uses accelerated date and source evidence. |
| Client report is generated | Original expiry, accelerated expiry and valuation basis are explainable. |

## Example 134. Covered-Bond Post-Waiver Monitoring Failure

### Scenario

A covered-bond collateral breach receives a waiver and revised cure deadline. Monitoring after the waiver misses a required weekly trustee update, weakening evidence that the cure is on track.

| Attribute | Value |
|---|---:|
| Required monitoring updates | 4 |
| Received monitoring updates | 2 |
| Missing updates | 2 |
| Remaining top-up | 11,500,000 |

### Monitoring gap

```text
missing_monitoring_updates = required_monitoring_updates - received_monitoring_updates
missing_monitoring_updates = 4 - 2 = 2
```

### Correct treatment

- preserve waiver terms, monitoring schedule, received trustee updates, missing update dates and escalation owner;
- keep waiver active while separately flagging post-waiver monitoring failure;
- avoid treating cure as on-track without required trustee evidence;
- escalate missing updates before revised cure deadline expires;
- show remaining top-up and monitoring gap together in risk reporting.

### QA assertions

| Test | Expected result |
|---|---|
| Required updates are missing | Monitoring failure is opened even if waiver remains valid. |
| Top-up is partial | Remaining top-up and missing updates are both visible. |
| Trustee update arrives late | Monitoring state records late evidence and recalculates gap. |
| Risk dashboard is generated | Waiver, cure, remaining top-up and monitoring status are separate fields. |

## Example 135. Private-Placement Lock-Up Period Release Dispute

### Scenario

A private-placement bond lock-up period appears to end based on trade date, but issuer counsel says the lock-up runs from settlement date. The release date dispute affects transferability and suitability review.

| Attribute | Value |
|---|---:|
| Trade-date based release date | Day 365 |
| Settlement-date based release date | Day 368 |
| Disputed release delay | 3 days |
| Restricted nominal | 2,400,000 |

### Lock-up release delay

```text
disputed_release_delay_days = settlement_date_based_release_date - trade_date_based_release_date
disputed_release_delay_days = 368 - 365 = 3
```

### Correct treatment

- preserve subscription agreement, trade date, settlement date, counsel interpretation, issuer notice and custodian restriction status;
- keep nominal restricted until the authoritative lock-up basis is resolved;
- avoid treating lock-up release as a trade, redemption or liquidity guarantee;
- explain disputed transferability in advisory and reporting workflows where a sale or transfer is requested;
- retain final interpretation as source evidence for future holdings under the same terms.

### QA assertions

| Test | Expected result |
|---|---|
| Release-date basis conflicts | Restriction remains active and dispute is visible. |
| Counsel opinion confirms settlement-date basis | Release date updates with source evidence. |
| Transfer is requested during disputed period | Transfer is blocked or routed to exception approval. |
| Report is generated | Lock-up end date, disputed basis and restricted nominal are visible. |

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

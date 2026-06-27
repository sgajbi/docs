# Cross-Product Worked Examples

This page provides compact worked examples across the product families covered so far.

Use these examples for learning, advisory review, business analysis, data modelling, reporting design, QA scenarios, and implementation planning. They are intentionally vendor-neutral and should be adapted to the target platform, jurisdiction, booking policy, product terms, and source-system contracts.

## Example 1: Unsettled Equity Sale Proceeds

| Dimension | Example |
|---|---|
| Product family | Cash, deposits, money market and FX; equities |
| Client objective | Sell an equity position and use proceeds for a new purchase. |
| Product terms | Sell USD 100,000 of listed equity on trade date T, settlement T+2. |
| Key distinction | Trade-date proceeds are not automatically available cash. |

### Lifecycle

| Date | Event | Platform treatment |
|---|---|---|
| T | Equity sell trade executed | Reduce trade-date exposure; create pending settlement receivable. |
| T to T+1 | Cash not settled | Show projected cash, not settled available cash. |
| T+2 | Cash settles | Increase settled and available USD cash if no restrictions apply. |

### Calculations

```text
Projected cash = ledger cash + pending sell settlement - pending buys - fees - tax
Available cash before settlement = ledger cash - reservations - restrictions
Available cash after settlement = ledger cash + settled proceeds - reservations - restrictions
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Do not fund a purchase from unsettled cash unless policy allows bridge funding. |
| DPM/mandate | Rebalance should know whether the cash is trade-date, settlement-date, or available. |
| Reporting | Show pending settlement separately from settled cash. |
| QA | A T+0 buy using T+2 proceeds should block or show explicit bridge-funding support. |
| Boundary | Do not net all currencies and dates into one available-cash number. |

## Example 2: Callable Corporate Bond With Yield-To-Worst

| Dimension | Example |
|---|---|
| Product family | Bonds |
| Client objective | Earn income from an investment-grade callable corporate bond. |
| Product terms | 5-year USD bond, 5 percent coupon, callable after year 3 at 101, maturity at 100. |
| Key distinction | Yield-to-maturity may overstate expected return if the issuer is likely to call. |

### Lifecycle

| Event | Treatment |
|---|---|
| Purchase | Book clean price, accrued interest, settlement cash, cost basis. |
| Coupon dates | Accrue and receive interest according to day-count convention. |
| Call date | If called, redeem at call price and stop future coupon schedule. |
| Maturity | If not called, redeem at par. |

### Calculations

| Calculation | Purpose |
|---|---|
| Accrued interest | Separates earned coupon from clean price movement. |
| Yield-to-maturity | Return if held to final maturity. |
| Yield-to-call | Return if redeemed at first call date. |
| Yield-to-worst | Lowest yield across eligible redemption scenarios. |
| Duration/spread | Rate and credit-risk sensitivity. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain reinvestment risk and issuer call optionality. |
| DPM/mandate | Include in fixed-income allocation, duration, issuer and credit limits. |
| Reporting | Show maturity ladder and callable date separately where relevant. |
| QA | Validate accrued interest, dirty value, call redemption, and yield-to-worst. |
| Boundary | Do not treat call exercise as a sale by the client. |

## Example 3: Equity Dividend With Withholding Tax

| Dimension | Example |
|---|---|
| Product family | Equities |
| Client objective | Hold dividend-paying shares for income and growth. |
| Product terms | 1,000 shares, dividend USD 2.00 per share, 15 percent withholding tax. |
| Key distinction | Gross dividend, tax, and net cash are separate economic facts. |

### Lifecycle

| Date | Event | Platform treatment |
|---|---|---|
| Ex-date | Entitlement determined | Position must be eligible on the correct date. |
| Record date | Issuer/custodian confirms entitlement | Capture receivable if supported. |
| Pay date | Cash received | Book gross income, withholding tax, and net cash. |

### Calculations

```text
Gross dividend = 1,000 x 2.00 = 2,000
Withholding tax = 2,000 x 15% = 300
Net cash = 1,700
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain that dividend yield is not guaranteed and tax can reduce cash received. |
| DPM/mandate | Income strategy should separate dividend income from price return. |
| Reporting | Show gross income, tax withheld, and net income. |
| QA | Validate ex-date entitlement, FX conversion if reporting currency differs, and tax classification. |
| Boundary | Do not classify tax withholding as investment loss. |

## Example 4: Fund Redemption Gate

| Dimension | Example |
|---|---|
| Product family | Funds |
| Client objective | Redeem a fund position for liquidity. |
| Product terms | Client requests 100 percent redemption; fund applies 25 percent gate. |
| Key distinction | Redemption request is not the same as completed redemption cash. |

### Lifecycle

| Step | Treatment |
|---|---|
| Submit redemption | Create pending order and reduce available units if policy requires. |
| Gate applied | Redeem 25 percent; keep 75 percent pending or continuing position. |
| NAV struck | Use correct dealing NAV and NAV date. |
| Settlement | Cash arrives on settlement date. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain liquidity limitation and timing uncertainty. |
| DPM/mandate | Do not assume full redemption cash is available for rebalance. |
| Reporting | Show redeemed, pending, and remaining units separately. |
| QA | Validate partial redemption, settlement date, remaining exposure, and stale NAV handling. |
| Boundary | Do not mark the full order as complete when only the gated portion settled. |

## Example 5: Lombard Availability And Margin Call

| Dimension | Example |
|---|---|
| Product family | Loans, Lombard, margin and collateral |
| Client objective | Borrow against an investment portfolio without selling assets. |
| Product terms | Approved facility USD 800,000; collateral market value USD 1,000,000; LTV 60 percent; exposure USD 300,000. |
| Key distinction | Availability is limited by both facility headroom and collateral headroom. |

### Calculations

```text
Lending value = 1,000,000 x 60% = 600,000
Facility headroom = 800,000 - 300,000 = 500,000
Collateral headroom = 600,000 - 300,000 = 300,000
Availability = min(500,000, 300,000) = 300,000
```

### Price-Fall Stress

```text
New collateral value = 850,000
New lending value = 850,000 x 60% = 510,000
Exposure = 550,000
Shortfall = 40,000
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain leverage, margin-call risk, interest cost, and forced-sale risk. |
| DPM/mandate | Enforce leverage limits and eligible-collateral policy. |
| Reporting | Show facility limit, drawn exposure, collateral value, lending value, utilization, and shortfall. |
| QA | Validate LTV, haircut, in-flight order reservation, stale price block, and margin-call trigger. |
| Boundary | Do not calculate buying power from market value alone. |

## Example 6: Private Equity Capital Call And Distribution

| Dimension | Example |
|---|---|
| Product family | Private markets |
| Client objective | Commit to a private equity fund and fund capital calls over time. |
| Product terms | Commitment USD 1,000,000; initial capital call USD 200,000; later distribution USD 100,000. |
| Key distinction | NAV is not commitment, and unfunded commitment is a future obligation. |

### Lifecycle And Calculations

| Event | Calculation | Treatment |
|---|---|---|
| Commitment | Unfunded = 1,000,000 | Record legal commitment without cash movement if no call yet. |
| Capital call | Paid-in = 200,000; unfunded = 800,000 | Cash decreases; paid-in capital increases. |
| NAV report | NAV = 190,000 | Market value reflects manager/admin valuation and date. |
| Distribution | Income 20,000; return of capital 50,000; gain 30,000 | Cash increases and reporting classification matters. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain illiquidity, unfunded obligations, valuation lag, and J-curve. |
| DPM/mandate | Track private-asset allocation using NAV and unfunded commitments where policy requires. |
| Reporting | Show commitment, paid-in, unfunded, NAV, distributions, IRR, TVPI, DPI, RVPI. |
| QA | Validate capital call, distribution classification, NAV date, stale NAV flag, and missing-history behavior. |
| Boundary | Do not fabricate IRR when historical cashflows are incomplete. |

## Example 7: FX Forward Hedge

| Dimension | Example |
|---|---|
| Product family | Derivatives; cash, deposits, money market and FX |
| Client objective | Hedge EUR portfolio exposure into USD reporting currency. |
| Product terms | Sell EUR 500,000 forward against USD, 3-month maturity. |
| Key distinction | FX forward is an open contract, not current cash. |

### Lifecycle

| Event | Treatment |
|---|---|
| Trade date | Book forward contract with buy/sell currency legs and forward rate. |
| Daily/monthly valuation | Mark to market using forward curve or valuation source. |
| Maturity | Settle currency legs or net cash depending contract type. |
| Roll | Close old forward and open new forward if hedge continues. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain hedge objective, obligation, costs, and residual basis risk. |
| DPM/mandate | Measure hedge ratio against underlying exposure and mandate permissions. |
| Reporting | Show gross currency exposure, hedge notional, net exposure, MTM, realized P&L. |
| QA | Validate value date, currency signs, FX rate convention, MTM, maturity settlement, and hedge link. |
| Boundary | Do not treat forward notional as cash balance before settlement. |

## Example 8: Autocallable Structured Product Path

| Dimension | Example |
|---|---|
| Product family | Structured products; structured notes |
| Client objective | Earn conditional income linked to an equity basket. |
| Product terms | USD autocallable note, quarterly observations, 8 percent annual coupon if basket above coupon barrier, autocall if basket above call level, downside barrier at maturity. |
| Key distinction | Observation results are lifecycle events; transactions occur only when cash or assets are posted. |

### Lifecycle

| Observation | Result | Treatment |
|---|---|---|
| Quarter 1 | Coupon barrier met, autocall not met | Book coupon only when confirmed and payable. |
| Quarter 2 | Autocall level met | Book redemption and coupon if terms require. |
| Maturity fallback | Barrier breached and no autocall | Calculate cash loss or physical delivery per terms. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain conditional income, path dependency, issuer risk, liquidity limits, and downside risk. |
| DPM/mandate | Monitor issuer concentration, underlying exposure, complexity, and suitability. |
| Reporting | Show note position, issuer, underlyings, barrier distance, next observation, coupon status, scenario outcomes. |
| QA | Validate observation schedule, coupon condition, autocall trigger, barrier event, and physical-delivery booking. |
| Boundary | Do not create separate client positions for embedded derivatives unless legal delivery occurs. |

## Example 9: Equity-Linked Note Physical Delivery

| Dimension | Example |
|---|---|
| Product family | Structured notes; equities |
| Client objective | Earn enhanced coupon while accepting potential equity delivery. |
| Product terms | Equity-linked note references one stock; if final price below strike, client receives shares instead of full cash redemption. |
| Key distinction | The client owns the note until shares are legally delivered. |

### Lifecycle

| Event | Treatment |
|---|---|
| Subscription | Book note position and cash outflow. |
| Coupon date | Book coupon if confirmed by terms and issuer notice. |
| Final fixing | Determine cash redemption or physical delivery. |
| Physical delivery | Close note and create equity position with cost-basis policy. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain that equity exposure is contingent before delivery and direct after delivery. |
| DPM/mandate | Check whether delivered shares are permitted and whether single-name limits are breached. |
| Reporting | Before delivery show note plus underlying look-through; after delivery show direct equity. |
| QA | Validate final fixing, share quantity, cash residual, cost basis, and note closure. |
| Boundary | Do not report direct equity ownership before physical settlement. |

## Example 10: Cross-Product Rebalance With Funding Constraint

| Dimension | Example |
|---|---|
| Product families | Cash, funds, equities, bonds, lending/collateral |
| Client objective | Rebalance toward target allocation while respecting liquidity and credit constraints. |
| Product terms | Need USD 150,000 for purchases; USD cash ledger shows 200,000; available cash only 90,000 after pending settlements and pledge reserve. |
| Key distinction | Rebalance feasibility depends on available cash and buying power, not ledger cash. |

### Decision Flow

1. Check available cash by currency and value date.
2. Check pending sell settlements and settlement dates.
3. Check pledge and collateral restrictions.
4. Check whether credit facility or margin buying power is permitted.
5. If funding is insufficient, either reduce purchases, add sell orders, wait for settlement, or flag blocked funding.

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain why a model-aligned trade can still be blocked by funding, settlement, or pledge rules. |
| DPM/mandate | Show drift, proposed rebalance, funding gap, and supportability state. |
| Reporting | Separate target drift from execution/funding readiness. |
| QA | Validate ledger cash, available cash, reserved cash, pledge reserve, and proposed order funding. |
| Boundary | Do not let a portfolio-construction view override source-owned cash and collateral limits. |

## Example 11: Precious Metal Exposure And Collateral

| Dimension | Example |
|---|---|
| Product families | Commodities, precious metals and real assets; lending/collateral |
| Client objective | Hold gold exposure and use part of it as Lombard collateral. |
| Product terms | 100 troy oz allocated gold account; price USD 2,300/oz; collateral haircut 25 percent; 40 oz pledged. |
| Key distinction | Commodity exposure, physical/account holding, and collateral availability are separate views. |

### Calculations

```text
Gold market value = 100 x 2,300 = 230,000
Pledged market value = 40 x 2,300 = 92,000
Lending value = 92,000 x (1 - 25%) = 69,000
Unpledged quantity = 60 oz
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain custody/provider risk, price volatility, liquidity, unit basis, and collateral haircut. |
| DPM/mandate | Include gold exposure in commodity allocation and pledged quantity in availability constraints. |
| Reporting | Show quantity, unit, price source, market value, pledged quantity, available quantity, and lending value. |
| QA | Validate troy-ounce unit, price unit, FX translation, pledge quantity, haircut, and stale-price block. |
| Boundary | Do not calculate lending value from total gold quantity when only part is pledged and eligible. |

## Example 12: ILP Policy Value And Surrender

| Dimension | Example |
|---|---|
| Product family | Insurance and annuities |
| Client objective | Hold an investment-linked policy for protection and long-term investment exposure. |
| Product terms | Annual premium USD 20,000; ILP account value USD 110,000; surrender charge USD 4,000; policy loan USD 15,000; death benefit USD 250,000. |
| Key distinction | Account value, surrender value, and death benefit are different values with different reporting meanings. |

### Calculations

```text
Net surrender value = 110,000 - 4,000 - 15,000 = 91,000
Protection benefit = 250,000
Investable/account value = 110,000 before surrender and loan adjustments
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain charges, market risk, protection benefit, policy loan impact, surrender consequences, and lapse risk. |
| DPM/mandate | Treat ILP sub-fund exposure as look-through only when source-backed; do not treat death benefit as investable assets. |
| Reporting | Show account value, net surrender value, death benefit, loan balance, premium schedule, source date, and guarantee/projection basis separately. |
| QA | Validate value basis labels, loan deduction, surrender charge, premium status, restricted beneficiary data, and stale insurer quote handling. |
| Boundary | Do not use death benefit or projected non-guaranteed value as current portfolio market value. |

## Example 13: Listed REIT Income Versus Real-Asset Exposure

| Dimension | Example |
|---|---|
| Product families | Real estate, REITs and infrastructure; equities; funds |
| Client objective | Hold listed REIT exposure for income and real-asset allocation. |
| Product terms | 10,000 REIT units; market price SGD 2.10; quarterly distribution SGD 0.035 per unit; reported property exposure is 60 percent logistics and 40 percent data centres. |
| Key distinction | The listed REIT trades like an equity, but allocation and risk reporting need real-estate sector and leverage context. |

### Calculations

```text
Market value = 10,000 x 2.10 = SGD 21,000
Gross distribution = 10,000 x 0.035 = SGD 350
Logistics exposure = 21,000 x 60% = SGD 12,600
Data centre exposure = 21,000 x 40% = SGD 8,400
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain that distribution yield is variable and affected by rents, occupancy, leverage, refinancing cost, and property cycle. |
| DPM/mandate | Treat trading, settlement and corporate actions like listed securities, but classify economic exposure as real estate by sector/geography where source-backed. |
| Reporting | Show listed holding, price, market value, distribution, property-sector exposure, geography, leverage notes, and source date. |
| QA | Validate market value, distribution entitlement, withholding/tax treatment if relevant, sector look-through, stale source date, and corporate-action handling. |
| Boundary | Do not infer full property-sector exposure when the manager/index/source file does not provide reliable look-through data. |

## Example 14: Daily TWR With External Cashflow

| Dimension | Example |
|---|---|
| Product families | Portfolio performance; cash; equities; funds |
| Client objective | Measure manager performance while excluding the timing effect of a client deposit. |
| Product terms | Portfolio starts at USD 1,000,000, receives external cashflow USD 100,000 at midday, and ends at USD 1,125,000. Valuation immediately before the cashflow is USD 1,020,000. |
| Key distinction | External client cashflow should not be treated as investment return. |

### Calculations

```text
Sub-period 1 return = 1,020,000 / 1,000,000 - 1 = 2.00%
Capital after external flow = 1,020,000 + 100,000 = 1,120,000
Sub-period 2 return = 1,125,000 / 1,120,000 - 1 = 0.4464%
Daily TWR = (1 + 2.00%) x (1 + 0.4464%) - 1 = 2.4553%
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain that the deposit increased portfolio value but is not manager return. |
| DPM/mandate | Use TWR for manager/model comparison and keep MWR for client experience where required. |
| Reporting | Show beginning value, external flow, ending value, TWR, and supportability state. |
| QA | Validate cashflow classification, intraday valuation availability, compounding, rounding, and missing-valuation fallback. |
| Boundary | Do not calculate TWR from beginning value, ending value, and net flow alone when required sub-period valuations are missing. |

## Example 15: Brinson Attribution With Sector Overweight

| Dimension | Example |
|---|---|
| Product families | Portfolio performance; equities; funds; model portfolios |
| Client objective | Explain why a portfolio outperformed its equity benchmark. |
| Product terms | Portfolio holds 30 percent technology versus benchmark 20 percent. Technology returns 8 percent; benchmark total return is 4 percent. |
| Key distinction | Outperformance can come from allocation, selection, interaction, currency, and residual effects. |

### Treatment

| Component | Meaning |
|---|---|
| Allocation effect | Did overweight or underweight sectors help relative return? |
| Selection effect | Did selected securities/funds outperform their benchmark segment? |
| Currency effect | Did reporting-currency translation help or hurt? |
| Residual | Should be explained or labelled partial, not buried silently. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain allocation versus selection in language suitable for portfolio review. |
| DPM/mandate | Compare actual portfolio exposures to model/benchmark targets and tolerance bands. |
| Reporting | Show attribution dimension, benchmark, coverage percentage, residual, and source date. |
| QA | Validate benchmark mapping, sector classification, weight timing, local/reporting currency basis, and residual threshold. |
| Boundary | Do not publish sector attribution when benchmark or portfolio classifications are missing for a material portion of the portfolio. |

## Example 16: Model Portfolio Version Rollout

| Dimension | Example |
|---|---|
| Product families | Model portfolios; funds; equities; bonds; DPM |
| Client objective | Roll a balanced model from version 12 to version 13 after an investment committee update. |
| Product terms | New target reduces equities from 55 percent to 50 percent, increases short-duration bonds from 25 percent to 30 percent, keeps alternatives at 10 percent, and cash at 10 percent. |
| Key distinction | A model change is not automatically an executable trade list. |

### Workflow

| Step | Expected treatment |
|---|---|
| Model approval | Store model version, approval date, effective date, and rationale. |
| Portfolio eligibility | Identify mandates, restrictions, exclusions, stale data, and client-specific overlays. |
| Drift calculation | Compare current exposure to new target using declared exposure lens. |
| Trade generation | Create candidate sells/buys subject to funding, tax, cost, liquidity, and product constraints. |
| Approval and execution | Separate review, approval, order handoff, execution acknowledgement, fills, and settlement. |
| Post-trade review | Measure residual drift and blocked or deferred actions. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain model change, rationale, expected impact, risks, costs, and client approval path where required. |
| DPM/mandate | Enforce mandate eligibility, model version, tolerance bands, override policy, and audit evidence. |
| Reporting | Show old target, new target, current exposure, proposed changes, blocked items, expected residual drift, and source state. |
| QA | Validate versioning, affected portfolio selection, restriction blocking, funding checks, and post-trade drift. |
| Boundary | Do not claim execution or settlement from a model-rollout artifact unless external order and settlement evidence is sourced. |

## Example 17: Advisory Proposal Expiry And Consent

| Dimension | Example |
|---|---|
| Product families | Advisory proposals; structured products; funds; equities |
| Client objective | Review and approve a recommendation before market conditions or suitability evidence becomes stale. |
| Product terms | Proposal recommends selling an overweight equity and buying a balanced fund. The proposal expires after seven calendar days or if risk profile changes. |
| Key distinction | A proposal is not an order until valid consent and pre-trade checks are complete. |

### Workflow

| Event | Treatment |
|---|---|
| Draft | Store recommendation, rationale, alternatives, assumptions, and source snapshot. |
| Review | Run suitability, concentration, liquidity, cost, product eligibility, and restriction checks. |
| Consent captured | Record authorized party, timestamp, consent scope, and proposal version. |
| Proposal expired | Block order creation and require refresh or new approval. |
| Market/source change | Re-run checks if price, risk profile, product eligibility, or portfolio state materially changed. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain recommendation rationale, alternatives, risks, cost, suitability posture, and approval validity. |
| DPM/mandate | Do not mix advisory consent workflow with DPM discretionary authority. |
| Reporting | Show proposal status, consent status, expiry, assumptions, and blocked reasons. |
| QA | Validate expiry, stale-source trigger, risk-profile change, partial approval, and order-block behavior. |
| Boundary | Do not treat expired or partially approved proposal actions as executable. |

## Example 18: Outcome Review After Rebalance

| Dimension | Example |
|---|---|
| Product families | DPM; portfolio performance; risk; reporting |
| Client objective | Review whether a rebalance achieved the intended portfolio outcome. |
| Product terms | Portfolio had equity overweight and insufficient cash buffer. Rebalance sold equities, bought short-duration bonds, and reserved cash for upcoming withdrawals. |
| Key distinction | Outcome review needs source-backed pre-trade, trade, post-trade, performance, risk, and report evidence. |

### Review Evidence

| Evidence | Expected use |
|---|---|
| Pre-trade state | Target, current exposure, drift, restrictions, cash, risk, and source quality. |
| Proposed actions | Candidate trades, alternatives, cost, tax, liquidity, and blocked reasons. |
| Execution evidence | Order handoff, acknowledgement, fills, settlement, and reconciliation when available. |
| Post-trade state | Holdings, cash, residual drift, mandate health, risk, and performance. |
| Report evidence | Advisor/PM notes, client-report sections, source dates, supportability, and archive status where relevant. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain outcome versus intent without claiming unavailable execution or suitability evidence. |
| DPM/mandate | Track residual drift, exceptions, deferred actions, breached limits, and follow-up review. |
| Reporting | Show pre/post allocation, executed versus proposed actions, cash buffer, risk movement, and supportability notes. |
| QA | Validate lineage from model target to proposal/trade/post-trade state and confirm blocked evidence remains visible. |
| Boundary | Do not reconstruct missing fills, settlement, client communication, or external execution status from internal rebalance evidence. |

## Example 19: Bond Downgrade And Collateral Haircut Change

| Dimension | Example |
|---|---|
| Product families | Bonds; loans, Lombard, margin and collateral; risk reporting |
| Client objective | Hold an income bond that is also pledged as collateral. |
| Product terms | USD 500,000 nominal corporate bond; market value USD 480,000; current collateral haircut 30 percent; issuer downgraded below eligible collateral threshold. |
| Key distinction | A credit event can affect valuation, risk classification, mandate eligibility, and collateral availability at the same time. |

### Lifecycle And Calculations

| Event | Treatment |
|---|---|
| Rating downgrade | Update issuer/rating, credit bucket, mandate status, and collateral eligibility. |
| Price update | Revalue bond using approved price source and stale-state rules. |
| Haircut change | Recalculate lending value or mark collateral ineligible if threshold is breached. |
| Margin review | Compare updated lending value to loan exposure and generate shortfall if needed. |

```text
Old lending value = 480,000 x (1 - 30%) = 336,000
If new haircut = 55%, new lending value = 480,000 x (1 - 55%) = 216,000
Availability impact = 120,000 reduction before other facility constraints
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain credit deterioration, price risk, income uncertainty, and collateral impact. |
| DPM/mandate | Check rating floor, issuer concentration, income mandate eligibility, and forced rebalance or exception workflow. |
| Reporting | Show rating, price date, yield basis, collateral value, haircut, lending value, and breach/shortfall state. |
| QA | Validate rating effective date, price source, haircut rule, pledged quantity, exposure, and margin-call trigger. |
| Boundary | Do not continue using old lending value after a source-backed downgrade or collateral-policy change. |

## Example 20: Fund Cut-Off, Stale NAV And Partial Redemption

| Dimension | Example |
|---|---|
| Product families | Funds; cash; portfolio construction; reporting |
| Client objective | Raise cash from a fund redemption for a planned rebalance. |
| Product terms | Fund deals weekly; order cut-off Monday 12:00; client submits after cut-off; latest NAV is five business days old; fund confirms only 60 percent redemption due to liquidity management. |
| Key distinction | Order request, accepted order, dealing NAV, confirmed redemption, and settlement cash are separate states. |

### Workflow

| Step | Expected treatment |
|---|---|
| Order submitted after cut-off | Assign next eligible dealing date, not current dealing date. |
| NAV stale | Mark valuation and drift as stale or partial until updated NAV is sourced. |
| Partial confirmation | Reduce units only for confirmed portion where booking policy permits. |
| Settlement | Cash becomes available only on settlement date and currency. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain liquidity timing, stale valuation, possible gates/partial fills, and funding uncertainty. |
| DPM/mandate | Treat rebalance funding as pending liquidity, not executable cash. |
| Reporting | Show requested, accepted, confirmed, pending, settled, NAV date, and settlement date. |
| QA | Validate cut-off calendar, stale NAV flag, partial redemption units, cash projection, and blocked funding. |
| Boundary | Do not use requested redemption amount as available cash. |

## Example 21: Rights Issue With Entitlement And Cost Basis

| Dimension | Example |
|---|---|
| Product families | Equities; tax reporting; advisory; DPM |
| Client objective | Decide whether to exercise rights on an existing equity holding. |
| Product terms | Client owns 1,000 shares; rights issue grants 1 right for every 5 shares; 1 right plus SGD 3.00 subscribes for 1 new share; market price is SGD 4.20. |
| Key distinction | A corporate action can create an entitlement, a decision deadline, cash funding need, and tax/cost-basis effect. |

### Calculations

```text
Rights received = 1,000 / 5 = 200 rights
Subscription cash required = 200 x 3.00 = SGD 600
New shares if exercised = 200
Total shares after exercise = 1,200
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain dilution, subscription cost, deadline, funding, and alternatives such as sell rights or lapse. |
| DPM/mandate | Check authority, cash availability, single-name limit, and model drift after exercise. |
| Reporting | Show entitlement, election status, deadline, required cash, resulting shares, and cost-basis policy. |
| QA | Validate entitlement ratio, fractional treatment, election deadline, cash reservation, resulting quantity, and cost basis. |
| Boundary | Do not book new shares before election and allocation are confirmed. |

## Example 22: Futures Variation Margin And Notional Exposure

| Dimension | Example |
|---|---|
| Product families | Derivatives; commodities; cash; risk |
| Client objective | Use futures for tactical commodity exposure. |
| Product terms | Long 2 futures contracts; contract multiplier 1,000 barrels; prior settlement USD 80; current settlement USD 82. |
| Key distinction | Daily variation margin changes cash, but notional exposure remains based on contract terms. |

### Calculations

```text
Notional exposure = 2 x 1,000 x 82 = USD 164,000
Variation margin = (82 - 80) x 1,000 x 2 = USD 4,000 cash inflow
```

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain leverage, margin, expiry, roll risk, and commodity price volatility. |
| DPM/mandate | Check derivative permission, notional exposure, margin availability, and concentration limits. |
| Reporting | Show notional, market value/MTM, margin cash, realized daily P&L, expiry, and underlying exposure. |
| QA | Validate multiplier, contract count, settlement price, cash movement, notional exposure, and expiry handling. |
| Boundary | Do not use margin cash balance as the measure of commodity exposure. |

## Example 23: Structured Product Scenario With Missing Observation

| Dimension | Example |
|---|---|
| Product families | Structured products; structured notes; reporting; QA |
| Client objective | Review an autocallable note before the next observation date. |
| Product terms | Note has quarterly autocall and coupon observations; latest underlying price is available but issuer observation result is missing. |
| Key distinction | Market data can support an indicative view, but the lifecycle event is not confirmed until the observation result is sourced. |

### Treatment

| View | Expected state |
|---|---|
| Indicative scenario | Can show illustrative barrier distance and possible outcome with clear source date. |
| Confirmed coupon/autocall | Must remain pending until issuer/custodian/source notice confirms observation result. |
| Reporting | Should separate indicative scenario from confirmed cashflow or redemption. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain conditionality, issuer confirmation dependency, liquidity limits, and worst-case outcome. |
| DPM/mandate | Keep replacement trade or reinvestment action pending until autocall/redemption is confirmed. |
| Reporting | Show next observation, indicative barrier distance, pending confirmation, and unsupported confirmed-cashflow state. |
| QA | Validate missing observation behavior, no premature coupon booking, no premature redemption, and scenario label. |
| Boundary | Do not book coupon, autocall redemption, or delivered assets from indicative market data alone. |

## Example 24: Private-Market NAV Restatement

| Dimension | Example |
|---|---|
| Product families | Private markets; performance; reporting; operations |
| Client objective | Correct historical reporting after a manager restates prior-quarter NAV. |
| Product terms | Q1 NAV was reported as USD 950,000 and later restated to USD 910,000; Q2 report and trailing performance already consumed the old NAV. |
| Key distinction | Restatement should preserve prior report lineage and trigger controlled recalculation, not silent overwrite. |

### Workflow

| Step | Expected treatment |
|---|---|
| Receive restatement | Store old NAV, new NAV, source notice, received date, effective valuation date, and reason where available. |
| Recalculate | Recompute affected market value, allocation, contribution, IRR/MWR where methodology allows. |
| Report | Mark revised figures and disclose restatement where reporting policy requires. |
| Audit | Preserve prior report snapshot and recalculation lineage. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain valuation lag, uncertainty, and why historical figures changed. |
| DPM/mandate | Reassess allocation, liquidity planning, and unfunded commitment if thresholds changed. |
| Reporting | Show restated NAV, valuation date, received date, prior value, and affected calculations. |
| QA | Validate restatement lineage, recalculation scope, stale report handling, and unchanged unrelated periods. |
| Boundary | Do not overwrite prior NAV without audit trail and recalculation evidence. |

## Example 25: Family Office Consolidated Reporting With Authority Limits

| Dimension | Example |
|---|---|
| Product families | Trusts, estate planning, family office, wealth structuring; reporting; advisory |
| Client objective | Produce a consolidated family report across individual, trust, holding-company, and policy-owned assets. |
| Product terms | One beneficiary can view trust distributions but not full trustee investment records; one holding company pledges shares to a credit facility; one policy has restricted beneficiary data. |
| Key distinction | Reporting perimeter is not the same as legal ownership or access authority. |

### Treatment

| Area | Expected treatment |
|---|---|
| Ownership | Separate legal owner, beneficial owner, controller, trustee, director, policyholder, insured, and beneficiary roles. |
| Access | Apply role-based visibility and restricted-data rules by entity, account, document, and report section. |
| Exposure | Consolidate where authorized while preserving entity-level source and pledge restrictions. |
| Reporting | Show consolidation basis, omitted/restricted sections, source dates, and authority caveats. |

### Reporting And QA

| Area | Expected treatment |
|---|---|
| Advisory | Explain consolidated view limits, authority, fiduciary roles, pledge constraints, and restricted information. |
| DPM/mandate | Avoid trading or rebalancing across entities without proper authority and mandate scope. |
| Reporting | Show entity hierarchy, included accounts, excluded/restricted sections, pledge state, and report audience. |
| QA | Validate access permissions, beneficial-owner mapping, pledge propagation, policy data restriction, and export controls. |
| Boundary | Do not expose restricted trust, policy, or holding-company data merely because it is part of a family consolidation. |

## Example Template

Use this structure when adding deeper examples to product packs:

1. Product family and product subtype.
2. Client objective and business purpose.
3. Product terms and assumptions.
4. Lifecycle timeline.
5. Transactions and cashflows.
6. Valuation and pricing treatment.
7. Performance, contribution, attribution, and risk treatment.
8. Portfolio modelling and exposure treatment.
9. Advisory, suitability, DPM, and mandate implications.
10. Reporting output.
11. Calculation requirements.
12. Edge cases and QA scenarios.
13. Implementation boundary and unsupported states.

## Disclaimer

These examples are for education, product analysis, architecture, documentation, QA, and implementation planning. They are not investment, legal, tax, accounting, regulatory, credit, trading, or client advice.

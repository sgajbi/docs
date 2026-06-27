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

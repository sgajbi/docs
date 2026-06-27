# 07 - Advisory, Mandate, Suitability, Reporting and Client Experience

## 1. Advisory positioning

Borrowing against a portfolio can be useful, but it changes the client's risk profile. Advisory should not present Lombard/margin lending only as "extra liquidity" or "higher buying power." It is leverage secured by assets that may fall in value.

A good advisor explains:

- purpose of borrowing
- cost of borrowing
- collateral pledged
- LTV/haircut and buffer
- what happens if markets fall
- margin call process
- possible forced liquidation
- impact on portfolio risk and return
- currency mismatch
- suitability with client profile
- impact on mandate constraints

## 2. Good versus weak advisory use cases

| Use case | Better fit? | Notes |
|---|---|---|
| Short-term liquidity without selling long-term assets | Often reasonable | Needs repayment source |
| Settlement bridge | Often reasonable | Tenor and cashflow visibility important |
| Diversified portfolio leverage | Suitable only for eligible clients | Requires risk tolerance and monitoring |
| Funding tax/property/business timing need | Case-specific | Avoid forced sale risk |
| Concentrated single-stock leverage | High risk | Collateral and investment risk are same source |
| Borrowing to buy high-risk structured product | High risk | Leverage + product complexity |
| Borrowing in foreign currency due to low rate | Risky | FX liability risk |
| Borrowing to meet lifestyle spending permanently | Weak | Can erode wealth and increase fragility |

## 3. Suitability dimensions

| Dimension | Advisory question |
|---|---|
| Risk profile | Can client tolerate leveraged losses? |
| Knowledge/experience | Does client understand margin calls and liquidation? |
| Liquidity | Can client meet margin call quickly? |
| Time horizon | Is borrowing period aligned with portfolio horizon? |
| Income/cashflow | Can client service interest? |
| Currency | Is loan currency aligned with income/assets? |
| Concentration | Are pledged assets diversified? |
| Product complexity | Is borrowing used to buy complex products? |
| Net worth | Is leverage proportionate? |
| Stress tolerance | What happens under 20-30% market fall? |
| Mandate | Does DPM/advisory mandate permit leverage? |

## 4. Client explanation template

Advisor-friendly explanation:

> A Lombard loan lets you borrow using your eligible investment portfolio as collateral. You keep your investments, but the bank assigns a lending value after applying haircuts. If collateral value falls or loan exposure rises, your available credit can reduce and a margin call may occur. You may need to add cash, pledge more assets, repay part of the loan, or sell assets. If the shortfall is not cured, the bank may liquidate collateral according to the agreement. This product can improve liquidity, but it also introduces leverage, interest cost, collateral risk and possible forced-sale risk.

## 5. Advisory risk questions

Before recommending lending:

1. What is the purpose of borrowing?
2. What is the expected repayment source?
3. How long is the loan needed?
4. What assets are pledged?
5. What is the current and stressed LTV?
6. What happens if collateral falls 10%, 20%, 30%?
7. Can client meet margin call without forced sale?
8. Is the loan currency aligned with income or assets?
9. Is client using leverage to buy more risk assets?
10. Does mandate allow leverage and borrowing?
11. Are pledged assets concentrated?
12. Are there illiquid assets or complex products in collateral?
13. Is interest cost sustainable?
14. What is the exit plan?

## 6. Mandate relevance

DPM and advisory mandates need explicit rules.

| Mandate rule | Example |
|---|---|
| Leverage allowed? | No leverage / max 20% / allowed for liquidity only |
| Borrowing purpose | Investment only / liquidity only / prohibited |
| Max LTV | Max loan exposure 30% of portfolio value |
| Max leverage ratio | Gross exposure / NAV <= 1.2x |
| Eligible collateral | IG bonds and cash only |
| Product exclusions | No structured notes as collateral |
| Currency mismatch | Loan currency must match collateral or base currency |
| Concentration | No more than 20% collateral from one issuer |
| Margin buffer | Maintain at least 15% buffer above call threshold |
| Forced-sale policy | Advisor/client notification before discretionary action |

## 7. Pre-trade advisory controls

Before allowing leveraged purchase:

```text
Check client eligibility
Check product eligibility for leverage
Check mandate leverage rule
Check suitability/appropriateness
Check buying power after order
Check collateral concentration
Check stressed margin buffer
Reserve buying power
Record advice/consent/disclosure
```

## 8. DPM portfolio manager view

A portfolio manager needs:

- current leverage ratio
- loan currency and interest cost
- available cash and credit
- collateral utilization
- margin buffer
- mandate headroom
- forecast coupon/dividend/income versus financing cost
- stress result and likely margin call trigger price
- ability to rebalance without breaching collateral constraints

## 9. Investment proposal treatment

A proposal involving borrowing should show before/after:

| Metric | Before | After |
|---|---:|---:|
| Portfolio market value | 1,000,000 | 1,250,000 |
| Loan exposure | 0 | 250,000 |
| Net equity | 1,000,000 | 1,000,000 |
| Leverage ratio | 1.0x | 1.25x |
| Expected income | 35,000 | 48,000 |
| Estimated interest cost | 0 | 15,000 |
| Net income | 35,000 | 33,000 |
| Margin buffer | N/A | 20% |
| Stress loss at -20% market | -200,000 | -250,000 less financing cost |

## 10. Reporting requirements

Client reports should include:

| Section | Content |
|---|---|
| Liabilities summary | Loans, overdrafts, accrued interest |
| Net worth | Assets minus liabilities |
| Pledged assets | Collateral holdings and market value |
| Lending value | Haircut-adjusted collateral value |
| Facility summary | Limit, exposure, availability |
| Margin status | Normal/warning/call/liquidation |
| Interest cost | Period interest and fees |
| Currency exposure | Loan currency and collateral currency mismatch |
| Leverage metrics | Gross assets, net equity, leverage ratio |
| Stress scenario | Impact on collateral and margin buffer |
| Movement summary | Drawdowns, repayments, interest, fees |

## 11. Portfolio analytics treatment

Analytics should support both asset and net views.

| View | Use |
|---|---|
| Asset view | Investment performance independent of financing |
| Net view | Client net worth including loans |
| Financing view | Interest/fees and liability FX impact |
| Collateral view | Pledged holdings and encumbrance |
| Exposure view | Gross risk including borrowed investment amount |
| Mandate view | Leverage and liquidity compliance |

## 12. Performance reporting examples

A leveraged portfolio can show positive asset performance but weak net performance if financing cost is high.

Example:

| Component | Amount |
|---|---:|
| Investment return | +40,000 |
| Dividends/coupons | +20,000 |
| Loan interest | -25,000 |
| Loan FX loss | -10,000 |
| Net contribution | +25,000 |

Reporting should avoid hiding financing cost inside miscellaneous cash movement.

## 13. Risk reporting examples

Useful risk charts/tables:

- top pledged assets by lending value
- collateral by asset class
- collateral by issuer
- loan exposure by currency
- LTV trend over time
- availability trend over time
- margin buffer trend
- stress shortfall table
- interest cost projection
- maturity ladder of drawdowns
- next rate reset dates

## 14. Suitability warning examples

Warning messages:

| Situation | Message |
|---|---|
| High utilization | Facility utilization exceeds internal comfort threshold |
| Low margin buffer | Small market fall may trigger margin call |
| Concentrated collateral | Collateral is highly dependent on one issuer/security |
| Currency mismatch | Loan currency differs from collateral/income currency |
| Complex product funded by leverage | Leverage increases downside risk of complex product |
| Illiquid collateral | Collateral may be difficult to sell during stress |
| Rising rate risk | Borrowing cost may increase at next reset |

## 15. Relationship-manager dashboard

A strong RM/advisor dashboard should show:

- facilities and utilization
- collateral pools and pledged assets
- available credit and buying power
- margin-call watchlist
- clients near threshold
- large intraday order reservations
- upcoming interest payments
- expiring facilities/drawdowns
- top collateral concentration
- stress shortfall view
- cross-pledge dependencies

## 16. Client experience principles

Clients should not see only one number called "available to invest" without explanation.

Show:

```text
Cash available
Credit available
Pledged collateral
In-flight orders
Buffer to margin call
Estimated interest cost
Stress result
```

This reduces misunderstanding and supports responsible advisory.

## 17. Office knowledge-sharing summary

Key message:

> Lending is not just a cash product. It changes the portfolio's risk structure. A wealth platform must connect facility terms, collateral eligibility, pledge relationships, exposure, buying power, suitability, mandate rules, performance, risk and reporting into one consistent model.

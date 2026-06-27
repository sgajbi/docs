# Product Payoff, Scenario And Stress Guide

This guide defines a reusable cross-product standard for explaining payoff behavior, scenario outcomes, stress testing, downside risk, upside participation, income reliability, liquidity effects, and client-reporting interpretation.

Use it when reviewing product documentation, designing advisory explanations, building analytics, writing acceptance criteria, preparing QA scenarios, or deciding whether a product can be compared fairly with another product.

## Core Rule

Payoff is not the same as return, risk, liquidity, income, valuation, or suitability.

A strong product explanation separates:

1. legal payoff promised by the contract or instrument,
2. expected cashflows under normal conditions,
3. market-value movement before exit or maturity,
4. downside path under stress,
5. upside participation,
6. income certainty or conditionality,
7. liquidity and exit constraints,
8. issuer, counterparty, manager, or insurer dependency,
9. fees, taxes, financing, and collateral effects,
10. reporting treatment and supportability state.

## Payoff Vocabulary

| Term | Meaning | Example | Common Mistake |
|---|---|---|---|
| Linear payoff | Value changes broadly one-for-one with the underlying exposure. | Direct equity, ETF, physical gold, FX spot. | Ignoring currency, fees, tax, liquidity, and source-date effects. |
| Fixed-income payoff | Contractual principal and income schedule subject to issuer, rate, call, and credit risk. | Bond, deposit, treasury bill. | Treating yield as guaranteed without credit, reinvestment, call, or breakage caveats. |
| Conditional payoff | Cashflow or redemption depends on observed conditions. | Autocall coupon, barrier note, range accrual. | Reporting projected coupon as confirmed income. |
| Convex payoff | Upside or downside changes nonlinearly with the underlying. | Option, warrant, leveraged structured product. | Showing notional exposure without explaining premium loss or leverage. |
| Path-dependent payoff | Outcome depends on the route taken, not only final level. | Barrier note, Asian option, range accrual, accumulator. | Using only maturity spot level to explain the result. |
| Leveraged payoff | Exposure exceeds cash invested or liability capacity. | Futures, options, margin loan, leveraged note. | Equating cash posted or premium paid with maximum risk. |
| Income payoff | Client receives income from coupon, dividend, distribution, rent, premium, or interest. | Bond coupon, dividend, REIT distribution, annuity payment. | Treating every income-like cashflow as stable or recurring. |
| Protection payoff | Product includes contractual or policy-defined protection, subject to terms. | Capital-protected note, insurance death benefit, guaranteed annuity. | Ignoring provider credit, conditions, surrender value, exclusions, and fees. |
| Liability payoff | Client repays or funds an obligation rather than receives investment payoff. | Loan repayment, margin call, capital call, premium due. | Treating limit, commitment, or collateral value as spendable wealth. |
| Residual-value payoff | Value depends on manager/appraiser/market exit and may be delayed. | Private fund NAV, direct property, infrastructure fund. | Treating reported NAV as immediately realizable cash. |

## Product Family Payoff Map

| Product Area | Payoff Driver | Normal Outcome | Stress Outcome | Key Explanation |
|---|---|---|---|---|
| Cash | Account currency and restrictions. | Principal remains available subject to ledger, settlement, and restriction rules. | Bank restriction, account block, FX loss in reporting currency, settlement delay. | Separate ledger, settled, available, restricted, and projected cash. |
| Deposits | Principal, rate, maturity, bank credit, breakage terms. | Principal plus agreed interest at maturity. | Early breakage penalty, bank credit event, currency mismatch, reinvestment risk. | Deposit value is not the same as free cash before maturity. |
| Money market funds | NAV, liquidity assets, fund rules, fees, gates. | Stable or low-volatility NAV and income distribution. | NAV movement, liquidity fee, gate, suspension, delayed redemption. | A money market fund is a fund, not cash. |
| FX spot and forwards | Currency pair, spot, forward points, value date. | Currency conversion or hedge result at settlement. | Adverse FX movement, failed settlement, NDF fixing loss, funding gap. | Separate trade economics from available cash timing. |
| Bonds | Coupon, principal, yield, duration, spread, issuer credit. | Coupons and principal according to schedule. | Price loss from rates/spreads, call, default, restructuring, liquidity discount. | Contractual cashflows are not the same as mark-to-market value. |
| Equities | Issuer performance, dividends, market sentiment, corporate actions. | Price appreciation/depreciation plus dividends. | Drawdown, dividend cut, suspension, delisting, dilution, corporate action loss. | Equity payoff is uncapped upside with full downside unless hedged. |
| Funds | Portfolio assets, manager decisions, NAV, fees, liquidity terms. | NAV growth and distributions according to fund strategy. | NAV drawdown, stale valuation, gate, side pocket, suspension, manager underperformance. | Client owns fund units; look-through explains exposure but not direct control. |
| Commodities and precious metals | Spot price, unit, storage/provider, roll, currency. | Price participation and possible collateral value. | Price shock, roll loss, storage cost, issuer/provider failure, liquidity gap. | Wrapper matters: physical, ETF, future, note, and equity proxy behave differently. |
| Derivatives | Underlying, strike, expiry, volatility, rates, margin, counterparty. | Hedge, premium income, or speculative payoff as terms define. | Premium loss, margin call, nonlinear loss, early termination, collateral dispute. | Notional, market value, delta exposure, and cash margin are different. |
| Structured products | Issuer, wrapper, underlyings, payoff formula, barriers, observations. | Conditional coupon, autocall, participation, protection, or redemption outcome. | Missed coupon, barrier breach, issuer credit event, physical delivery, illiquidity. | Explain scenario path, not only headline coupon. |
| Structured notes | Debt note plus embedded derivative. | Coupon/redemption as note terms define. | Issuer loss plus underlying-linked payoff loss or delivery outcome. | Separate issuer credit from underlying market exposure. |
| Private markets | Manager, portfolio exits, NAV, calls/distributions, valuation lag. | Capital calls, NAV growth, distributions over fund life. | Delayed exits, NAV write-down, liquidity lock-up, additional calls, valuation restatement. | Commitment, paid-in, unfunded, NAV, and distributions are separate. |
| Real estate and infrastructure | Income, occupancy, rates, leverage, appraisal, regulation. | Rental/infrastructure income plus capital value movement. | Vacancy, refinancing stress, appraisal write-down, redemption queue, regulatory change. | Listed, fund, private, and direct wrappers have different liquidity and valuation behavior. |
| Insurance and annuities | Policy terms, insurer, premiums, guarantees, charges, mortality/longevity. | Protection benefit, account value, surrender value, or income stream. | Lapse, missed premium, surrender charge, insurer stress, guarantee condition failure. | Death benefit, account value, cash value, and surrender value are different. |
| Loans and collateral | Facility terms, rate, collateral value, haircut, utilization. | Liquidity access with interest and collateral constraints. | Margin call, forced liquidation, rate reset, collateral ineligibility, covenant breach. | Loan proceeds are cash, but liability and collateral risk remain. |

## Scenario Types

| Scenario Type | Purpose | Required Inputs | Output Should Show |
|---|---|---|---|
| Base case | Expected or central assumption. | Current value, terms, schedule, rates, source date. | Expected cashflows, value path, income, and caveats. |
| Upside case | Demonstrate participation and caps. | Underlying upside, cap/participation, fees, taxes, call terms. | Maximum or likely upside, caps, early exit behavior. |
| Downside case | Demonstrate loss path and client impact. | Underlying shock, rate/spread shock, NAV shock, collateral shock. | Market value loss, cash need, covenant breach, liquidity effect. |
| Income interruption | Test reliability of income. | Coupon/dividend/distribution/premium terms, condition, issuer/fund state. | Missed income, delayed income, reduced income, reporting state. |
| Liquidity stress | Test exit feasibility. | Settlement, dealing terms, gates, bid/ask, lock-up, pledge state. | Available cash timing, blocked sale/redemption, haircut, queue. |
| Credit or counterparty stress | Test dependency on issuer, bank, insurer, manager, or counterparty. | Credit event terms, exposure amount, recovery assumption. | Write-down, delayed payment, replacement cost, supportability state. |
| FX stress | Test reporting currency and funding impact. | Local value, FX rate, hedge, settlement currency. | Local return, FX translation, hedge result, cash mismatch. |
| Operational stress | Test source, event, or processing failure. | Missing price, stale NAV, failed settlement, missing observation. | Blocked output, partial state, manual review, audit trail. |

## Scenario Explanation Pattern

Every scenario should answer:

1. What product subtype and wrapper is being tested?
2. What legal holding, obligation, or contract is affected?
3. Which underlying exposure drives the result?
4. What dates matter: trade, valuation, observation, payment, maturity, expiry, settlement, or received date?
5. What is the starting value and value basis?
6. What assumption changes?
7. What cashflows occur and when?
8. What position, liability, or obligation changes?
9. What income, fee, tax, or financing effect appears?
10. What reporting labels must be shown?
11. What decision should an advisor, portfolio manager, operations user, or client understand?
12. What data or source limitation could make the scenario partial or unsupported?

## Worked Scenario Templates

| Template | Product Fit | Example Inputs | Expected Interpretation |
|---|---|---|---|
| Rate shock | Bonds, deposits, loans, swaps, REITs, infrastructure. | +100 bps yield/rate shift, duration, reset date, leverage. | Bond price falls, floating-rate income may reset, loan cost rises, leveraged real assets may weaken. |
| Credit spread shock | Bonds, credit funds, structured notes, private credit. | Spread widening, rating downgrade, recovery assumption. | Market value falls, issuer/manager risk rises, liquidity may deteriorate. |
| Equity drawdown | Equities, ETFs, funds, structured products, margin loans. | -20 percent index or stock move, beta/delta, barrier level, collateral haircut. | Direct equity falls, note barrier may become at risk, margin availability may shrink. |
| Commodity price shock | Physical metals, commodity ETPs, futures, commodity notes. | Spot move, unit, contract size, FX, roll assumption. | Physical value changes linearly, futures margin changes cash, note payoff depends on terms. |
| FX depreciation | Foreign assets, deposits, loans, forwards, NDFs. | Local value, reporting FX, hedge notional, settlement currency. | Local return may differ from reporting return; hedge may offset or add cashflow. |
| NAV stale or restated | Funds, private markets, real estate funds. | Current NAV date, restated NAV, cashflows since NAV. | Reported value may be partial; performance may need recomputation. |
| Liquidity gate | Money market funds, hedge funds, private funds, property funds. | Redemption amount, gate percentage, settlement delay. | Only confirmed portion becomes projected cash; residual remains invested or pending. |
| Margin call | Margin loans, Lombard facilities, futures, OTC derivatives. | Collateral value shock, haircut, exposure, threshold. | Client must repay, add collateral, or face liquidation; investment value is not available cash. |
| Insurance lapse | Protection and ILP policies. | Premium due, grace period, account value, charges. | Coverage may reduce or terminate; value basis and client notification matter. |
| Structured note barrier breach | Autocall, reverse convertible, participation note. | Initial level, current level, barrier, coupon condition, maturity terms. | Coupon or principal outcome depends on observation and final terms; do not infer from market value alone. |

## Advisory And Suitability Use

Payoff scenarios should support better advice, not just richer analytics.

| Advisory Question | Scenario Evidence Needed |
|---|---|
| Can the client afford the downside? | Maximum plausible loss, liquidity need, margin/collateral call, locked-up capital, income interruption. |
| Is income reliable? | Income source, condition, payment history, issuer/fund/tenant dependency, tax/fee treatment. |
| Is the product too complex? | Number of payoff conditions, path dependency, wrapper complexity, source-data availability, client knowledge. |
| Is concentration acceptable? | Issuer, manager, counterparty, underlying, sector, geography, currency, collateral concentration. |
| Does the product fit the mandate? | Allowed product type, risk class, liquidity bucket, income/growth objective, leverage, derivatives permission. |
| What could surprise the client? | Missed coupon, early call, physical delivery, gate, side pocket, lapse, breakage cost, FX loss, margin call. |
| Can the outcome be reported truthfully? | Source terms, valuation basis, scenario assumptions, observation evidence, supportability state. |

## Reporting Rules

Scenario and payoff reporting should:

1. label scenario results as illustrative unless they are confirmed contractual events,
2. separate confirmed cashflows from projected cashflows,
3. separate market value from redemption value, surrender value, notional, exposure, and collateral value,
4. show key assumptions and source dates,
5. show whether income is fixed, variable, discretionary, conditional, projected, or confirmed,
6. show whether downside is capped, floored, collateralized, insured, hedged, or unlimited,
7. show whether liquidity is daily, settlement-based, periodic, gated, locked-up, pledged, or unsupported,
8. show issuer, counterparty, manager, insurer, lender, or provider dependency,
9. avoid ranking products by headline yield without risk, liquidity, and payoff context,
10. avoid reporting scenario results when required payoff terms or source inputs are missing.

## QA Checklist

Good scenario QA should prove:

1. base, upside, downside, and stress outputs use the same source terms,
2. missing term-sheet or contract fields block scenario output,
3. conditional income is not shown as confirmed before condition is met,
4. barrier, cap, floor, participation, leverage, and protection terms are applied correctly,
5. cashflow dates and settlement dates are distinct,
6. notional, market value, exposure, collateral value, and available cash are not conflated,
7. stale prices or NAVs carry stale-state labels,
8. look-through scenarios show coverage percentage and source date,
9. FX translation is separated from local product return where supported,
10. margin, collateral, and lending scenarios keep gross asset and liability views,
11. insurance scenarios use the right value basis for surrender, death benefit, and account value,
12. private-market scenarios do not treat unfunded commitment as current NAV,
13. client-reporting labels match the scenario status,
14. unsupported products or missing source data fail closed.

## Product Review Prompts

When reviewing a product area, ask:

1. What is the payoff shape: linear, fixed-income, conditional, convex, leveraged, path-dependent, income, protection, liability, or residual-value?
2. What are the main base, upside, downside, income interruption, liquidity, credit, FX, and operational scenarios?
3. Which source fields are required to calculate each scenario?
4. Which scenario outputs are current support, partial support, future candidate, or unsupported?
5. How should the scenario be explained to a client or advisor?
6. How does the scenario affect mandate checks, DPM limits, concentration, and suitability?
7. How does the scenario affect performance, attribution, risk, and reporting?
8. What QA evidence proves the scenario is reliable?

## Related Guides

Use this guide together with:

1. [Product Taxonomy And Vocabulary Guide](product-taxonomy-and-vocabulary-guide.md)
2. [Product Lifecycle, Cashflow And Event Guide](product-lifecycle-cashflow-and-event-guide.md)
3. [Product Calculation Example Catalog](product-calculation-example-catalog.md)
4. [Product Performance, Attribution And Risk Guide](product-performance-attribution-risk-guide.md)
5. [Advisory, Mandate And Reporting Decision Guide](advisory-mandate-reporting-decision-guide.md)
6. [Product Capability Boundary Matrix](product-capability-boundary-matrix.md)
7. [Product QA Regression Matrix](product-qa-regression-matrix.md)

## Disclaimer

This document is for product knowledge, platform design, analytics, documentation, and QA work. It is not investment, legal, tax, regulatory, or client advice.

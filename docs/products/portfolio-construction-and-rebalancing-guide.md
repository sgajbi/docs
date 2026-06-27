# Portfolio Construction And Rebalancing Guide

This guide defines a reusable cross-product standard for portfolio construction, target allocation, rebalancing, funding, liquidity, tax-aware implementation, mandate controls, execution readiness, reporting, and QA.

Use it when designing advisory proposals, DPM model portfolios, portfolio review workflows, drift monitoring, rebalance tools, transition plans, cash deployment, tax-aware trade lists, or implementation acceptance criteria.

For detailed workflow examples, use [`model-portfolios-rebalancing-advisory-suitability/11-worked-examples-and-implementation-patterns.md`](model-portfolios-rebalancing-advisory-suitability/11-worked-examples-and-implementation-patterns.md).

## Core Principle

Portfolio construction is not just target weights.

A practical construction workflow must connect:

1. client objective,
2. mandate or service model,
3. portfolio perimeter,
4. target allocation,
5. current allocation,
6. drift,
7. product eligibility,
8. liquidity and cash availability,
9. tax and cost impact,
10. risk and concentration,
11. execution feasibility,
12. reporting and evidence.

A target model that ignores cash settlement, collateral, taxes, gates, pending orders, product restrictions, lot sizes, authority, or source-data state is not implementation-ready.

## Construction Vocabulary

| Term | Meaning | Common Mistake |
|---|---|---|
| Portfolio perimeter | Accounts, sleeves, structures, and mandates included in the construction view. | Mixing advisory, DPM, execution-only, pledged, or restricted accounts without labels. |
| Target allocation | Desired exposure by asset class, product family, strategy, currency, sector, liquidity bucket, or mandate sleeve. | Treating a target as executable without product and cash checks. |
| Current allocation | Current supported allocation using a declared exposure lens. | Using market value when mandate requires economic or look-through exposure. |
| Drift | Difference between current and target allocation. | Treating every drift as a trade without materiality and turnover checks. |
| Tolerance band | Allowed range around target. | Missing hard versus soft band distinction. |
| Rebalance trigger | Event that creates review or trade proposal. | Rebalancing purely on schedule while ignoring client cashflows and risks. |
| Trade list | Proposed buys, sells, subscriptions, redemptions, switches, hedges, or transfers. | Generating trades before checking constraints and settlement. |
| Funding plan | How buys, subscriptions, loan repayments, calls, or withdrawals will be funded. | Assuming projected proceeds are available cash. |
| Transition plan | Staged movement from legacy holdings to target state. | Selling all legacy positions without tax, liquidity, mandate, or suitability review. |
| Execution readiness | Whether a proposed trade can actually be executed and settled. | Treating model output as order instruction without best execution and operations checks. |

## Construction Workflow

| Step | Key Questions | Evidence |
|---|---|---|
| Define perimeter | Which accounts, portfolios, family entities, sleeves, loans, collateral, and mandates are in scope? | Account list, mandate scope, ownership and authority, reporting perimeter. |
| Confirm objective | Income, growth, preservation, liquidity, hedging, decumulation, tax-aware transition, DPM model tracking? | Client profile, mandate, proposal, investment policy, review notes. |
| Select exposure lens | Market value, economic exposure, look-through, liquidity, currency, risk, commitment, collateral? | Exposure methodology and source-quality state. |
| Compare target and current | What is overweight, underweight, unavailable, stale, restricted, or unsupported? | Target model, current holdings, drift report, valuation date. |
| Check constraints | Product eligibility, risk, concentration, liquidity, currency, leverage, tax, authority, restriction, mandate. | Pre-trade controls, suitability result, mandate checks. |
| Build trade list | What buys/sells/switches/hedges/transfers/redemptions/subscriptions are proposed? | Trade proposal, alternatives, rationale, scenario impact. |
| Validate funding | Which cash, proceeds, facility headroom, maturities, redemptions, or income fund the plan? | Available cash, projected cash, settlement calendar, collateral state. |
| Assess implementation cost | Fees, bid/ask, spread, tax, stamp duty, market impact, breakage, FX, surrender, redemption fees. | Cost estimate, tax-lot data, execution assumptions. |
| Stage execution | Immediate, phased, conditional, pending consent, pending liquidity, pending source refresh. | Execution plan, dependencies, order validity. |
| Report and monitor | What changed, what remains, what is blocked, and what needs follow-up? | Client report, DPM review, exception queue, audit trail. |

## Product Construction Roles

| Product Area | Construction Role | Rebalancing Constraint |
|---|---|---|
| Cash and deposits | Liquidity reserve, income on liquidity, funding source, maturity ladder. | Available cash differs from ledger/projected cash; deposits may have breakage penalties. |
| Money market funds | Liquidity sleeve, yield alternative, cash-like allocation where policy permits. | Fund gates, NAV timing, settlement, liquidity fees, and classification must be explicit. |
| Bonds | Income, capital preservation, duration, credit, maturity ladder, liability matching. | Accrued interest, liquidity, minimum denominations, callability, credit events, tax lots. |
| Equities | Growth, income, factor/sector/country exposure, tactical or model sleeve. | Lot size, liquidity, corporate actions, tax lots, restrictions, single-name limits. |
| Funds | Core allocation building blocks, active/passive exposure, manager allocation. | NAV date, dealing cut-off, gates, stale look-through, fees, share-class eligibility. |
| Derivatives | Hedge, overlay, tactical exposure, income strategy, downside protection. | Notional, margin, collateral, Greeks, expiry, K&E, mandate permission, counterparty. |
| Structured products | Yield enhancement, defined payoff, tactical exposure, downside-buffer structures. | Issuer risk, term-sheet fields, liquidity, payoff conditions, observation dates, complexity. |
| Private markets | Long-term illiquidity premium, diversification, manager/vintage allocation. | Capital calls, unfunded, lock-up, NAV lag, transfer restrictions, minimum commitments. |
| Real estate and infrastructure | Real-asset income, inflation linkage, diversification, private/public real asset sleeve. | Appraisal lag, leverage, redemption queue, direct-property constraints, REIT equity behavior. |
| Commodities and precious metals | Diversifier, inflation hedge, collateral, crisis hedge, tactical exposure. | Wrapper differences, unit conversion, roll cost, custody, delivery, liquidity, issuer/counterparty. |
| Insurance and annuities | Protection, estate planning, income stream, wrapper-based exposure. | Surrender value, charges, premium obligations, policy loans, beneficiary/privacy limits. |
| Loans and collateral | Liquidity access, leverage, transition funding, cashflow smoothing. | Availability, LTV, haircuts, pledge scope, margin calls, forced liquidation risk. |
| Tax and regulatory reporting | Tax-aware trade selection, documentation readiness, reportable event visibility. | Tax-lot history, withholding, cross-border documentation, reportability, disclosure. |
| Trusts and wealth structures | Ownership, authority, reporting perimeter, family allocation, succession objectives. | Legal owner, beneficial owner, trustee/executor authority, access rights, restrictions. |

## Rebalancing Trigger Matrix

| Trigger | Examples | Action |
|---|---|---|
| Scheduled review | Monthly, quarterly, annual, mandate review. | Check drift and update proposal or DPM action list. |
| Allocation drift | Equity overweight, cash underweight, issuer concentration, currency mismatch. | Compare drift with tolerance band and materiality threshold. |
| Cashflow event | Deposit, withdrawal, coupon, dividend, maturity, capital call, premium due. | Deploy, reserve, fund, or rebalance according to cash policy. |
| Product lifecycle | Bond maturity, structured note autocall, fund gate, option expiry, policy lapse notice. | Replace exposure, update target, or block until outcome is confirmed. |
| Risk event | Rating downgrade, volatility jump, margin call, issuer alert, corporate action. | Reassess suitability, mandate, collateral, and risk limits. |
| Model change | New target weights, instrument replacement, house view update, strategy version. | Govern rollout, eligibility, transition, client-specific exclusions. |
| Client change | Objective, domicile, risk profile, liquidity need, restriction, authority, family structure. | Reprofile, amend mandate, create transition plan. |
| Data-quality event | Stale NAV, missing price, unresolved corporate action, incomplete tax lots. | Degrade, block, or route to exception before trading. |

## Funding And Liquidity Rules

Before any rebalance is executable, prove funding:

1. available cash by currency,
2. settlement date and value date,
3. pending trades and projected cash,
4. restricted or pledged cash,
5. deposit maturity and breakage terms,
6. fund redemption timing and gates,
7. private-market capital-call obligations,
8. insurance premium obligations,
9. loan availability and collateral impact,
10. FX conversion requirement,
11. tax and fee cash needs,
12. minimum cash buffer after rebalance.

Do not fund a buy from projected sale proceeds unless the platform explicitly supports settlement bridging and the risk is approved.

## Tax And Cost-Aware Implementation

Construction should consider:

| Area | Required Treatment |
|---|---|
| Tax lots | Identify lots, cost basis, holding period, transferred-in unknown lots, and realized gain/loss. |
| Withholding and income | Preserve gross/net income and tax treatment. |
| Turnover | Report expected turnover and whether it is policy-consistent. |
| Transaction cost | Include brokerage, spread, stamp duty, exchange fees, product charges, redemption fees, surrender charges. |
| FX cost | Include conversion spread, settlement currency, and funding currency. |
| Product breakage | Deposit breakage, structured-product secondary liquidity, fund redemption fees, policy surrender charges. |
| Market impact | Large or illiquid orders require staged execution or alternatives. |
| Reporting | Show expected realized P&L, tax drag, fee impact, and implementation caveats where source-backed. |

## Advisory Versus DPM Treatment

| Dimension | Advisory | DPM |
|---|---|---|
| Decision authority | Client approves each recommendation. | Portfolio manager acts within mandate. |
| Rebalance output | Proposal requiring consent. | Approved trade list under mandate workflow. |
| Suitability | Trade/recommendation-level and portfolio-level. | Mandate-level plus pre/post-trade compliance. |
| Client communication | Explain rationale, risks, costs, alternatives, and consent scope. | Explain model changes, performance, drift, breaches, and decisions. |
| Legacy assets | Recommend hold/sell/exclude with client decision. | Transition plan decides keep, sell, exclude, or transfer. |
| Exceptions | Client-specific and documented. | Mandate-governed and monitored. |

## Implementation Checklist

1. Define portfolio perimeter and service model.
2. Define target model and exposure lens.
3. Source current holdings, valuations, cash, liabilities, commitments, pending trades, and restrictions.
4. Calculate drift against target and tolerance bands.
5. Generate proposed actions with rationale and alternatives.
6. Run suitability, mandate, product, concentration, liquidity, currency, collateral, tax, and authority checks.
7. Calculate funding, settlement, projected cash, and minimum cash buffer.
8. Estimate transaction costs, tax impact, spread, fees, and market impact.
9. Classify actions as executable, pending consent, pending liquidity, blocked, partial, or unsupported.
10. Store proposal version, source data, assumptions, approvals, and execution evidence.
11. Reconcile post-trade holdings, cash, drift, performance, and reporting.
12. Track open exceptions and follow-up review dates.

## QA Scenarios

1. Model says buy equities, but available cash is insufficient after settlement and cash buffer.
2. Proposed sale creates large taxable gain and system labels tax-aware alternative lots.
3. Fund redemption is gated, so only confirmed portion is available for rebalance.
4. DPM model update excludes client account due to restriction.
5. Advisory recommendation expires before consent is captured.
6. Bond maturity creates cash that is projected but not available until pay date.
7. Structured note autocall is expected but not confirmed, so replacement trade remains pending.
8. Loan availability falls after collateral haircut change and blocks additional purchase.
9. Private-market capital call reserves cash before model rebalance.
10. Trust account rebalance requires trustee authority, not beneficiary approval.
11. Stale NAV causes allocation and drift report to be partial.
12. Post-trade report reconciles target, executed trades, cash, costs, residual drift, and blocked actions.

## Reporting Labels

Use clear labels:

| Label | Meaning |
|---|---|
| Target allocation | Approved model or proposal target. |
| Current allocation | Current source-backed exposure view. |
| Drift | Current minus target under declared exposure lens. |
| Proposed rebalance | Recommended or DPM-approved actions before execution. |
| Executable | All required checks passed and funding is available. |
| Pending consent | Client or authorized party approval is required. |
| Pending liquidity | Funding depends on settlement, redemption, maturity, or other event. |
| Blocked | A hard control failed. |
| Partial | Data or product support is incomplete. |
| Residual drift | Drift remaining after execution or blocked actions. |

## Related Guides

Use this guide together with:

1. [Portfolio Exposure Modelling Guide](portfolio-exposure-modelling-guide.md)
2. [Product Lifecycle, Cashflow And Event Guide](product-lifecycle-cashflow-and-event-guide.md)
3. [Product Payoff, Scenario And Stress Guide](product-payoff-scenario-and-stress-guide.md)
4. [Advisory, Mandate And Reporting Decision Guide](advisory-mandate-reporting-decision-guide.md)
5. [Client Reporting And Portfolio Review Guide](client-reporting-and-portfolio-review-guide.md)
6. [Tax, Regulatory And Reporting](tax-regulatory-and-reporting.md)
7. [Wealth Structuring, Trusts And Family Office](wealth-structuring-trusts-and-family-office.md)
8. [Private Banking Operating Model Reference](../reference/private-banking-operating-model/README.md)

## Disclaimer

This document is for product knowledge, platform design, analytics, reporting, documentation, and QA work. It is not investment, legal, tax, regulatory, accounting, fiduciary, credit, trading, or client advice.

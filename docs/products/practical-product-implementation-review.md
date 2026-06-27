# Practical Product Implementation Review

This review layer turns the product packs into reusable working knowledge for private-banking, wealth-management, advisory, DPM, reporting, analytics, operations, QA, and platform implementation.

It is intentionally vendor-neutral. Use it to decide what a platform should represent, calculate, explain, block, or mark as unsupported for each product family.

## Cross-Product Review Lens

Use this lens when improving any product document or designing a product feature:

| Lens | Questions To Answer |
|---|---|
| Product understanding | What does the client legally own, why would they buy it, and what business need does it serve? |
| Taxonomy and coverage | Which product subtypes are in scope, adjacent, or explicitly out of scope? |
| Payoff behavior | What determines gain, loss, income, redemption, conversion, or termination? |
| Cashflow behavior | Which cashflows are contractual, conditional, discretionary, projected, confirmed, or operationally booked? |
| Valuation and pricing | Is value market-traded, NAV-based, model-based, issuer-quoted, administrator-reported, stale, or estimated? |
| Performance and attribution | How should price return, income, FX, fees, taxes, internal flows, external flows, contribution, and attribution be treated? |
| Portfolio modelling | What is the legal position, accounting position, analytical exposure, look-through exposure, and commitment or contingent exposure? |
| Advisory and suitability | What must an advisor explain before recommendation or review? What should trigger escalation or a documented limitation? |
| DPM and mandate relevance | Which mandate limits, eligibility rules, concentration checks, liquidity rules, and rebalancing constraints apply? |
| Reporting | What should appear on client statements, portfolio reviews, risk reports, income reports, maturity ladders, and exception reports? |
| Calculation requirements | Which formulas, schedules, dates, source fields, FX treatments, and rounding rules need deterministic implementation? |
| Edge cases | What breaks simple models: stale data, partial data, lifecycle events, restatements, physical delivery, gates, missing classifications, or negative balances? |
| Implementation boundary | What should the platform support directly, what should be imported from source systems, and what should remain explicitly unsupported until evidence exists? |

## Product Family Matrix

| Product Family | Practical Examples | Cashflow And Payoff Focus | Valuation And Analytics Focus | Advisory, DPM, And Reporting Focus | Implementation Edge Cases |
|---|---|---|---|---|---|
| Bonds | Government bond, investment-grade corporate, high-yield bond, callable bond, floater, zero coupon, amortising bond. | Coupons, accrued interest, clean/dirty price, redemption, call/put, amortisation, default, recovery. | Yield, duration, convexity, spread, credit rating, accrued interest, realized/unrealized P&L, income contribution. | Maturity ladder, income schedule, issuer concentration, credit quality, duration target, liquidity, currency exposure. | Ex-coupon periods, day-count conventions, negative yields, partial redemptions, default/write-down, stale prices, callable bond yield-to-worst. |
| Cash, deposits, money market and FX | Current account, call account, fixed deposit, T-bill, commercial paper, repo, money market fund, FX spot, FX forward, FX swap, NDF. | Interest, accrual, maturity, rollover, breakage, settlement, value-date cash, FX conversion, forward settlement, NDF net cash settlement. | Available cash, projected cash, yield, accrued interest, buying power, liquidity bucket, FX exposure, realized/unrealized FX P&L, cash drag. | Liquidity reserve, cash concentration, funding check, DPM cash drift, currency mismatch, client cash/yield report, settlement warnings. | Ledger versus available mismatch, unsettled cash, wrong-currency cash, stale FX rate, holiday calendar, deposit early break, MMF gate, failed settlement, overdraft. |
| Equities | Listed common stock, ADR/GDR, ETF-like exchange security, REIT, rights issue, spin-off, stock split. | Buy/sell, dividend, withholding tax, corporate action entitlement, stock distribution, rights exercise. | Market value, P&L, dividend yield, total return, beta, factor exposure, sector/country exposure, cost basis. | Single-name limits, sector/geography drift, restricted lists, income reporting, taxable events, corporate-action decisions. | Pending settlement, short settlement cycles, missing corporate-action election, fractional shares, dual-listed instruments, suspended trading. |
| Funds | Mutual fund, unit trust, ETF, money market fund, bond fund, multi-asset fund, fund of funds. | Subscription, redemption, switch, transfer, distribution, reinvestment, fee, NAV dealing. | NAV, stale/estimated NAV, expense ratio, trailer fee, look-through exposure, TWR, contribution, benchmark comparison. | Share-class suitability, liquidity terms, dealing cut-off, fund risk rating, concentration by manager/fund, look-through reporting. | Redemption gates, suspended NAV, side pockets, swing pricing, cut-off missed, duplicate orders, accumulating versus distributing share classes. |
| Loans, Lombard, margin and collateral | Overdraft, revolving credit line, term loan, Lombard loan, margin loan, portfolio loan, pledged collateral pool, cross-pledge. | Drawdown, repayment, interest accrual, fee, pledge, release, substitution, reservation, margin call, cure, forced liquidation. | Utilization, LTV, haircut, lending value, collateral coverage, buying power, leverage, interest cost, shortfall, stress loss. | Leverage suitability, collateral concentration, liquidity warning, margin-call risk, DPM leverage rule, pledged-asset reporting, credit exposure. | Wrong pledge direction, stale collateral price, missing in-flight order, transitive pledge assumption, rating downgrade, corporate action on pledged asset, forced sale. |
| Private markets | Private equity fund, venture capital, buyout, secondaries, private credit, infrastructure, real estate, hedge fund. | Commitment, capital call, contribution, distribution, return of capital, recallable capital, redemption, secondary transfer. | NAV lag, IRR, TVPI, DPI, RVPI, PME, J-curve, unfunded commitment, liquidity bucket, valuation uncertainty. | Illiquidity suitability, call-capacity planning, vintage diversification, manager concentration, private-asset allocation, client report caveats. | Missing historical cashflows, NAV restatement, unclassified distributions, in-kind distributions, transfer restrictions, gates, side pockets, negative unfunded balances. |
| Derivatives | Listed option, OTC option, futures, forwards, FX forward, swap, CDS, commodity future. | Premium, margin, variation margin, exercise, expiry, settlement, coupon/leg payment, close-out, collateral. | Greeks, notional exposure, mark-to-market, initial/variation margin, counterparty exposure, stress loss, hedge effectiveness. | Hedging purpose, leverage, suitability, mandate eligibility, collateral impact, exposure reporting, scenario explanation. | Nonlinear exposure, expired contracts, early exercise, collateral disputes, missing fixings, compression, novation, valuation-model gaps. |
| Structured products | Principal-protected note, reverse convertible, autocallable, DCI, CLN, certificate, warrant, accumulator, decumulator. | Conditional coupon, barrier, autocall, knock-in/knock-out, participation, cap/floor, alternate-currency redemption, physical delivery. | Issuer quote, model value, payoff scenario, barrier distance, underlying exposure, volatility sensitivity, issuer credit exposure. | Complexity disclosure, worst-case outcome, liquidity limits, issuer concentration, underlying concentration, suitability review, scenario reporting. | Barrier observation mismatch, stale issuer bid, physical settlement creates new asset, worst-of baskets, early termination, missing termsheet fields. |
| Structured notes | Medium-term note wrapper, equity-linked note, fixed coupon note, range accrual, credit-linked note, dual-currency note. | Coupon schedule, observation schedule, maturity redemption, autocall redemption, credit event, physical delivery. | Note valuation, clean/dirty treatment where relevant, embedded payoff state, issuer credit risk, underlying look-through, cashflow projection. | Note-specific suitability, term sheet review, coupon reliability, maturity/call ladder, underlying and issuer exposure reporting. | Note subtype sprawl, coupon condition not met, credit event write-down, alternate settlement currency, delivered shares, missing observation result. |

## Generic Platform Capability Baseline

Many wealth platforms can support these capabilities generically across product families:

1. instrument and product master,
2. portfolio, account, holding, and transaction records,
3. market value and reporting-currency restatement,
4. cashflow and transaction ledgers,
5. basic position, asset-class, issuer, sector, currency, and geography exposure,
6. time-weighted and money-weighted performance inputs,
7. contribution and attribution inputs when source data is sufficient,
8. concentration, drawdown, rolling risk, stress, and scenario inputs,
9. advisory proposal, suitability review, and policy evidence workflows,
10. mandate, eligibility, target-model, and rebalancing review workflows,
11. client statement, portfolio review, and exception reporting inputs,
12. data-quality posture such as missing, stale, estimated, unsupported, partial, or manually overridden.

Do not assume a platform supports product-specific behavior merely because it supports the generic layer. Product-specific behavior needs explicit data, methodology, source ownership, validation, and user-facing degraded-state handling.

## Candidate Product-Specific Extensions

Use these as future roadmap candidates when they are not already implementation-backed:

| Product Family | Candidate Extensions |
|---|---|
| Bonds | Yield-to-worst, callable schedule modelling, amortisation schedules, default/recovery workflow, issuer event history, accrued interest audit. |
| Cash, deposits, money market and FX | Buying-power engine, projected cash ladder, deposit accrual/breakage engine, MMF liquidity state, FX funding workflow, FX forward/NDF settlement and P&L workflow. |
| Equities | Corporate-action election workflow, tax-lot-aware realized P&L, restricted stock handling, rights/entitlement processing, multi-listing normalization. |
| Funds | Dealing calendar engine, cut-off control, stale NAV posture, gate/suspension state, share-class fee comparison, look-through file lineage. |
| Loans, Lombard, margin and collateral | Facility/loan ledger, collateral eligibility engine, pledge graph, LTV/haircut engine, buying-power reservations, margin-call workflow, forced-liquidation workflow. |
| Private markets | Commitment ledger, capital-call notice processing, distribution classification, NAV restatement, IRR/TVPI/DPI/RVPI engine, unfunded liquidity stress. |
| Derivatives | Greeks engine, margin/collateral ledger, exercise/expiry workflow, counterparty exposure, hedge-effectiveness reporting, OTC confirmation matching. |
| Structured products | Payoff term schema, observation scheduler, barrier/autocall state engine, scenario payoff calculator, issuer quote lineage, physical delivery workflow. |
| Structured notes | Note term-sheet parser, note lifecycle event store, coupon condition engine, note maturity/call ladder, credit-event handling, delivered-asset booking. |

## Product Example Expansion Backlog

Add worked examples in this order when deepening the packs:

1. **Cash, deposits, money market and FX:** unsettled sell proceeds; fixed deposit maturity and breakage; T-bill purchase and maturity; MMF redemption gate; FX spot funding; FX forward hedge; NDF fixing and net settlement.
2. **Bonds:** callable corporate bond with yield-to-worst; floater with reset date; amortising bond; default and recovery example.
3. **Equities:** cash dividend with withholding tax; rights issue; stock split; spin-off; partial sale with tax lots.
4. **Funds:** amount-based subscription; unit-based redemption; ETF trade; reinvested distribution; redemption gate; stale NAV report.
5. **Loans, Lombard, margin and collateral:** simple Lombard availability; in-flight order reservation; price-fall margin call; FX haircut; collateral substitution; forced liquidation.
6. **Private markets:** commitment plus capital calls; distribution split; NAV restatement; secondary purchase; in-kind distribution; capital-call stress.
7. **Derivatives:** protective put; covered call; FX forward hedge; interest-rate swap cashflow; futures margin; CDS credit event.
8. **Structured products:** autocallable note path; reverse convertible physical settlement; DCI alternate-currency redemption; accumulator knock-out; CLN credit event.
9. **Structured notes:** fixed coupon note; worst-of autocallable; range accrual; equity-linked note with delivered shares; credit-linked note write-down.

Each worked example should include:

1. client objective,
2. product terms,
3. lifecycle timeline,
4. transactions and cashflows,
5. valuation and risk view,
6. advisory and suitability notes,
7. reporting output,
8. QA scenarios and expected results,
9. implementation boundary and unsupported states.

## Review Checklist

Before calling a product pack polished, verify that it answers:

1. What does the client legally own?
2. What drives payoff and cashflow?
3. What are the key subtypes?
4. Which dates matter?
5. Which source owns each critical data element?
6. What is a transaction versus a lifecycle event?
7. What is legal holding versus analytical exposure?
8. How is valuation sourced and how is stale data shown?
9. How are income, performance, contribution, attribution, and risk treated?
10. What must advisory, DPM, operations, reporting, and QA teams know?
11. What can be supported generically and what needs product-specific implementation?
12. What examples would make the product easier to understand and test?

## Disclaimer

This review is for education, product analysis, documentation, platform design, and implementation planning. It is not investment, legal, tax, accounting, regulatory, or client advice.

# Product Lifecycle, Cashflow And Event Guide

This guide defines a reusable cross-product standard for modelling product lifecycle events, economic transactions, orders, cashflows, accruals, income, fees, taxes, liabilities, and reporting effects.

Use it when reviewing a product pack, designing source intake, writing acceptance criteria, reconciling portfolio data, building client reports, or deciding whether an event should affect cash, position, income, performance, risk, or only product state.

## Core Rule

Do not treat every event as a transaction and do not treat every transaction as a cashflow.

A robust product model separates:

1. orders and instructions,
2. lifecycle events,
3. economic transactions,
4. cash movements,
5. non-cash postings,
6. accruals,
7. entitlements,
8. obligations,
9. accounting or performance classifications,
10. reporting labels,
11. source evidence,
12. reversal and correction behavior.

## Lifecycle Vocabulary

| Term | Meaning | Examples | Typical System Effect |
|---|---|---|---|
| Order | Client or advisor instruction before execution or acceptance. | Buy order, fund subscription, FX order, policy application, loan drawdown request. | Creates pending workflow state; should not change confirmed holdings until accepted/executed. |
| Trade | Executed market or product transaction before final settlement. | Equity trade, bond trade, FX spot trade, listed option trade. | Creates trade-date exposure and pending settlement. |
| Booking | Internal recording of an executed or accepted event. | Booked buy, booked coupon, booked capital call, booked drawdown. | Updates records according to source and posting rules. |
| Settlement | Completion of cash, security, fund unit, or contract delivery. | T+2 equity settlement, FX value date, bond delivery-versus-payment. | Moves pending items into settled position or cash. |
| Lifecycle event | Product state event that may or may not create an economic posting. | Barrier observation, corporate action announcement, margin call notice, policy lapse notice. | Updates state, creates tasks, or triggers future transactions. |
| Economic transaction | Posting that changes position, cash, income, expense, liability, cost basis, or obligation. | Buy, sell, coupon, dividend, premium, fee, tax, drawdown, capital call. | Feeds portfolio ledger, performance, reporting, reconciliation, and audit. |
| Cashflow | Movement or projected movement of cash. | Settlement payment, coupon receipt, dividend payment, premium paid, redemption proceeds. | Affects cash ledger, liquidity, projected cash, and reporting. |
| Non-cash transaction | Economic change without immediate cash movement. | Stock split, bonus issue, reinvested distribution, PIK interest, in-kind distribution. | Updates quantity, cost basis, income classification, or exposure without cash. |
| Accrual | Earned or incurred amount before cash payment. | Bond accrued interest, loan accrued interest, fund expense accrual, performance fee accrual. | Affects valuation, income estimate, performance, or liability depending on methodology. |
| Entitlement | Right to receive cash, securities, benefits, or election option. | Dividend entitlement, rights issue, insurance claim right, distribution notice. | May create pending receivable, election workflow, or future cash/security movement. |
| Obligation | Amount or action the client may owe or need to fund. | Capital call, premium due, loan repayment, margin call, tax withholding. | Drives liquidity planning, alerts, restrictions, and client communication. |
| Correction | Change to prior source event or derived result. | Reversal, cancel/rebook, restated NAV, corrected dividend, amended capital call. | Must preserve audit trail and recompute dependent outputs. |

## Event-To-Posting Decision Rules

Use these questions before classifying any product event:

1. Did a source owner confirm execution or acceptance?
2. Does the event change legal holding, liability, or obligation?
3. Does the event change settled cash?
4. Does it create pending cash, receivable, payable, or entitlement?
5. Does it change cost basis, accrued income, realized P&L, or performance classification?
6. Does it only change product state, eligibility, risk, or workflow status?
7. Is settlement complete, pending, failed, cancelled, or partially settled?
8. Is the event source-backed, estimated, manual, stale, or unsupported?
9. Is there a later reversal, correction, or restatement path?
10. Which client-reporting label should be used, and what should not be inferred?

## Operational Correction Patterns

| Pattern | Required Treatment |
|---|---|
| Allocation correction | Link original order, execution, trade, allocation, approval evidence, corrected account split, position/cash/lot delta and report impact. |
| Linked FX settlement | Preserve security trade, FX order, funding dependency, value date, cash reservation and settlement exception state as connected facts. |
| Income reversal | Reverse gross income, tax and net cash with lineage; create receivable or reclaim workflow when cash/tax cannot reverse immediately. |
| Trade cancellation | Preserve original order/execution/trade and cancel through a versioned lifecycle event rather than deleting history. |
| Late fee or tax posting | Post fees/taxes as linked transaction legs or adjustments; decide cost-basis and performance treatment by policy. |
| Performance restatement | Preserve original output, corrected inputs, recalculation result, materiality decision and report/client notice decision. |
| Reconciliation break closure | Store break components, source evidence, resolution action, tolerance/materiality decision and close/reopen state. |

## Cross-Product Event Matrix

| Product Area | Event Or Transaction | Cash Effect | Position Or Obligation Effect | Reporting And Analytics Treatment |
|---|---|---|---|---|
| Bonds | Buy trade | Cash outflow on settlement; accrued interest paid separately if applicable. | Increases nominal position after settlement; creates trade-date exposure earlier if tracked. | Separate clean price, accrued interest, fees, settlement date, cost basis, and yield impact. |
| Bonds | Coupon record/payment | Cash receipt on payment date. | No nominal change. | Income contribution; may have tax withholding and accrued-versus-received distinction. |
| Bonds | Call, put, maturity, redemption | Cash receipt on event settlement date. | Reduces or closes nominal position. | Realized P&L and income treatment depends on price, premium/discount, accrued interest, and methodology. |
| Bonds | Default, write-down, restructuring | May reduce, delay, or replace expected cashflows. | May change claim, nominal, recovery asset, or status. | Must label credit event, impairment, recovery assumptions, and stale valuation risk. |
| Cash and deposits | Time deposit placement | Cash outflow from available cash; deposit position created. | Deposit claim created with maturity and rate. | Separate cash ledger from deposit position; report maturity ladder and breakage terms. |
| Cash and deposits | Deposit maturity | Cash inflow if principal and interest are paid. | Deposit closes. | Split principal return from interest income; handle rollover separately. |
| Money market funds | Subscription/redemption | Cash outflow/inflow based on dealing and settlement. | Fund units increase/decrease. | NAV date, dealing date, gates, liquidity fee, and settlement must be explicit. |
| FX | Spot trade | Two currency legs settle on value date. | Creates pending currency exchange until settlement. | Separate trade date, value date, realized FX, funding effect, and available cash. |
| FX | Forward maturity | Currency exchange or cash settlement. | Forward contract closes. | Separate hedge result, spot component, forward points, and settlement currency. |
| Equities | Buy/sell trade | Cash settles on market cycle. | Shares change after settlement; trade-date exposure may change earlier. | Track lots, cost basis, fees, taxes, settlement, and realized P&L. |
| Equities | Dividend | Cash or stock entitlement, then payment. | Usually no share change unless stock dividend or reinvestment. | Income, withholding tax, ex-date, record date, pay date, and currency conversion matter. |
| Equities | Split or consolidation | No cash unless fractional settlement. | Quantity and per-share cost basis change. | Non-cash event; performance should not show artificial gain/loss. |
| Equities | Rights issue | Cash outflow if exercised; sale proceeds if rights sold. | May create rights position, new shares, or lapse. | Election workflow, entitlement ratio, subscription price, expiry, and client decision status matter. |
| Funds | Subscription | Cash outflow on settlement or dealing. | Units confirmed after NAV calculation. | Pending order must remain separate from confirmed holding; NAV date and price are critical. |
| Funds | Redemption | Cash inflow after dealing and settlement. | Units decrease when confirmed. | Redemption gates, suspension, swing pricing, fees, and stale NAV affect reporting. |
| Funds | Distribution | Cash or reinvested units. | No unit change for cash distribution; units increase for reinvestment. | Classify income, return of capital, tax, and reinvestment separately. |
| Derivatives | Option premium | Cash outflow/inflow at trade settlement. | Option position created. | Premium is not notional; track Greeks, market value, expiry, and realized P&L. |
| Derivatives | Daily futures variation margin | Cash movement, often daily. | Contract remains open until close/expiry. | Margin cashflow is performance-relevant but not the same as exposure. |
| Derivatives | OTC collateral call | Collateral movement or cash pledge. | Collateral obligation changes; derivative position may not change. | Separate collateral from derivative value, exposure, and P&L. |
| Derivatives | Exercise or assignment | Cash/security delivery or contract settlement. | Underlying position may be created or removed. | Exercise terms, settlement style, fees, taxes, and resulting position must be explicit. |
| Structured products | Observation event | Usually no immediate cash movement. | Product state may change; autocall/coupon condition may be triggered. | Do not post coupon or redemption until confirmed; record observation source and outcome. |
| Structured products | Autocall | Cash inflow on call settlement date. | Product closes. | Split redemption principal, coupon, fees/taxes, realized result, and issuer quote lineage. |
| Structured notes | Physical settlement | Underlying securities delivered instead of cash redemption. | Note closes; delivered asset position opens. | Cost basis, delivered asset value, issuer event, and suitability implications must be clear. |
| Private markets | Commitment | Usually no immediate cash movement. | Unfunded obligation created. | Commitment is not NAV; track unfunded liquidity requirement and exposure commitment. |
| Private markets | Capital call | Cash outflow when funded. | Paid-in capital increases; unfunded decreases. | Classify call purpose: investment, fee, expense, equalization, recallable amount if applicable. |
| Private markets | Distribution | Cash or in-kind receipt. | May reduce NAV, create realized proceeds, or return capital. | Split return of capital, gain, income, recallable distribution, and tax where available. |
| Private markets | NAV restatement | No cash movement. | Reported value changes retroactively or prospectively. | Preserve prior value, restatement date, source date, and performance recomputation policy. |
| Real estate and infrastructure | Fund capital call | Cash outflow. | Paid-in and unfunded commitment update. | Similar to private markets; also capture asset strategy, leverage, and valuation lag. |
| Real estate and infrastructure | REIT distribution | Cash or stock distribution. | Listed unit position may remain unchanged or increase if reinvested. | Split income, return of capital, withholding tax, and currency conversion. |
| Direct property | Rental income | Cash receipt or receivable. | Property holding unchanged. | Separate gross rent, expenses, taxes, vacancy, and net operating income if source-backed. |
| Insurance | Premium payment | Cash outflow. | Policy remains active; account value may change after charges/allocation. | Split premium, charges, allocation to sub-funds, protection component, and commission if known. |
| Insurance | Surrender | Cash inflow net of charges/tax. | Policy closes or reduces. | Use surrender value, not death benefit or account value, unless explicitly reconciled. |
| Insurance | Claim event | Cash or benefit entitlement after acceptance. | Coverage may close, reduce, or continue. | Privacy, beneficiary, claim status, and benefit basis require controlled reporting. |
| Loans and collateral | Facility setup | May have fee cashflow; no drawdown yet. | Credit limit and covenants created. | Limit is not debt; report availability separately from outstanding balance. |
| Loans and collateral | Drawdown | Cash inflow to client account or payment to third party. | Loan liability increases. | Separate principal, interest accrual, fees, maturity, rate reset, and collateral linkage. |
| Loans and collateral | Interest accrual/payment | Accrual before payment; cash outflow on payment. | Liability may increase if capitalized. | Accrued interest, paid interest, capitalized interest, and performance treatment must be separate. |
| Loans and collateral | Margin call | Usually no immediate cash movement at notice time. | Obligation to add collateral, repay, or liquidate risk. | State deadline, deficit, eligible collateral, cure action, and blocked/forced-sale path. |

## Cashflow Classification

Classify every cashflow before it feeds reporting or analytics.

| Classification | Use For | Examples | Analytics Notes |
|---|---|---|---|
| External contribution | Client adds money or assets to portfolio. | Deposit, transfer-in, security transfer-in. | Usually excluded from investment performance return numerator. |
| External withdrawal | Client removes money or assets. | Cash withdrawal, transfer-out, security transfer-out. | Usually treated as external flow. |
| Investment purchase | Cash used to acquire product exposure. | Equity buy, bond buy, fund subscription. | Affects cost basis and exposure; not income or performance gain. |
| Investment sale or redemption | Cash from reducing or closing exposure. | Equity sale, bond redemption, fund redemption. | Splits proceeds, realized P&L, fees, tax, and principal return. |
| Income | Return generated by holding the product. | Coupon, dividend, fund distribution, rental income, deposit interest. | Feeds income reporting and contribution depending on methodology. |
| Return of capital | Cash returned that reduces invested capital. | Private fund ROC, fund capital distribution, amortizing principal. | Not the same as income; affects cost basis and multiples. |
| Fee or expense | Charge paid by client, portfolio, product, or vehicle. | Brokerage, custody, management fee, performance fee, loan fee. | Treatment depends on gross/net performance methodology. |
| Tax | Withholding, transaction tax, stamp duty, reclaim, tax refund. | Dividend withholding, stamp duty, transaction levy. | Separate from fees and income; preserve jurisdiction/source if available. |
| Financing cashflow | Loan principal, interest, collateral, margin, securities financing. | Drawdown, repayment, margin payment, collateral movement. | Separate liability and collateral from investment return. |
| Protection or policy cashflow | Insurance and annuity policy cashflows. | Premium, surrender, claim, annuity payout. | Value basis and benefit classification must be explicit. |
| Derivative settlement | Contract settlement and margin cashflows. | Option premium, variation margin, swap settlement, forward settlement. | Notional and exposure are not cashflow. |
| Internal transfer | Movement between accounts under same reporting perimeter. | Account transfer, currency funding sweep. | Avoid double-counting as external contribution/withdrawal. |
| Correction or reversal | Source correction to prior cashflow. | Cancel/rebook, amended tax, corrected distribution. | Must link to original event and recompute impacted outputs. |

## Performance And Attribution Treatment

Lifecycle and cashflow classification has direct impact on performance:

1. Purchases and sales change exposure but are not investment gain by themselves.
2. Coupons, dividends, distributions, rent, and deposit interest are income components.
3. Fees and taxes may be included or excluded depending on gross/net methodology, but must be labelled.
4. External contributions and withdrawals should not be confused with performance return.
5. Capital calls and private-market distributions require commitment, paid-in, unfunded, NAV, and cashflow history.
6. Derivative margin and collateral movements must not be treated as exposure changes unless methodology explicitly says so.
7. Stock splits, consolidations, bonus issues, and many corporate actions should not create artificial performance.
8. FX effects must separate local-currency product return from reporting-currency translation when source data supports it.
9. Accruals and actual cash receipts must be reconciled, especially for bonds, loans, deposits, and swaps.
10. Restatements require a documented recomputation policy and visible lineage.

## Reporting Rules

Client and management reports should make these distinctions visible:

1. Confirmed holdings versus pending orders.
2. Trade date versus settlement date.
3. Ledger cash versus settled cash versus available cash versus projected cash.
4. Market value versus notional versus exposure.
5. Income versus return of capital versus principal repayment.
6. Fees versus taxes versus product charges.
7. Commitment versus paid-in versus unfunded versus NAV.
8. Account value versus surrender value versus death benefit for insurance.
9. Market value versus collateral value after haircut.
10. Stale, estimated, manual, blocked, and unsupported data states.

## Implementation Design Checklist

Use this checklist when designing or reviewing product lifecycle support:

1. Define allowed lifecycle states for each product subtype.
2. Define which states are source-owned, calculated, or manually maintained.
3. Define event identifiers and idempotency rules.
4. Define trade date, effective date, record date, ex-date, value date, settlement date, payment date, maturity date, and received date separately where relevant.
5. Define whether the event creates cash, position, income, expense, liability, accrual, receivable, payable, entitlement, or only state.
6. Define cancellation, reversal, amendment, restatement, and replay behavior.
7. Define partial settlement and failed settlement behavior.
8. Define currency and FX conversion treatment.
9. Define cost-basis and lot treatment.
10. Define performance and reporting classification.
11. Define source ownership, lineage, freshness, and supportability state.
12. Define reconciliation controls and exception states.
13. Define audit trail and user-visible explanation.
14. Define QA scenarios for normal, partial, stale, corrected, unsupported, migrated, and backdated events.

## QA Scenario Checklist

Good QA coverage should include:

1. pending order does not become confirmed holding too early,
2. trade-date exposure and settlement-date cash are both correct,
3. partial settlement is represented without corrupting position or cash,
4. cancelled order leaves no confirmed holding,
5. reversal links to original event and recomputes dependent totals,
6. corporate action adjusts quantity and cost basis without false P&L,
7. coupon or dividend separates gross amount, tax, net amount, and FX conversion,
8. capital call reduces unfunded commitment and increases paid-in capital,
9. distribution splits income, gain, return of capital, and recallable amount when source-backed,
10. derivative margin does not overwrite derivative market value,
11. loan drawdown increases liability and cash separately,
12. margin call notice does not automatically post a repayment or liquidation,
13. insurance surrender uses surrender value and charges, not death benefit,
14. stale NAV blocks unsupported performance claims,
15. manual override carries reason, owner, timestamp, and audit trail,
16. migration preserves historical cashflow classification,
17. reporting labels match the economic event type,
18. unsupported lifecycle behavior is withheld or labelled instead of inferred.

## Product Review Prompts

When reviewing any product pack, use these prompts:

1. What are the order, event, transaction, cashflow, and settlement states?
2. Which state changes are client-visible?
3. Which events create cash, position, income, liability, or only workflow status?
4. What date fields are mandatory?
5. What source owns each lifecycle event?
6. What happens when the source sends a correction?
7. How does the event affect performance and attribution?
8. How does it affect advisory, suitability, concentration, mandate, or DPM controls?
9. How should it appear in client reporting?
10. What should be blocked when data is stale, partial, or unsupported?

## Related Guides

Use this guide together with:

1. [Product Taxonomy And Vocabulary Guide](product-taxonomy-and-vocabulary-guide.md)
2. [Product Source Intake And Migration Guide](product-source-intake-and-migration-guide.md)
3. [Source Ownership, Calculation And Reporting Matrix](source-ownership-calculation-reporting-matrix.md)
4. [Product Performance, Attribution And Risk Guide](product-performance-attribution-risk-guide.md)
5. [Product Calculation Example Catalog](product-calculation-example-catalog.md)
6. [Product QA Regression Matrix](product-qa-regression-matrix.md)
7. [Client Reporting And Portfolio Review Guide](client-reporting-and-portfolio-review-guide.md)

## Disclaimer

This document is for product knowledge, platform design, analytics, documentation, and QA work. It is not investment, legal, tax, regulatory, or client advice.

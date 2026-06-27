# Product Capability Boundary Matrix

This matrix helps decide whether a product feature can be supported through a generic portfolio platform capability or needs product-specific implementation, source ownership, calculations, controls, and QA.

Use it when writing requirements, reviewing API scope, designing UI states, planning product roadmap, preparing QA scenarios, or deciding whether a feature should be marked supported, partial, source-limited, or unsupported.

## Support Classification

| Classification | Meaning | Product And Delivery Implication |
|---|---|---|
| Generic baseline | The platform can usually represent the feature with common account, holding, transaction, valuation, FX, reporting, and data-quality models. | Still requires source ownership, dates, currency treatment, reconciliation, and user-visible stale/missing states. |
| Source-backed extension | The feature is product-specific and should be supported only when the required source fields, formulas, lifecycle events, and controls exist. | Needs explicit data contract, calculation method, UI labels, regression tests, and exception handling. |
| Support-limited | The platform can store or display partial information, but should not claim full calculation, advisory, reporting, or workflow support. | Must show limitations clearly and avoid inferred results. |
| Future candidate | Valuable capability that should be considered for roadmap, but should not appear as current behavior until implementation evidence exists. | Capture source needs, expected outputs, acceptance tests, and operational owners before build. |

## Generic Baseline Capabilities

Most product families can share these capabilities:

1. account, portfolio, product, instrument, holding, and transaction records,
2. trade date, value date, settlement date, booking date, valuation date, observation date, and maturity date fields,
3. market value, reporting-currency value, local-currency value, FX source, and FX timestamp,
4. basic realized/unrealized P&L, income cashflow, fees, taxes, and external flow classification,
5. asset class, product family, issuer, currency, country, sector, region, and counterparty tags when source-backed,
6. basic TWR inputs and contribution inputs when valuations and cashflows are complete,
7. data-quality states such as stale, missing, estimated, manual, corrected, partial, disputed, restricted, and unsupported,
8. reconciliation status against custodian, broker, administrator, issuer, insurer, lender, or internal ledger sources,
9. client reporting labels, source dates, and supportability notes,
10. audit trail for overrides, source corrections, restatements, and manually enriched data.

These baseline capabilities are not enough to claim full product support. Product-specific behavior should be treated as a separate capability until proven.

## Product Family Boundary Matrix

| Product Family | Generic Baseline Can Usually Handle | Product-Specific Support Requires | Do Not Claim Supported Until | High-Value Future Candidate |
|---|---|---|---|---|
| Bonds | Position, price, market value, coupon cash, maturity date, issuer, rating, currency, basic income reporting. | Day count, accrued interest, clean/dirty price, coupon schedule, call/put schedule, amortisation, default/recovery, yield convention. | Yield, duration, accrued income, call treatment, default status, and maturity ladder reconcile to source terms and valuation policy. | Callable yield-to-worst engine, default/recovery workflow, issuer-event timeline, credit-spread attribution. |
| Cash, deposits, money market and FX | Cash balances by account/currency, transactions, FX translation, basic deposit maturity, basic liquidity report. | Ledger/settled/available/projected cash split, deposit accrual and breakage, MMF gates, FX forwards/swaps/NDF lifecycle, buying-power basis. | Available cash is source-owned and not inferred from ledger balance alone; value dates, restrictions, reservations, and pending settlements are handled. | Projected cash ladder, funding-gap engine, deposit breakage calculator, FX hedge-ratio workflow. |
| Commodities, precious metals and real assets | Holding quantity, price, market value, currency, basic commodity classification, simple ETP/fund/security wrapper. | Unit-of-measure validation, physical/allocated/unallocated distinction, futures contract size, roll, storage fees, delivery workflow, collateral haircuts, issuer/provider risk. | Unit, grade, wrapper type, price unit, custody/provider, notional exposure, pledge state, and stale-price behavior are explicit. | Commodity exposure engine across physical, ETP, derivative, note, and equity proxy wrappers. |
| Equities | Listed holding, trade, settlement, price, market value, dividends, sector/country/currency tags. | Corporate-action election, entitlement, tax-lot cost basis, withholding tax, ADR/GDR mapping, restricted stock, suspended/delisted handling. | Dividends, splits, rights, spin-offs, mergers, restrictions, tax lots, and settlement states reconcile to source records. | Corporate-action decision workflow, tax-lot-aware realized P&L, multi-listing normalization, restricted-stock lifecycle. |
| Funds | Fund holding, NAV, market value, subscription/redemption, distribution, share class, manager, basic look-through tag. | Dealing calendar, cut-off, order status, stale/estimated NAV, gates, suspensions, side pockets, fee/share-class logic, look-through lineage. | NAV date, order state, liquidity state, share-class identity, distribution classification, and look-through source are present. | Dealing-calendar engine, gate/suspension workflow, look-through quality scoring, share-class suitability comparison. |
| Insurance and annuities | Policy record, insurer, premium cashflows, basic value display, product type, client/advisor linkage. | Policy roles, coverages, riders, account/cash/surrender/death-benefit value basis, guarantee/projection labels, policy loans, claims, annuity payout schedule. | Value basis, source date, guarantee status, loan impact, premium status, beneficiary privacy, and claim/surrender lifecycle are explicit. | Policy contract model, ILP sub-fund look-through, lapse-risk workflow, annuity retirement-income projection. |
| Loans, Lombard, margin and collateral | Loan/facility record, cash drawdown/repayment, interest/fee cashflows, collateral holding reference, basic utilization. | Facility limit, drawn exposure, accrued interest, pledge graph, eligibility, haircut/LTV, buying-power reservations, margin calls, cure/liquidation workflows. | Availability reconciles to facility headroom, collateral value, haircut rules, FX, reservations, pledge direction, and stale source states. | Collateral optimization, cross-pledge allocation, haircut stress engine, forced-liquidation workflow. |
| Private markets | Fund record, commitment, capital call, distribution, NAV, valuation date, basic IRR inputs when complete. | Paid-in/unfunded/recallable logic, distribution classification, NAV lag/restatement, side pockets, transfer restrictions, secondary transactions, PME/multiples. | Cashflow history, commitment ledger, NAV date/received date, distribution split, and partial-history state are source-backed. | Notice ingestion workflow, IRR/TVPI/DPI/RVPI engine, unfunded liquidity stress, NAV restatement lineage. |
| Real estate, REITs and infrastructure | Listed REIT/security/fund holding, price/NAV, market value, distribution, basic sector/geography tags. | Property/infrastructure look-through, appraisal values, leverage, occupancy, cap-rate sensitivity, redemption queue, private fund calls, direct-property documents. | Wrapper behavior and economic exposure are separate; valuation date, source type, leverage, liquidity, and look-through coverage are explicit. | Real-asset exposure taxonomy, appraisal restatement workflow, property/infrastructure income-quality reporting. |
| Derivatives | Contract record, notional, market value, counterparty, expiry, underlying, basic P&L and settlement cashflows. | Greeks, delta/DV01/CS01 exposure, margin/collateral, fixings, exercise/expiry, resets, close-out, CSA terms, OTC confirmation matching. | Contract terms, lifecycle events, valuation inputs, collateral, expired-state handling, and exposure measures are source-backed. | Central exposure engine, margin/collateral ledger, hedge-effectiveness reporting, OTC lifecycle automation. |
| Structured products | Note/product position, issuer, underlyings, market value/issuer quote, maturity, coupon/redemption cashflows. | Payoff formula, barriers, observations, autocall/coupon conditions, scenario payoff, issuer credit, physical delivery, term-sheet lineage. | Observations, barrier states, coupon conditions, issuer quote date, payoff terms, and settlement outcomes are explicit. | Payoff schema, observation scheduler, scenario calculator, physical-delivery workflow. |
| Structured notes | Note wrapper, issuer, coupon schedule, maturity date, valuation, underlying references, basic income report. | Note subtype terms, coupon conditions, autocall/barrier logic, credit event handling, delivered asset booking, cost-basis policy. | Term sheet fields, observation results, coupon status, credit events, delivery terms, and issuer quote lineage are reconciled. | Note lifecycle event store, term-sheet parser, coupon-condition engine, maturity/call ladder. |

## Capability Decision Checklist

Before marking a capability as supported, answer:

1. What product family and subtype is in scope?
2. What generic capability is being reused?
3. What product-specific source fields are required?
4. Which system or external party owns each field?
5. Which dates, currencies, units, and identifiers are required?
6. Which calculation formula, convention, and rounding basis applies?
7. What lifecycle events change status, value, exposure, cashflow, or eligibility?
8. What should the API return when data is stale, missing, estimated, disputed, restricted, partial, or unsupported?
9. What should the UI show so business users do not mistake partial support for full support?
10. What reconciliation source proves the output?
11. What regression scenario proves normal behavior?
12. What regression scenario proves degraded behavior?
13. Which reports, workflows, and downstream analytics consume the output?
14. Which user roles may view, override, approve, or export the data?

## Reporting Boundary Examples

| Claim | Safe Only When | Otherwise Show |
|---|---|---|
| Available cash | Ledger, settlement, restrictions, reservations, value dates, and credit policy are source-backed. | Ledger cash, projected cash, or support-limited availability. |
| Bond yield-to-worst | Coupon, day count, clean/dirty price, call/put schedule, settlement date, and yield convention are available. | Yield unavailable or yield-to-maturity only with limitation label. |
| Fund liquidity | Dealing calendar, cut-off, redemption terms, gate/suspension state, and settlement cycle are current. | Liquidity terms unknown, stale, or source-limited. |
| Private-market IRR | Complete dated cashflow history and terminal NAV are available. | Partial-history IRR unavailable or support-limited. |
| Derivative exposure | Contract terms, notional, underlying, multiplier, valuation input, and sensitivity basis are present. | Market value only, with exposure unsupported. |
| Structured-product scenario | Payoff terms, observations, barriers, underlyings, market inputs, and settlement rules are captured. | Illustrative scenario unavailable or support-limited. |
| Insurance surrender value | Insurer quote, account value, charges, policy loans, guarantee status, and quote date are available. | Account value or policy value shown separately, not as accessible cash. |
| Commodity collateral value | Quantity, unit, price source, FX, eligibility, haircut, pledge state, and custody/provider are current. | Market value only or collateral value unavailable. |
| Real-asset allocation | Wrapper, property/infrastructure sector, geography, source date, and look-through coverage are available. | Generic asset-class allocation with partial look-through warning. |

## QA Evidence Standard

A capability should have QA evidence for:

1. normal happy-path calculation,
2. stale source data,
3. missing mandatory field,
4. conflicting source values,
5. corrected source value or restatement,
6. partial support state,
7. unsupported subtype,
8. permission or restricted-data behavior,
9. downstream report output,
10. reconciliation against source owner.

## Disclaimer

This matrix is for education, product analysis, documentation, QA, architecture, and platform-design work only. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.

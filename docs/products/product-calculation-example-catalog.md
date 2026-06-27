# Product Calculation Example Catalog

This catalog provides compact calculation examples across the covered product families.

Use it when turning product knowledge into requirements, API contracts, QA scenarios, data migration checks, reporting specs, or implementation acceptance criteria. The examples are intentionally simple so the formula, source inputs, reporting labels, and failure behavior are visible.

For lifecycle-heavy examples, also use [`cross-product-worked-examples.md`](cross-product-worked-examples.md). For performance, attribution, and risk treatment, use [`product-performance-attribution-risk-guide.md`](product-performance-attribution-risk-guide.md). For source ownership and failure states, use [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md). For support boundaries, use [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md).

## Calculation Pattern

Each calculation should define:

1. required source inputs,
2. formula and convention,
3. output labels,
4. reporting meaning,
5. degraded-state behavior,
6. QA assertion.

If any required input is missing, stale, estimated, disputed, restricted, or unsupported, the output should carry that state instead of looking final.

## Examples By Product Family

| Product Family | Example | Core Formula | Reporting Output | QA Assertion |
|---|---|---|---|---|
| Cash, deposits, money market and FX | Available cash after unsettled trades | `available_cash = ledger_cash + settled_receivables - settled_payables - restrictions - reservations` | Ledger cash, settled cash, projected cash, available cash, restrictions, reservations. | A pending sell settling T+2 increases projected cash before T+2 but not available cash unless bridge funding is explicitly supported. |
| Cash, deposits, money market and FX | Fixed deposit accrued interest | `accrued_interest = principal x annual_rate x day_count_fraction` | Principal, rate, accrued interest, maturity value, maturity date, breakage terms. | Accrual uses product day-count convention and stops or changes when deposit is broken early. |
| Cash, deposits, money market and FX | FX forward mark-to-market | `mtm_reporting_ccy = present_value(forward_leg_receive - forward_leg_pay)` | Forward notional, buy/sell currency, value date, MTM, hedge link, realized P&L at settlement. | Forward notional never appears as cash balance before settlement. |
| Bonds | Dirty market value | `dirty_value = nominal x dirty_price / price_factor` | Clean value, accrued interest, dirty value, local/reporting currency value. | Clean value plus accrued interest equals dirty value within rounding tolerance. |
| Bonds | Coupon accrual | `accrued_interest = nominal x coupon_rate x day_count_fraction` | Accrued income, coupon receivable, income report, accrued-interest source date. | Ex-coupon or default state changes accrual treatment according to source terms. |
| Bonds | Yield-to-worst | `yield_to_worst = min(yield_to_each_valid_call_or_maturity_date)` | Yield-to-maturity, yield-to-call, yield-to-worst, call schedule, maturity ladder. | Yield-to-worst is blocked when callable schedule or price basis is missing. |
| Equities | Dividend entitlement | `gross_dividend = eligible_shares x dividend_per_share` | Gross dividend, tax withheld, net cash, ex-date, record date, pay date. | Shares bought after ex-date do not receive dividend even if held on pay date. |
| Equities | Realized P&L on partial sale | `realized_pnl = sale_proceeds - allocated_cost_basis - fees - tax` | Proceeds, cost basis, fees, tax, realized gain/loss, remaining tax lots. | FIFO/average/specific-lot method is explicit and repeatable. |
| Equities | Stock split quantity | `new_quantity = old_quantity x split_ratio` | Adjusted quantity, adjusted cost per share, unchanged total cost basis. | Total cost basis remains unchanged after split unless cash-in-lieu applies. |
| Funds | NAV-based market value | `market_value = units x nav_per_unit` | Units, NAV, NAV date, market value, stale NAV flag. | Market value is stale when NAV date breaches policy threshold. |
| Funds | Redemption settlement | `cash_settlement = redeemed_units x dealing_nav - fees - tax` | Redeemed units, remaining units, settlement cash, dealing NAV, order status. | Gated redemption books only confirmed portion and leaves residual pending or invested. |
| Funds | Distribution reinvestment | `new_units = net_distribution / reinvestment_nav` | Gross distribution, tax, net reinvestment, new units, NAV. | Reinvested distribution increases units, not cash, unless cash payout elected. |
| Commodities, precious metals and real assets | Precious-metal market value | `market_value = quantity x price_per_unit x fx_rate` | Quantity, unit, price source, market value, FX, custody/provider. | Troy ounce, gram, kilogram, or contract unit must match price unit before publishing value. |
| Commodities, precious metals and real assets | Futures notional exposure | `notional = futures_price x contract_multiplier x contract_count` | Contract count, multiplier, notional exposure, margin cash, variation margin. | Margin cash is not substituted for notional exposure in risk reporting. |
| Commodities, precious metals and real assets | Pledged metal lending value | `lending_value = pledged_quantity x price_per_unit x fx_rate x (1 - haircut)` | Pledged quantity, market value, haircut, lending value, availability. | Unpledged quantity is excluded from lending value even if held in the account. |
| Insurance and annuities | Net surrender value | `net_surrender_value = account_or_cash_value - surrender_charges - policy_loans - accrued_loan_interest` | Account value, cash value, surrender value, loan balance, quote date. | Death benefit and projected value are never used as accessible surrender cash. |
| Insurance and annuities | Premium sufficiency | `coverage_status = active if paid_to_date >= required_paid_to_date else grace_or_lapse_workflow` | Premium due, paid-to date, grace period, lapse risk, policy status. | Missed premium creates a visible lifecycle state and does not silently keep policy fully active. |
| Insurance and annuities | Annuity annual income | `annual_income = periodic_payment x payments_per_year` | Payment amount, frequency, start date, end/lifetime basis, guarantee period. | Annuitized income is reported as income stream, not liquid market value unless source provides surrender value. |
| Loans, Lombard, margin and collateral | Lombard availability | `availability = min(facility_limit - exposure, collateral_lending_value - exposure)` | Facility limit, exposure, collateral value, lending value, headroom, availability. | Availability fails closed when collateral price, FX, haircut, or pledge state is stale or missing. |
| Loans, Lombard, margin and collateral | Margin shortfall | `shortfall = max(0, exposure - required_collateral_value_after_haircut)` | Exposure, required collateral, actual lending value, shortfall, cure deadline. | Price shock that breaches threshold creates margin-call workflow with cure options. |
| Loans, Lombard, margin and collateral | Loan interest accrual | `accrued_interest = drawn_principal x rate x day_count_fraction` | Drawn principal, rate, accrued interest, next payment date, utilization. | Interest accrual follows loan day-count convention and rate reset schedule. |
| Private markets | Unfunded commitment | `unfunded = commitment - paid_in + recallable_capital - released_commitment` | Commitment, paid-in, unfunded, recallable, remaining obligation. | Negative unfunded requires exception review unless source explicitly supports overcall/adjustment. |
| Private markets | TVPI, DPI, RVPI | `TVPI = (distributions + residual_value) / paid_in`; `DPI = distributions / paid_in`; `RVPI = residual_value / paid_in` | Paid-in, distributions, NAV, TVPI, DPI, RVPI, valuation date. | Multiples are marked partial when historical cashflows or terminal NAV are incomplete. |
| Private markets | IRR | `IRR = discount_rate where NPV(cashflows + terminal_value) = 0` | IRR, cashflow history, terminal NAV, valuation date, partial-history flag. | IRR is unavailable when dated cashflow history is incomplete. |
| Real estate, REITs and infrastructure | REIT distribution yield | `distribution_yield = annualized_distribution_per_unit / market_price` | Distribution yield, distribution history, market price, income caveat. | Distribution yield is labelled historical/indicated and not guaranteed. |
| Real estate, REITs and infrastructure | Real-asset look-through allocation | `sector_exposure = market_value x source_weight` | Sector/geography exposure, source date, coverage percentage. | Look-through exposure is partial when source coverage is below policy threshold. |
| Real estate, REITs and infrastructure | Appraisal-based value | `reported_value = appraised_value x ownership_percentage x fx_rate` | Appraised value, ownership percentage, valuation date, received date, stale state. | Appraisal value is not reported as current market-traded value. |
| Derivatives | Option premium cashflow | `premium_cash = option_price x contract_multiplier x contract_count` | Premium paid/received, contract count, multiplier, fees, cash movement. | Premium cashflow is separate from later MTM changes. |
| Derivatives | Delta-adjusted exposure | `delta_exposure = underlying_price x multiplier x contracts x delta` | Notional exposure, delta, delta-adjusted exposure, underlying, valuation date. | Exposure is unavailable when delta or multiplier is missing. |
| Derivatives | Futures daily variation margin | `variation_margin = (settlement_price_today - settlement_price_prior) x multiplier x contracts` | Daily cash movement, realized daily P&L, notional exposure, margin balance. | Variation margin changes cash but does not close the futures contract. |
| Structured products | Conditional coupon | `coupon = nominal x coupon_rate x accrual_fraction if observation_condition_met else 0` | Coupon status, observation date, barrier/coupon condition, payable amount. | Coupon remains pending until observation result and issuer notice are confirmed. |
| Structured products | Barrier distance | `barrier_distance = (underlying_level - barrier_level) / underlying_level` | Barrier level, underlying level, distance, observation date, source. | Barrier distance is blocked when underlying mapping or observation level is missing. |
| Structured products | Physical delivery shares | `delivered_shares = nominal / strike_price` adjusted for contract terms | Delivered asset, quantity, residual cash, cost basis policy, note closure. | Direct asset position is created only after legal delivery, not before final fixing. |
| Structured notes | Coupon schedule projection | `projected_coupon = nominal x coupon_rate x accrual_fraction` when unconditional; conditional coupons require observation rule. | Coupon schedule, projected/confirmed state, payment date, source terms. | Conditional coupons are not reported as confirmed income before condition is met. |
| Structured notes | Issuer concentration | `issuer_exposure = sum(note_market_values_by_issuer)` | Issuer exposure, product family exposure, maturity/call ladder. | Issuer exposure includes notes even when underlying exposure belongs to another asset class. |
| Structured notes | Credit event write-down | `post_event_value = nominal x recovery_or_write_down_factor` | Credit event status, write-down, recovery assumption, source notice. | Credit event processing requires issuer/source notice and should preserve prior valuation state. |
| Cash, deposits, money market and FX | Treasury bill discount yield | `discount_yield = (face_value - price) / face_value x 360 / days_to_maturity` | Face value, discount price, maturity date, yield convention, maturity proceeds. | Yield label must state discount-yield convention and should not be confused with bond-equivalent yield. |
| Cash, deposits, money market and FX | Repo collateral cash | `repo_cash = collateral_market_value x (1 - haircut)` before fees and margin calls | Collateral value, pledged amount, haircut, repo cash, maturity, counterparty. | Pledged securities are not treated as fully available liquidity while repo is open. |
| Bonds | Floating-rate note coupon reset | `coupon_rate = reference_fixing + spread`, adjusted for cap/floor; `coupon = nominal x coupon_rate x day_count_fraction` | Reference rate, spread, cap/floor, reset date, current coupon, next pay date. | Coupon accrual changes only after the source-backed reset fixing and terms are applied. |
| Funds | Swing-priced redemption | `swung_nav = published_nav x (1 - swing_factor)` for a redemption swing | Published NAV, swing factor, swung NAV, redeemable units, settlement cash. | Redemption proceeds use the confirmed swung NAV where the fund applies swing pricing. |
| Funds | Side-pocket split | `side_pocket_units = original_units x side_pocket_percentage`; `redeemable_units = original_units - side_pocket_units` | Redeemable units, restricted units, side-pocket source, liquidity state. | Side-pocket units remain restricted and cannot be used as immediate rebalance funding. |
| Equities | Tender offer mixed consideration | `accepted_shares = elected_shares x proration`; `cash = accepted_shares x cash_per_share`; `new_shares = accepted_shares x exchange_ratio` | Election status, accepted shares, cash consideration, share consideration, residual position. | Position changes only for accepted shares after proration and source confirmation. |
| Insurance and annuities | Participating policy declared bonus | `updated_cash_value = prior_cash_value + declared_bonus`; projected bonus remains separate | Cash value, declared bonus, projected bonus, quote date, guarantee basis. | Declared and projected bonuses are labelled separately and projected value is not current market value. |
| Insurance and annuities | Assigned policy accessible value | `accessible_value = cash_value - policy_loan - assignment_restriction_amount - charges` | Cash value, loan balance, assignment state, release status, accessible value. | Assigned policy value is not treated as freely available collateral until release evidence is sourced. |
| Structured products | Structured deposit conditional coupon | `coupon = principal x annual_coupon_rate x tenor_fraction` if condition met, otherwise zero unless terms state otherwise | Principal, payoff condition, observation result, confirmed coupon, maturity cash. | Principal protection is labelled as maturity protection and not current break value. |

## Reporting Label Rules

| Output Type | Required Label |
|---|---|
| Calculated value | Formula basis, source date, currency, rounding, and supportability state. |
| Projected value | Projection assumptions, source, date, and non-guaranteed status. |
| Guaranteed value | Guarantee provider, conditions, date, and excluded assumptions. |
| Estimated value | Estimate basis, stale/missing source explanation, and review status. |
| Income | Gross/net basis, tax/fee treatment, confirmation state, and payment date. |
| Exposure | Legal holding versus analytical exposure versus look-through exposure. |
| Liquidity | Legal liquidity, operational liquidity, settlement timing, restrictions, and pledge state. |

## QA Scenario Template

Use this table shape when turning any calculation into a test case:

| Field | Example Content |
|---|---|
| Product subtype | Callable corporate bond, ILP, REIT, FX forward, private equity fund. |
| Source inputs | Exact terms, quantity, price/NAV, cashflows, dates, FX, tax/fees, source date. |
| Expected calculation | Formula and expected numeric result. |
| Expected labels | Confirmed, projected, guaranteed, estimated, stale, partial, restricted, unsupported. |
| Expected reporting | Which report fields show the result and which fields should not. |
| Negative case | Missing schedule, stale NAV, wrong unit, unresolved observation, partial history. |
| Reconciliation source | Custodian, broker, fund admin, insurer, issuer, lender, manager, appraiser, internal ledger. |

## Disclaimer

This catalog is for education, product analysis, documentation, calculation design, reporting design, QA, and implementation planning. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.

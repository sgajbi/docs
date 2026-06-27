# Source Ownership, Calculation And Reporting Matrix

This matrix helps convert product knowledge into implementation-ready requirements.

Use it when designing APIs, data models, QA scenarios, reporting views, advisory workflows, DPM controls, operations dashboards, migration checks, or product-support runbooks. The purpose is to make source ownership, calculation inputs, degraded states, and reporting obligations explicit before implementation begins.

## Core Principle

Every product feature should answer four questions:

1. **Who owns the source truth?**
2. **What calculation depends on that truth?**
3. **What should the user see when the truth is stale, missing, estimated, disputed, or unsupported?**
4. **Which report or workflow consumes the result?**

If any answer is unclear, the platform should expose a supportability or data-quality state instead of silently inferring a business conclusion.

## Cross-Product Source Ownership

| Product Family | Critical Source Truth | Typical Source Owner Candidates | Data-Quality States To Preserve |
|---|---|---|---|
| Cash, deposits, money market and FX | Ledger cash, settled cash, available cash, projected cash, deposit terms, interest rate, maturity, FX rate, value date, MMF NAV, money market price/yield. | Custodian, core banking ledger, treasury system, deposit system, fund administrator, market-data vendor, FX dealing system, settlement system. | Missing balance, stale FX, stale NAV, unsettled cash, restricted cash, wrong value date, pending settlement, duplicate cashflow, unsupported buying-power basis. |
| Bonds | Instrument terms, coupon schedule, day count, call/put schedule, accrued interest, clean/dirty price, rating, issuer, maturity, default/recovery status. | Security master, custodian, market-data vendor, issuer data, ratings provider, pricing vendor, operations override. | Stale price, missing rating, missing call schedule, incorrect day count, unresolved default event, accrued-interest mismatch, unsupported yield basis. |
| Commodities, precious metals and real assets | Wrapper type, commodity family, grade, unit, price source, physical quantity, metal account balance, ETP NAV, futures contract terms, storage fee, margin, collateral haircut. | Commodity master, vault/custodian, broker/FCM, exchange, clearing house, issuer, fund administrator, market-data vendor, collateral system, operations workflow. | Missing unit conversion, stale commodity price, missing bar list, stale NAV, expired futures contract, disputed margin, unsupported delivery state, pledged quantity mismatch. |
| Equities | Instrument identity, listing, price, trade, settlement, corporate action, dividend, withholding tax, cost basis, restriction/pledge status. | Exchange/market data, custodian, broker, corporate-action vendor, tax lot ledger, transfer agent, operations workflow. | Halted security, stale price, missing corporate-action election, pending settlement, missing tax lot, restricted quantity, fractional entitlement, duplicate dividend. |
| Funds | Fund/share-class identity, NAV, dealing calendar, cut-off, fees, distribution, order status, look-through exposure, liquidity state. | Fund platform, transfer agent, fund administrator, custodian, fund data vendor, operations workflow, manager file. | Stale NAV, estimated NAV, suspended fund, gated redemption, missing cut-off, unknown share class, unavailable look-through, partial order fill. |
| Insurance and annuities | Policy terms, policy roles, coverage/rider terms, premium schedule, account value, cash value, surrender value, death benefit, guarantee basis, claim/maturity/payout state. | Insurer, policy administration system, product master, insurer illustration, custodian statement, cash ledger, lending/collateral system, operations workflow. | Missing value basis, stale insurer value, projected value treated as guaranteed, missing beneficiary data, missed premium/lapse state, unresolved claim, restricted sensitive data, unsupported annuity payout state. |
| Loans, Lombard, margin and collateral | Facility limit, drawdown, accrued interest, collateral holding, pledged quantity, LTV/haircut, eligibility, pledge direction, margin-call status. | Credit system, loan system, collateral management system, custodian, market-data vendor, legal pledge records, risk/credit workflow. | Stale collateral price, missing FX, missing pledge, expired facility, suspended line, duplicate reservation, negative availability, unresolved margin call. |
| Private markets | Commitment, paid-in, unfunded, recallable capital, capital call notice, distribution split, NAV, valuation date, fund terms, liquidity terms. | GP/manager, fund administrator, custodian, investor portal, capital-account statement, operations enrichment, valuation committee. | Missing cashflow history, stale NAV, restated NAV, unclassified distribution, missing unfunded, side pocket, transfer restriction, partial historical support. |
| Real estate, REITs and infrastructure | Wrapper type, REIT/security identity, property/infrastructure sector, geography, NAV/appraisal value, valuation date, distribution split, leverage, occupancy, liquidity terms, collateral eligibility. | Security master, exchange, custodian, fund administrator, manager report, appraiser, product master, market-data vendor, credit/collateral system, operations enrichment. | Stale appraisal, stale NAV, missing sector/geography, missing leverage data, unclassified distribution, redemption queue/gate, missing valuation date, unsupported direct-property document state. |
| Derivatives | Contract terms, confirmation, underlying, notional, strike, expiry, fixing, valuation model/source, Greeks, margin, collateral, counterparty. | Trade capture, broker/clearing house, counterparty confirmation, valuation vendor, market-data source, collateral system, operations workflow. | Missing confirmation, stale fixing, expired contract still open, missing Greeks, disputed collateral, missing CSA terms, unsupported model input. |
| Structured products | Wrapper terms, issuer, payoff formula, underlyings, barriers, observation schedule, coupon conditions, issuer quote, lifecycle event, settlement terms. | Term sheet, issuer notice, product master, market-data vendor, custodian, valuation source, operations workflow. | Missing term sheet, stale issuer quote, missing observation, unresolved barrier state, unsupported payoff, missing underlying mapping, uncertain physical delivery. |
| Structured notes | Note terms, issuer, coupon schedule, observation schedule, barrier/autocall terms, valuation, issuer credit state, maturity/redemption, delivered asset. | Term sheet, issuer notice, product master, custodian, valuation vendor, market data, lifecycle processing workflow. | Missing observation result, stale quote, coupon condition unresolved, credit event pending, delivery not booked, missing cost-basis policy, unsupported subtype. |

## Calculation Dependency Matrix

| Calculation Area | Required Inputs | Product Families Most Affected | Failure Behavior |
|---|---|---|---|
| Market value | Position quantity/notional, price/NAV/quote/model value, FX rate, valuation date. | All product families. | Show stale/missing/estimated valuation state; do not present unsupported market value as final. |
| Available cash | Ledger cash, settled cash, pending settlements, reservations, restrictions, fees, tax, credit/collateral policy. | Cash/FX, equities, funds, lending, private markets. | Block or degrade funding workflows when availability basis is incomplete. |
| Accrued income | Coupon/rate/dividend/accrual terms, day count, holding period, entitlement, tax treatment. | Bonds, deposits, loans, funds, equities, structured notes. | Separate estimated accrual from confirmed cash; flag missing terms. |
| Yield | Price, cashflow schedule, maturity/call schedule, coupon/rate, day count, compounding convention. | Bonds, deposits, money market, loans, structured notes. | Label basis clearly; block yield-to-worst if call schedule is missing. |
| FX translation | Local value, reporting currency, FX rate, rate timestamp, translation policy. | All multi-currency products. | Show FX-source and stale-rate state; do not hide currency exposure. |
| TWR | Valuation series, external cashflows, dates, portfolio membership, corrections. | All product families. | Exclude or degrade periods with unreconciled valuations/cashflows. |
| MWR/IRR | Cashflow dates/amounts, terminal value, reporting currency, complete history. | Private markets, loans, structured products, insurance/annuities, portfolios. | Mark partial/unavailable when cashflow history is incomplete. |
| Contribution | Beginning value, returns, weights, cashflows, income, FX treatment. | All product families. | Show coverage gap by position/product; do not force residuals into unsupported buckets. |
| Attribution | Return series, benchmark/exposure classification, factor/sector/currency mappings. | Equities, bonds, funds, derivatives, portfolios. | Mark unsupported attribution dimensions when source classifications are missing. |
| Risk concentration | Issuer, counterparty, underlying, currency, sector, region, collateral, manager, product family. | All product families. | Report uncovered positions and denominator basis. |
| Liquidity | Settlement dates, dealing terms, maturity, lock-up, gate, side pocket, pledge, collateral restriction, surrender term, delivery state. | Cash/FX, funds, private markets, real estate/infrastructure, lending, structured products, commodities, insurance/annuities. | Show legal/operational liquidity constraints separately from market value. |
| Real-asset exposure | Wrapper, property/infrastructure sector, geography, NAV/appraisal, listed price, leverage, income split, valuation date, look-through source. | Real estate, REITs, infrastructure, funds, private markets, equities. | Mark sector/geography/income analytics partial when look-through or valuation source is missing. |
| Buying power | Available cash, facility limit, exposure, collateral value, haircuts, reservations, eligibility, mandate blocks. | Cash/FX, lending, equities, bonds, funds. | Fail closed when collateral, price, FX, or pledge truth is stale/missing. |
| Scenario payoff | Product terms, underlyings, barriers, rates, volatility, observations, settlement terms. | Structured products, structured notes, derivatives. | Mark scenario as illustrative/support-limited if terms or market inputs are incomplete. |
| Benefit and surrender value | Policy terms, value basis, surrender charges, loans, guarantees, account value, claim status, insurer quote date. | Insurance and annuities. | Label basis and source date; do not treat death benefit, projected value, and surrender value as interchangeable. |
| Commodity exposure | Wrapper type, commodity reference, quantity, contract size, unit, price source, delta, notional, issuer/counterparty, FX. | Commodities, derivatives, structured products, funds, equities. | Block or degrade exposure when unit conversion, wrapper classification, or derivative terms are missing. |

## Reporting Requirements By Product Family

| Product Family | Minimum Reporting Output | Professional Reporting Enhancements |
|---|---|---|
| Cash, deposits, money market and FX | Cash by currency, available vs restricted cash, projected cash, deposits by maturity, FX exposure, money market holdings. | Cash drag, liquidity ladder, yield on liquidity, stale FX/NAV flags, funding gap, pending settlement calendar, hedge ratio. |
| Bonds | Position, market value, accrued interest, coupon, maturity, yield, issuer, rating, duration, currency. | Yield-to-worst, call schedule, maturity ladder, credit bucket, spread/duration contribution, default/recovery notes. |
| Commodities, precious metals and real assets | Wrapper, commodity, quantity/unit, price source, market value, notional exposure, custody/provider/issuer, collateral state. | Commodity-family exposure, spot-vs-wrapper return, roll yield, storage/fee impact, unit-source lineage, expiry/delivery alerts, issuer/provider risk, pledged-metal lending value. |
| Equities | Position, price, market value, cost, unrealized P&L, dividend income, sector/country/currency. | Corporate-action status, tax-lot view, restricted/pledged quantity, factor exposure, dividend yield, realized P&L. |
| Funds | Fund units, NAV, NAV date, market value, share class, fees, distribution, liquidity terms. | Look-through exposure, stale NAV state, dealing calendar, gates/suspensions, manager concentration, fund-of-funds look-through quality. |
| Insurance and annuities | Policy type, insurer, status, premium schedule, account/cash/surrender value, death benefit, beneficiary summary, annuity payout. | Value-basis labels, guaranteed vs non-guaranteed values, premium sufficiency, lapse warning, policy loan impact, claim/maturity state, restricted-data flag, retirement-income projection. |
| Loans, Lombard, margin and collateral | Facility limit, drawn amount, utilization, collateral value, lending value, availability, margin call status. | Pledge graph, haircut/LTV explainability, stress shortfall, in-flight reservations, liquidation risk, collateral concentration. |
| Private markets | Commitment, paid-in, unfunded, NAV, NAV date, distributions, IRR, TVPI, DPI, RVPI. | Vintage analysis, J-curve, cash-call projection, stale/restated NAV flag, liquidity terms, manager/strategy concentration. |
| Real estate, REITs and infrastructure | Wrapper, holding, market value/NAV/appraisal value, valuation date, distribution income, sector, geography, leverage, liquidity state. | Real-asset allocation, income quality, occupancy/cap-rate/leverage notes, stale appraisal flag, redemption queue/gate, collateral eligibility, look-through coverage. |
| Derivatives | Contract type, notional, market value, underlying, expiry, counterparty, margin/collateral, P&L. | Greeks, delta-equivalent exposure, DV01/CS01, lifecycle event calendar, hedge purpose, stress loss, collateral disputes. |
| Structured products | Wrapper, issuer, underlyings, market value, payoff type, next observation, barrier/coupon/autocall state. | Scenario payoff table, barrier distance, issuer concentration, worst-case outcome, liquidity limitation, term-sheet lineage. |
| Structured notes | Note position, issuer, coupon, maturity, observations, valuation, next event, underlyings. | Autocall/coupon status, delivered-asset risk, credit-event state, note maturity/call ladder, physical-delivery implications. |

## QA And Reconciliation Scenarios

| Scenario | Products | Expected Control |
|---|---|---|
| Stale price or NAV | Bonds, funds, private markets, structured products, equities. | Report stale state and block calculations that require current valuation. |
| Unit-of-measure mismatch | Commodities, precious metals, derivatives. | Block market value and exposure until unit, contract size, and price unit are reconciled. |
| Missing FX rate | All multi-currency products. | Block reporting-currency value or mark it unsupported until FX is sourced. |
| Transaction exists without settlement | Equities, bonds, funds, FX, cash. | Separate trade-date position from settled cash/holding. |
| Lifecycle event without economic posting | Structured products, notes, derivatives, funds. | Store lifecycle state without creating false cash/position transactions. |
| Cashflow classification missing | Private markets, funds, loans, equities. | Queue exception; do not classify all cash as income. |
| Corporate action affects collateral | Equities, bonds, lending. | Recalculate collateral quantity/value and buying power. |
| Rating downgrade | Bonds, lending/collateral. | Update risk reporting and collateral haircut where rules depend on rating. |
| Physical delivery | Structured notes, structured products, derivatives. | Close source contract and create delivered asset only when legally delivered. |
| Surrender value mismatch | Insurance and annuities. | Preserve value basis, quote date, charges, loans, and net surrender calculation; do not use account value as accessible value. |
| Missed premium or lapse | Insurance and annuities. | Move policy to grace/lapse workflow and disclose effect on coverage, value, and reporting. |
| Gate/suspension | Funds, private markets, MMFs. | Do not assume redemption cash is available; show blocked/pending state. |
| Margin call | Lending/collateral, derivatives. | Calculate shortfall, cure deadline, permitted cures, and escalation state. |
| Restatement | Private markets, NAV products, performance. | Preserve prior value, restatement link, recalculation evidence, and report note. |
| Appraisal or NAV lag | Real estate, infrastructure, private markets, funds. | Preserve valuation date and received date; show stale or estimated state instead of presenting current market certainty. |
| Partial migration history | Private markets, performance, loans. | Mark IRR/MWR/performance metrics partial or unavailable rather than fabricating history. |

## Implementation Boundary Checklist

Use this checklist before marking a feature as supported:

1. Source owner is named for each required input.
2. Calculation formula and rounding basis are documented.
3. Dates are explicit: trade, order, value, settlement, booking, valuation, observation, maturity.
4. Currency basis and FX source are explicit.
5. Legal holding and analytical exposure are separate.
6. Lifecycle events and transactions are separate.
7. Degraded states are user-visible.
8. Unsupported product subtypes are explicit.
9. Reporting output includes source date and supportability status.
10. QA covers normal, stale, missing, corrected, partial, and unsupported states.
11. Reconciliation source and tolerance are defined.
12. Manual override path preserves approval, reason, timestamp, and audit trail.

## Common Anti-Patterns

Avoid:

1. treating ledger cash as available cash,
2. treating NAV as current when valuation date is stale,
3. treating unfunded commitment as zero exposure,
4. treating embedded structured-product components as separately held positions,
5. treating margin/collateral movements as investment P&L without policy support,
6. treating gross dividend as net cash,
7. treating all private-market distributions as income,
8. treating market value as full liquidity,
9. treating cross-pledges as transitive without explicit legal support,
10. treating commodity exposure as identical across physical, account, ETP, derivative, note, and equity wrappers,
11. treating death benefit, projected value, account value, and surrender value as the same policy value,
12. treating missing source data as a valid zero.

## Disclaimer

This matrix is for education, product analysis, documentation, platform design, QA, and implementation planning. It is not investment, legal, tax, accounting, regulatory, credit, trading, or client advice.

# Client Reporting And Portfolio Review Guide

This guide defines what product-aware client statements, portfolio reviews, advisor reviews, and exception reports should show across the covered product families.

Use it when designing client-facing reports, advisor review packs, DPM review decks, management dashboards, reporting APIs, report QA, or migration sign-off evidence. It complements [`advisory-mandate-reporting-decision-guide.md`](advisory-mandate-reporting-decision-guide.md), [`product-performance-attribution-risk-guide.md`](product-performance-attribution-risk-guide.md), and [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md).

## Reporting Principle

A report should never make unsupported certainty look like truth.

Every client-facing or advisor-facing product report should show:

1. product family and subtype,
2. legal holding,
3. market/value basis,
4. valuation date and source,
5. currency and FX basis,
6. income/cashflow status,
7. liquidity or restriction state,
8. risk/exposure summary,
9. supportability state,
10. exception or data-quality notes where relevant.

## Portfolio Review Pack Structure

| Section | Purpose | Product-Aware Requirements |
|---|---|---|
| Executive summary | Explain what changed and why it matters. | Separate market moves, income, FX, cashflows, valuations, lifecycle events, and exceptions. |
| Asset allocation | Show portfolio composition. | Distinguish legal asset class from analytical exposure and look-through coverage. |
| Performance summary | Show return and contribution. | Label stale/partial data, external flows, income, FX, fees, taxes, and unsupported attribution. |
| Income and cashflow | Show cash generation and future obligations. | Include coupons, dividends, distributions, deposits, premiums, annuities, capital calls, loan interest, maturities, and derivative settlements. |
| Risk and concentration | Show what can hurt the portfolio. | Include issuer, counterparty, insurer, manager, sector, country, currency, leverage, liquidity, and underlying exposure. |
| Liquidity and funding | Show what is accessible and what is restricted. | Separate available cash, projected cash, redemptions, surrender value, gates, lock-ups, pledges, collateral, and unfunded commitments. |
| Product lifecycle calendar | Show upcoming events. | Include coupon dates, maturities, calls, observations, expiries, capital calls, premium due dates, annuity payouts, redemption windows, and margin deadlines. |
| Exceptions and data quality | Show what is incomplete or support-limited. | Include stale prices/NAVs, missing FX, unresolved lifecycle events, unsupported subtypes, reconciliation breaks, and restricted data. |

## Product Family Reporting Matrix

| Product Family | Holdings View | Income/Cashflow View | Risk/Liquidity View | Required Labels |
|---|---|---|---|---|
| Bonds | Nominal, clean value, accrued interest, dirty value, issuer, rating, coupon, maturity, callability. | Coupon accrual, coupon paid, redemption, call/put, amortisation, default/recovery cashflows. | Duration, spread, rating, issuer concentration, maturity ladder, callable risk, liquidity. | Clean/dirty basis, accrued-interest basis, yield basis, price date, rating date. |
| Cash, deposits, money market and FX | Cash by account/currency and balance type; deposit terms; MMF units/NAV; open FX contracts. | Interest, maturity proceeds, breakage cost, settlement cash, FX settlement, forward/NDF cash settlement. | Available cash, projected cash, restrictions, value dates, cash drag, currency exposure, MMF gate. | Ledger/settled/projected/available, value date, FX source, NAV date, restriction reason. |
| Commodities, precious metals and real assets | Wrapper, commodity, quantity, unit, price source, custody/provider, ETP/derivative/note relationship. | Storage fees, roll cashflows, variation margin, structured coupon, sale proceeds. | Unit/grade, custody/provider, issuer/counterparty, notional exposure, delivery risk, pledged quantity. | Unit basis, price unit, wrapper type, custody/provider, notional versus market value. |
| Equities | Security, listing, quantity, price, market value, cost, unrealized P&L, restriction/pledge state. | Dividends, withholding tax, realized P&L, corporate-action proceeds, cash-in-lieu. | Single-name, sector, country, currency, factor, liquidity, corporate-action status. | Trade/settlement status, gross/net dividend, tax basis, cost-basis method, corporate-action state. |
| Funds | Fund/share class, units, NAV, NAV date, market value, order status, fees, look-through coverage. | Distributions, reinvestment, redemption settlement, fees, tax, pending orders. | Manager, strategy, liquidity terms, gates, suspensions, side pockets, look-through quality. | NAV date, estimated/stale NAV, dealing date, order status, gate/suspension state. |
| Insurance and annuities | Policy type, insurer, policy status, account value, cash value, surrender value, death benefit. | Premiums, charges, withdrawals, policy loans, claims, maturity proceeds, annuity payouts. | Insurer, premium sufficiency, surrender restrictions, policy loan impact, guarantee/projection basis, privacy limits. | Value basis, quote date, guaranteed/non-guaranteed, death benefit not market value, restricted data. |
| Loans, Lombard, margin and collateral | Facility limit, drawn amount, exposure, accrued interest, collateral, pledged assets, utilization. | Drawdown, repayment, interest, fees, margin call cure, liquidation proceeds. | LTV/haircut, lending value, availability, shortfall, collateral concentration, liquidation risk. | Limit versus drawn, market value versus lending value, haircut rule, pledge direction, cure deadline. |
| Private markets | Commitment, paid-in, unfunded, NAV, NAV date, manager, strategy, vintage, liquidity terms. | Capital calls, distributions, return of capital, income, gains, recallable capital. | NAV lag, valuation uncertainty, unfunded obligations, manager/vintage/strategy concentration, gates/side pockets. | NAV date, received date, distribution classification, partial-history flag, restatement state. |
| Real estate, REITs and infrastructure | Listed holding/fund/direct asset, NAV/appraisal/market value, sector, geography, leverage. | REIT distributions, property income, fund distributions, capital calls, redemption proceeds. | Property/infrastructure sector, geography, tenant/offtaker, leverage, refinancing, appraisal lag, liquidity. | Wrapper, valuation basis, appraisal date, look-through coverage, leverage source, liquidity state. |
| Derivatives | Contract type, underlying, notional, market value, expiry, counterparty, margin/collateral. | Premium, variation margin, settlement, exercise, close-out, swap legs, collateral interest. | Greeks, delta/DV01/CS01, notional, leverage, margin, collateral, counterparty, expiry/delivery. | Market value versus notional, sensitivity basis, margin state, hedge link, expiry status. |
| Structured products | Wrapper, issuer, underlyings, valuation, payoff type, next observation, coupon/barrier/autocall state. | Coupons, autocall redemption, maturity redemption, physical delivery, early termination. | Issuer, underlying exposure, barrier distance, worst-case outcome, liquidity, scenario support. | Issuer quote date, observation date, conditional coupon, barrier state, scenario supportability. |
| Structured notes | Note, issuer, coupon schedule, maturity/call date, underlyings, valuation, next event. | Coupon paid/projected, maturity redemption, autocall, credit event, delivered asset. | Issuer concentration, underlying look-through, coupon condition, maturity/call ladder, credit event, liquidity. | Coupon confirmed/projected, note value basis, credit-event state, delivery state, issuer exposure. |

## Standard Report Labels

| Label | Use When |
|---|---|
| Confirmed | Source owner has confirmed the value, event, cashflow, or status. |
| Projected | Future cashflow or value is expected but not confirmed. |
| Estimated | Value or metric is calculated from incomplete or model-based inputs. |
| Stale | Source date breaches reporting policy. |
| Partial | Some positions or inputs are covered, but not all. |
| Restricted | Data exists but is permission-limited. |
| Pending | Event, order, settlement, claim, observation, or lifecycle step is not final. |
| Unsupported | Product subtype or calculation is not implementation-backed. |
| Disputed | Source systems disagree or reconciliation is unresolved. |
| Manual | Value or classification was manually enriched or overridden. |

## Client Explanation Patterns

| Topic | Plain Explanation Pattern |
|---|---|
| Stale NAV | "This value uses the latest available NAV from the fund administrator dated `<date>`. Performance and allocation may change when a newer NAV is received." |
| Structured coupon | "This coupon is conditional. It is shown as payable only after the observation condition is confirmed by the issuer." |
| Surrender value | "Surrender value is the estimated accessible amount if the policy is terminated, after charges and loans. It is different from death benefit or projected value." |
| Available cash | "Available cash excludes unsettled proceeds, restricted cash, reservations, and amounts blocked by funding or collateral rules." |
| Derivative exposure | "Market value is the current mark-to-market. Notional or sensitivity exposure may be much larger and is shown separately for risk review." |
| Private-market return | "IRR and multiples depend on complete cashflow history and latest NAV. Metrics are marked partial if history or valuation data is incomplete." |
| Appraisal value | "This value is based on an appraisal or manager report, not an exchange-traded market price." |
| Pledged collateral | "Pledged assets may still appear in holdings but are not freely available for sale or withdrawal without affecting borrowing capacity." |

## Exception Panel Requirements

Every portfolio review should have an exception panel when any of these are present:

1. stale price, NAV, quote, appraisal, or insurer value,
2. missing FX rate,
3. unresolved corporate action, observation, claim, maturity, expiry, or capital call,
4. pending settlement or failed settlement,
5. missing product master term required for reporting,
6. unsupported product subtype,
7. missing look-through exposure,
8. partial performance history,
9. reconciliation break,
10. restricted data hidden from the current viewer.

## Report QA Checklist

Before releasing a report template or report API:

1. Check every product family has a holdings row with value basis and source date.
2. Check income distinguishes gross, tax, fee, net, projected, and confirmed states.
3. Check performance does not include unsupported or stale values without labels.
4. Check contribution and attribution show coverage gaps.
5. Check risk uses appropriate denominator: market value, notional, delta, commitment, surrender value, lending value, or look-through exposure.
6. Check liquidity is not inferred from market value alone.
7. Check exceptions appear in both client/advisor view and operations view where appropriate.
8. Check restricted data is hidden or redacted.
9. Check report labels match API supportability states.
10. Check exported reports preserve source dates, value basis, and disclaimers.

## Common Reporting Failures

Avoid:

1. showing all values as if they are equally current,
2. hiding valuation dates,
3. mixing market value, notional, death benefit, commitment, and facility limit in one total,
4. showing projected income as confirmed income,
5. omitting cash restrictions and settlement timing,
6. treating all distributions as income,
7. showing look-through exposure without coverage percentage,
8. omitting issuer, counterparty, insurer, or manager concentration,
9. hiding unsupported product subtypes in "other",
10. removing exception labels in PDF/export output.

## Disclaimer

This guide is for education, product analysis, documentation, client-reporting design, portfolio-review design, QA, and implementation planning. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.

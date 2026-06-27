# Advisory, Mandate And Reporting Decision Guide

This guide turns product knowledge into reusable front-office, advisory, DPM, reporting, analytics, operations, and QA decisions.

Use it when designing proposal workflows, mandate checks, portfolio-review views, client statements, risk reports, exception queues, data-quality rules, or regression scenarios. For performance and risk methodology, use [`product-performance-attribution-risk-guide.md`](product-performance-attribution-risk-guide.md). For deeper QA coverage, use [`product-qa-regression-matrix.md`](product-qa-regression-matrix.md). It is intentionally platform-neutral and should be adapted to the target policy, jurisdiction, product catalogue, and source-system contracts.

## Core Decision Questions

Every product workflow should answer these questions before it is considered project-ready:

1. **Client purpose:** What client need does the product serve: liquidity, income, growth, protection, leverage, hedging, diversification, yield enhancement, tax planning, estate planning, or tactical exposure?
2. **Legal holding:** What does the client legally own or owe?
3. **Economic exposure:** What market, credit, rate, FX, liquidity, counterparty, leverage, issuer, insurer, manager, property, commodity, or contingent exposure is created?
4. **Cashflow behavior:** Which cashflows are contractual, discretionary, conditional, projected, operationally pending, or already confirmed?
5. **Valuation basis:** Is value market-traded, NAV-based, model-based, issuer-quoted, insurer-quoted, administrator-reported, appraisal-based, stale, estimated, or unavailable?
6. **Liquidity basis:** Can the client sell, redeem, surrender, mature, transfer, borrow against, or close out the position, and under what restrictions?
7. **Mandate fit:** Which eligibility, concentration, leverage, liquidity, complexity, currency, issuer, sector, geography, or product-subtype limits apply?
8. **Reporting truth:** What should appear in holdings, income, performance, contribution, attribution, risk, liquidity, maturity, and exception reports?
9. **Supportability:** What is fully supported, source-limited, partial, restricted, or unsupported?
10. **QA proof:** Which normal, degraded, stale, missing, corrected, and unsupported scenarios prove the workflow?

## Product Family Decision Matrix

| Product Family | Advisory Explanation | Mandate And DPM Controls | Reporting Must Show | QA And Operations Focus |
|---|---|---|---|---|
| Bonds | Income, issuer credit, rate sensitivity, maturity, call risk, currency risk, liquidity, default/recovery risk. | Duration, issuer, rating, maturity bucket, currency, concentration, callable/high-yield eligibility. | Clean value, accrued interest, dirty value, coupon, maturity, yield basis, duration, rating, issuer, currency. | Accrued interest, day count, coupon schedule, call event, stale price, rating downgrade, default/write-down. |
| Cash, deposits, money market and FX | Liquidity, settlement timing, yield on cash, deposit lock-up, early breakage, FX conversion and hedge purpose. | Minimum cash, maximum cash drag, currency exposure, deposit tenor, MMF liquidity, available cash, buying-power limits. | Ledger/settled/projected/available cash, restrictions, value dates, deposit maturity, FX exposure, MMF NAV/liquidity. | Value-date cash, unsettled trades, stale FX, deposit breakage, MMF gate, failed settlement, NDF fixing. |
| Commodities, precious metals and real assets | Wrapper matters: physical, account, ETP, derivative, note, or proxy equity; explain unit, custody, roll, delivery, collateral, and volatility. | Commodity allocation, leverage, complexity, delivery risk, issuer/provider/counterparty concentration, collateral eligibility. | Quantity, unit, price source, wrapper type, market value, notional exposure, pledged quantity, provider/issuer/custody risk. | Unit conversion, stale price, futures expiry/roll, storage fee, variation margin, pledged metal haircut, ETN issuer downgrade. |
| Equities | Ownership of company exposure, price risk, dividends, corporate actions, voting/rights where applicable, liquidity and concentration. | Single-name, sector, country, currency, restricted list, liquidity, ESG/exclusion, corporate-action election rules. | Position, price, cost, market value, realized/unrealized P&L, dividends, tax, sector, country, corporate-action status. | Trade settlement, dividend entitlement, withholding tax, tax lots, split/spin-off/rights, suspended/delisted security. |
| Funds | Pooled vehicle, strategy, share class, fees, NAV timing, dealing terms, look-through quality, liquidity gates/suspensions. | Fund eligibility, share-class suitability, manager concentration, liquidity terms, look-through allocation, cut-off/dealing controls. | Units, NAV, NAV date, market value, fees, distribution, order status, liquidity terms, look-through coverage. | Stale NAV, cut-off missed, gate/suspension, side pocket, partial fill, distribution reinvestment, share-class mismatch. |
| Insurance and annuities | Purpose first: protection, savings, investment-linked growth, retirement income, estate planning, policy loans, surrender consequences. | Product suitability, premium affordability, liquidity, insurer concentration, ILP fund exposure, leverage/premium financing, privacy restrictions. | Policy status, value basis, surrender value, account/cash value, death benefit, premiums, loans, annuity income, source date. | Value-basis labels, missed premium/lapse, surrender quote, policy loan, claim event, restricted beneficiary/health data. |
| Loans, Lombard, margin and collateral | Borrowing purpose, leverage, interest cost, collateral risk, margin-call risk, forced-sale risk, pledge restrictions. | Facility limit, utilization, LTV/haircuts, collateral eligibility, pledge graph, leverage, buying power, margin-call escalation. | Limit, drawn exposure, accrued interest, collateral value, lending value, availability, pledged assets, shortfall, cure deadline. | Haircut changes, stale collateral price, FX shock, in-flight order reservation, pledge release, margin call, liquidation workflow. |
| Private markets | Illiquidity, commitment, capital calls, distributions, NAV lag, J-curve, manager/vintage strategy, valuation uncertainty. | Eligibility, private-asset allocation, unfunded commitment, liquidity/call capacity, vintage, manager, strategy, concentration. | Commitment, paid-in, unfunded, NAV, NAV date, distributions, IRR, TVPI, DPI, RVPI, liquidity terms. | Capital call, distribution split, stale/restated NAV, missing history, side pocket, transfer restriction, secondary transaction. |
| Real estate, REITs and infrastructure | Wrapper plus economic exposure: listed REIT, fund, private fund, direct property, infrastructure cashflow, leverage, income quality. | Real-asset allocation, property/infrastructure sector, geography, leverage, liquidity, issuer/manager, collateral eligibility. | Listed/NAV/appraisal value, distribution income, valuation date, sector/geography, leverage, liquidity, look-through coverage. | REIT corporate action, distribution classification, stale appraisal, redemption queue, missing look-through, leverage/refinancing data. |
| Derivatives | Purpose, hedge/speculation distinction, notional versus market value, leverage, margin, expiry, counterparty, nonlinear payoff. | Eligibility, hedge link, notional exposure, delta/DV01/CS01, leverage, counterparty, margin, collateral, expiry/delivery rules. | Contract terms, market value, notional, exposure, Greeks, margin/collateral, expiry, underlying, counterparty, P&L. | Fixing, exercise/expiry, variation margin, collateral dispute, missing Greeks, expired contract still open, OTC confirmation. |
| Structured products | Payoff path, conditional income, barriers, autocall, issuer risk, liquidity, worst-case outcome, physical delivery. | Complexity, issuer, underlying, worst-of basket, concentration, liquidity, suitability, observation schedule, payoff subtype. | Issuer, underlyings, valuation, coupon/autocall/barrier state, next observation, scenario payoff, liquidity limitation. | Observation result, coupon condition, barrier event, stale issuer quote, physical delivery, missing term-sheet field. |
| Structured notes | Debt wrapper plus embedded payoff; explain issuer credit, coupon reliability, maturity/call path, underlying exposure, delivery risk. | Issuer concentration, note subtype, maturity ladder, underlying exposure, credit event terms, physical delivery eligibility. | Note value, issuer, coupon schedule, maturity, next event, underlyings, coupon status, issuer exposure. | Coupon schedule, autocall/barrier result, credit event, delivered shares, note closure, cost-basis policy. |

## Proposal And Review Workflow

Use this sequence for advisory proposals, DPM product inclusion, or portfolio review:

1. Identify the client objective and whether the product is necessary for that objective.
2. Classify the legal holding and economic exposure separately.
3. Check product eligibility, client suitability, and mandate permission.
4. Check liquidity, cashflow timing, funding, collateral, and settlement impact.
5. Check concentration, leverage, issuer/counterparty/insurer/manager exposure, and currency exposure.
6. Confirm valuation basis and whether the price/NAV/quote/appraisal is current enough.
7. Confirm whether performance, income, contribution, attribution, and risk outputs are fully supported or partial.
8. Record required disclosures, data-quality limitations, and unsupported states.
9. Define the report fields that will evidence the recommendation or review.
10. Define QA scenarios before implementation or migration sign-off.

## Reporting View Checklist

| Report Type | Minimum Product-Aware Requirements |
|---|---|
| Holdings report | Legal position, market/value basis, currency, valuation date, source, and supportability state. |
| Income report | Gross/net income, tax/fee treatment, confirmed versus projected income, payment date, and source. |
| Performance report | Return basis, cashflow classification, stale/partial data handling, and product-specific exclusions. |
| Contribution report | Product family, legal holding, analytical exposure, FX effect, income effect, and coverage gaps. |
| Risk report | Issuer/counterparty/insurer/manager exposure, underlying/look-through exposure, leverage, liquidity, and concentration. |
| Liquidity report | Settlement timing, redemption/surrender/maturity terms, lock-ups, gates, queues, pledged state, and operational restrictions. |
| Maturity/cashflow report | Coupons, maturities, calls, deposits, premiums, annuity income, capital calls, distributions, loan interest, and derivative settlements. |
| Exception report | Missing source fields, stale values, unsupported subtype, unresolved lifecycle event, reconciliation break, and manual override. |

## QA Evidence Checklist

Every product-family workflow should have evidence for:

1. normal booking or position creation,
2. valuation update,
3. cashflow posting,
4. source-date and stale-state handling,
5. missing mandatory source data,
6. unsupported subtype,
7. lifecycle event without false transaction,
8. transaction without completed settlement,
9. reporting output and label correctness,
10. reconciliation against the source owner,
11. mandate/suitability pass and fail cases,
12. permission or restricted-data behavior where relevant.

## Common Review Failures

Avoid these patterns:

1. treating legal holding and analytical exposure as the same concept,
2. presenting projected or estimated values as confirmed,
3. presenting death benefit, issuer quote, appraisal value, or NAV as fully liquid cash,
4. hiding stale data behind a normal-looking market value,
5. allowing mandate checks to use incomplete exposure,
6. reporting income without gross/net/tax/fee basis,
7. treating missing source data as zero,
8. claiming performance or attribution support when coverage is partial,
9. using market value as a substitute for liquidity or collateral value,
10. omitting QA for degraded and unsupported states.

## Disclaimer

This guide is for education, product analysis, documentation, advisory workflow design, mandate design, reporting design, QA, and implementation planning. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.

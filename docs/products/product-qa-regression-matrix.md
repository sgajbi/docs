# Product QA Regression Matrix

This matrix turns the product knowledge base into reusable QA and regression scenarios.

Use it when creating acceptance criteria, test cases, migration validation packs, support runbooks, data-quality checks, or release gates for product support. It is intentionally platform-neutral and should be adapted to the target product catalogue, data contracts, policy rules, and source systems.

## Regression Principles

Every product-family regression suite should prove:

1. source-owned inputs are present and traceable,
2. the legal position is correct,
3. the analytical exposure is correct,
4. cashflows are classified correctly,
5. valuation basis and source date are visible,
6. degraded states are visible,
7. unsupported subtypes fail safely,
8. reports show correct labels and do not overclaim support,
9. reconciliation evidence exists,
10. downstream advisory, mandate, risk, and reporting workflows receive the correct state.

## Cross-Product Regression Matrix

| Product Family | Normal Scenario | Degraded Scenario | Reporting Assertion | Reconciliation Evidence |
|---|---|---|---|---|
| Bonds | Buy bond, accrue coupon, receive coupon, revalue, mature or call. | Missing call schedule, stale price, missing rating, default event pending. | Shows clean value, accrued interest, dirty value, yield basis, duration, rating, maturity/call status. | Custodian position, security master, pricing source, coupon schedule, cash ledger. |
| Cash, deposits, money market and FX | Book cash, settle trade proceeds, open deposit, accrue interest, settle FX spot/forward. | Stale FX, unsettled cash, restricted cash, failed settlement, MMF gate. | Separates ledger, settled, projected, and available cash; shows value dates and restrictions. | Core ledger, custodian cash, settlement system, FX trade record, deposit system. |
| Commodities, precious metals and real assets | Buy allocated gold, value in reporting currency, pledge part as collateral. | Unit mismatch, stale metal price, missing bar list, futures near expiry, ETN issuer downgrade. | Shows wrapper, unit, quantity, price source, custody/provider, notional exposure, pledge state. | Vault/custodian statement, commodity master, market data, broker/FCM, collateral system. |
| Equities | Buy stock, settle, receive dividend, process split, sell partial lot. | Missing corporate-action election, suspended security, stale price, missing tax lot. | Shows settled/pending position, cost, P&L, dividend gross/net, tax, corporate-action status. | Broker/custodian trade, market data, corporate-action feed, tax-lot ledger, cash ledger. |
| Funds | Subscribe, allocate units at NAV, receive distribution, redeem units. | Stale NAV, missed cut-off, redemption gate, suspension, side pocket, partial fill. | Shows units, NAV date, order status, liquidity state, distribution classification, look-through coverage. | Fund platform, transfer agent, fund admin, custodian, manager look-through file. |
| Insurance and annuities | Create policy, post premium, update account value, report surrender value, process annuity payout. | Missed premium, lapse risk, stale insurer value, missing beneficiary, restricted claim data. | Separates account/cash/surrender value, death benefit, premium status, policy loan, guarantee/projection basis. | Insurer statement, policy admin record, cash ledger, policy document, operations workflow. |
| Loans, Lombard, margin and collateral | Approve facility, draw loan, pledge collateral, calculate availability, accrue interest. | Stale collateral price, missing FX, missing pledge, haircut change, margin call overdue. | Shows limit, exposure, utilization, collateral value, lending value, availability, shortfall, cure deadline. | Loan system, credit system, collateral system, custodian, market data, margin-call workflow. |
| Private markets | Commit, process capital call, update NAV, classify distribution, calculate multiples. | Missing cashflow history, stale/restated NAV, unclassified distribution, side pocket, transfer restriction. | Shows commitment, paid-in, unfunded, NAV date, distribution split, IRR/TVPI/DPI/RVPI supportability. | GP/admin notice, capital-account statement, cash ledger, manager portal, operations enrichment. |
| Real estate, REITs and infrastructure | Buy listed REIT, receive distribution, update look-through sector exposure, report income. | Stale appraisal, missing look-through, redemption queue, missing leverage data, unclassified distribution. | Shows wrapper, market/NAV/appraisal value, valuation date, income, sector/geography, leverage, liquidity state. | Exchange/custodian, fund admin, manager report, appraiser, property documents, cash ledger. |
| Derivatives | Book option/future/forward, revalue, post margin, process expiry or settlement. | Missing fixing, expired contract still open, missing Greeks, collateral dispute, missing confirmation. | Shows contract terms, MTM, notional, exposure, Greeks, margin/collateral, expiry, counterparty. | Trade capture, broker/clearing house, counterparty confirmation, valuation source, collateral system. |
| Structured products | Subscribe, observe coupon/autocall condition, receive coupon, redeem or physically settle. | Missing term sheet, stale issuer quote, unresolved observation, unsupported payoff subtype. | Shows issuer, underlyings, valuation, next observation, coupon/barrier/autocall state, scenario support. | Term sheet, issuer notice, custodian, product master, market data, lifecycle workflow. |
| Structured notes | Book note, project coupon schedule, process observation, mature/call, handle delivered asset. | Coupon condition unresolved, credit event pending, missing delivered cost basis, stale quote. | Shows note value, issuer, coupon status, maturity/call ladder, underlying exposure, delivery state. | Term sheet, issuer notice, custodian, pricing source, lifecycle event store, cash ledger. |

## Minimum Test Case Shape

Every regression case should include:

| Field | Requirement |
|---|---|
| Product subtype | Specific product, not only broad asset class. |
| Business purpose | Why the product exists in the portfolio. |
| Source inputs | Exact source fields, dates, identifiers, units, currencies, prices, and terms. |
| Booking steps | Transactions and lifecycle events in order. |
| Expected positions | Legal holding, status, quantity/nominal/units, pledged or restricted state. |
| Expected exposures | Analytical exposure, look-through, notional, delta-equivalent, issuer, counterparty, manager, sector, region, or currency exposure. |
| Expected cashflows | Gross/net cash, income, fees, taxes, margin, premiums, claims, distributions, repayments, or settlements. |
| Expected valuation | Value basis, source, date, stale/estimated/manual state, reporting-currency translation. |
| Expected reports | Holdings, income, performance, contribution, risk, liquidity, maturity/cashflow, and exception views. |
| Expected failure behavior | Missing, stale, partial, restricted, disputed, unsupported, or conflicting-source state. |
| Reconciliation evidence | Source record that proves the output. |

## Degraded-State Regression Set

Run these scenarios across all applicable product families:

1. stale valuation source,
2. missing FX rate,
3. missing product master term,
4. unsupported subtype,
5. lifecycle event received before cash posting,
6. cash posting received before lifecycle confirmation,
7. partial migration history,
8. source correction or restatement,
9. manual override with approval and reason,
10. restricted data visible only to permitted roles,
11. reconciliation break,
12. report generation with partial supportability labels.

## Release Gate Checklist

Before calling product support release-ready, verify:

1. at least one happy-path test per supported subtype,
2. at least one stale/missing source test per product family,
3. at least one report-output test per product family,
4. at least one reconciliation test per source owner,
5. support-limited and unsupported states are visible in API and UI,
6. source dates and value bases appear in client-facing and operations-facing outputs,
7. mandate/suitability pass and fail paths are tested where relevant,
8. corrected/restated source data recalculates dependent outputs,
9. migration samples include representative edge cases,
10. test evidence is tied to product terms rather than only generic asset-class behavior.

## Common Defects This Matrix Should Catch

1. Ledger cash used as available cash.
2. Stale NAV reported as current.
3. Death benefit shown as liquid wealth.
4. Margin cash treated as derivative exposure.
5. Commodity unit mismatch creating wrong market value.
6. Callable bond reported only by final maturity.
7. Private-market IRR fabricated from partial history.
8. REIT treated as generic equity with no real-asset exposure.
9. Structured product coupon booked before observation confirmation.
10. Corporate action processed without entitlement or cost-basis impact.

## Disclaimer

This matrix is for education, product analysis, documentation, QA, controls, migration validation, release planning, and implementation support. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.

# Execution, Custody, Corporate Actions And Lending Controls

This standard consolidates the former wiki material on order management, best execution, trade lifecycle, trade errors, custody chain, asset servicing, corporate actions, Lombard lending, collateral monitoring, and margin-call controls.

Use it when designing order workflows, execution evidence, custody and entitlement models, corporate-action processing, lending availability, collateral monitoring, margin calls, operations dashboards, or QA scenarios.

## Order Management Standard

An order should capture:

1. client or authorized party,
2. account and portfolio,
3. decision model: advisory, DPM, execution-only, trustee, family office, or other authorized instruction,
4. side,
5. instrument,
6. quantity or amount,
7. order type,
8. limit or stop conditions,
9. validity window,
10. currency,
11. funding source,
12. suitability or mandate result,
13. restriction checks,
14. timestamp,
15. channel and evidence.

## Order Types

| Order Type | Strength | Risk |
|---|---|---|
| Market | High execution likelihood. | Price slippage and gap risk. |
| Limit | Price control. | No-fill or partial-fill risk. |
| Stop-loss | Triggered downside action. | Converts into market behavior after trigger and may fill far from stop. |
| Stop-limit | Trigger plus minimum/maximum fill control. | May not execute in a gap. |
| Algorithmic order | Slices large order by volume, time, or other logic. | Requires eligibility, monitoring, and execution-quality review. |
| OTC request for quote | Useful for bonds, structured products, and bespoke instruments. | Requires quote evidence and fair-value control. |

Client-facing labels should explain the behavior of the order, not only the order name.

## Best Execution Evidence

Best execution should consider:

1. price,
2. total cost,
3. speed,
4. execution likelihood,
5. settlement likelihood,
6. size,
7. venue,
8. product liquidity,
9. client instruction,
10. market condition.

Evidence differs by product:

| Product Area | Typical Evidence |
|---|---|
| Listed equities and ETFs | Exchange, timestamp, price, bid/ask, market volume, broker, partial fills. |
| Bonds | Quotes, evaluated price, dealer, spread, size, liquidity, settlement date. |
| Funds | Dealing date, NAV date, cut-off, order status, confirmation, fees. |
| FX | Quote, value date, spread, counterparty, settlement currency, NDF fixing where relevant. |
| Structured products | Term sheet, issuer quote, pricing timestamp, secondary-liquidity caveat. |
| Derivatives | Contract terms, premium, valuation inputs, exchange/counterparty, margin terms. |

## Trade Lifecycle And Corrections

| Stage | Control |
|---|---|
| Capture | Instruction complete and authorized. |
| Pre-trade checks | Suitability, mandate, product, restriction, funding, collateral, and tax/reporting checks. |
| Routing | Venue/counterparty/broker selection is recorded. |
| Execution | Fill or quote evidence is captured. |
| Booking | Trade reaches book of record with correct trade date, settlement date, currency, and fees. |
| Settlement | Cash/security/fund/contract settlement is tracked. |
| Reconciliation | Broker, custodian, core ledger, cash, and position records reconcile. |
| Correction | Cancel/rebook, reversal, amendment, compensation, and incident evidence are preserved. |

Trade errors should be neutralized promptly, investigated with evidence, and corrected so the client is not disadvantaged by operational error. Error P&L, compensation, approvals, and audit trail should be explicit.

## Custody Chain

Custody models should distinguish:

| Role | Meaning |
|---|---|
| Beneficial owner | Party entitled to economic benefit. |
| Legal owner or nominee | Registered holder or custodian acting for the beneficial owner. |
| Custodian | Institution maintaining account and safekeeping record. |
| Sub-custodian | Local market safekeeping provider. |
| CSD or depository | Market infrastructure where securities are ultimately recorded. |
| Account operator | Party authorized to instruct or administer activity. |
| Reporting recipient | Party allowed to receive statements or reports. |

This distinction matters for tax, proxy voting, corporate actions, trust structures, family-office access, and regulatory reporting.

## Corporate-Action Lifecycle

Corporate actions should model:

1. announcement date,
2. ex-date,
3. record date,
4. election deadline,
5. market deadline,
6. pay date,
7. eligible position,
8. entitlement,
9. election options,
10. default option,
11. client instruction,
12. cash/security result,
13. tax and withholding,
14. cost-basis impact,
15. reconciliation state.

| Event Type | Default Behavior |
|---|---|
| Mandatory | Process automatically after source confirmation. |
| Mandatory with options | Apply default if no valid election is received by deadline. |
| Voluntary | Do nothing or lapse unless valid instruction is received. |
| Tax or documentation event | Apply documented rate or blocked/default treatment when documentation is missing. |
| Proxy or vote | Capture eligibility, instruction, deadline, and transmission evidence. |

## Lending And Collateral Controls

Lombard and margin lending should preserve:

1. borrower,
2. facility,
3. drawdown,
4. exposure,
5. collateral owner,
6. pledge relationship,
7. market value,
8. eligibility,
9. haircut or LTV,
10. lending value,
11. currency mismatch,
12. concentration adjustment,
13. buying power,
14. margin-call status,
15. close-out authority.

## Lending Calculations

| Metric | Meaning |
|---|---|
| Market value | Current value of collateral before haircut. |
| Lending value | Eligible collateral value after haircut and concentration rules. |
| Total exposure | Drawn loans, overdrafts, accrued interest, and supported potential future exposure. |
| Availability | Facility headroom constrained by limit, exposure, lending value, reservations, and restrictions. |
| Coverage ratio | Lending value divided by exposure, where policy supports that view. |
| Shortfall | Amount required to restore coverage or meet margin requirement. |

Availability should fail closed when collateral price, FX, haircut, pledge, eligibility, or facility truth is stale or missing.

## Margin-Call Lifecycle

| Zone | Trigger | Action |
|---|---|---|
| Warning | Buffer breach or projected shortfall. | Notify, restrict withdrawals where policy requires, monitor cure plan. |
| Margin call | Coverage below requirement. | Formal demand, reduce-only state, cure deadline, escalation. |
| Close-out | Cure failure or severe breach. | Forced sale or liquidation according to facility terms and authority. |

Cure methods may include cash deposit, eligible security pledge, asset sale, loan repayment, or other approved remediation.

## QA Checklist

1. Advisory order cannot execute without valid consent where required.
2. DPM order cannot violate hard mandate constraints.
3. Market, limit, stop, stop-limit, and algorithmic orders have distinct behavior.
4. OTC quote evidence is stored for products that require it.
5. Trade date, booking date, settlement date, and value date are separate.
6. Partial fills and failed settlements do not corrupt position or cash.
7. Correction links to original trade and preserves audit trail.
8. Dividend entitlement uses ex-date and eligible settled/contractual position correctly.
9. Rights issue default/lapse behavior is explicit.
10. Corporate-action cost basis updates do not create false P&L.
11. Beneficial owner and nominee/legal owner are separate.
12. Collateral value, lending value, exposure, and availability are separate.
13. Cross-pledge checks pledgor authority and notification scope.
14. Margin-call status blocks withdrawals or new risk where policy requires.
15. Forced liquidation evidence includes trigger, authority, assets sold, and client impact.

## Related Guides

Use this standard together with:

1. [`../../products/direct-equities-and-market-operations.md`](../../products/direct-equities-and-market-operations.md)
2. [`../../products/lending-collateral-and-buying-power.md`](../../products/lending-collateral-and-buying-power.md)
3. [`../../products/product-lifecycle-cashflow-and-event-guide.md`](../../products/product-lifecycle-cashflow-and-event-guide.md)
4. [`../../products/portfolio-exposure-modelling-guide.md`](../../products/portfolio-exposure-modelling-guide.md)
5. [`../../products/tax-regulatory-and-reporting.md`](../../products/tax-regulatory-and-reporting.md)
6. [`../../products/wealth-structuring-trusts-and-family-office.md`](../../products/wealth-structuring-trusts-and-family-office.md)

## Disclaimer

This document is for operating-model, platform-design, documentation, reporting, QA, and governance work. It is not investment, legal, tax, regulatory, fiduciary, credit, trading, or client advice.

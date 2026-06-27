# Advisory And DPM Operating Model

This standard consolidates the former wiki material on advisory, discretionary portfolio management, hybrid structures, model portfolios, rebalancing, mandate controls, client reviews, consent, decision logs, exceptions, and operating governance.

Use it when designing advisory workflows, DPM mandates, portfolio review packs, proposal approvals, model portfolio governance, rebalancing tools, breach monitoring, consent evidence, or audit trails.

## Core Distinction

The difference between advisory and discretionary portfolio management is decision authority.

| Dimension | Advisory | DPM |
|---|---|---|
| Decision maker | Client decides whether to accept the recommendation. | Portfolio manager acts within the signed mandate. |
| Trade approval | Client consent is required before execution. | Client consent is not required for each trade if the mandate permits it. |
| Suitability lens | Trade-level recommendation and client context at time of advice. | Mandate-level suitability and continuous mandate compliance. |
| Monitoring | Point-in-time unless ongoing advisory is contracted. | Continuous monitoring against mandate rules. |
| Reporting | Explain recommendations, consent, execution, and follow-up. | Explain mandate performance, decisions, drift, breaches, and model changes. |
| Main conduct risk | Acting without valid consent or creating shadow discretion. | Trading outside mandate, unfair allocation, unmanaged breach, or weak governance. |

## Advisory Lifecycle

| Stage | Required Evidence |
|---|---|
| Idea source | House view, research note, client request, portfolio review, risk trigger, maturity event, concentration alert. |
| Client fit | Objective, risk profile, capacity for loss, knowledge and experience, liquidity need, restrictions, existing holdings. |
| Proposal | Rationale, product description, risks, costs, alternatives, downside scenario, conflicts, time validity, required action. |
| Suitability check | Product risk, complexity, concentration, liquidity, currency, horizon, tax/reporting considerations, client restrictions. |
| Consent | Client approval tied to exact proposal version, timestamp, channel, approver, and expiry. |
| Execution | Order details, suitability still valid, market conditions, price/quote, best execution evidence. |
| Post-trade | Confirmation, monitoring trigger, follow-up task, review date, exception status. |

Advisory systems should make sell, hold, reduce, hedge, and switch recommendations as first-class outcomes. Good advice is not only new product buying.

## DPM Lifecycle

| Stage | Required Evidence |
|---|---|
| Mandate onboarding | Mandate type, objectives, risk profile, base currency, benchmark, restrictions, fees, eligible products, accounts in scope. |
| Model mapping | Model portfolio, strategy version, effective date, client-level constraints, exclusions. |
| Initial deployment | Transition plan for legacy assets, target allocation, liquidity plan, staged execution, exception treatment. |
| Ongoing management | Drift checks, model updates, rebalancing, cash handling, risk budget, breach monitoring, performance review. |
| Trade waves | Eligible accounts, allocation method, exclusions, fair-allocation evidence, execution batch, post-trade reconciliation. |
| Review cadence | Monthly or quarterly reporting, mandate review, benchmark comparison, risk review, decision logs. |
| Change or termination | Client approval where needed, effective date, fee cut-off, unwind or in-specie transfer, handover responsibilities. |

## Constraint Model

Mandate controls should distinguish:

| Constraint Type | Behavior |
|---|---|
| Hard block | Trade cannot proceed. Example: prohibited issuer, product not permitted, breached client restriction. |
| Soft limit | Trade or position may exist temporarily but requires monitoring and remediation. Example: cash target drift. |
| Passive breach | Market movement or external event causes breach. Requires logging and remediation plan. |
| Active breach | Decision or execution creates breach. Requires escalation, incident review, and client-impact assessment. |
| Client-specific override | Allowed only where mandate and policy permit; must carry approval, expiry, and scope. |
| Model-level exception | Applies to strategy or model version; must be governed centrally. |

Every breach should have status, owner, cause, affected accounts, client impact, remediation action, due date, and closure evidence.

## Rebalancing Standard

DPM rebalancing should be disciplined, not reactive noise.

| Trigger | Examples | Required Control |
|---|---|---|
| Drift | Asset class or security moves outside band. | Drift threshold, band type, materiality, reason code. |
| Model update | CIO or investment committee changes target weights. | Model version, approval, effective date, rollout log. |
| Cashflow | Deposit, withdrawal, fee, income, capital call, maturity. | Cash handling rule and minimum trade size. |
| Risk change | Volatility, drawdown, rating downgrade, liquidity change. | Risk review and mandate impact. |
| Client change | Objective, restriction, risk profile, tax residence, account scope. | Mandate amendment and transition plan. |

Rebalancing should consider taxes, transaction costs, liquidity, minimum lot sizes, client restrictions, concentration, execution windows, and fairness across accounts.

## Model Portfolio Governance

Model portfolios require:

1. owner,
2. benchmark,
3. target weights,
4. permitted ranges,
5. instrument universe,
6. risk budget,
7. investment rationale,
8. version history,
9. approval record,
10. rollout state,
11. affected mandates,
12. transition guidance,
13. retirement policy for old versions.

Client portfolios mapped to the same model may still differ because of tax, currency, cashflow, restrictions, legacy holdings, minimum trade size, or account availability. Reporting should distinguish model drift from client-specific approved divergence.

## Advisory/DPM Hybrid Structures

Common hybrid patterns:

| Pattern | Control Point |
|---|---|
| Core DPM plus advisory satellite | DPM and advisory perimeters must be separate for fees, performance, mandate limits, and consent. |
| Multi-account household mandate | Mandate must list included accounts and allocation rules. |
| Family office oversight | Authority and reporting scope must be explicit. |
| Advisory to DPM transition | Decision rights flip on an effective date; legacy assets need transition plan. |
| DPM to advisory termination | PM monitoring stops; advisory suitability and client communication responsibilities resume elsewhere. |

## Client Review Standard

Client review packs should connect:

1. objectives and mandate,
2. performance versus benchmark,
3. contribution and attribution,
4. risk and drawdown,
5. changes made and why,
6. current allocation and drift,
7. cash and liquidity,
8. income and fees,
9. breaches and exceptions,
10. recommendations or next actions,
11. unsupported or stale data states,
12. open decisions requiring client action.

## Operating Control Checklist

1. Advisory trades have proposal, suitability, consent, order, execution, and post-trade evidence.
2. DPM trades are permitted by mandate and model version.
3. Advisory accounts do not receive shadow-discretion trades.
4. DPM allocations are fair and reproducible.
5. Rebalancing has a trigger, threshold, approval path, and execution evidence.
6. Breaches are classified as active/passive and hard/soft.
7. Model changes are versioned and rolled out with client-level exceptions.
8. Client communications are archived and tied to the relevant proposal, review, or decision.
9. Transitions between advisory and DPM have clear effective dates and responsibility handoff.
10. Reporting does not mix DPM and advisory performance scopes without labels.

## Related Guides

Use this standard together with:

1. [`../../products/advisory-mandate-reporting-decision-guide.md`](../../products/advisory-mandate-reporting-decision-guide.md)
2. [`../../products/portfolio-construction-and-rebalancing-guide.md`](../../products/portfolio-construction-and-rebalancing-guide.md)
3. [`../../products/portfolio-exposure-modelling-guide.md`](../../products/portfolio-exposure-modelling-guide.md)
4. [`../../products/client-reporting-and-portfolio-review-guide.md`](../../products/client-reporting-and-portfolio-review-guide.md)
5. [`../../products/product-payoff-scenario-and-stress-guide.md`](../../products/product-payoff-scenario-and-stress-guide.md)
6. [`../../products/product-qa-regression-matrix.md`](../../products/product-qa-regression-matrix.md)

## Disclaimer

This document is for operating-model, platform-design, documentation, reporting, QA, and governance work. It is not investment, legal, tax, regulatory, fiduciary, credit, trading, or client advice.

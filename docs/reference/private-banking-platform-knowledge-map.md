# Private Banking Platform Knowledge Map

This map turns product reference material into reusable project knowledge for wealth-management and private-banking platform work.

## Core Domains

| Domain | What To Know | Why It Matters In Projects |
|---|---|---|
| Instrument master | Product type, issuer, identifiers, currency, terms, schedules, classification. | Drives booking, valuation, suitability, reporting, search, and downstream analytics. |
| Transactions | Buy, sell, coupon, fee, tax, redemption, conversion, write-down, recovery, correction. | Accounting truth and performance measurement depend on correct transaction semantics. |
| Positions | Current legal holdings, nominal, units, cost, market value, status. | Front-office, reporting, risk, and reconciliation views depend on accurate positions. |
| Lifecycle events | Observation, coupon determination, call, put, maturity, default, restructuring, corporate action. | Many events affect status or payoff before they become transactions. |
| Valuation | Price source, clean/dirty value, accrued interest, model/quote hierarchy, stale flags. | Client reporting and advisory decisions require explainable valuation posture. |
| Risk | Market risk, credit risk, rate risk, spread risk, FX risk, liquidity, concentration, look-through. | Product risk must be visible without confusing legal positions and analytical exposure. |
| Suitability | Risk profile, product knowledge, horizon, liquidity need, concentration, currency, downside scenario. | Advisory workflows need auditable rationale, not just product availability. |
| Reporting | Income, P&L, performance, exposure, cashflow schedule, maturity ladder, risk concentration. | Client and management reporting must reconcile to source data and business meaning. |
| Operations | Reconciliation, stale prices, failed lifecycle events, corrections, manual overrides, audit trail. | Production trust depends on supportability and exception handling. |

## Product-Specific Modelling Reminders

| Product Area | Modelling Reminder |
|---|---|
| Bonds | Separate clean price, dirty value, accrued interest, yield, duration, spread, call/put schedules, and credit events. |
| Cash, deposits, money market and FX | Separate ledger, settled, projected, and available cash; model value dates, restrictions, reservations, deposit terms, MMF liquidity, and FX lifecycle explicitly. |
| Commodities, precious metals and real assets | Separate wrapper, unit, custody/provider, commodity exposure, notional exposure, collateral value, roll/delivery risk, and issuer/counterparty risk. |
| Derivatives | Separate contract terms, lifecycle events, valuation, notional, exposure, Greeks, margin, collateral, counterparty, and suitability concepts. |
| Equities | Separate issuer, instrument, listing, trade, settlement, lot, corporate-action, valuation, P&L, risk, and entitlement concepts. |
| Funds | Separate legal fund units from look-through exposure; model NAV, share class, dealing terms, distributions, fees, gates, suspensions, and pending orders explicitly. |
| Insurance and annuities | Separate policy contract, roles, coverage, riders, premium obligations, value basis, surrender value, death benefit, guarantees, loans, claims, and privacy restrictions. |
| Loans, Lombard, margin and collateral | Separate facility, drawdown, exposure, collateral, pledge graph, haircut/LTV, reservations, availability, margin call, and liquidation workflows. |
| Private markets | Separate commitment, paid-in capital, unfunded commitment, capital calls, distributions, NAV lag, restatement, liquidity terms, and partial-history analytics. |
| Real estate, REITs and infrastructure | Separate listed/fund/private/direct wrapper behavior from property or infrastructure economic exposure, valuation basis, leverage, income, liquidity, and look-through coverage. |
| Structured notes | Separate legal note position from embedded derivative terms; model barriers, observations, autocalls, payoff terms, physical settlement, and issuer risk explicitly. |
| Structured products | Separate wrapper, issuer, payoff formula, underlyings, barriers, observations, scenario outcomes, liquidity limits, and settlement terms. |

## Source-Ownership Questions

Use these before designing APIs or UI:

1. Who owns the product master?
2. Who owns transaction booking?
3. Who owns portfolio holdings and positions?
4. Who owns equity corporate actions, entitlements, elections, lots, cost basis, and listing status?
5. Who owns derivative contract terms, lifecycle events, fixings, margin, collateral, counterparty, and exposure measures?
6. Who owns valuation and price source selection?
7. Who owns risk methodology?
8. Who owns performance methodology?
9. Who owns advisory suitability and proposal state?
10. Who owns reporting, rendering, archive, and legal evidence?
11. Who owns fund NAV, share-class terms, dealing status, fees, distributions, and look-through files?
12. Which data is source truth, derived view, cached projection, or user-entered override?
13. How does the system fail closed when the source owner cannot provide evidence?

## API Design Checklist

1. Use business nouns in paths and models.
2. Separate command endpoints from read/view endpoints.
3. Include supportability status for source-backed features.
4. Include lineage, source timestamp, source owner, and reason codes where data is composed.
5. Model errors explicitly and safely.
6. Do not expose raw internal implementation names in business-facing contracts.
7. Add examples that reflect real private-banking scenarios.
8. Document when to use each endpoint and what not to infer from it.
9. Validate every important response field in tests.
10. Keep Swagger useful for engineers, QA, product owners, and operations.

## UI Design Checklist

1. Show the decision the user needs to make.
2. Show the evidence supporting the decision.
3. Show what action is allowed.
4. Show what is blocked and why.
5. Keep source and lineage available without overwhelming the primary view.
6. Use domain labels instead of technical labels.
7. Avoid presenting unsupported states as successful product outcomes.
8. Make empty, partial, stale, degraded, blocked, and error states distinct.
9. Keep tables dense but readable.
10. Use consistent status treatment across product areas.

## Documentation Checklist

Good project documentation should include:

1. business purpose,
2. product scope and non-scope,
3. source authority and ownership,
4. lifecycle and transaction model,
5. API contracts and examples,
6. data lineage and supportability,
7. controls and reconciliation,
8. risks and edge cases,
9. QA scenarios,
10. implementation-backed supported features,
11. known unsupported states and future work.

## Cross-Product Operating Guides

| Guide | Use |
|---|---|
| [`../products/advisory-mandate-reporting-decision-guide.md`](../products/advisory-mandate-reporting-decision-guide.md) | Advisory explanation, mandate controls, reporting requirements, QA evidence, and common review failures across product families. |
| [`../products/client-reporting-and-portfolio-review-guide.md`](../products/client-reporting-and-portfolio-review-guide.md) | Client statements, portfolio reviews, report labels, exception panels, client explanations, and report QA. |
| [`../products/product-capability-boundary-matrix.md`](../products/product-capability-boundary-matrix.md) | Generic versus product-specific support boundaries, support-limited states, future candidates, and QA evidence. |
| [`../products/product-calculation-example-catalog.md`](../products/product-calculation-example-catalog.md) | Compact calculation examples, reporting labels, degraded states, and QA assertions. |
| [`../products/product-lifecycle-cashflow-and-event-guide.md`](../products/product-lifecycle-cashflow-and-event-guide.md) | Standard for modelling orders, lifecycle events, transactions, settlement, cashflows, accruals, obligations, reporting treatment, and QA. |
| [`../products/product-payoff-scenario-and-stress-guide.md`](../products/product-payoff-scenario-and-stress-guide.md) | Standard for payoff shapes, scenario outcomes, stress behavior, income reliability, liquidity effects, advisory explanation, reporting, and QA. |
| [`../products/product-performance-attribution-risk-guide.md`](../products/product-performance-attribution-risk-guide.md) | Performance, contribution, attribution, risk denominators, degraded analytics states, and analytics QA. |
| [`../products/product-qa-regression-matrix.md`](../products/product-qa-regression-matrix.md) | QA and regression scenarios for normal, degraded, reporting, reconciliation, migration, and release-gate evidence. |
| [`../products/product-source-intake-and-migration-guide.md`](../products/product-source-intake-and-migration-guide.md) | Source-intake and migration guidance for field profiling, source ownership, reconciliation, degraded states, and sign-off evidence. |
| [`../products/product-taxonomy-and-vocabulary-guide.md`](../products/product-taxonomy-and-vocabulary-guide.md) | Vocabulary standard for separating product family, subtype, wrapper, legal holding, exposure, lifecycle, valuation, liquidity, reporting, and supportability states. |
| [`../products/source-ownership-calculation-reporting-matrix.md`](../products/source-ownership-calculation-reporting-matrix.md) | Source ownership, calculation dependencies, reporting requirements, QA controls, and implementation boundaries. |

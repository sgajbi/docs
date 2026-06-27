# Private Banking Platform Knowledge Map

This map turns product notes into reusable project knowledge for wealth-management and private-banking platform work.

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

## Source-Ownership Questions

Use these before designing APIs or UI:

1. Who owns the product master?
2. Who owns transaction booking?
3. Who owns portfolio holdings and positions?
4. Who owns valuation and price source selection?
5. Who owns risk methodology?
6. Who owns performance methodology?
7. Who owns advisory suitability and proposal state?
8. Who owns reporting, rendering, archive, and legal evidence?
9. Which data is source truth, derived view, cached projection, or user-entered override?
10. How does the system fail closed when the source owner cannot provide evidence?

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

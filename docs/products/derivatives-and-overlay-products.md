# Derivatives And Overlay Products Knowledge Map

This page connects derivatives knowledge to reusable private-banking platform decisions.

## Current Pack

| Product Area | Pack | Main Use |
|---|---|---|
| Derivatives | [`derivatives/`](derivatives/README.md) | Options, futures, forwards, FX derivatives, commodities, swaps, CDS, OTC contracts, valuation, Greeks, margin, collateral, counterparty risk, advisory, mandate controls, reporting, implementation, and QA. |

## Practical Worked Examples

Use [`derivatives/09-worked-examples-and-implementation-patterns.md`](derivatives/09-worked-examples-and-implementation-patterns.md) for concrete examples covering option purchase, covered-call assignment, futures variation margin, FX forward hedge settlement, NDF fixing, interest-rate swap resets, CDS credit events, exposure lenses, support boundaries and regression tests.

## Core Derivatives Platform Distinctions

| Concept | Project Implication |
|---|---|
| Contract position | Derivatives should be modelled as contracts with terms, underlyings, lifecycle events, valuation, exposure, margin, and counterparty relationships. |
| Market value vs exposure | Small or zero market value can still carry large notional, delta, DV01, CS01, or stress exposure. |
| Listed vs OTC | Listed contracts often use exchange, clearing, margin, expiry, exercise, assignment, and standardized contract terms; OTC contracts require counterparty, confirmation, CSA/collateral, fixing, schedule, and valuation controls. |
| Lifecycle events | Exercise, assignment, expiry, fixing, reset, roll, cash settlement, physical delivery, coupon/reset/payment, margin call, collateral movement, and close-out must be explicit. |
| Margin and collateral | Initial margin, variation margin, collateral pledges, collateral returns, and margin interest should not be confused with investment P&L. |
| Risk measures | Notional, delta-equivalent, beta-adjusted exposure, Greeks, DV01, CS01, VaR, stress loss, counterparty exposure, and concentration serve different purposes. |
| Advisory and mandate controls | Derivatives require eligibility, product knowledge, leverage, hedging purpose, exposure limits, liquidity, complexity, and suitability controls. |

## Questions To Ask Before Building A Derivatives Feature

1. Is this an exchange-traded derivative, OTC derivative, embedded derivative, hedge overlay, structured-product component, or reporting-only exposure?
2. Is the feature about trade booking, lifecycle event processing, valuation, exposure, margin/collateral, counterparty risk, advisory control, mandate control, or reporting?
3. Which underlying risk factor drives the contract: equity, index, FX, rate, credit, commodity, inflation, volatility, or basket?
4. Which measure does the user need: market value, notional, delta-equivalent exposure, DV01, CS01, Greeks, margin requirement, collateral balance, or stress exposure?
5. Does the contract settle physically, in cash, by netting, by delivery, by premium payment, by variation margin, or by periodic reset?
6. Who owns confirmation, valuation model, market data, fixing, margin, collateral, and counterparty data?
7. How does the platform distinguish hedging, income generation, leverage, speculative exposure, and efficient portfolio management?
8. What should happen when valuation, fixing, market data, margin, collateral, or counterparty data is stale or unavailable?

## API And UI Implications

Derivative APIs and UI should make these states explicit:

1. contract identity and product family,
2. underlying/reference identity,
3. direction and payoff profile,
4. notional and market value,
5. exposure measure appropriate to product,
6. valuation source and stale status,
7. lifecycle event status,
8. maturity, expiry, fixing, reset, and settlement schedule,
9. margin and collateral state,
10. counterparty and clearing posture,
11. advisory eligibility and mandate limit posture,
12. hedge or overlay purpose where formally recorded,
13. unsupported states and blocked capabilities.

## QA Scenarios To Preserve

1. long call purchase, valuation, expiry, and exercise,
2. short option assignment and premium treatment,
3. protective put and covered call reporting,
4. futures variation margin and daily settlement,
5. FX forward maturity and cash settlement,
6. NDF fixing and settlement currency treatment,
7. interest-rate swap reset and payment,
8. cross-currency swap notional exchange and FX exposure,
9. CDS premium leg and credit event posture,
10. margin call and collateral movement,
11. stale volatility, curve, FX, or fixing input,
12. counterparty exposure limit breach,
13. mandate leverage or derivative eligibility breach,
14. structured-note embedded derivative displayed as terms, not client-held derivative position.

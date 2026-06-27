# Fixed Income And Structured Products Knowledge Map

This page connects product reference packs that are often used together in private-banking platform work.

## Current Packs

| Product Area | Pack | Main Use |
|---|---|---|
| Bonds | [`bonds/`](bonds/README.md) | Fixed-income product modelling, coupon/accrual treatment, yield, duration, credit risk, valuation, lifecycle, and advisory controls. |
| Structured notes | [`structured-notes/`](structured-notes/README.md) | Structured payoff modelling, embedded derivative terms, lifecycle events, barrier/autocall/physical settlement handling, valuation, risk, and suitability controls. |

## Platform Design Distinctions

| Question | Bonds | Structured Notes |
|---|---|---|
| Core instrument nature | Debt security with coupon/principal mechanics. | Issuer debt obligation with embedded payoff formula. |
| Primary position quantity | Usually nominal amount and units/lots. | Usually note nominal and units/lots. |
| Main schedule focus | Coupon, accrual, call/put, amortisation, maturity. | Observation, coupon condition, barrier, autocall, fixing, settlement. |
| Main valuation focus | Clean/dirty price, accrued interest, yield, spread, duration, DV01. | Issuer quote/model value, payoff path, option inputs, barrier/autocall state, look-through exposure. |
| Main risk split | Issuer, rates, spread, liquidity, currency, seniority, default/recovery. | Issuer, underlying/payoff, barrier, path dependency, liquidity, complexity, currency, physical settlement. |
| Accounting warning | Do not confuse coupon, accrued interest, clean price, and dirty value. | Do not create separate client positions for embedded derivatives unless assets are legally delivered. |

## Reusable Modelling Pattern

For both products, keep the model layered:

```text
Instrument master
  Product contract terms
  Schedules and lifecycle events
  Economic transactions
  Positions
  Valuation records
  Risk and look-through exposure
  Advisory/suitability controls
  Reporting views
```

The platform should not create new accounting positions for every analytical or lifecycle concept. Positions represent what the client legally holds. Schedules, events, valuations, and risk views explain what happens to that holding.

## Questions To Ask Before Building A Feature

1. Is this a product term, lifecycle event, economic transaction, position field, valuation input, risk measure, or reporting view?
2. Which system is the source of truth for the instrument, price, rate, FX, rating, issuer, underlying, holding, transaction, and client suitability data?
3. Does the feature require accounting-grade booking, advisory explanation, portfolio analytics, operational control, or all of them?
4. Is the UI showing legal holdings or analytical exposure?
5. Are unsupported states explicit, or is the platform silently inventing conclusions?

## Useful Project Workflows

Use these packs when working on:

1. product-master design,
2. portfolio position modelling,
3. transaction taxonomy,
4. lifecycle event processing,
5. valuation and risk API design,
6. reporting data requirements,
7. advisor suitability and disclosure workflows,
8. QA scenario design,
9. stakeholder, practitioner, and project preparation for wealth-platform work,
10. Lotus domain vocabulary and user-facing language.

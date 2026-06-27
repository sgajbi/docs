# Fixed Income And Structured Products Knowledge Map

This page connects product reference packs that are often used together in private-banking platform work.

## Current Packs

| Product Area | Pack | Main Use |
|---|---|---|
| Bonds | [`bonds/`](bonds/README.md) | Fixed-income product modelling, coupon/accrual treatment, yield, duration, credit risk, valuation, lifecycle, and advisory controls. |
| Structured products | [`structured-products/`](structured-products/README.md) | Broad wrapper-plus-payoff modelling for notes, deposits, certificates, warrants, structured funds, accumulators, decumulators, autocallables, participation products, buffers, and leveraged structures. |
| Structured notes | [`structured-notes/`](structured-notes/README.md) | Structured payoff modelling, embedded derivative terms, lifecycle events, barrier/autocall/physical settlement handling, valuation, risk, and suitability controls. |

## Practical Worked Examples

| Example Pack | Use |
|---|---|
| [`bonds/05-worked-examples-and-implementation-patterns.md`](bonds/05-worked-examples-and-implementation-patterns.md) | Bond settlement, accrued interest, yields, duration, callable bonds, downgrades, default/recovery, inflation-linked uplift, amortising schedules, convertibles, ABS prepayments and multi-currency performance examples. |
| [`structured-products/08-worked-examples-and-implementation-patterns.md`](structured-products/08-worked-examples-and-implementation-patterns.md) | Autocallable, memory coupon, principal-protection, buffer, dual-currency, accumulator, decumulator, certificate, valuation-control and mandate examples. |
| [`structured-notes/05-worked-examples-and-implementation-patterns.md`](structured-notes/05-worked-examples-and-implementation-patterns.md) | Note-wrapper examples for fixed coupon notes, reverse convertibles, Phoenix autocalls, credit-linked notes, dual currency notes, barrier reporting and lifecycle support boundaries. |

## Structured Products Versus Structured Notes

Use `structured-products/` as the broad product-family pack. It covers the wrapper-plus-payoff architecture across note, deposit, certificate, warrant, fund, OTC, accumulator, decumulator, participation, buffer, and leveraged forms.

Use `structured-notes/` when the work is specifically about issuer-note wrappers, medium-term note treatment, note lifecycle processing, note valuation, note reporting, or physical settlement from a note.

## Platform Design Distinctions

| Question | Bonds | Structured Notes |
|---|---|---|
| Core instrument nature | Debt security with coupon/principal mechanics. | Issuer debt obligation with embedded payoff formula. |
| Primary position quantity | Usually nominal amount and units/lots. | Usually note nominal and units/lots. |
| Main schedule focus | Coupon, accrual, call/put, amortisation, maturity. | Observation, coupon condition, barrier, autocall, fixing, settlement. |
| Main valuation focus | Clean/dirty price, accrued interest, yield, spread, duration, DV01. | Issuer quote/model value, payoff path, option inputs, barrier/autocall state, look-through exposure. |
| Main risk split | Issuer, rates, spread, liquidity, currency, seniority, default/recovery. | Issuer, underlying/payoff, barrier, path dependency, liquidity, complexity, currency, physical settlement. |
| Accounting warning | Do not confuse coupon, accrued interest, clean price, and dirty value. | Do not create separate client positions for embedded derivatives unless assets are legally delivered. |

## Fixed-Income Edge Cases To Model Explicitly

| Case | Why It Matters |
|---|---|
| Inflation-linked bonds | Principal, coupon and performance depend on sourced index ratios; reporting must separate real return from inflation uplift. |
| Amortising bonds | Outstanding principal, coupon accrual, maturity ladder and liquidity forecasts change after each principal repayment. |
| Convertible bonds | Legal holding remains a bond until conversion, while risk and advisory views need equity optionality and conversion-value context. |
| Asset-backed securities | Factor, scheduled principal, prepayment and remittance source dates drive valuation, yield and reinvestment-risk reporting. |
| Multi-currency bonds | Local bond return, income, FX translation and hedge effects must be explainable separately in reporting-currency performance. |

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
10. domain vocabulary and user-facing language for wealth-platform work.

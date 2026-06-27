# Private Markets And Alternatives Knowledge Map

This page connects the private-markets reference pack to reusable wealth-platform design decisions.

Private markets should be handled as a product family with commitment, cashflow, valuation, liquidity, suitability, and reporting behavior that differs materially from daily-priced public securities.

## Current Pack

| Product Area | Pack | Main Use |
|---|---|---|
| Private markets and alternatives | [`private-markets/`](private-markets/README.md) | Commitment, capital-call, distribution, NAV-lag, private equity, private credit, real assets, hedge fund, alternatives, advisory, mandate, reporting, implementation, controls, and QA reference. |

## Practical Worked Examples

Use [`private-markets/11-worked-examples-and-implementation-patterns.md`](private-markets/11-worked-examples-and-implementation-patterns.md) for concrete examples covering commitment ledgers, capital calls, distribution classification, NAV lag, restatements, partial-history IRR, private credit distributions, hedge fund redemption gates, secondary sales, support boundaries and regression tests.

## Platform Design Distinctions

| Question | Public Securities | Private Markets And Alternatives |
|---|---|---|
| Primary position view | Units, nominal, or shares with frequent market prices. | Commitment, paid-in capital, unfunded commitment, recallable capital, residual NAV, and cashflow history. |
| Valuation cadence | Often daily or intraday from market data. | Often quarterly, manager-reported, lagged, estimated, or restated. |
| Liquidity | Sale or redemption may be routine depending on instrument. | Lock-ups, gates, queues, transfer restrictions, secondaries, and manager consent can dominate. |
| Performance focus | TWR, contribution, attribution, income, P&L. | IRR, TVPI, DPI, RVPI, PME, vintage-year analysis, J-curve, and TWR integration for total portfolio reporting. |
| Risk focus | Market, credit, rate, FX, volatility, concentration. | Illiquidity, valuation uncertainty, unfunded obligations, manager concentration, vintage concentration, strategy exposure, leverage, and operational document quality. |
| Source truth | Custodian, broker, market data, corporate actions, transaction ledger. | Fund administrator, GP/manager notices, capital-account statements, portals, custodians, documents, and internal enrichment. |

## Reusable Modelling Pattern

Use separate layers:

```text
Product / Fund Master
  Legal entity, manager, strategy, vintage, domicile, terms, fees, liquidity, eligibility

Commitment Layer
  Commitment amount, paid-in capital, unfunded commitment, recallable capital, commitment currency

Notice And Cashflow Layer
  Capital calls, distributions, recallable amounts, taxes, fees, secondary transfers, in-kind deliveries

Valuation Layer
  NAV, valuation date, received date, adjusted NAV, stale state, restatement lineage

Analytics Layer
  IRR, TVPI, DPI, RVPI, PME, J-curve, liquidity buckets, unfunded stress, concentration

Control Layer
  Reconciliation, document evidence, overrides, exception queue, audit history
```

Do not treat current NAV as the full exposure. A private-market position can have low current NAV and high future funding obligation because of unfunded commitments.

## Source Ownership Questions

Before building or changing a feature, identify the source of truth for:

| Data Area | Typical Source Candidates |
|---|---|
| Fund identity and terms | Product master, onboarding documents, administrator records, manager documents. |
| Commitment and unfunded balance | Subscription agreement, administrator statement, custodian, internal commitment ledger. |
| Capital calls and distributions | GP notices, administrator notices, cash ledger, custodian confirmations. |
| NAV and valuation date | Administrator statement, manager report, valuation committee, internal override. |
| Distribution classification | Distribution notice, tax statement, administrator classification, internal operations review. |
| Liquidity terms | Offering document, side letter, fund terms, adviser restrictions. |
| Eligibility and suitability | Product governance, client profile, mandate policy, jurisdictional rules. |
| Performance metrics | Cashflow ledger, valuation records, performance engine, reporting policy. |

If the source is a PDF, portal, or manually enriched document, the platform should preserve document provenance and review status instead of presenting the data as fully automated truth.

## API And UI Implications

APIs should expose:

1. commitment, paid-in, unfunded, recallable, and NAV values separately,
2. valuation date and received date,
3. stale valuation flags and restatement lineage,
4. cashflow classifications, including return of capital, income, realised gain, recallable capital, fees, and taxes,
5. liquidity terms and redemption constraints,
6. performance metric basis and unavailable/partial-history states,
7. source provenance for notices, statements, and overrides.

UIs should make these states visible:

1. NAV is stale or estimated,
2. unfunded commitments create future cash requirements,
3. redemption is gated, queued, locked, suspended, or restricted,
4. distribution classification is pending,
5. IRR or TVPI is partial because historical cashflows are missing,
6. a NAV or notice has been restated,
7. a mandate, suitability, liquidity, or concentration rule is near breach or breached.

## QA Scenarios

High-value scenarios:

1. commitment created, capital call paid, unfunded reduced, and NAV updated,
2. distribution split into income, return of capital, realised gain, and recallable capital,
3. stale NAV appears in a client report with clear valuation date,
4. NAV restatement preserves prior value and recalculates performance,
5. secondary purchase carries assumed unfunded commitment,
6. in-kind distribution creates delivered security and restriction state,
7. hedge fund redemption is partially gated and remains pending,
8. liquidity stress compares expected capital calls against available liquidity,
9. historical cashflows are missing and IRR is blocked rather than fabricated,
10. mandate rule tests both NAV allocation and unfunded commitment exposure.

## Useful Project Workflows

Use this pack when working on:

1. alternative-investment product master,
2. commitment and capital-call ledgers,
3. private-market transaction taxonomy,
4. document-driven notice processing,
5. NAV lag and stale valuation controls,
6. private-market performance and multiples,
7. liquidity planning and cash-call stress,
8. suitability and mandate eligibility,
9. client statement and portfolio-review design,
10. migration from spreadsheet, custodian, or administrator records.

## Related Packs

| Pack | Relationship |
|---|---|
| [`funds/`](funds/README.md) | Public and pooled-fund baseline; private funds add commitment, call, distribution, and illiquidity complexity. |
| [`pooled-investment-products.md`](pooled-investment-products.md) | Useful for fund, ETF, fund-of-funds, hedge-fund, and private-fund comparisons. |
| [`bonds/`](bonds/README.md) | Useful for private credit, direct lending, distressed debt, spread, covenant, repayment, and credit-risk language. |
| [`equities/`](equities/README.md) | Useful for private equity, direct private-company exposure, in-kind distributions, and eventual listed-share handling. |
| [`derivatives/`](derivatives/README.md) | Useful for hedge-fund and alternative-strategy exposure, leverage, shorting, swaps, options, and risk measures. |
| [`structured-products/`](structured-products/README.md) | Useful when alternative exposure is delivered through notes, certificates, feeder structures, or structured wrappers. |

## Disclaimer

This map is for education, product analysis, architecture, documentation, and platform-design work only. It is not investment, legal, tax, accounting, regulatory, or client advice.

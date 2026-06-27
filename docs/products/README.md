# Product Documentation

Product documentation belongs here.

Each product family should have its own folder with a local `README.md`, a clear reading order, and source provenance.

## Product Packs

| Product Pack | Location | Purpose |
|---|---|---|
| Bonds | [`bonds/`](bonds/README.md) | Wealth-management reference for bond fundamentals, lifecycle, transaction and position modelling, valuation, yield, duration, credit risk, advisory, implementation, and QA. |
| Equities | [`equities/`](equities/README.md) | Wealth-management reference for direct equities, trade lifecycle, settlement, corporate actions, dividends, cost basis, valuation, P&L, performance, risk, advisory, DPM, reporting, implementation, and QA. |
| Funds | [`funds/`](funds/README.md) | Wealth-management reference for funds, ETFs, share classes, lifecycle, subscriptions/redemptions, NAV, fees, look-through, performance, advisory, implementation, and QA. |
| Structured notes | [`structured-notes/`](structured-notes/README.md) | Wealth-management reference for notes, structured notes, lifecycle, data model, valuation, risk, advisory, implementation, and QA. |

## Product Area Maps

| Map | Purpose |
|---|---|
| [`direct-equities-and-market-operations.md`](direct-equities-and-market-operations.md) | Captures reusable platform-design distinctions for direct equities, trading, corporate actions, P&L, risk, and market operations. |
| [`fixed-income-and-structured-products.md`](fixed-income-and-structured-products.md) | Compares bonds and structured notes and captures reusable platform-design distinctions across fixed income and structured products. |
| [`pooled-investment-products.md`](pooled-investment-products.md) | Captures reusable platform-design distinctions for funds, ETFs, fund of funds, hedge funds, and private market funds. |

## Suggested Pack Structure

```text
docs/products/<product-slug>/
  README.md
  01-overview-and-taxonomy.md
  02-lifecycle-and-transactions.md
  03-data-model-and-risk.md
  04-platform-implementation.md
```

Use this structure when it fits. If source material is already well organized, preserve the source sequence and update the local README.

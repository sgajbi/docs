# 01 - Market Data and Reference Data Fundamentals

## 1. What this domain covers

Market data and reference data are the foundation of a wealth platform. They define what a product is, how it is identified, how it is priced, how it behaves, how it is classified, how it is reported and how it is controlled.

In a private-bank platform, the data domain usually covers:

| Data family | Examples | Used by |
|---|---|---|
| Instrument reference data | ISIN, ticker, name, asset class, issuer, maturity, coupon, fund class, option terms | Product master, holdings, orders, suitability, reporting |
| Entity reference data | issuer, guarantor, counterparty, fund manager, exchange, custodian, LEI | risk, concentration, issuer exposure, counterparty exposure |
| Price data | equity close, bond bid/ask, fund NAV, derivative marks, structured note issuer bid | valuation, performance, reporting, collateral |
| FX rates | spot, close, fixing, forward points, cross rates | multi-currency valuation, cash, reporting, performance |
| Interest-rate data | rates, curves, discount factors, benchmark fixings | bonds, derivatives, loans, deposits, structured products |
| Index and benchmark data | index levels, component weights, returns, rebalances | benchmark return, attribution, model portfolios |
| Corporate action data | dividends, splits, rights, mergers, spin-offs, redemptions | positions, cost basis, income, performance |
| Calendar data | market holidays, business days, settlement calendars, fixing calendars | settlement, valuation, accrual, order cut-off |
| Classification data | asset class, sector, country, rating, region, risk bucket, liquidity bucket | allocation, risk, advisory, DPM limits |
| Data quality metadata | stale flag, source, confidence, manual override, pricing level | controls, reporting disclosure, QA |

## 2. Market data vs reference data

| Dimension | Market data | Reference data |
|---|---|---|
| Meaning | Time-varying values used for valuation and analytics | Descriptive/static or slow-changing attributes |
| Examples | prices, FX rates, index levels, yield curves | ISIN, maturity date, coupon type, issuer, country |
| Change frequency | intraday/daily/monthly/event-driven | creation/update/event-driven |
| Primary risk | stale, missing, wrong timestamp, wrong source, wrong currency | incorrect product setup, duplicate security, wrong classification |
| Consumer impact | wrong valuation, P&L, risk, buying power | wrong suitability, reporting, exposures, lifecycle handling |

The boundary is not always clean. A credit rating, sector classification, issuer parent, fund liquidity term or benchmark composition can behave like reference data but still change over time and must be effective-dated.

## 3. The data consumer view

Different consumers need different versions of the same data.

| Consumer | Needs |
|---|---|
| Portfolio valuation | price, FX, accrued income, price quality, valuation timestamp |
| Performance | market values, cashflows, returns, corporate action-adjusted prices |
| Risk analytics | exposure, volatility series, factor data, rating, duration, Greeks |
| Advisory | product risk, liquidity, complexity, eligibility, target market |
| DPM mandates | asset class, issuer, country, currency, liquidity, ESG, restricted lists |
| Client reporting | report labels, price date, indicative/executable flag, stale price disclosure |
| Buying power / collateral | eligible collateral value, haircut, LTV, FX haircut, pledge status |
| Tax/reporting | income classification, tax lot data, withholding attributes |
| Accounting | fair value level, amortized cost support, accruals, price hierarchy |

## 4. Common failure modes

| Failure | Example | Impact |
|---|---|---|
| Duplicate instrument | Same bond loaded under two security IDs | Wrong positions, concentration, tax lots |
| Wrong instrument context | ADR priced as local share | Wrong valuation and exposure |
| Wrong currency | GBp equity price treated as GBP | 100x valuation error |
| Wrong price type | Bond clean price treated as dirty price | Market value or accrued double count |
| Stale price | Fund NAV not updated for 5 days | Client report and mandate breach may be misleading |
| Wrong source priority | Indicative quote overrides executable exchange price | Reporting inconsistency |
| Missing corporate action | Split not applied | Incorrect quantity and cost basis |
| Wrong FX fixing | Trade settlement FX used for performance valuation | Distorted returns |
| Missing effective dating | New sector overwrites historical sector | Attribution history changes incorrectly |
| Manual override without lineage | Price changed to pass report | Audit failure |

## 5. Data domain mental model

A robust platform should separate the following layers:

```text
External sources
  -> raw feed storage
  -> validation and normalization
  -> canonical master data
  -> golden copy selection
  -> business-specific derived data
  -> consumption by valuation, risk, performance, advisory and reporting
```

Do not let every consuming application implement its own mapping, stale-price policy, currency treatment, classification hierarchy or source priority. That creates inconsistent client reporting and makes issue triage very hard.

## 6. Data freshness concepts

| Term | Meaning |
|---|---|
| As-of date | Business date for which the data applies |
| Timestamp | Time the value was generated, received or stored |
| Effective date | Date from which an attribute becomes valid |
| Publication date | Date the source published the value |
| Knowledge date | Date the platform first knew the value |
| Valuation date | Date used for portfolio valuation |
| Cut-off time | Time after which late data rolls to next cycle or triggers rerun |

For backdated corrections and corporate actions, knowledge date and effective date are both important. A platform may need to restate calculations from the effective date while preserving audit evidence that the correction arrived later.

## 7. Architecture principles

1. Keep raw feed data immutable.
2. Store normalized canonical records separately from raw records.
3. Make every derived value traceable.
4. Effective-date reference data.
5. Version product terms and classifications.
6. Treat price source and price quality as first-class data.
7. Support multiple valuation policies for different use cases.
8. Separate accounting valuation, collateral valuation, advisory view and report view when rules differ.
9. Allow controlled manual overrides with maker-checker approval.
10. Expose data quality status to downstream applications rather than hiding it.

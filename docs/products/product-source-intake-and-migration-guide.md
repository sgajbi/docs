# Product Source Intake And Migration Guide

This guide helps turn source files, statements, spreadsheets, PDFs, extracts, and legacy records into reliable product knowledge, requirements, validation packs, and implementation-ready data models.

Use it when importing product documents, migrating product data, designing source ingestion, validating sample portfolios, writing API contracts, or preparing QA evidence. It complements [`source-ownership-calculation-reporting-matrix.md`](source-ownership-calculation-reporting-matrix.md), [`product-qa-regression-matrix.md`](product-qa-regression-matrix.md), and [`product-capability-boundary-matrix.md`](product-capability-boundary-matrix.md).

## Intake Principle

Do not start by mapping columns to fields.

Start by identifying the business truth:

1. What product family and subtype is represented?
2. What does the client legally own or owe?
3. What source owns the position, transaction, lifecycle event, valuation, cashflow, and classification?
4. Which fields are source truth, derived values, manual enrichments, projections, or stale snapshots?
5. Which calculations and reports depend on each field?
6. What should happen when the field is missing, stale, disputed, restricted, partial, or unsupported?

## Intake Workflow

| Step | Action | Output |
|---|---|---|
| 1. Source inventory | List files, systems, providers, dates, owners, formats, frequency, and sample coverage. | Source inventory and ownership map. |
| 2. Product classification | Identify product family, subtype, wrapper, legal holding, economic exposure, and adjacent product relationships. | Product classification table. |
| 3. Field profiling | Profile identifiers, dates, currencies, quantities, prices, rates, fees, statuses, cashflows, and free-text fields. | Field dictionary with data-quality notes. |
| 4. Business mapping | Map source fields to product concepts: position, transaction, lifecycle event, valuation, exposure, cashflow, restriction, report label. | Business mapping table. |
| 5. Calculation dependency | Identify formulas and downstream outputs that depend on each mapped field. | Calculation dependency matrix. |
| 6. Degraded-state design | Define missing, stale, estimated, manual, restricted, disputed, partial, and unsupported behavior. | Supportability rules. |
| 7. Reconciliation design | Define source-to-target counts, balances, values, cashflows, tolerances, and exception workflow. | Reconciliation plan. |
| 8. QA sample pack | Create normal and degraded examples with expected outputs and reports. | Regression sample pack. |
| 9. Sign-off evidence | Record source lineage, assumptions, open gaps, unsupported areas, and owner approval. | Migration or intake sign-off note. |

## Source Type Matrix

| Source Type | Common Strength | Common Risk | Required Intake Control |
|---|---|---|---|
| Custodian position file | Strong position and custody truth. | May lack product terms, look-through, or lifecycle context. | Reconcile quantity/nominal, account, currency, valuation date, and restrictions. |
| Broker trade file | Strong trade execution truth. | May not prove settlement, fees, taxes, or final entitlement. | Separate trade, settlement, fee, tax, and correction events. |
| Cash ledger | Strong booked cash truth. | Ledger cash is not necessarily available cash. | Preserve value date, restrictions, reservations, and settlement state. |
| Security master | Strong identifiers and listed instrument metadata. | May be weak for structured products, private markets, insurance, or collateral terms. | Identify missing product-specific fields and support-limited behavior. |
| Market-data feed | Strong price/rate/fixing truth when current. | Stale, wrong unit, wrong exchange, or wrong price basis. | Preserve price source, timestamp, unit, currency, and stale policy. |
| Fund administrator file | Strong NAV/order/unit truth. | May lag, restate, or omit look-through and liquidity details. | Preserve NAV date, received date, order status, restatement lineage, and liquidity state. |
| Manager or GP notice | Strong private-market event detail. | Often PDF/manual and not normalized. | Preserve document provenance, review status, cashflow classification, and notice date. |
| Issuer notice or term sheet | Strong structured-product lifecycle and payoff truth. | May require manual interpretation and field extraction. | Preserve term-sheet lineage, observation schedule, payoff rules, and unresolved terms. |
| Insurer statement | Strong policy value and status truth. | Value bases may be ambiguous or mixed with projections. | Separate account value, cash value, surrender value, death benefit, guarantees, and quote date. |
| Credit or collateral system | Strong facility, exposure, haircut, and pledge truth. | May not align to portfolio holdings or market values. | Reconcile pledge graph, facility exposure, collateral eligibility, and price/FX source. |
| Appraisal or property report | Strong real-asset valuation context. | Stale, subjective, document-heavy, or non-market value. | Preserve appraisal date, received date, method, ownership percentage, and stale state. |
| Legacy spreadsheet | Rich local business context. | Manual, inconsistent, undocumented, duplicate, or stale. | Profile every field, preserve assumptions, and require reconciliation before using as source truth. |

## Product-Specific Intake Questions

| Product Family | Questions To Answer Before Mapping |
|---|---|
| Bonds | Are coupon schedule, day count, clean/dirty price, accrued interest, call/put terms, rating, maturity, and default state present? |
| Cash, deposits, money market and FX | Are ledger, settled, projected, restricted, reserved, and available cash separated? Are value dates and FX conventions explicit? |
| Commodities, precious metals and real assets | Are wrapper type, unit, grade, custody/provider, price unit, contract size, delivery state, and pledge state explicit? |
| Equities | Are listing, settlement, tax lots, corporate actions, entitlement dates, restrictions, and dividend tax treatment available? |
| Funds | Are fund/share-class identity, NAV date, dealing calendar, order status, liquidity state, distribution classification, and look-through lineage present? |
| Insurance and annuities | Are policy roles, coverage, riders, premium schedule, value basis, surrender quote, loan state, beneficiary/privacy restrictions, and guarantees separated? |
| Loans, Lombard, margin and collateral | Are facility, exposure, accrued interest, pledge graph, haircut/LTV, eligibility, availability, reservations, and margin-call state reconciled? |
| Private markets | Are commitment, paid-in, unfunded, recallable, NAV date, distribution split, cashflow history, liquidity terms, and restatement lineage available? |
| Real estate, REITs and infrastructure | Are wrapper, sector/geography look-through, appraisal/NAV/market value basis, leverage, income classification, liquidity terms, and valuation date present? |
| Derivatives | Are contract terms, confirmation, notional, multiplier, underlying, expiry, fixing, valuation, Greeks, margin, collateral, and counterparty present? |
| Structured products | Are wrapper, issuer, payoff terms, underlyings, barriers, observations, coupons, issuer quote, settlement rules, and term-sheet lineage present? |
| Structured notes | Are note terms, coupon schedule, observations, maturity/call, issuer, underlyings, credit event terms, and delivered-asset policy available? |

## Migration Validation Pack

Each migrated product family should include:

1. source inventory,
2. field dictionary,
3. product classification mapping,
4. source-to-target mapping,
5. source ownership and lineage,
6. required calculations and formulas,
7. reconciliation totals and tolerances,
8. sample normal records,
9. sample degraded records,
10. unsupported subtype list,
11. report-output examples,
12. sign-off assumptions and open issues.

## Reconciliation Checks

| Check | Purpose |
|---|---|
| Record count by source and product family | Detect missing files, filters, duplicate loads, or unsupported families. |
| Position quantity/nominal/unit total | Detect quantity, unit, multiplier, or split errors. |
| Market value by currency and account | Detect price, FX, valuation date, and account-mapping errors. |
| Cash balance by value date and currency | Detect ledger versus settlement errors. |
| Transaction cashflow total by type | Detect missing fees, tax, income, settlements, corrections, or reversals. |
| Lifecycle event count by status | Detect observations, corporate actions, calls, maturities, claims, gates, or expiries that were not processed. |
| Source-date age by product family | Detect stale NAV, appraisal, insurer quote, issuer quote, market price, or FX rate. |
| Unsupported subtype count | Ensure unsupported items are visible rather than silently mapped to generic products. |

## Common Intake Failures

Avoid:

1. mapping source columns before understanding product subtype,
2. treating source-provided market value as sufficient without quantity, price, FX, and date,
3. treating free-text product names as reliable taxonomy,
4. treating all cashflows as income,
5. treating all NAV products as daily liquid,
6. treating issuer quotes, insurer values, and appraisals as market-traded values,
7. collapsing lifecycle events into transactions,
8. collapsing legal holding into analytical exposure,
9. ignoring restricted, pledged, or unavailable quantities,
10. marking missing source fields as zero,
11. overwriting restated values without preserving lineage,
12. importing sensitive policy, beneficiary, health, or estate data without access controls.

## Sign-Off Questions

Before accepting migrated or newly ingested product data, confirm:

1. Which source owns each critical field?
2. Which fields are manually enriched?
3. Which records are unsupported or partial?
4. Which calculations are blocked by missing inputs?
5. Which reports can be trusted immediately?
6. Which reports need supportability labels?
7. Which reconciliation breaks remain open?
8. Which product subtypes need future implementation?
9. Which data-quality rules should become automated controls?
10. Which owner approved the assumptions?

## Disclaimer

This guide is for education, product analysis, documentation, source-data review, migration planning, QA, operations, and implementation support. It is not investment, legal, tax, accounting, regulatory, insurance, credit, property, trading, or client advice.

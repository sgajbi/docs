# Real Estate, REITs And Infrastructure Knowledge Map

This page connects real estate, listed REITs, infrastructure, and real-asset fund products to reusable wealth-platform design decisions.

Real estate and infrastructure are wrapper-sensitive product families. A listed REIT behaves operationally like an equity, a REIT fund behaves like a fund, a private real estate vehicle behaves like a private-market fund, and a direct property holding behaves like a document-heavy real asset. The platform should model both the accounting wrapper and the economic real-asset exposure.

## Current Pack

| Product Area | Pack | Main Use |
|---|---|---|
| Real estate, REITs, infrastructure and real-asset funds | [`real-estate-reits-infrastructure/`](real-estate-reits-infrastructure/README.md) | Listed REITs, business trusts, property securities, private real estate funds, direct property exposure, infrastructure funds, lifecycle, valuation, income, risk, advisory, reporting, implementation, controls, and QA reference. |

## Platform Design Distinctions

| Question | Listed Security Or Fund View | Real Estate And Infrastructure View |
|---|---|---|
| Primary position view | Share, unit, fund holding, or private-fund interest. | Wrapper plus property/infrastructure sector, geography, income model, leverage, valuation basis, liquidity terms, and look-through exposure. |
| Lifecycle behavior | Trade, subscription, redemption, distribution, corporate action. | Exchange trade, NAV dealing, capital call, appraisal update, lease/rent income, concession cashflow, property sale, refinancing, redemption queue. |
| Valuation basis | Market price, NAV, or administrator value. | Listed price, NAV, appraisal, transaction value, manager value, income-capitalization model, discounted cashflow, stale/restated valuation. |
| Performance drivers | Price return, income, FX, fees. | Rental income, distributions, occupancy, cap rates, leverage, refinancing cost, development risk, inflation linkage, regulatory/concession risk. |
| Risk focus | Market, issuer, manager, liquidity, currency. | Property type, tenant concentration, geography, leverage, rates, refinancing, vacancy, valuation lag, lock-up, gate, infrastructure regulation, concession duration. |
| Reporting challenge | Holdings, market value, income, allocation. | Separate listed/liquid wrapper from underlying real-asset exposure, income quality, valuation date, leverage, liquidity, and concentration. |

## Reusable Modelling Pattern

Use separate layers:

```text
Wrapper Layer
  Listed REIT, property company, business trust, ETF/fund, private fund, direct property, infrastructure vehicle

Real-Asset Exposure Layer
  Property sector, infrastructure sector, geography, tenant/offtaker type, lease/concession duration, inflation linkage

Holding And Commitment Layer
  Shares/units, NAV interest, commitment, unfunded amount, direct ownership share, restrictions

Cashflow Layer
  Dividends, distributions, rental income, capital calls, redemptions, asset sales, financing costs, taxes/fees

Valuation Layer
  Market price, NAV, appraisal, manager value, valuation date, received date, stale/restated state

Risk And Analytics Layer
  Yield, occupancy, leverage, cap rate sensitivity, rate sensitivity, liquidity, concentration, look-through exposure

Control Layer
  Source lineage, corporate actions, NAV/appraisal reconciliation, document evidence, suitability, audit history
```

Do not classify every real-asset product as private markets or every REIT as generic equity. The wrapper controls transactions and settlement; the economic exposure controls allocation, risk, suitability, and reporting.

## Source Ownership Questions

Before building or changing a feature, identify the source of truth for:

| Data Area | Typical Source Candidates |
|---|---|
| Listed REIT/security identity | Security master, exchange, custodian, market-data vendor, corporate-action source. |
| Fund or private vehicle identity | Fund administrator, manager, product master, transfer agent, custodian, subscription documents. |
| Direct property details | Property documents, appraisal reports, title records, manager files, operations enrichment. |
| Sector and geographic exposure | Product master, manager report, index provider, data vendor, internal enrichment. |
| NAV, appraisal and valuation date | Fund administrator, manager report, appraiser, valuation committee, custodian statement. |
| Distributions and income split | Issuer notice, fund administrator, custodian, tax statement, cash ledger. |
| Leverage and financing metrics | Manager report, property/fund reporting, lender statement, internal enrichment. |
| Liquidity and redemption terms | Offering document, fund terms, exchange liquidity data, manager notice, side letter. |
| Collateral eligibility and haircut | Credit policy, collateral system, product master, risk engine, manual override register. |

When exposure comes from manager reports or documents, preserve reporting date, received date, source document, and manual review status.

## API And UI Implications

APIs should expose:

1. wrapper type and economic real-asset exposure separately,
2. listed price, NAV, appraisal value, valuation date, and received date,
3. income/distribution classification and source status,
4. sector, geography, tenant/offtaker, leverage, maturity/refinancing, and liquidity attributes where source-backed,
5. private-fund commitment, paid-in, unfunded, NAV, capital calls, and distributions where applicable,
6. corporate-action, redemption, gate, queue, suspension, appraisal, and restatement lifecycle states,
7. collateral eligibility, haircut, pledged amount, and availability where used for lending.

UIs should make these states visible:

1. listed REIT exposure is liquid market-traded but economically property-linked,
2. private real estate NAV is stale, estimated, manager-reported, or appraised,
3. distribution yield is not the same as guaranteed income,
4. leverage and refinancing risk affect both income and valuation,
5. liquidity differs by wrapper, exchange volume, lock-up, gate, queue, or transfer restriction,
6. infrastructure cashflows may depend on regulation, concession, offtake, inflation linkage, or project lifecycle,
7. source coverage is partial when look-through sector/geography is unavailable.

## QA Scenarios

High-value scenarios:

1. listed REIT buy/sell settles like equity while real-estate exposure updates in allocation reports,
2. REIT distribution books gross income, tax withholding, net cash, and income classification,
3. REIT corporate action changes units without losing cost basis or exposure mapping,
4. private real estate capital call reduces unfunded commitment and cash,
5. private real estate NAV update preserves valuation date and stale/restated state,
6. infrastructure fund distribution is split by income, return of capital, and realized gain when source-backed,
7. redemption queue or gate prevents expected cash from appearing as available cash,
8. appraised direct property value is labelled as appraisal-based rather than market-traded,
9. leverage increase affects risk reporting and collateral eligibility,
10. missing look-through data marks sector/geography analytics partial instead of inferring false exposure.

## Useful Project Workflows

Use this pack when working on:

1. REIT and property-security product master design,
2. real-asset sector and geography exposure mapping,
3. listed REIT, REIT fund, private real estate fund, and direct property comparisons,
4. NAV/appraisal valuation controls and stale-state reporting,
5. real estate and infrastructure income reporting,
6. capital-call and distribution workflows for private vehicles,
7. mandate allocation, concentration, liquidity, and suitability checks,
8. collateral eligibility and haircut treatment for REITs or real assets,
9. client reporting for income, liquidity, leverage, and real-asset allocation,
10. migration from custodian, administrator, manager, appraisal, property, or spreadsheet records.

## Related Packs

| Pack | Relationship |
|---|---|
| [`equities/`](equities/README.md) | Listed REITs, property companies, business trusts, trading, settlement, dividends, and corporate actions. |
| [`funds/`](funds/README.md) | REIT funds, infrastructure funds, NAV, dealing, fees, distributions, and look-through exposure. |
| [`private-markets/`](private-markets/README.md) | Private real estate, private infrastructure, commitments, capital calls, NAV lag, distributions, and illiquidity. |
| [`commodities-precious-metals-real-assets/`](commodities-precious-metals-real-assets/README.md) | Real-asset theme comparison, natural resources, commodity-linked proxies, and collateral exposure. |
| [`loans-lombard-margin-collateral/`](loans-lombard-margin-collateral/README.md) | REIT collateral, direct property collateral, LTV, haircuts, pledges, refinancing, and buying power. |
| [`bonds/`](bonds/README.md) | Real estate debt, infrastructure debt, secured credit, covenants, duration, spread, and refinancing risk. |
| [`cash-deposits-money-market-fx/`](cash-deposits-money-market-fx/README.md) | Distributions, capital calls, cross-border income, FX translation, settlement cash, and liquidity planning. |

## Disclaimer

This map is for education, product analysis, architecture, documentation, and platform-design work only. It is not investment, legal, tax, accounting, regulatory, property, credit, or client advice.

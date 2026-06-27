# Commodities, Precious Metals And Real Assets Knowledge Map

This page connects commodities, precious metals, commodity-linked instruments, and real-asset-linked trading exposures to reusable wealth-platform design decisions.

Commodity exposure is wrapper-driven. Physical gold, an allocated metal account, an unallocated account, an ETF, an ETC, an ETN, a futures position, an option, a swap, a structured note, and a mining equity can all reference the same commodity theme while creating different legal holdings, cashflows, risk, valuation, and reporting behavior.

## Current Pack

| Product Area | Pack | Main Use |
|---|---|---|
| Commodities, precious metals and real-asset-linked trading exposures | [`commodities-precious-metals-real-assets/`](commodities-precious-metals-real-assets/README.md) | Physical and account-based precious metals, commodity ETPs, derivatives, structured products, real-asset exposures, lifecycle, valuation, performance, risk, advisory, reporting, implementation, controls, and QA reference. |

## Practical Worked Examples

Use [`commodities-precious-metals-real-assets/10-worked-examples-and-implementation-patterns.md`](commodities-precious-metals-real-assets/10-worked-examples-and-implementation-patterns.md) for concrete examples covering allocated and unallocated gold, commodity futures rolls, ETN issuer risk, pledged gold haircuts, commodity-linked note observations, fund look-through, support boundaries and regression tests.

## Platform Design Distinctions

| Question | Ordinary Security View | Commodity And Real-Asset Exposure View |
|---|---|---|
| Primary position view | Shares, units, nominal, or contract count. | Wrapper plus commodity family, grade, unit of measure, price source, custody/provider, and look-through exposure. |
| Legal holding | Security or cash balance. | Physical title, allocated custody, unallocated provider claim, fund/ETP share, derivative contract, note, receipt, or equity/fund proxy. |
| Valuation basis | Market price, NAV, or model value. | Spot, benchmark, unit conversion, NAV, issuer quote, futures curve, option model, storage/financing cost, FX rate. |
| Performance drivers | Price return, income, FX, fees. | Spot return, roll yield, storage cost, financing, management fees, issuer spread, derivative P&L, FX, collateral yield. |
| Risk focus | Market, issuer, liquidity, currency. | Unit/grade mismatch, custody/vault risk, provider credit, roll risk, physical delivery, margin, basis, volatility, collateral haircut, concentration. |
| Reporting challenge | Holdings and market value. | Separate legal holding from commodity exposure, notional exposure, collateral value, liquidity, provider risk, and wrapper-specific limitations. |

## Reusable Modelling Pattern

Use separate layers:

```text
Wrapper Layer
  Physical holding, allocated account, unallocated account, ETF/ETC/ETN, future, option, swap, note, certificate

Commodity Reference Layer
  Family, commodity, grade, unit, benchmark, price source, exchange, delivery location, currency

Legal Holding Layer
  Quantity, contract size, title/custody, provider, issuer, counterparty, account, encumbrance state

Valuation Layer
  Spot/NAV/quote/model value, unit conversion, FX, curve, volatility, storage/financing assumptions, valuation date

Exposure Layer
  Commodity-family exposure, notional, delta-adjusted exposure, issuer exposure, counterparty exposure, collateral value

Lifecycle Layer
  Purchase/sale, storage fee, roll, expiry, delivery, exercise, margin, observation, redemption, corporate action

Control Layer
  Unit validation, stale price, source lineage, custody reconciliation, margin reconciliation, suitability, audit history
```

Do not model commodity exposure as a single asset class field. Model the legal wrapper and analytical commodity exposure separately.

## Source Ownership Questions

Before building or changing a feature, identify the source of truth for:

| Data Area | Typical Source Candidates |
|---|---|
| Commodity identity, grade and unit | Commodity master, market-data vendor, exchange contract specification, product master. |
| Physical metal quantity and bar list | Vault/custodian statement, allocated inventory file, operations reconciliation. |
| Unallocated metal account balance | Provider statement, custodian, account platform, operations workflow. |
| ETP security, NAV and holdings | Security master, exchange, issuer, fund administrator, market-data vendor. |
| Futures/options contract details | Exchange contract specification, broker/FCM, clearing house, trade capture. |
| OTC derivative terms and margin | Counterparty confirmation, broker, collateral system, valuation vendor, trade capture. |
| Commodity-linked note terms | Term sheet, issuer, product master, custodian, valuation source. |
| Prices, curves and volatility | Market-data vendor, exchange, pricing vendor, internal valuation model. |
| Collateral haircuts and eligibility | Credit policy, collateral system, product master, risk engine, manual override register. |

Unit-of-measure and price-unit lineage should be treated as critical source truth. Troy ounce, gram, kilogram, barrel, tonne, contract size, and price currency errors can create large valuation defects.

## API And UI Implications

APIs should expose:

1. wrapper type and legal holding separately from commodity exposure,
2. commodity family, commodity, grade, unit, benchmark, price source, and valuation date,
3. position quantity, contract size, notional, delta-adjusted exposure, and collateral value separately,
4. custody/provider/issuer/counterparty fields depending on wrapper,
5. storage fees, roll activity, futures margin, derivative settlement, and lifecycle events,
6. stale price, stale NAV, stale curve, missing unit conversion, and missing FX states,
7. collateral haircut, pledged quantity, eligible quantity, and availability when used for lending,
8. suitability and mandate flags for leverage, complexity, delivery risk, and concentration.

UIs should make these states visible:

1. gold exposure is physical, allocated, unallocated, ETP, derivative, note, or proxy equity,
2. value depends on a specific unit and price source,
3. futures exposure is not the same as margin cash,
4. ETP performance may differ from spot because of fees, roll, or issuer mechanics,
5. unallocated metal and ETNs carry provider or issuer credit exposure,
6. contracts near expiry require roll, close-out, or delivery handling,
7. pledged metal is not freely available,
8. delivery, storage, liquidity, and suitability limitations are explicit.

## QA Scenarios

High-value scenarios:

1. physical gold purchase validates troy-ounce quantity, price unit, cash debit, cost basis, and market value,
2. allocated and unallocated gold both show commodity exposure but different custody/provider risk,
3. storage fee posts as expense without changing quantity unless paid in metal,
4. futures daily variation margin posts cash movement while notional exposure remains separate,
5. futures roll closes the old contract, opens the new contract, and preserves exposure history,
6. contract expiry workflow prevents unintended delivery in a non-delivery mandate,
7. ETN downgrade changes issuer-risk reporting while commodity exposure remains,
8. commodity-linked note observation updates payoff state without creating a directly held commodity,
9. pledged gold recalculates lending value after price shock and haircut application,
10. unit conversion defect is blocked before market value is published.

## Useful Project Workflows

Use this pack when working on:

1. commodity and precious-metal product master design,
2. physical metal and account-based metal position modelling,
3. ETP, ETN, certificate, and structured commodity exposure mapping,
4. futures/options/swaps lifecycle and margin treatment,
5. commodity exposure, notional, and delta-adjusted reporting,
6. collateral eligibility and precious-metal haircut logic,
7. unit-of-measure validation and price-source governance,
8. suitability, complexity, leverage, and delivery-risk controls,
9. client reporting for commodity exposure and wrapper risk,
10. migration from vault, broker, exchange, issuer, custodian, or spreadsheet records.

## Related Packs

| Pack | Relationship |
|---|---|
| [`derivatives/`](derivatives/README.md) | Futures, forwards, options, swaps, margin, collateral, expiry, and close-out behavior. |
| [`structured-products/`](structured-products/README.md) | Commodity-linked notes, certificates, barriers, observations, issuer quotes, and payoff scenarios. |
| [`funds/`](funds/README.md) | Commodity ETFs, ETCs, funds, share classes, NAV, fees, and look-through exposure. |
| [`equities/`](equities/README.md) | Mining equities, resource equities, REITs, corporate actions, and listed proxy exposures. |
| [`loans-lombard-margin-collateral/`](loans-lombard-margin-collateral/README.md) | Gold collateral, haircuts, pledges, lending value, and buying power. |
| [`private-markets/`](private-markets/README.md) | Real estate, infrastructure, natural resources, illiquid real assets, and valuation uncertainty. |
| [`cash-deposits-money-market-fx/`](cash-deposits-money-market-fx/README.md) | Commodity settlement cash, USD pricing, FX translation, collateral cash, and liquidity. |

## Disclaimer

This map is for education, product analysis, architecture, documentation, and platform-design work only. It is not investment, legal, tax, accounting, regulatory, trading, commodity, insurance, or client advice.

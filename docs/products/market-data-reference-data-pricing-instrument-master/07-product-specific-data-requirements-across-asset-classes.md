# 07 - Product-Specific Data Requirements Across Asset Classes

## 1. Why product-specific data matters

A generic security master is not enough. Each product family requires specific fields for valuation, lifecycle handling, suitability, risk and reporting.

## 2. Equities

Required data:

- instrument identifiers,
- exchange/MIC,
- ticker and local symbol,
- currency and trading unit,
- lot size and board lot,
- country of listing, incorporation and risk,
- sector/industry,
- dividend currency,
- corporate action feed,
- settlement calendar,
- short-sale eligibility where applicable.

Critical controls:

- GBp vs GBP handling,
- ADR ratio mapping,
- suspended/delisted status,
- split adjustment,
- corporate-action entitlement.

## 3. Bonds and fixed income

Required data:

- issuer, guarantor, seniority,
- coupon type and rate,
- day count convention,
- payment frequency,
- maturity date,
- call/put/sink schedule,
- issue currency,
- rating and rating source,
- clean/dirty price basis,
- accrued interest rules,
- yield convention,
- minimum denomination,
- settlement calendar.

Critical controls:

- coupon schedule validation,
- accrued interest validation,
- callable bond yield selection,
- default/restructuring status,
- price basis and quote basis.

## 4. Funds and ETFs

Required data:

- fund umbrella and share class,
- ISIN/security ID,
- NAV currency,
- dealing frequency,
- cut-off time,
- subscription/redemption settlement days,
- distribution vs accumulation class,
- fees/TER/management fee,
- fund manager,
- domicile,
- liquidity gates/suspension flags,
- look-through holdings where available.

Critical controls:

- latest official NAV,
- NAV lag disclosure,
- share class mapping,
- distribution reinvestment,
- fund split/class merger.

## 5. Structured notes and structured products

Required data:

- issuer,
- note terms,
- underlyings,
- initial fixing,
- strike,
- barrier,
- coupon schedule,
- observation schedule,
- autocall terms,
- settlement type,
- physical-delivery terms,
- valuation source,
- issuer bid/indicative price,
- lifecycle event status.

Critical controls:

- barrier/event observation,
- final fixing,
- issuer quote freshness,
- underlying mapping,
- payoff explanation.

## 6. Derivatives

Required data:

- derivative type,
- underlying,
- expiry,
- strike,
- option style,
- multiplier,
- contract size,
- settlement type,
- clearing/counterparty,
- margin model,
- mark price,
- Greeks,
- collateral currency.

Critical controls:

- multiplier correctness,
- expiry exercise handling,
- margin/collateral updates,
- valuation model version,
- counterparty exposure.

## 7. FX and cash products

Required data:

- currency codes,
- currency minor units,
- value date conventions,
- holiday calendars,
- spot/forward rates,
- deposit rate basis,
- interest accrual convention,
- settlement status.

Critical controls:

- direct/inverted FX logic,
- NDF fixing source,
- cash balance reconciliation,
- overdraft and restricted cash flags.

## 8. Private markets and alternatives

Required data:

- vehicle name and structure,
- commitment amount,
- funded/unfunded commitment,
- capital-call notices,
- distribution notices,
- NAV date and publication date,
- vintage year,
- strategy,
- manager,
- liquidity terms,
- lock-up/gate,
- valuation policy.

Critical controls:

- NAV lag disclosure,
- recallable distributions,
- capital-call due dates,
- IRR cashflow classification,
- side-pocket handling.

## 9. Loans, lending and collateral

Required data:

- facility ID,
- credit line,
- borrower,
- interest basis,
- benchmark/spread,
- maturity,
- collateral link,
- pledged assets,
- LTV/haircut,
- exposure amount,
- margin-call status,
- cross-pledge relationships.

Critical controls:

- collateral eligibility,
- valuation haircut,
- FX haircut,
- cross-pledge allocation,
- in-flight order impact.

## 10. Insurance and annuities

Required data:

- policy number,
- insurer,
- policyholder,
- insured life,
- beneficiary,
- premium schedule,
- cash value,
- surrender value,
- account value,
- riders,
- policy loan,
- annuity income terms.

Critical controls:

- policy value freshness,
- surrender charge,
- premium holiday/lapse,
- beneficiary authority,
- reporting category.

## 11. Cross-product rule

Every product family must define:

1. minimum data required to create an instrument,
2. minimum data required to trade/order,
3. minimum data required to value,
4. minimum data required to report,
5. minimum data required to include in advisory/suitability,
6. fallback/degraded-state behavior when data is incomplete.

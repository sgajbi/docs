# 02 - Instrument Master, Identifiers, Symbology and Classification

## 1. What an instrument master is

The instrument master is the canonical record of investable and reportable financial instruments. It is not just a static security table. It is the contract and classification foundation used by trading, valuation, performance, risk, suitability, reporting and accounting.

A mature instrument master supports:

- multiple identifiers for the same instrument,
- multiple venues/listings for the same economic instrument,
- multiple share classes or tranches,
- issuer and guarantor linkage,
- product subtype and payoff terms,
- classification hierarchy,
- lifecycle status,
- pricing setup,
- corporate action treatment,
- entitlement and income rules,
- eligibility and risk attributes.

## 2. Instrument, listing, venue and quote context

A common mistake is to model a security as a single row with one ticker. In reality, platforms need at least four concepts.

| Concept | Example | Why it matters |
|---|---|---|
| Economic instrument | Apple Inc. common share | Underlying legal/economic security |
| Listing | Apple listed on NASDAQ | Venue-specific tradability |
| Quote | AAPL US price in USD on XNAS | Price source and currency context |
| Position instrument | Client holding mapped to instrument ID | Accounting and reporting position |

For listed equities, the same company may trade on multiple venues, in multiple currencies or through depository receipts. For bonds, the same issuer may have many issues. For funds, each share class is usually a separate instrument because NAV, fees, currency, distribution policy and eligibility differ.

## 3. Key identifiers

| Identifier | Scope | Use |
|---|---|---|
| ISIN | International security identifier | Settlement, custody, reporting, instrument matching |
| CUSIP | US/Canada identifier | US and Canadian securities |
| SEDOL | UK/Irish market identifier | UK and exchange/security identification |
| FIGI | Open global instrument identifier | Cross-asset symbology and mapping |
| Ticker | Market symbol | Trading, UI, search; not globally unique |
| MIC | Market Identifier Code | Exchange/venue identification |
| LEI | Legal entity identifier | Issuer, counterparty, fund manager, regulatory reporting |
| CFI | Classification of financial instruments | Standardized instrument classification |
| Internal security ID | Platform identifier | Stable internal join key |
| Vendor ID | Bloomberg/Refinitiv/Morningstar/etc. | Feed mapping |

## 4. Identifier modelling

Use a many-to-one identifier table rather than storing identifiers only as columns on the instrument.

```text
instrument
  instrument_id
  canonical_name
  product_family
  lifecycle_status

instrument_identifier
  instrument_id
  identifier_type
  identifier_value
  identifier_scope
  source
  effective_from
  effective_to
  status
```

This supports identifier changes, vendor-specific IDs, legacy IDs, migrated IDs and multiple listings.

## 5. ISIN considerations

An ISIN is very useful but not enough on its own.

| Issue | Why it matters |
|---|---|
| Same ISIN can trade on multiple venues | Need MIC/listing context for price and execution |
| Some OTC/private products may not have ISIN immediately | Need temporary internal ID and later enrichment |
| Derivatives may use exchange-specific symbols | Need contract terms, expiry and strike context |
| Fund share classes have separate identifiers | Share-class-level modelling is required |
| Loans/private placements may lack standard identifiers | Need source-controlled internal identity and reference linkage |

## 6. LEI and entity master

Issuer, guarantor, counterparty, fund manager, trustee, custodian and broker should be modelled as entities, not free-text names.

```text
entity
  entity_id
  legal_name
  entity_type
  lei
  domicile_country
  regulator
  ultimate_parent_entity_id
  status
```

LEI is important because issuer/counterparty exposure, regulatory reporting and cross-entity aggregation depend on entity identity. GLEIF describes the LEI as a unique 20-character alphanumeric code that provides clear identification data for a legal entity, with information that helps answer "who is who" and "who owns whom".

## 7. Classification hierarchy

A wealth platform usually needs multiple classification schemes.

| Classification | Example | Consumer |
|---|---|---|
| Product family | Equity / Bond / Fund / Note / Derivative | Product master and reporting |
| Asset class | Equity / Fixed income / Cash / Alternatives | Allocation and mandate |
| Risk asset class | Equity risk / Credit risk / Rates risk / FX risk | Risk engine |
| Regulatory class | Listed SIP / unlisted SIP / complex product | Suitability and disclosure |
| Accounting class | FVTPL / FVOCI / amortized cost | Accounting |
| GICS/sector | Technology / Financials | Equity allocation |
| Country exposure | issuer country / risk country / listing country | concentration and reporting |
| Liquidity bucket | daily / weekly / monthly / gated / illiquid | advisory and DPM |
| Collateral category | eligible / ineligible / haircut group | buying power |

Do not assume one classification hierarchy can satisfy all consumers.

## 8. Product-family-specific master fields

| Product family | Key fields |
|---|---|
| Equity | share type, listing venue, lot size, voting class, dividend currency |
| Bond | issuer, coupon, maturity, day count, seniority, rating, call schedule |
| Fund | share class, NAV currency, dealing frequency, fees, distribution policy |
| Structured note | issuer, underlying, barrier, coupon, observation schedule, payoff terms |
| Derivative | contract type, underlying, expiry, strike, multiplier, settlement type |
| FX | currency pair, value-date convention, fixing source |
| Loan | borrower, facility, rate basis, maturity, collateral linkage |
| Private market | fund vehicle, commitment, vintage, strategy, NAV lag |
| Insurance | policy type, insured life, premium, cash/surrender value, riders |

## 9. Lifecycle status

| Status | Meaning |
|---|---|
| Draft | Created but not approved |
| Active | Tradable/reportable |
| Hold-only | Existing positions allowed, new buys blocked |
| Suspended | Temporarily unavailable |
| Matured/redeemed | Lifecycle ended |
| Delisted | No longer listed on venue |
| Terminated | Instrument no longer active |
| Merged/converted | Replaced by another instrument |
| Data quality blocked | Not usable due to unresolved master-data issue |

## 10. Governance rule

The instrument master should be controlled like production-critical infrastructure. Any change to core fields such as currency, multiplier, maturity, coupon, issue size, payoff terms, listing status or classification should have source evidence, maker-checker workflow, effective date and downstream impact assessment.

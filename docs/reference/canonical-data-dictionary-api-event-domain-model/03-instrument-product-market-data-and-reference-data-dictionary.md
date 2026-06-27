# 03 - Instrument, Product, Market Data and Reference Data Dictionary

## 1. Instrument master

| Field | Type | Description |
|---|---|---|
| instrument_id | string | internal instrument identifier |
| instrument_type | enum | cash, equity, bond, fund, note, derivative, loan, policy, commodity |
| instrument_subtype | enum | product-specific subtype |
| name | string | display name |
| short_name | string | abbreviated name |
| isin | string | ISIN if available |
| cusip | string | CUSIP if available |
| sedol | string | SEDOL if available |
| figi | string | FIGI if available |
| ticker | string | exchange ticker |
| exchange_mic | string | market identifier code |
| issue_currency | ISO currency | currency of issue |
| trading_currency | ISO currency | currency of trading |
| country_of_risk | ISO country | risk country |
| issuer_id | string | issuer entity |
| asset_class | enum | equity, fixed_income, cash, alternatives, derivative, real_asset |
| product_family | enum | bond, fund, equity, note, option, etc. |
| status | enum | active, suspended, matured, expired, delisted |
| source_system | string | source |

## 2. Product governance fields

| Field | Type | Description |
|---|---|---|
| product_id | string | product governance ID |
| instrument_id | string | instrument link |
| approved_product_universe_flag | boolean | whether approved |
| approved_countries | array | countries/booking centres |
| target_market | array | intended client segments |
| negative_target_market | array | excluded segments |
| product_risk_rating | enum | low, medium, high, very_high |
| complexity_flag | boolean | complex product indicator |
| liquidity_classification | enum | daily, weekly, monthly, illiquid, locked |
| knowledge_required | enum | basic, advanced, expert |
| suitability_required | boolean | suitability check required |
| disclosure_document_ids | array | linked KID/PHS/term sheet |
| effective_from | date | product eligibility start |
| effective_to | date | product eligibility end |
| status | enum | approved, hold_only, suspended, closed |

## 3. Bond terms

| Field | Type | Description |
|---|---|---|
| maturity_date | date | maturity |
| coupon_type | enum | fixed, floating, zero, step_up |
| coupon_rate | decimal | fixed coupon rate |
| coupon_frequency | enum | annual, semiannual, quarterly, monthly |
| day_count | enum | ACT/360, ACT/365, 30/360 |
| issue_price | decimal | issue price |
| redemption_price | decimal | redemption price |
| callable_flag | boolean | callable |
| puttable_flag | boolean | puttable |
| seniority | enum | senior, subordinated, secured |
| credit_rating | string | rating if available |

## 4. Fund terms

| Field | Type | Description |
|---|---|---|
| nav_frequency | enum | daily, weekly, monthly, quarterly |
| dealing_frequency | enum | daily, weekly, monthly, subscription_window |
| fund_structure | enum | mutual_fund, unit_trust, ETF, hedge_fund, private_fund |
| share_class | string | share class |
| accumulation_distribution | enum | accumulating, distributing |
| hedged_share_class | boolean | hedged |
| management_fee | decimal | annual fee |
| total_expense_ratio | decimal | TER |
| redemption_notice_days | integer | notice period |
| lockup_period | string | lockup if any |

## 5. Structured note terms

| Field | Type | Description |
|---|---|---|
| note_subtype | enum | FCN, ELN, CLN, DCI, autocallable, PPN |
| principal_protection_type | enum | none, conditional, full_at_maturity |
| underlying_ids | array | linked underlyings |
| barrier_terms | array | KI/KO/coupon barrier |
| observation_schedule_id | string | observation schedule |
| coupon_schedule_id | string | coupon schedule |
| settlement_type | enum | cash, physical, cash_or_physical |
| issuer_credit_risk_flag | boolean | true for notes |

## 6. Market price

| Field | Type | Description |
|---|---|---|
| price_id | string | price record ID |
| instrument_id | string | instrument |
| price_date | date | price date |
| price_timestamp | datetime | timestamp |
| price_type | enum | close, bid, ask, mid, nav, evaluated, indicative |
| price_value | decimal | price |
| price_currency | ISO currency | price currency |
| quote_basis | enum | per_unit, percent_of_par, yield, nav |
| clean_dirty | enum | clean, dirty, not_applicable |
| source | string | data source |
| stale_flag | boolean | stale indicator |
| quality_score | decimal | optional |
| approved_for_reporting | boolean | reporting use flag |

## 7. FX rate

| Field | Type | Description |
|---|---|---|
| fx_rate_id | string | FX rate record |
| base_currency | ISO currency | base currency in quote |
| quote_currency | ISO currency | quote currency |
| rate | decimal | quoted rate |
| rate_date | date | date |
| rate_timestamp | datetime | timestamp |
| rate_type | enum | spot, close, official, internal |
| source | string | source |
| inverted_flag | boolean | whether derived by inversion |
| approved_for_reporting | boolean | reporting use flag |

## 8. Corporate action / income event

| Field | Type | Description |
|---|---|---|
| event_id | string | event ID |
| instrument_id | string | instrument |
| event_type | enum | dividend, coupon, split, merger, rights, maturity, redemption |
| announcement_date | date | announcement |
| ex_date | date | ex date |
| record_date | date | record date |
| payment_date | date | payment date |
| effective_date | date | effective date |
| rate_or_ratio | decimal | ratio/rate |
| cash_amount | decimal | cash amount |
| currency | ISO currency | event currency |
| status | enum | announced, confirmed, cancelled, processed |
| source | string | source |

## 9. Validation rules

- instrument type and subtype must be consistent.
- every active tradable instrument needs currency.
- reportable prices must have source and price date.
- NAV-based instruments require NAV date and NAV frequency.
- bond valuation needs clean/dirty handling.
- structured notes need issuer, maturity and payoff terms.
- product governance eligibility must be effective-dated.
- corporate actions must be idempotent.

## 10. Key takeaway

Instrument and market data are not passive reference data; they drive valuation, suitability, risk, reporting and AI explanations.

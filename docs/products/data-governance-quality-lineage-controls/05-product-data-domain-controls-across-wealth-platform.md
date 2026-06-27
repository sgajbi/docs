# 05 - Product Data Domain Controls Across Wealth Platforms

## 1. Why product-specific controls are needed

Generic controls such as mandatory fields and file counts are not enough. Each product family has unique lifecycle, valuation, exposure, risk, tax, reporting and advisory behaviour. A platform must combine common controls with product-specific rules.

## 2. Cross-product control map

| Product family | Core data risk | Control focus |
|---|---|---|
| Cash/deposits/FX | Available cash, value date, FX quote direction, settlement. | Cash reconciliation, FX rate validation, value-date controls. |
| Bonds | Coupon, accrued interest, maturity, callability, clean/dirty price. | Day-count, accrual, yield, maturity and price checks. |
| Notes/structured products | Payoff terms, issuer, underlyings, barriers, observations, liquidity. | Termsheet completeness, lifecycle events, issuer quotes, scenario metadata. |
| Equities | Corporate actions, dividends, listing, exchange, settlement. | CA event controls, dividend entitlement, split/rights cost-basis checks. |
| Funds/ETFs | NAV, share class, distribution, dealing cycle, liquidity. | NAV freshness, unit precision, share-class mapping, liquidity flags. |
| Derivatives | Notional, multiplier, margin, Greeks, collateral, expiry. | Contract term validation, valuation/margin reconciliation, expiry controls. |
| Private markets | Commitments, capital calls, NAV lag, distributions. | Commitment and unfunded exposure controls, notice processing, NAV status. |
| Lending/collateral | Pledges, LTV, exposure, margin calls, collateral eligibility. | Collateral source, haircut rules, exposure reconciliation. |
| Insurance/annuities | Policy value, surrender value, premium, riders, claims, beneficiaries. | Policy data controls, cash value validation, beneficiary/coverage lineage. |
| Real estate/REITs | Appraisal/NAV, distributions, leverage, liquidity. | NAV/appraisal freshness, distribution classification, leverage flags. |
| Reporting/tax | Tax lot, withholding, income/capital classification, statement basis. | Tax classification, report snapshot and disclosure controls. |

## 3. Instrument master controls

Instrument master should control:

| Field | Control |
|---|---|
| Instrument ID | Unique and stable across systems. |
| Identifier mapping | ISIN/CUSIP/SEDOL/FIGI/ticker mapped to correct listing/security. |
| Asset class | Valid classification hierarchy; effective dated. |
| Currency | Valid ISO currency and consistent with price/settlement. |
| Issuer | Valid legal entity and LEI where available. |
| Country/sector | Valid taxonomy and source. |
| Listing/exchange | MIC/ticker mapping and active/inactive status. |
| Lifecycle dates | Issue, maturity, expiry, call dates, valuation dates. |
| Product complexity | Simple/complex/SIP/derivative-linked flags. |
| Suitability attributes | Risk rating, liquidity, target market, eligible client segment. |
| Reporting attributes | Statement category, income classification, look-through eligibility. |

## 4. Price and valuation controls

| Control | Product examples |
|---|---|
| Missing price | All reportable products except certain commitment-only records. |
| Stale price | Listed equities, ETFs, bonds, notes, funds. |
| Price source hierarchy | Listed price vs evaluated vs issuer vs model. |
| Price basis | Clean/dirty, bid/mid/ask, NAV, indicative value, appraisal. |
| Outlier movement | Equity price movement, NAV jump, bond yield shift, structured note value move. |
| Model input completeness | Volatility, curve, spread, correlation, FX, collateral. |
| Valuation status | Final, preliminary, estimated, stale, overridden. |
| Fair-value level | Level 1/2/3 style reporting where required by policy. |

## 5. Transaction controls

Common transaction checks:

- source transaction ID is unique;
- transaction type is valid for product family;
- trade date, settlement date and value date are valid;
- quantity/nominal/cash signs align with transaction type;
- fees and taxes are separated from gross amount;
- currency conversion uses correct FX convention;
- cancellation/reversal references original transaction;
- transaction group links multi-leg events;
- cash and security legs balance where required;
- performance cashflow classification is assigned.

## 6. Corporate action and lifecycle event controls

| Event | Control |
|---|---|
| Dividend | Entitlement based on record date/ex-date, tax withholding and cash receipt. |
| Stock split | Quantity and price/cost adjusted; total value unchanged before market move. |
| Rights issue | Entitlement, exercise, lapse, subscription and new shares modelled correctly. |
| Bond coupon | Coupon schedule, day count, accrued and cash receipt reconcile. |
| Bond maturity | Position closed and principal cash received. |
| Structured note autocall | Trigger event separated from redemption settlement. |
| Fund distribution | Cash or reinvested units booked according to election. |
| Private-market capital call | Commitment reduced/invested capital increased, cash outflow booked. |
| Derivative expiry | Contract closed, settlement/P&L booked. |
| Insurance claim | Policy status, benefit payment and coverage termination/reduction controlled. |

## 7. Position controls

| Control | Description |
|---|---|
| Opening + activity = closing | Position continuity check. |
| Quantity precision | Product-specific decimal support. |
| Nominal vs units | Bonds/notes use nominal; funds use units; options use contracts x multiplier. |
| Settled vs trade-date position | Both basis views available where needed. |
| Cost basis | Updated for buys, sells, corporate actions, conversions and transfers. |
| Accrued income | Stored separately or included based on valuation/report basis. |
| Negative positions | Allowed only for shorts, overdrafts, liabilities or timing. |
| Closed positions | No active market value unless unsettled/receivable/payable remains. |

## 8. Exposure controls

Exposure is not always equal to market value.

| Product | Exposure control |
|---|---|
| Equity | Market value and beta/sector/country exposure. |
| Bond | Market value, duration, issuer, rating and spread exposure. |
| Option | Delta-adjusted and notional exposure. |
| Future | Contract notional and margin exposure. |
| FX forward | Currency pair notional and MTM. |
| Structured note | Issuer exposure plus underlying look-through/scenario exposure. |
| Fund | Fund unit value plus look-through allocation where available. |
| Private market | NAV + unfunded commitment + sector/vintage exposure. |
| Lombard | Pledged collateral value, eligible value and credit exposure. |

## 9. Reporting controls by product

| Product | Reporting control |
|---|---|
| Bonds | Show coupon, maturity, rating, yield/price basis and accrued treatment. |
| Notes | Show issuer, underlying, barrier/observation status and liquidity caveat. |
| Funds | Show NAV date, share class and distribution treatment. |
| Equities | Show corporate action adjustments and dividend income. |
| Derivatives | Show notional, MTM, margin/collateral and risk classification. |
| Private markets | Show NAV date, commitment, funded/unfunded, distributions and valuation lag. |
| Lending | Show loan balance, interest, collateral, LTV and availability. |
| Insurance | Show policy value, surrender value, premiums, beneficiaries and coverage status where appropriate. |

## 10. Product onboarding controls

Before a new product family is enabled, ensure:

- product taxonomy exists;
- CDEs are defined;
- source ownership is assigned;
- transaction types are mapped;
- lifecycle events are modelled;
- valuation source and fallback are defined;
- report treatment is approved;
- advisory/suitability attributes exist;
- mandate treatment is defined;
- reconciliation rules are implemented;
- QA scenarios include edge cases;
- degraded-state behaviour is agreed.

## 11. Practitioner takeaway

A generic platform becomes credible only when product-specific controls are added. The platform should support a common canonical model, but data quality must understand product behaviour. Bonds, notes, funds, equities, private markets and lending do not fail in the same way; controls must reflect that.

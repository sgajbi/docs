# 07 — Data Model, Valuation, NAV, Performance, Risk and Analytics

## 1. Data model overview

A private-markets platform needs several domain objects:

```text
Instrument / Fund Master
  ├── Share Class / Series / Vehicle
  ├── Commitment Terms
  ├── Liquidity Terms
  ├── Fee and Waterfall Terms
  ├── Underlying Strategy / Exposure Classification
  ├── Capital Call Notices
  ├── Distribution Notices
  ├── NAV / Valuation Records
  ├── Position / Capital Account
  ├── Look-through Holdings / Exposures
  ├── Performance Metrics
  └── Documents and Disclosures
```

## 2. Instrument master fields

| Field | Meaning |
|---|---|
| instrument_id | Unique fund/security identifier |
| legal_name | Fund legal name |
| display_name | Client-facing name |
| product_type | Private equity, private credit, real estate, infrastructure, hedge fund, etc. |
| wrapper_type | LP, feeder, SICAV, SPV, direct private security, evergreen fund |
| manager_id / GP | Fund manager |
| administrator | Fund administrator |
| custodian | Custodian if applicable |
| domicile | Legal domicile |
| regulatory_category | Jurisdiction-specific classification |
| eligibility_category | Accredited/professional/expert/SIP/etc. |
| base_currency | Fund currency |
| share_class_currency | Investor class currency |
| vintage_year | Fund vintage |
| strategy | Detailed strategy |
| sector_focus | Sector/theme |
| geography_focus | Region/country |
| fund_life_years | Initial term |
| extension_options | GP extension rights |
| investment_period_end | End of investment period |
| expected_harvest_period | Expected exit period |
| target_size | Target fund size |
| hard_cap | Maximum fund size |
| minimum_commitment | Minimum investor commitment |
| valuation_frequency | Monthly/quarterly/semi-annual |
| reporting_lag_days | Expected lag |

## 3. Liquidity terms model

| Field | Meaning |
|---|---|
| liquidity_type | Closed-end / open-ended / evergreen / listed / semi-liquid |
| lockup_period | Initial lock-up |
| redemption_frequency | Monthly/quarterly/annual/none |
| notice_period_days | Advance notice required |
| gate_type | Investor-level / fund-level / both |
| gate_percentage | Max redemption allowed |
| redemption_fee | Fee/penalty if early redemption |
| transfer_allowed | Whether secondary transfer is permitted |
| gp_consent_required | Whether GP approval is needed |
| side_pocket_allowed | Whether illiquid assets can be side-pocketed |
| suspension_rights | Fund can suspend NAV/redemption |

## 4. Fee and waterfall model

Private-market fees can be complex. Store terms separately rather than trying to infer from transactions only.

| Field | Meaning |
|---|---|
| management_fee_rate | Annual fee rate |
| management_fee_base | Commitment / invested capital / NAV / step-down basis |
| management_fee_step_down_date | Date fee base/rate changes |
| carried_interest_rate | GP performance share |
| preferred_return_rate | Hurdle/preferred return |
| catch_up | GP catch-up mechanism |
| waterfall_type | European/whole fund or American/deal-by-deal |
| clawback | Whether clawback exists |
| recycling_allowed | Reuse of distributions allowed |
| organization_expenses_cap | Cap on org expenses |
| fund_expense_policy | What is charged to fund |
| fee_offset | Transaction/monitoring fees offset management fee |

## 5. Valuation and NAV concepts

Private-market valuation is usually based on manager/admin NAV rather than daily market prices.

| NAV concept | Explanation |
|---|---|
| Reported NAV | NAV reported by manager/admin |
| Estimated NAV | Preliminary NAV before final accounts |
| Final NAV | Finalised NAV |
| Restated NAV | Corrected NAV after revision/audit |
| Stale NAV | NAV older than expected reporting frequency |
| Adjusted NAV | Platform-adjusted NAV using cashflows after NAV date |
| Capital account | Partnership accounting balance for investor |
| Fair value | Valuation estimate under applicable accounting policy |

## 6. NAV lag problem

Private-market NAV can be materially lagged. Example:

```text
Today: 27 June 2026
Latest fund NAV date: 31 March 2026
NAV received: 20 May 2026
Public markets moved significantly after 31 March
```

Reporting must show both:

- valuation amount;
- valuation date.

Do not hide the date. A current report using stale NAV must clearly label stale valuation risk.

## 7. Adjusted NAV roll-forward

A platform may need to estimate current value by rolling forward from last NAV:

```text
Adjusted NAV = last reported NAV
             + contributions after NAV date
             - distributions after NAV date
             +/- known valuation adjustments
             +/- FX movement if reporting currency differs
```

This is not the same as a true manager NAV. Label it clearly as adjusted/estimated.

## 8. Valuation hierarchy

| Level | Source | Confidence |
|---|---|---|
| Level 1 | Listed market price for listed alternative vehicle | High if liquid |
| Level 2 | Observable inputs/comparable transactions | Medium |
| Level 3 | Manager model/appraisal/unobservable inputs | Lower, requires disclosure |
| Admin NAV | Fund administrator reported NAV | Common but may lag |
| Internal estimate | Roll-forward/proxy model | Must be labelled |

## 9. Performance metrics

### 9.1 IRR

Private-market IRR uses actual cashflow timing.

```text
Cashflows:
- contributions
+ distributions
+ ending NAV
IRR = rate that sets NPV to zero
```

Use XIRR when cashflows occur on irregular dates.

### 9.2 TVPI

```text
TVPI = (Distributions + Residual NAV) / Paid-in Capital
```

TVPI measures total value multiple.

### 9.3 DPI

```text
DPI = Distributions / Paid-in Capital
```

DPI measures realised cash returned.

### 9.4 RVPI

```text
RVPI = Residual NAV / Paid-in Capital
```

RVPI measures remaining unrealised value.

### 9.5 PIC

```text
PIC = Paid-in Capital / Commitment
```

PIC measures how much of commitment has been drawn.

### 9.6 PME

Public Market Equivalent compares private investment cashflows to a public-market index. It answers: did the private investment outperform a comparable public-market investment?

PME requires:

- private-market cashflow dates;
- private-market cashflow amounts;
- terminal NAV;
- selected benchmark index;
- benchmark returns/levels on cashflow dates.

## 10. Fee and subscription-line impact on metrics

IRR can be distorted by:

- early small distributions;
- subscription-line facilities delaying capital calls;
- short measurement period;
- residual NAV based on manager estimates;
- recycling and recallable distributions;
- partial secondary purchases;
- late investor equalisation.

Therefore, reporting should show IRR together with TVPI, DPI, RVPI and paid-in metrics.

## 11. Performance treatment in total portfolio

For portfolio analytics:

| Event | TWR treatment | Private-market IRR treatment |
|---|---|---|
| Commitment only | No market value/cashflow unless policy shows exposure | Not a cashflow |
| Capital call paid from portfolio cash | Internal investment | Negative cashflow |
| Capital call funded by new client cash | External inflow then internal investment | Negative cashflow for investment |
| Distribution kept in cash | Internal investment result | Positive cashflow |
| Distribution withdrawn | External outflow after investment result | Positive cashflow for investment |
| NAV update | Valuation movement | Ending value or interim value |
| Secondary sale | Disposal | Positive exit cashflow |
| In-kind distribution | Internal asset conversion if retained | Positive distribution plus new asset treatment depending policy |

## 12. Contribution and attribution

Private markets can contribute to total portfolio return through:

- NAV appreciation/depreciation;
- income distributions;
- realised gains;
- FX movement;
- valuation restatements;
- write-downs/write-offs;
- secondary sale gains/losses.

Attribution is harder because look-through data is limited. Possible attribution levels:

| Level | Use |
|---|---|
| Asset class | Private equity, private credit, real estate, infrastructure, hedge funds |
| Strategy | Buyout, growth, VC, direct lending, core real estate, macro hedge fund |
| Manager | GP/manager contribution |
| Vintage | Vintage-year allocation and performance |
| Geography | Regional exposure |
| Sector | Sector exposure |
| Currency | FX attribution |
| Realised/unrealised | Split between distributions and residual NAV |

## 13. Risk analytics

Private-market risk cannot be measured only with daily volatility.

| Risk | Measurement idea |
|---|---|
| Illiquidity risk | Liquidity bucket, lock-up, redemption terms |
| Cash-call risk | Unfunded commitment, expected call schedule, stress call scenario |
| Valuation risk | Stale NAV days, level-3 exposure, manager-estimated percentage |
| Concentration risk | Manager, strategy, vintage, sector, geography, deal concentration |
| Leverage risk | Fund leverage or portfolio company leverage if available |
| Vintage risk | Exposure clustered in one vintage/cycle |
| FX risk | Fund currency vs client/reporting currency |
| Manager risk | Exposure by GP and fund family |
| J-curve risk | Early negative returns and fee load |
| Liquidity mismatch | Illiquid assets versus client liquidity needs |
| Default risk | Private credit defaults/non-accruals |
| Exit risk | IPO/M&A/refinancing environment |

## 14. Liquidity analytics

Recommended buckets:

| Bucket | Description |
|---|---|
| Daily/weekly | Listed/liquid alternatives |
| Monthly | Hedge fund or semi-liquid fund with monthly dealing |
| Quarterly | Common hedge/private evergreen redemption |
| Annual | Annual redemption or long notice |
| Locked/closed-end | No ordinary redemption |
| Transfer only | Liquidity via secondary sale/GP consent |
| Suspended/gated | Redemption currently restricted |

Also show:

```text
Current NAV by liquidity bucket
Unfunded commitment by expected call period
Expected distributions by period if available
```

## 15. Cashflow forecasting

For advisory and buying power, forecast:

- expected capital calls over 12/24/36 months;
- stressed capital calls;
- expected distributions;
- unfunded obligations;
- recallable distributions;
- maturity/harvest period;
- currency of future calls/distributions.

Example stress:

```text
Assume 40% of unfunded commitment called within 12 months
Assume no distributions during stress
Check portfolio liquidity and borrowing availability
```

## 16. Reporting metrics dashboard

A strong private-markets report should include:

| Section | Metrics |
|---|---|
| Commitment summary | Commitment, paid-in, unfunded, recallable |
| Valuation | NAV, NAV date, NAV status, stale flag |
| Performance | IRR, TVPI, DPI, RVPI, PME if available |
| Cashflows | Calls and distributions by period |
| Exposure | Strategy, manager, vintage, geography, sector |
| Liquidity | Lock-up, redemption terms, liquidity buckets |
| Risk | Concentration, unfunded stress, FX, stale NAV |
| Income | Income vs ROC vs gain distributions |
| Pipeline | Upcoming calls, redemptions, extensions |

## 17. Data quality controls

| Control | Purpose |
|---|---|
| Paid-in <= commitment + recallable/recycling allowance | Prevent impossible balances |
| Unfunded >= 0 unless overcall allowed | Prevent negative commitments |
| NAV date <= report date | Prevent future valuations |
| NAV stale flag | Highlight old valuations |
| Distribution classification sum = gross distribution | Cashflow integrity |
| Tax + net = gross where applicable | Settlement integrity |
| IRR cashflows reconcile to ledger | Performance integrity |
| FX conversion uses correct rate date | Multi-currency integrity |
| Commitment currency vs reporting currency stored separately | Avoid conversion errors |
| Restated NAV creates audit trail | Avoid silent history changes |

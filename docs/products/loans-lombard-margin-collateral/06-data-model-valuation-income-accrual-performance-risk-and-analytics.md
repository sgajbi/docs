# 06 - Data Model, Valuation, Income Accrual, Performance, Risk and Analytics

## 1. Domain model overview

A lending/collateral platform should model the following bounded concepts:

```text
Client / Entity
Account / Portfolio
Credit Facility
Loan Drawdown / Liability Position
Collateral Pool
Pledge Relationship
Collateral Position
Exposure
Availability / Buying Power
Margin Call / Shortfall
Interest Accrual
Fees
Lifecycle Events
Transactions
Risk Metrics
Reports
```

## 2. Core facility table

| Field | Notes |
|---|---|
| credit_line_id | Unique line/facility |
| borrower_id | CIF/BR/entity |
| facility_type | Lombard, margin, overdraft, term loan |
| limit_amount | Approved limit |
| limit_currency | Credit line currency |
| collateral_pool_id | Linked pool |
| committed_flag | Committed/uncommitted |
| revolving_flag | Redraw allowed |
| start_date | Effective date |
| maturity_date | Expiry |
| base_rate_index | SOFR/SORA/Euribor/base rate |
| spread_bps | Client spread |
| floor_rate | Optional |
| cap_rate | Optional |
| day_count | ACT/360 etc. |
| interest_payment_frequency | Monthly/quarterly/etc. |
| margin_call_threshold | Policy threshold |
| liquidation_threshold | Policy threshold |
| status | Active/suspended/closed |

## 3. Loan drawdown table

| Field | Notes |
|---|---|
| loan_id | Drawdown/liability ID |
| credit_line_id | Parent facility |
| currency | Borrowing currency |
| principal_outstanding | Current principal |
| accrued_interest | Unpaid accrued interest |
| fees_outstanding | Charged unpaid fees |
| all_in_rate | Current rate |
| rate_reset_date | Next reset |
| value_date | Drawdown date |
| maturity_date | Drawdown maturity |
| repayment_style | Bullet/amortizing/revolving |
| status | Active/repaid/defaulted |

## 4. Collateral pool table

| Field | Notes |
|---|---|
| collateral_pool_id | Unique pool |
| pool_owner_id | Collateral owner or relationship group |
| pool_currency | Reporting/base currency |
| valuation_date | As-of date |
| total_market_value | Gross collateral market value |
| eligible_market_value | Eligible portion |
| lending_value | After LTV/haircuts |
| reserved_lending_value | Consumed by orders/exposure |
| available_lending_value | Unused lending value |
| status | Active/frozen/released |

## 5. Collateral holding table

| Field | Notes |
|---|---|
| collateral_position_id | Specific pledged holding |
| portfolio_id | Source portfolio |
| instrument_id | Security/cash asset |
| quantity | Holding quantity |
| market_price | Latest price |
| market_value | Value in local currency |
| fx_rate | To facility/pool currency |
| market_value_base | Base value |
| eligibility_status | Eligible/ineligible/restricted |
| base_ltv | Policy LTV |
| adjusted_ltv | After overlays |
| lending_value | Market value x adjusted LTV |
| encumbrance_status | Pledged/blocked/reserved |
| price_quality | Official/stale/manual/missing |

## 6. Pledge relationship table

| Field | Notes |
|---|---|
| pledge_id | Unique pledge |
| provider_entity_id | Collateral provider |
| provider_portfolio_id | Optional portfolio scope |
| beneficiary_entity_id | Borrower/beneficiary |
| credit_line_id | Optional facility-specific pledge |
| pledge_scope | Entity/portfolio/instrument/amount |
| max_support_amount | Cap if any |
| priority_rank | Priority order |
| transitive_allowed | Default false |
| effective_date | Start |
| expiry_date | End |
| legal_status | Draft/effective/released/disputed |

## 7. Exposure table

| Field | Notes |
|---|---|
| exposure_id | Unique exposure record |
| credit_line_id | Facility |
| borrower_id | Borrower |
| exposure_type | Principal, interest, FX, OTC, order, settlement |
| amount | Exposure amount |
| currency | Exposure currency |
| base_amount | Converted amount |
| status | Booked/reserved/pending/cancelled |
| source_system | Loan system, trading, OTC, OMS |
| effective_datetime | Exposure effective time |
| expiry_datetime | For reservations |

## 8. Availability snapshot table

| Field | Notes |
|---|---|
| snapshot_id | Unique calc run |
| as_of_datetime | Calculation timestamp |
| credit_line_id | Facility |
| collateral_lending_value | Total eligible lending value |
| exposure_booked | Booked exposure |
| exposure_reserved | Reserved/in-flight exposure |
| facility_headroom | Limit minus exposure |
| collateral_headroom | Lending value minus exposure |
| available_credit | Lower of facility/collateral headroom |
| buying_power | Final tradeable capacity after controls |
| margin_call_status | Normal/warning/call/liquidation |
| calc_version | Rules version |

## 9. Valuation of loan liabilities

For client reporting, loan liabilities are usually shown at outstanding principal plus accrued interest.

```text
Loan Carrying Value = Principal Outstanding + Accrued Interest + Fees Outstanding
```

For fair-value reporting, a loan can be valued by discounting future cashflows using current rates and client credit spread. However, many wealth statements use contractual outstanding balance rather than fair value.

## 10. Collateral valuation

Collateral is valued according to asset type:

| Collateral | Valuation basis |
|---|---|
| Cash | Balance x FX |
| Equity | Market close or real-time price x quantity |
| Bond | Clean/dirty price x nominal, plus accrued if applicable |
| Fund | Latest NAV x units |
| Structured note | Issuer/evaluated/model price x nominal |
| Private market fund | Latest reported NAV, often lagged |
| Property | Appraisal value, periodic refresh |

For lending value, market value alone is not enough. Apply eligibility, LTV, haircuts and caps.

## 11. Interest accrual engine

Inputs:

- principal outstanding
- rate index value
- spread
- floor/cap
- day-count convention
- accrual period
- compounding/capitalization rules
- holiday/calendar treatment
- currency precision

Formula:

```text
Interest = Principal x Rate x Days / DayCountBasis
```

Floating-rate formula:

```text
All-in Rate = max(floor, min(cap, Base Rate + Spread))
```

## 12. Performance treatment

Lending affects portfolio analytics in multiple ways.

| Item | Performance treatment |
|---|---|
| Loan drawdown | Financing cashflow/liability creation; not investment gain |
| Loan repayment | Liability reduction; not investment loss |
| Interest paid | Financing expense |
| Fees paid | Expense |
| Collateral pledged | No performance effect by itself |
| Collateral released | No performance effect by itself |
| Forced sale | Realized P&L on sold asset |
| FX movement on loan | Liability FX P&L if reporting in another currency |
| Margin call | No direct performance effect until cure transaction |

## 13. Gross, net and leveraged performance

Reports may show:

| View | Meaning |
|---|---|
| Asset-only performance | Performance of investments ignoring financing |
| Net-of-liability performance | Includes loan liability and financing costs |
| Net-worth performance | Assets minus liabilities |
| Leveraged return | Return on client equity after borrowing |
| Gross exposure return | Return based on total assets controlled |

Example:

| Item | Amount |
|---|---:|
| Client equity | 600,000 |
| Loan | 400,000 |
| Invested assets | 1,000,000 |
| Asset return | 5% = 50,000 |
| Interest cost | 20,000 |
| Net gain | 30,000 |
| Return on client equity | 30,000 / 600,000 = 5% |
| Asset-only return | 5% |

If assets fall 10%:

| Item | Amount |
|---|---:|
| Asset loss | -100,000 |
| Interest cost | -20,000 |
| Net loss | -120,000 |
| Return on client equity | -120,000 / 600,000 = -20% |

This is why leverage magnifies outcomes.

## 14. Risk metrics

| Metric | Meaning |
|---|---|
| LTV ratio | Exposure / collateral market value |
| Coverage ratio | Lending value / exposure |
| Excess collateral | Lending value - exposure |
| Margin buffer | Distance to margin call threshold |
| Liquidation buffer | Distance to liquidation threshold |
| Leverage ratio | Gross assets / net equity |
| Debt-to-portfolio value | Loan / portfolio value |
| Interest coverage | Income or liquidity / interest cost |
| Collateral concentration | Largest issuer/asset share of collateral |
| FX mismatch | Loan currency versus collateral currency |
| Stressed shortfall | Shortfall under scenario |
| Liquidity score | Ability to sell collateral quickly |
| Wrong-way risk score | Correlation between borrower risk and collateral |

## 15. Stress analytics

Stress scenarios:

| Scenario | Example |
|---|---|
| Equity shock | Equities -20% |
| Bond shock | Rates +100 bps, spreads +150 bps |
| FX shock | Collateral currency -10% vs loan currency |
| Concentration shock | Largest holding -35% |
| Liquidity shock | LTV reduced by 20% for illiquid assets |
| Rate shock | Borrowing cost +200 bps |
| Multi-asset crisis | Combined equity, credit spread and FX shock |

Stress output:

| Output | Meaning |
|---|---|
| Stressed collateral value | Market value after shock |
| Stressed lending value | LTV-adjusted value after shock |
| Stressed availability | Availability after shock |
| Shortfall | Exposure above stressed lending value |
| Required cure amount | Cash/asset needed |
| Assets likely to be sold | Liquidation simulation |

## 16. Attribution and contribution

Lending introduces financing effects.

Break performance contribution into:

- asset return contribution
- income contribution
- FX contribution
- financing cost contribution
- liability FX contribution
- leverage effect
- forced-sale effect

Example reporting decomposition:

| Component | Contribution |
|---|---:|
| Equity price return | +4.2% |
| Bond income | +1.1% |
| FX impact | -0.5% |
| Loan interest cost | -0.8% |
| Leverage amplification | +1.4% |
| Net portfolio return | +5.4% |

## 17. Buying-power analytics

Dashboard metrics:

| Metric | Purpose |
|---|---|
| Available cash | Cash that can settle purchases |
| Available credit | Borrowing headroom |
| Buying power | Tradable capacity after controls |
| In-flight reservation | Pending orders consuming capacity |
| Collateral lending value | Asset support after LTV |
| Collateral utilization | Exposure / lending value |
| Top collateral contributors | Which assets support availability |
| Top availability consumers | Which orders/exposures consume capacity |
| Stress availability | Capacity after market fall |

## 18. Reconciliation

Reconcile against:

- loan system principal outstanding
- interest accrual system
- custody collateral holdings
- market price source
- credit limit system
- pledge/legal document system
- order management system reservations
- general ledger cash and liability balances
- margin-call workflow system

## 19. Auditability

Every availability number should be explainable.

Example drill-down:

```text
Available credit = 225,000
  Approved limit = 1,000,000
  Booked exposure = 600,000
  Reserved exposure = 50,000
  Facility headroom = 350,000
  Collateral lending value = 875,000
  Collateral headroom = 225,000
  Final availability = min(350,000, 225,000)
```

## 20. Golden rule

For lending analytics, do not store only final availability. Store the components and rule version. Otherwise users cannot explain business outcomes, resolve disputes, or reproduce historical calculations.

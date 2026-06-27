# 03 — Fund Data Model, NAV, Valuation, Fees and Risk

## 1. Core Data-Modelling Principle

A fund platform should separate:

```text
Fund family / fund house
Umbrella fund
Sub-fund / compartment
Share class
Client position
Fund NAV / price
Fund distribution
Fund holdings / look-through
Fund transactions
Fund risk and suitability attributes
```

The investable object is normally the **share class**, because currency, NAV, fee, hedging, distribution policy and eligibility are share-class specific.

---

## 2. Recommended Conceptual Model

```text
FundHouse
  └── FundUmbrella
        └── SubFund
              └── ShareClass / Instrument
                    ├── NAVHistory
                    ├── DistributionSchedule
                    ├── DealingTerms
                    ├── FeeTerms
                    ├── RiskClassification
                    ├── HoldingsLookThrough
                    └── ClientPosition
```

Avoid modelling only one flat `fund` table. Real fund products have multiple levels.

---

## 3. Instrument and Share-Class Master

| Field | Example | Nullable? |
|---|---|---|
| instrument_id | FUND_SC_123 | No |
| security_type | FUND | No |
| fund_type | MUTUAL_FUND / UNIT_TRUST / ETF / HEDGE_FUND / PRIVATE_FUND | No |
| legal_form | SICAV / OEIC / UNIT_TRUST / VCC / LP / TRUST / CORPORATION | Nullable |
| fund_house_id | PIMCO / BlackRock / Fidelity | No |
| umbrella_id | Global Funds Umbrella | Nullable |
| sub_fund_id | Global Income Fund | Nullable |
| share_class_name | A USD Acc | No |
| isin | LUxxxx / SGxxxx | Usually no, but may be null for private funds |
| ticker | ETF ticker | Nullable |
| exchange_mic | XSES / XNYS | Nullable |
| base_currency | USD | No |
| share_class_currency | SGD / USD / EUR | No |
| pricing_currency | SGD / USD | No |
| distribution_policy | ACCUMULATING / DISTRIBUTING | No |
| hedging_flag | true/false | No |
| hedged_currency | SGD | Nullable |
| investor_category | RETAIL / ACCREDITED / INSTITUTIONAL / PROFESSIONAL | Nullable |
| domicile | Luxembourg / Singapore / Ireland | Nullable |
| registration_country | Singapore / Hong Kong / etc. | Nullable |
| launch_date | 2020-01-01 | Nullable |
| termination_date | null | Nullable |
| status | ACTIVE / SUSPENDED / TERMINATED | No |

---

## 4. Fund Strategy and Classification Model

| Field | Example |
|---|---|
| asset_class | Equity / Fixed Income / Multi-Asset / Alternative / Money Market |
| strategy | Global Equity / High Yield / Multi-Asset Income |
| benchmark_id | MSCI World / Bloomberg Global Agg |
| active_passive | ACTIVE / PASSIVE / ENHANCED_INDEX |
| geography | Global / Asia / US / Europe |
| sector | Technology / Healthcare / Financials |
| style | Growth / Value / Quality / Income |
| duration_bucket | Short / Intermediate / Long for bond funds |
| credit_quality_bucket | IG / HY / Mixed |
| liquidity_bucket | Daily / Weekly / Monthly / Quarterly / Locked |
| complexity_flag | Simple / Complex / Alternative |
| leverage_allowed | true/false |
| derivative_usage | Hedging / Efficient portfolio management / Investment / Leverage |
| ESG_classification | Internal or regulatory classification if applicable |

This classification drives:

- Search/filtering
- Advisory product selection
- Suitability
- Asset allocation
- Concentration risk
- Reporting
- Performance grouping
- Benchmark mapping

---

## 5. Dealing Terms Model

| Field | Example |
|---|---|
| subscription_allowed | true |
| redemption_allowed | true |
| dealing_frequency | DAILY / WEEKLY / MONTHLY / QUARTERLY |
| dealing_calendar | Fund-specific calendar |
| subscription_cutoff_time | 15:00 Singapore time |
| redemption_cutoff_time | 15:00 Singapore time |
| nav_lag | T / T+1 / T+2 |
| settlement_cycle_subscription | T+3 |
| settlement_cycle_redemption | T+3 |
| min_initial_subscription | 10,000 |
| min_subsequent_subscription | 1,000 |
| min_redemption_amount | 1,000 |
| min_holding_amount | 1,000 |
| decimal_units_allowed | true |
| unit_precision | 4 / 6 / 8 decimals |
| amount_precision | 2 decimals |
| redemption_notice_period | 30 days for hedge/private funds |
| lockup_period | 1 year |
| gate_terms | 25% fund-level gate |
| suspension_status | Active/Suspended |

---

## 6. NAV and Pricing

NAV stands for **Net Asset Value**.

Generic formula:

```text
NAV per Unit = (Total Fund Assets - Fund Liabilities) / Units Outstanding
```

For an investor position:

```text
Market Value = Units Held × NAV per Unit
```

For ETFs and closed-end funds:

```text
Market Value = Shares Held × Exchange Market Price
Premium/Discount = (Market Price - NAV per Share) / NAV per Share
```

### NAV Types

| NAV Type | Meaning |
|---|---|
| Official NAV | Final NAV published by fund administrator. |
| Estimated NAV | Preliminary or estimated NAV, common for hedge/private funds. |
| Dealing NAV | NAV used for subscriptions/redemptions. |
| Bid NAV | Price used for redemptions, if bid/offer pricing. |
| Offer NAV | Price used for subscriptions, if bid/offer pricing. |
| Swing NAV | NAV adjusted for transaction costs caused by subscriptions/redemptions. |
| Stale NAV | Last known NAV when new NAV is delayed/unavailable. |
| Indicative NAV / iNAV | Intraday indicative NAV, common for ETFs. |
| Adjusted NAV | NAV adjusted by platform/valuer due to fair value or stale data. |

### NAV Record Fields

| Field | Meaning |
|---|---|
| instrument_id | Share class/instrument. |
| nav_date | Date NAV applies to. |
| nav_value | NAV per unit/share. |
| nav_currency | NAV currency. |
| nav_type | Official / estimated / stale / adjusted / swing. |
| bid_price | If available. |
| offer_price | If available. |
| mid_price | If available. |
| source | Fund admin / vendor / exchange / custodian / manual. |
| received_timestamp | When platform received NAV. |
| valuation_status | Final / estimated / corrected / cancelled. |
| correction_reference_id | Link to prior NAV if restated. |

---

## 7. Forward Pricing

Open-ended funds usually use forward pricing. The investor does not know the exact NAV at order entry.

Example:

| Step | Date/Time |
|---|---|
| Client submits order | Monday 10:00 |
| Fund cut-off | Monday 15:00 |
| NAV date | Monday close |
| NAV published | Tuesday morning |
| Trade confirmed | Tuesday |
| Settlement | Thursday |

Platform implication:

```text
Order amount and expected units are estimates until NAV confirmation.
Confirmed transaction should use official dealing NAV.
```

---

## 8. Bid/Offer Pricing and Swing Pricing

Some funds use different prices for subscription and redemption.

| Price | Use |
|---|---|
| Offer price | Price at which investor subscribes. |
| Bid price | Price at which investor redeems. |
| NAV/mid price | Reference valuation. |
| Swing price | NAV adjusted to allocate transaction costs to transacting investors. |

Example:

| Item | Value |
|---|---:|
| Mid NAV | 10.00 |
| Offer price | 10.05 |
| Bid price | 9.95 |

Subscription of SGD 100,000:

```text
Units = 100,000 / 10.05 = 9,950.248756 units
```

Redemption of 9,950.248756 units:

```text
Cash = 9,950.248756 × 9.95 = 99,005.00
```

The spread is an economic cost.

---

## 9. Fee Model

Fund fees can exist at multiple layers.

| Fee Type | Paid By | How Modelled |
|---|---|---|
| Management fee | Fund assets | Reflected in NAV/TER, usually not client transaction. |
| Trustee/custody/admin/audit fees | Fund assets | Reflected in NAV/TER. |
| Total expense ratio / ongoing charges | Fund assets | Reflected in NAV and performance. |
| Performance fee | Fund assets | Reflected in NAV; may need disclosure. |
| Front-end sales charge | Client | Transaction fee/load at subscription. |
| Back-end/deferred sales charge | Client | Transaction fee/load at redemption. |
| Platform fee | Client/platform | Explicit fee or embedded/rebate arrangement. |
| Advisory fee | Client | Explicit portfolio/account fee. |
| Trailer fee | Fund manager/distributor arrangement | May be disclosed/rebated depending on policy. |
| Switching fee | Client | Explicit switch fee. |
| Redemption fee | Client or fund | Transaction charge or anti-dilution levy. |
| Anti-dilution levy | Fund/subscriber/redeemer | May be embedded in dealing price or explicit fee. |

Important modelling rule:

```text
Do not double count fund expenses.
If management fee and TER are already reflected in NAV, do not also subtract them as client transactions.
```

Explicit client charges should be booked as transactions. Fund-level expenses are usually reflected inside NAV.

---

## 10. Sales Charge and Net Investment Amount

Example:

| Item | Value |
|---|---:|
| Client cash paid | 100,000 |
| Front-end sales charge | 2,000 |
| Net invested | 98,000 |
| NAV | 10 |
| Units | 9,800 |

Two valid models:

### Model A — Explicit fee leg

| Leg | Amount |
|---|---:|
| Subscription | -98,000 |
| Sales charge | -2,000 |
| Total cash | -100,000 |

### Model B — Embedded fee in transaction

| Field | Amount |
|---|---:|
| gross_amount | 100,000 |
| fee_amount | 2,000 |
| net_investment_amount | 98,000 |
| net_cash_amount | -100,000 |

Both are acceptable. Model A is more transparent for reporting and fee analytics. Model B is simpler for accounting integration.

---

## 11. Distribution Model

Distribution fields:

| Field | Meaning |
|---|---|
| distribution_id | Unique event. |
| instrument_id | Fund share class. |
| ex_date | Date from which units no longer carry entitlement. |
| record_date | Date holder eligibility is determined. |
| payable_date | Payment date. |
| distribution_per_unit | Cash per unit. |
| currency | Distribution currency. |
| distribution_type | Income / capital gain / return of capital / equalisation / mixed. |
| reinvestment_allowed | Yes/no. |
| reinvestment_price | NAV used for reinvestment. |
| withholding_tax_rate | If applicable. |
| source | Fund administrator/custodian. |

Distribution impact:

| Share Class | Distribution Impact |
|---|---|
| Accumulating | Usually no cash distribution; value retained in NAV. |
| Distributing cash | Cash paid to client/account. |
| Distributing reinvest | Income entitlement reinvested into more units. |

---

## 12. Return of Capital

A distribution may be classified partly or fully as **return of capital**.

This is not the same as investment income.

Example:

| Distribution | Classification |
|---:|---|
| SGD 1,000 | 600 income + 400 return of capital |

Platform treatment depends on accounting/performance policy:

| Treatment | Meaning |
|---|---|
| Income treatment | Entire distribution treated as investment income. Simpler but may be misleading. |
| Component treatment | Income part as income, return-of-capital part reduces cost basis. More accurate. |
| Unknown classification | Book as distribution pending classification, adjust later. |

---

## 13. Valuation Sources

| Fund Type | Preferred Valuation Source |
|---|---|
| Mutual fund / unit trust | Official NAV from fund administrator/vendor/custodian. |
| ETF | Exchange price for market value; NAV/iNAV for analytics. |
| Listed closed-end fund | Exchange price for market value; NAV for premium/discount. |
| Hedge fund | Administrator NAV, often estimated/final with lag. |
| Private fund | GP/administrator reported NAV, quarterly or periodic. |
| Money market fund | Official NAV or amortized/stable NAV based on product structure. |
| Suspended fund | Last NAV with stale/suspended flag, or fair-value adjustment policy. |

Valuation hierarchy:

```text
1. Official market/exchange price where applicable
2. Official fund administrator NAV
3. Custodian/vendor NAV
4. Estimated NAV
5. Stale NAV with disclosure
6. Internal fair-value adjustment/manual valuation under governance
```

---

## 14. Valuation Frequency

| Fund Type | Typical Frequency |
|---|---|
| ETF | Intraday exchange price; daily NAV. |
| Daily mutual fund/unit trust | Daily NAV. |
| Weekly/monthly dealing fund | Weekly/monthly NAV. |
| Hedge fund | Monthly or quarterly, often with lag. |
| Private equity/private credit fund | Quarterly, sometimes delayed. |
| Suspended/side-pocket fund | Irregular or stale NAV. |

Do not assume every fund has daily valuation.

---

## 15. Look-Through Exposure

Fund look-through means analyzing the underlying holdings of the fund.

Common look-through dimensions:

| Dimension | Example |
|---|---|
| Asset class | Equity, bond, cash, alternative. |
| Geography | US, Europe, Asia, Emerging Markets. |
| Sector | Technology, financials, healthcare. |
| Currency | USD, EUR, JPY, SGD. |
| Credit quality | AAA, AA, BBB, HY. |
| Duration | Short, medium, long. |
| Issuer | Concentration by company/government. |
| Market cap | Large, mid, small. |
| ESG | Sustainability classification. |

Look-through methods:

| Method | Description |
|---|---|
| Full holdings | Security-level holdings from fund. Most accurate. |
| Top holdings | Use only top positions. Partial. |
| Asset allocation breakdown | Use published percentage breakdowns. |
| Benchmark proxy | Use benchmark composition as proxy. |
| Fund classification | Use broad category where no holdings available. |

Important limitations:

- Holdings may be delayed.
- Fund may not disclose full holdings.
- Fund may use derivatives/leverage.
- Fund-of-funds requires recursive look-through.
- Multiple funds may hold same underlying issuer, creating hidden concentration.

---

## 16. Look-Through Exposure Calculation

Client holds SGD 100,000 in Fund A.

Fund A allocation:

| Asset Class | Weight |
|---|---:|
| Equity | 60% |
| Bond | 35% |
| Cash | 5% |

Look-through exposure:

| Asset Class | Exposure |
|---|---:|
| Equity | 60,000 |
| Bond | 35,000 |
| Cash | 5,000 |

Formula:

```text
Look-through Exposure = Client Fund Market Value × Fund Holding Weight
```

For fund-of-funds:

```text
Client Exposure = Client Fund MV × Fund A weight in Fund B × Fund B underlying weight
```

Use effective dates carefully.

---

## 17. Fund Performance

Fund return is usually calculated from NAV and distributions.

### Price Return

```text
Price Return = (Ending NAV - Beginning NAV) / Beginning NAV
```

### Total Return with Distribution

```text
Total Return = (Ending NAV + Distribution per Unit - Beginning NAV) / Beginning NAV
```

If distribution is reinvested, total return should assume reinvestment according to fund methodology.

For client portfolio performance:

```text
Fund contribution = position weight × fund holding-period return
```

But client return also depends on:

- Subscription timing
- Redemption timing
- Fees/sales charges
- FX conversion
- Distributions retained or withdrawn
- Tax withholding
- Stale or corrected NAVs

---

## 18. Fund Risk Metrics

Common metrics:

| Metric | Meaning |
|---|---|
| Volatility | Variability of fund returns. |
| Sharpe ratio | Excess return per unit of total risk. |
| Sortino ratio | Excess return per downside risk. |
| Maximum drawdown | Worst peak-to-trough fall. |
| Beta | Sensitivity to benchmark. |
| Tracking error | Volatility of active return vs benchmark. |
| Information ratio | Active return per unit of tracking error. |
| Alpha | Return unexplained by benchmark exposure. |
| Duration | Interest-rate sensitivity for bond funds. |
| Credit spread duration | Credit-spread sensitivity. |
| Yield to maturity | Bond portfolio yield estimate. |
| Distribution yield | Distributions relative to price/NAV. |
| VaR / CVaR | Tail-risk estimate. |
| Liquidity score | Ability to redeem/sell under stress. |

---

## 19. Fund Risk Classification

A platform may classify funds using several dimensions.

| Dimension | Examples |
|---|---|
| Product complexity | Simple / complex / alternative. |
| Liquidity | Daily / weekly / monthly / quarterly / locked. |
| Asset class risk | Money market, bond, equity, alternative. |
| Leverage | None / moderate / high. |
| Derivative use | Hedging only / investment / leverage. |
| Concentration | Broad diversified / concentrated / thematic. |
| Investor eligibility | Retail / accredited / professional. |
| Risk rating | Internal 1–5 or 1–7 scale. |
| Currency risk | Hedged / unhedged / multi-currency. |

Suitability rules should use these fields rather than relying only on product name.

---

## 20. Data Quality Controls

Fund data quality controls:

| Control | Purpose |
|---|---|
| NAV freshness check | Detect stale NAVs. |
| NAV movement tolerance | Detect large unexpected price movements. |
| Currency consistency | NAV currency must match share-class pricing currency. |
| Distribution reconciliation | Cash received equals units × distribution per unit less tax. |
| Unit balance reconciliation | Custodian units match platform units. |
| Pending order ageing | Identify stuck unconfirmed orders. |
| Duplicate NAV check | Avoid duplicate NAV for same date/source/type. |
| NAV restatement handling | Correct historical valuations/performance. |
| Dealing calendar validation | Orders use correct cut-off and NAV date. |
| Minimum holding validation | Prevent invalid residual balances. |
| Fee double-counting control | Avoid subtracting TER as explicit fee. |
| Look-through effective date check | Avoid using outdated holdings without warning. |

---

## 21. Recommended Data Tables

| Table | Purpose |
|---|---|
| fund_house | Asset manager/fund sponsor. |
| fund_umbrella | Umbrella/legal umbrella. |
| fund_subfund | Strategy/compartment. |
| instrument | Share-class investable instrument. |
| fund_share_class_terms | Currency, distribution, hedging, eligibility. |
| fund_dealing_terms | Cut-off, frequency, settlement, minimums. |
| fund_fee_terms | Sales charge, management fee, TER, rebate rules. |
| fund_nav_price | NAV/price history. |
| fund_distribution | Distribution events. |
| fund_holding_lookthrough | Holdings/allocation breakdown. |
| fund_risk_profile | Risk, liquidity, complexity and suitability flags. |
| client_fund_order | Subscription/redemption/switch orders. |
| transaction | Economic transaction header. |
| transaction_leg | Cash/security legs. |
| position | Client holdings. |
| tax_lot | Optional lot-level cost basis. |
| valuation | Client-level valuation records. |
| private_fund_commitment | Commitment, paid-in, unfunded, distributions. |

---

## 22. Example Common Fund Object

```json
{
  "instrumentId": "FUND_SC_123",
  "securityType": "FUND",
  "fundType": "UNIT_TRUST",
  "fundHouseId": "ASSET_MANAGER_X",
  "umbrellaName": "Global Funds Umbrella",
  "subFundName": "Global Income Fund",
  "shareClassName": "A SGD Hedged Distributing",
  "isin": "LU0000000000",
  "baseCurrency": "USD",
  "shareClassCurrency": "SGD",
  "distributionPolicy": "DISTRIBUTING",
  "hedging": {
    "hedged": true,
    "hedgedCurrency": "SGD"
  },
  "classification": {
    "assetClass": "FIXED_INCOME",
    "strategy": "GLOBAL_INCOME",
    "activePassive": "ACTIVE",
    "liquidityBucket": "DAILY",
    "complexity": "NON_COMPLEX"
  },
  "dealingTerms": {
    "dealingFrequency": "DAILY",
    "subscriptionCutoffTime": "15:00",
    "redemptionCutoffTime": "15:00",
    "subscriptionSettlementCycle": "T+3",
    "redemptionSettlementCycle": "T+3",
    "minInitialSubscription": "10000",
    "unitPrecision": 4
  },
  "feeTerms": {
    "frontEndSalesChargePct": "0.02",
    "managementFeePctPa": "0.0125",
    "totalExpenseRatioPctPa": "0.0160"
  },
  "valuation": {
    "preferredValuationSource": "OFFICIAL_NAV",
    "fallbackSource": "CUSTODIAN_NAV"
  }
}
```

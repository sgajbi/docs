# 02 — Credit Lines, Lombard Lending, LTV, Haircuts and Availability

## 1. Credit facility structure

A private-bank lending setup normally has these layers:

```text
Client / Borrower
  └── Credit Facility / Line
        ├── Approved Limit
        ├── Currency
        ├── Purpose Rules
        ├── Pricing Terms
        ├── Collateral Pool
        ├── Drawdowns / Loans
        ├── Exposure Calculation
        └── Availability / Buying Power
```

The facility is the contractual container. Drawdowns are actual utilization. Collateral determines secured availability.

## 2. Core credit line fields

| Field | Meaning |
|---|---|
| credit_line_id | Unique facility ID |
| borrower_id | Client / CIF / booking entity |
| facility_type | Lombard, overdraft, margin, mortgage, term loan |
| approved_limit | Bank-approved maximum borrowing amount |
| limit_currency | Currency of approved limit |
| availability_currency | Currency used for buying power display |
| secured_flag | Whether collateral-backed |
| committed_flag | Committed or uncommitted facility |
| revolving_flag | Whether repay/redraw allowed |
| start_date | Facility start |
| maturity_date | Facility expiry |
| interest_rate_type | Fixed, floating, base + spread |
| base_rate | SOFR, SORA, Euribor, bank base rate |
| spread | Client margin above base |
| purpose | Investment, liquidity, property, working capital |
| collateral_pool_id | Linked collateral pool |
| margin_call_threshold | Level where call is triggered |
| liquidation_threshold | Level where liquidation may happen |
| status | Approved, active, suspended, expired, closed |

## 3. Facility limit versus collateral limit

Availability is constrained by both credit approval and collateral.

```text
Facility Headroom = Approved Limit - Current Exposure
Collateral Headroom = Lending Value - Current Exposure
Available Credit = min(Facility Headroom, Collateral Headroom)
```

Example:

| Item | Amount |
|---|---:|
| Approved limit | USD 1,000,000 |
| Eligible collateral lending value | USD 750,000 |
| Current exposure | USD 400,000 |
| Facility headroom | USD 600,000 |
| Collateral headroom | USD 350,000 |
| Available credit | USD 350,000 |

## 4. Collateral eligibility

Not every holding is eligible collateral.

Eligibility may depend on:

- asset class
- instrument type
- issuer
- rating
- exchange/listing status
- liquidity
- price availability
- currency
- jurisdiction
- concentration
- product complexity
- mandate restrictions
- sanctions/restrictions
- whether holding is already pledged elsewhere
- whether asset is blocked, pending settlement or under corporate action

## 5. LTV grid

A simplified LTV grid:

| Asset type | Example LTV |
|---|---:|
| Major currency cash | 90–100% |
| Developed-market government bonds | 70–95% |
| Investment-grade corporate bonds | 50–85% |
| High-yield bonds | 20–60% |
| Large-cap listed equities | 40–70% |
| ETFs | 40–80% |
| Mutual funds | 30–75% |
| Structured notes | 0–60% depending on type |
| Private equity funds | 0–30% |
| Hedge funds | 0–50% depending liquidity |
| Concentrated single stock | Lower than standard equity LTV |

Actual values are policy-driven and bank-specific. The key design point is that LTV is not a static field only on asset class; it can be overridden by instrument, issuer, account, jurisdiction, concentration, liquidity, rating and risk events.

## 6. Haircut hierarchy

A robust platform should support hierarchical LTV/haircut rules.

Priority example:

1. instrument-specific override
2. issuer-specific override
3. product subtype override
4. rating bucket rule
5. asset-class rule
6. jurisdiction/listing rule
7. concentration adjustment
8. currency mismatch adjustment
9. stale-price / no-price override
10. manual credit override

Example:

| Rule layer | Effect |
|---|---:|
| Base LTV for large-cap equity | 65% |
| Concentration penalty | -10% |
| FX mismatch penalty | -5% |
| Stale price penalty | -20% |
| Final LTV | 30% |

## 7. Lending value calculation

Position-level lending value:

```text
Collateral Market Value = Quantity × Price × FX Rate
Base Lending Value = Collateral Market Value × LTV
Adjusted Lending Value = Base Lending Value - Encumbrances - Reservations
```

Portfolio-level lending value:

```text
Total Lending Value = Sum(Eligible Position Lending Values)
```

With concentration adjustment:

```text
Adjusted Lending Value = min(Position Lending Value, Concentration Cap)
```

## 8. Concentration caps

A collateral pool may be diversified on paper but dependent on one issuer, region or product type.

Common caps:

| Cap type | Example |
|---|---|
| Single issuer cap | Max 25% of lending value from one issuer |
| Single equity cap | Max 15% from one listed share |
| Sector cap | Max 40% technology collateral |
| Country cap | Max 50% emerging market collateral |
| Product cap | Max 20% structured notes |
| Currency cap | Max 30% non-loan-currency collateral |
| Illiquid asset cap | Max 10% private market/fund collateral |

## 9. Currency mismatch

If the loan is in USD and collateral is in EUR, SGD, JPY or another currency, the bank needs an FX haircut or stress.

```text
Collateral Value in Loan CCY = Collateral Market Value × FX Rate
FX-Adjusted Lending Value = Collateral Value × LTV × FX Haircut Factor
```

Example:

| Item | Value |
|---|---:|
| SGD collateral market value | SGD 1,000,000 |
| USD/SGD rate | 1.35 |
| USD collateral value | USD 740,741 |
| Base LTV | 70% |
| FX haircut | 8% |
| Lending value | 740,741 × 70% × 92% = USD 477,037 |

## 10. Current exposure

Current exposure may include more than drawn principal.

```text
Current Exposure = Drawn Principal
                 + Accrued Interest
                 + Fees Due
                 + OTC Positive Exposure
                 + Pending Settlement Exposure
                 + In-flight Order Reservation
                 + Other Reserved Exposure
```

Systems should track both:

- **booked exposure**, already posted to ledger; and
- **reserved exposure**, created by orders, approvals, pending trades or pending drawdowns.

## 11. Available-to-invest / buying power

A private-bank buying-power engine usually needs more than credit availability.

```text
Buying Power = Available Credit
             + Available Cash
             - Order Reservations
             - Settlement Reservations
             - Product-Specific Requirements
             - Suitability / Mandate Blocks
             - Concentration / Compliance Blocks
```

A client may have credit availability but still not be allowed to buy a product because of:

- product not eligible for leverage
- client not eligible for complex product
- mandate forbids leverage
- concentration breach
- insufficient cash for fees/tax
- collateral pool already stressed
- pledged assets not allowed to support that product

## 12. Intraday versus EOD availability

Availability can be calculated on different baselines.

| Baseline | Description | Pros | Cons |
|---|---|---|---|
| EOD | Prior-day official positions, collateral, exposure | Stable, reconciled | Does not reflect intraday activity |
| Intraday | Includes live trades, deposits, loans, market moves | Better control | Requires reliable event feeds |
| Hybrid | EOD baseline + intraday deltas | Practical for banks | Complex reconciliation |

For buying power, a common enterprise pattern is:

```text
Intraday Availability = EOD Collateral Lending Value
                      - EOD Exposure
                      + Confirmed Intraday Cash/Collateral Deltas
                      - In-flight Order Reservations
                      - Intraday Drawdowns/Repayments
                      - Risk Buffers
```

## 13. Maintenance, margin-call and liquidation thresholds

A lending product may have multiple thresholds:

| Threshold | Meaning |
|---|---|
| Initial LTV | Maximum borrowing at inception |
| Maintenance LTV | Ongoing safe threshold |
| Margin-call LTV | Level where client must act |
| Liquidation LTV | Level where bank may liquidate collateral |
| Stop-trading threshold | Level where new purchases are blocked |

Example:

| Metric | Threshold |
|---|---:|
| Initial LTV | 60% |
| Maintenance LTV | 65% |
| Margin-call LTV | 70% |
| Liquidation LTV | 80% |

If portfolio falls, the loan-to-collateral ratio rises.

## 14. Key formula: loan-to-value ratio

```text
Loan-to-Value = Total Exposure / Collateral Market Value
```

or after haircuts:

```text
Coverage Ratio = Lending Value / Total Exposure
```

Example:

| Item | Amount |
|---|---:|
| Collateral market value | USD 1,000,000 |
| Lending value at 60% LTV | USD 600,000 |
| Exposure | USD 500,000 |
| LTV ratio | 50% |
| Coverage ratio | 600,000 / 500,000 = 120% |

## 15. Stress testing availability

A platform should calculate stressed availability.

```text
Stressed Lending Value = Sum(Position MV × Stress Price Factor × FX Stress Factor × LTV)
Stressed Shortfall = Exposure - Stressed Lending Value
```

Example stresses:

| Asset | Stress |
|---|---:|
| Developed-market equity | -20% |
| Emerging-market equity | -35% |
| Investment-grade bond | -5% to -15% |
| High-yield bond | -20% |
| Structured product | -25% to -60% |
| FX mismatch | additional -5% to -15% |

## 16. Advisory lens

A client may ask: “How much can I borrow?”

A better advisor answer is:

- how much is approved?
- how much is currently available?
- what happens if the market falls 10%, 20%, 30%?
- what assets are pledged?
- what assets could be sold in a margin event?
- what is the interest cost?
- is borrowing aligned with the client’s risk profile and mandate?
- can the client meet a margin call without forced selling?

A platform should help advisors show this clearly.

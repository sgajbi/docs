# 05 - Loans, Lombard, Collateral, Margin and Buying Power Worked Examples

## 1. Simple Lombard availability

### Business scenario

Client has pledged portfolio market value USD 1,000,000. Average LTV 60%. Current loan exposure USD 400,000.

### Calculation

```text
Collateral lending value = 1,000,000 x 60% = 600,000
Availability = 600,000 - 400,000 = 200,000
```

### Reporting

| Metric | Value |
|---|---:|
| Pledged MV | 1,000,000 |
| Lending value | 600,000 |
| Exposure | 400,000 |
| Available credit | 200,000 |

### QA assertions

- Availability uses lending value, not raw market value.
- Exposure includes all relevant loans/OD/derivative exposure per policy.
- Currency conversion uses approved FX.

## 2. Haircut-based collateral value

### Business scenario

Portfolio has:

| Asset | Market value | Haircut |
|---|---:|---:|
| Equity | 300,000 | 40% |
| Bond | 500,000 | 20% |
| Cash | 200,000 | 0% |

### Calculation

```text
Equity lending value = 300,000 x (1 - 40%) = 180,000
Bond lending value = 500,000 x (1 - 20%) = 400,000
Cash lending value = 200,000
Total lending value = 780,000
```

### QA assertions

- Haircut and LTV are not inverted.
- Product-specific eligibility applied before haircut.
- Non-eligible assets have zero lending value.

## 3. In-flight order impact on buying power

### Business scenario

Available buying power USD 200,000. Client places buy order requiring USD 50,000.

### Calculation

```text
Post-order availability = 200,000 - 50,000 = 150,000
```

### QA assertions

- In-flight order reduces available buying power before settlement.
- Cancelled/rejected orders release availability.
- Partial fills reduce only filled/pending amount correctly.

## 4. One-way pledge A -> B

### Business scenario

BR A pledges support to BR B. B has own surplus USD 80,000. B places in-flight buy order USD 120,000.

### Rule

B consumes own surplus first. Only excess consumes shared support from A.

### Calculation

```text
Excess using A = 120,000 - 80,000 = 40,000
Impact to A = 40,000
```

### QA assertions

- B own surplus consumed first.
- A impact is not full 120,000.
- No transitive support unless explicitly supported.
- Shared asset availability is not double counted.

## 5. Cross-pledged portfolio within same BR

### Business scenario

BR A has portfolios A-10 and A-20.

- A-10 pledged to B credit line
- A-10 and A-20 pledged to A credit line
- A-10 is shared collateral

### Expected model

Represent collateral at pledge-link level:

| Portfolio | Pledged to | Role |
|---|---|---|
| A-10 | A line | own collateral |
| A-20 | A line | own collateral |
| A-10 | B line | shared/provider collateral |

### QA assertions

- A-10 lending value is not double-used without allocation logic.
- Credit-line availability considers pledge relationship.
- In-flight usage of shared collateral is traceable.
- Reporting explains shared collateral exposure.

## 6. Margin call

### Business scenario

Required collateral value USD 600,000. Actual collateral value after market fall USD 520,000.

```text
Shortfall = 600,000 - 520,000 = 80,000
```

### Expected workflow

```text
Shortfall detected
-> margin call event
-> client/advisor notified
-> cure deadline set
-> collateral top-up or exposure reduction
-> unresolved breach escalated
```

### QA assertions

- Margin call generated only after thresholds/rules.
- Cure period tracked.
- Liquidation workflow controlled and auditable.
- Client/advisor reporting shows shortfall.

## 7. Loan interest accrual

### Business scenario

Loan principal USD 500,000, interest 6% p.a., ACT/360, 30 days.

```text
Interest = 500,000 x 6% x 30 / 360 = 2,500
```

### Transaction

| Transaction | Amount |
|---|---:|
| LOAN_INTEREST_ACCRUAL | 2,500 |
| LOAN_INTEREST_PAYMENT | cash -2,500 |

### QA assertions

- Interest accrues daily or by schedule.
- Payment reduces accrued interest payable, not principal.
- Capitalized interest increases exposure if applicable.

## 8. Key reporting outputs

A lending report should show:

- facility limit
- outstanding exposure
- collateral market value
- lending value
- LTV
- availability
- shortfall
- margin call status
- pledged portfolios/assets
- in-flight order impact
- concentration and currency risk

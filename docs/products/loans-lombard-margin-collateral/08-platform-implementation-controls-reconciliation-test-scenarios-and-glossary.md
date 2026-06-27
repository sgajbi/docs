# 08 - Platform Implementation, Controls, Reconciliation, Test Scenarios and Glossary

## 1. Platform capability map

| Capability | Description |
|---|---|
| Credit line master | Facility limit, currency, status, terms |
| Loan position | Drawn exposure, interest, maturity |
| Collateral pool | Pledged assets and lending value |
| Pledge graph | Directional pledge relationships |
| LTV engine | Haircuts, eligibility, overrides |
| Exposure engine | Booked and reserved exposure |
| Buying-power engine | Available cash/credit/trade capacity |
| Interest engine | Accrual, payment, capitalization |
| Margin-call engine | Threshold detection and workflow |
| Liquidation workflow | Forced-sale modelling and audit |
| Reporting engine | Client, advisor, risk and ops views |
| Reconciliation engine | Loan/collateral/exposure/prices |
| Rule versioning | Explainable historical calculations |

## 2. Architecture pattern

Recommended services/modules:

```text
credit-line-service
loan-position-service
collateral-service
pledge-graph-service
ltv-rules-engine
exposure-service
buying-power-service
interest-accrual-service
margin-call-service
reporting-analytics-service
reconciliation-service
```

Keep the domain logic independent from UI, persistence and vendor feeds.

## 3. Canonical APIs

Useful APIs:

| API | Purpose |
|---|---|
| GET /credit-lines/{id} | Facility details |
| GET /credit-lines/{id}/availability | Availability drill-down |
| POST /buying-power/check | Pre-trade check |
| POST /buying-power/reservations | Reserve capacity for order |
| DELETE /buying-power/reservations/{id} | Release reservation |
| GET /collateral-pools/{id} | Collateral pool details |
| POST /collateral/substitution/check | Validate substitution |
| GET /margin-calls | Margin call worklist |
| POST /margin-calls/{id}/cure | Record cure action |
| GET /loans/{id}/accrued-interest | Interest detail |
| GET /reports/leverage-summary | Reporting summary |

## 4. Buying-power calculation flow

```text
Load borrower and facility
Load own collateral pools
Load pledge relationships
Load eligible holdings and prices
Apply LTV/haircut rules
Apply concentration and FX adjustments
Load booked exposure
Load reserved exposure / in-flight orders
Apply facility limit
Apply collateral limit
Apply mandate/suitability/product controls
Return availability with drill-down
```

## 5. Data quality controls

| Control | Reason |
|---|---|
| Price freshness check | Stale collateral values create wrong availability |
| FX freshness check | Currency mismatch can be material |
| LTV rule version check | Historical reproducibility |
| Missing rating check | Bond LTV may depend on rating |
| Missing maturity check | Bond/loan risk classification |
| Collateral duplicate check | Prevent double pledge/counting |
| Pledge effective-date check | Avoid using expired pledge |
| Facility status check | Suspended/expired lines cannot provide credit |
| Negative availability handling | Must trigger warning/call |
| In-flight order expiry | Avoid stuck reservations |

## 6. Reconciliation controls

| Reconciliation | Compare |
|---|---|
| Loan principal | Platform vs loan system/GL |
| Accrued interest | Platform vs loan accrual engine |
| Credit limit | Platform vs credit approval system |
| Collateral holdings | Platform vs custodian/portfolio book |
| Pledge status | Platform vs legal/collateral system |
| Prices | Platform vs market data/vendor/custodian |
| FX rates | Platform vs official FX source |
| Exposure | Platform vs trading/OTC/loan systems |
| Margin calls | Platform vs risk/operations workflow |
| Availability | Platform vs source/bank calculator if available |

## 7. Operational controls

| Control | Description |
|---|---|
| Maker-checker override | Manual LTV/pledge/limit changes require approval |
| Audit trail | Who changed what and why |
| Reason codes | Margin call, block, liquidation, override |
| Deterministic calculation | Same inputs produce same result |
| Idempotency | Replaying events does not duplicate transactions |
| Event ordering | Trades, prices, FX, pledge updates processed correctly |
| Backdated correction | Recalculate affected availability snapshots |
| Graceful degradation | If price missing, block or haircut according to policy |
| Explainability | Every blocked order has business-readable reason |

## 8. Common failure scenarios

| Failure | Impact |
|---|---|
| Stale collateral price | Overstated/understated availability |
| Missing in-flight reservation | Over-trading risk |
| Duplicate reservation | Client buying power understated |
| Wrong pledge direction | Unauthorized collateral support |
| Transitive pledge assumed | Legal/risk breach |
| Cross-currency haircut missing | FX risk understated |
| Corporate action not reflected | Wrong collateral quantity/value |
| Rating downgrade not applied | LTV too high |
| Collateral sold but pledge not released | Encumbrance mismatch |
| Loan repayment not reflected | Availability understated |
| Interest accrual missing | Exposure understated |

## 9. Test scenario catalogue

### Scenario 1: Simple Lombard availability

| Input | Value |
|---|---:|
| Collateral MV | 1,000,000 |
| LTV | 60% |
| Approved limit | 800,000 |
| Exposure | 300,000 |

Expected:

```text
Lending value = 600,000
Facility headroom = 500,000
Collateral headroom = 300,000
Availability = 300,000
```

### Scenario 2: Facility limit binding

| Input | Value |
|---|---:|
| Lending value | 900,000 |
| Approved limit | 700,000 |
| Exposure | 400,000 |

Expected availability = 300,000.

### Scenario 3: Collateral limit binding

| Input | Value |
|---|---:|
| Lending value | 600,000 |
| Approved limit | 1,000,000 |
| Exposure | 400,000 |

Expected availability = 200,000.

### Scenario 4: In-flight buy order

| Input | Value |
|---|---:|
| Availability before order | 200,000 |
| Order reservation | 80,000 |

Expected availability after reservation = 120,000.

### Scenario 5: A -> B own surplus first

| Input | Value |
|---|---:|
| B own surplus | 100,000 |
| B order | 160,000 |

Expected support consumed from A = 60,000.

### Scenario 6: No transitive support

Pledge graph:

```text
A -> B
B -> C
```

Expected: C cannot consume A collateral unless explicit A -> C pledge exists.

### Scenario 7: Margin call due to price fall

| Input | Value |
|---|---:|
| Collateral MV before | 1,000,000 |
| LTV | 60% |
| Exposure | 550,000 |
| Collateral MV after | 850,000 |

Expected:

```text
New lending value = 510,000
Shortfall = 40,000
Margin call triggered if threshold exceeded
```

### Scenario 8: Currency mismatch

| Input | Value |
|---|---:|
| EUR collateral value | 1,000,000 |
| EUR/USD | 1.10 |
| USD value | 1,100,000 |
| LTV | 60% |
| FX haircut | 10% |

Expected lending value = 1,100,000 x 60% x 90% = 594,000.

### Scenario 9: Loan interest accrual

| Input | Value |
|---|---:|
| Principal | 500,000 |
| Rate | 6% |
| Days | 30 |
| Day count | 360 |

Expected interest = 2,500.

### Scenario 10: Forced liquidation

Expected grouped transactions:

1. sell collateral security;
2. receive cash proceeds;
3. repay loan principal/interest;
4. book realized P&L;
5. update margin call status.

## 10. Migration considerations

During migration, reconcile:

- facility ID mapping
- borrower/entity mapping
- collateral pool mapping
- pledge direction and scope
- LTV rules and overrides
- outstanding principal
- accrued interest
- pledged holdings and quantities
- encumbrance flags
- margin call open cases
- in-flight orders/reservations
- historical interest transactions
- historical repayments/drawdowns

## 11. Production-support triage questions

When availability looks wrong, ask:

1. Is the facility active and within limit?
2. Is the collateral pledge effective?
3. Are prices and FX current?
4. Are all holdings eligible?
5. Did any rating/LTV/concentration rule change?
6. Are there in-flight orders/reservations?
7. Are exposure and interest up to date?
8. Is cross-pledge support being allocated correctly?
9. Is there a margin call or suspended status?
10. Was there a corporate action or trade settlement today?

## 12. Glossary

| Term | Meaning |
|---|---|
| Approved limit | Maximum facility amount approved by credit |
| Availability | Amount currently borrowable after exposure/collateral constraints |
| Buying power | Amount client can use for trading after cash, credit and controls |
| Collateral | Asset pledged to secure borrowing |
| Collateral pool | Group of pledged assets supporting exposure |
| Coverage ratio | Lending value divided by exposure |
| Credit line | Approved borrowing facility |
| Drawdown | Actual loan utilization |
| Encumbrance | Restriction on asset due to pledge/reservation/legal hold |
| Exposure | Amount owed or at risk |
| Haircut | Reduction applied to collateral market value |
| Initial margin | Minimum margin to open a position |
| LTV | Loan-to-value or lending-value percentage |
| Maintenance margin | Ongoing margin required to hold position |
| Margin call | Request to cure collateral/equity shortfall |
| Pledge | Legal/security relationship over collateral |
| Reservation | Temporary hold on availability for pending order/exposure |
| Shortfall | Amount by which exposure exceeds required coverage |
| Utilization | Exposure as a share of limit or lending value |

## 13. Practitioner-ready explanation

> Wealth lending is a secured borrowing arrangement where a client uses eligible portfolio assets as collateral. The bank applies LTVs or haircuts to calculate lending value, compares this with approved limits and current exposure, and derives available credit or buying power. The hard part is not the basic formula; it is modelling eligibility, pledge direction, cross-pledging, concentration, FX mismatch, in-flight orders, interest accrual, margin calls and forced liquidation in a way that is legally correct, explainable and auditable. For advisory and DPM, lending must be linked to suitability, leverage limits, mandate rules, stress testing, reporting and client understanding of margin-call risk.

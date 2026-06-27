# 07 — Platform Implementation, Controls, Reconciliation and Test Scenarios

## 1. Platform capabilities required

A derivatives-capable wealth platform requires more than basic trade capture.

| Capability | Why it matters |
|---|---|
| Product/contract master | Derivative terms drive payoff and risk |
| Underlying/reference data | Prices, fixings, rates, curves, credit data |
| Lifecycle engine | Expiry, exercise, assignment, reset, margin, settlement |
| Transaction engine | Premiums, margin, settlement, coupon, close-out |
| Position engine | Contracts, notional, MTM, exposures, lots/legs |
| Valuation engine | Prices, models, curves, volatility, Greeks |
| Risk engine | Delta, DV01, CS01, vega, VaR, stress |
| Margin/collateral engine | Margin calls, collateral movements, eligibility |
| Mandate/rules engine | Suitability and investment constraints |
| Reporting engine | Client/advisor/regulatory reporting |
| Reconciliation | Broker/custodian/dealer/valuation reconciliation |
| Audit | Full trace from source event to report output |

## 2. Event-driven architecture

Recommended event flow:

```text
Trade Capture
  → Contract Master Enrichment
  → Position Update
  → Premium/Margin/Settlement Posting
  → Lifecycle Schedule Creation
  → Valuation and Greeks
  → Exposure and Mandate Checks
  → Reporting and Reconciliation
```

Lifecycle event flow:

```text
Market Data / Fixing / Exchange Event / Dealer Notice
  → Lifecycle Event
  → Entitlement or Settlement Determination
  → Transaction Generation
  → Position Update
  → Valuation/Risk Refresh
  → Reconciliation and Reporting
```

## 3. Derivative lifecycle event types

| Lifecycle event type | Products |
|---|---|
| OPTION_EXPIRY | Options |
| OPTION_EXERCISE | Options |
| OPTION_ASSIGNMENT | Options |
| OPTION_CONTRACT_ADJUSTMENT | Options |
| FUTURE_EXPIRY | Futures |
| FUTURE_DELIVERY_NOTICE | Physical futures |
| FX_FORWARD_FIXING | FX forwards/NDFs |
| NDF_FIXING | NDFs |
| SWAP_RATE_RESET | Swaps |
| SWAP_PAYMENT_DETERMINATION | Swaps |
| SWAP_TERMINATION | Swaps |
| COLLATERAL_CALL | OTC derivatives |
| MARGIN_CALL | Exchange/broker derivatives |
| CREDIT_EVENT_TRIGGER | CDS/credit derivatives |
| CREDIT_EVENT_SETTLEMENT | CDS/credit derivatives |
| NOVATION | OTC derivatives |
| COMPRESSION | OTC derivatives |

## 4. Transaction design pattern

Use a header/leg model.

### Transaction header

| Field | Meaning |
|---|---|
| transaction_id | Unique ID |
| transaction_group_id | Groups related legs |
| account_id | Portfolio/account |
| product_family | OPTION/FUTURE/FORWARD/SWAP/CDS |
| transaction_type | Economic event type |
| trade_date | Trade date |
| value_date | Settlement/value date |
| booking_date | Booking date |
| lifecycle_event_id | Link to lifecycle event |
| source_system | Broker/custodian/dealer/exchange/internal |
| status | Pending/confirmed/reversed |

### Transaction leg

| Field | Meaning |
|---|---|
| leg_id | Unique leg |
| leg_type | SECURITY / CONTRACT / CASH / COLLATERAL / FEE / TAX |
| instrument_or_contract_id | Derivative or underlying |
| quantity_delta | Contract or security quantity |
| notional_delta | Notional change |
| cash_amount | Cash movement |
| currency | Currency |
| price_or_rate | Trade price/fixing/rate |
| performance_classification | Income, P&L, collateral, external flow |

## 5. Position design pattern

| Position layer | Purpose |
|---|---|
| Contract position | Legal derivative holding |
| Cash position | Premium, margin, settlement cash |
| Collateral position | Posted/received collateral |
| Underlying actual position | Created only if physical delivery/exercise occurs |
| Look-through exposure | Analytical exposure, not legal holding |
| Risk position | Delta/DV01/CS01/vega buckets |

## 6. Valuation controls

| Control | Description |
|---|---|
| Source hierarchy | Exchange > independent vendor > dealer > model > fallback |
| Stale price control | Flag if valuation older than allowed threshold |
| Model parameter control | Validate curve, vol surface, dividend and FX inputs |
| Dealer-vs-independent tolerance | Compare OTC valuation sources |
| Day-over-day move control | Explain large valuation changes |
| Greek completeness | Required Greeks by product type |
| Expired contract check | No active valuation after expiry without event |
| Wrong currency check | Valuation currency matches contract terms |
| Negative/zero price sanity | Allowed only where product supports it |

## 7. Reconciliation controls

| Reconciliation | Source comparison |
|---|---|
| Trade reconciliation | Internal trades vs broker/custodian/dealer |
| Position reconciliation | Contract quantity/notional vs custodian/broker |
| Cash reconciliation | Premium, margin, settlement cash vs cash ledger |
| Margin reconciliation | Internal margin vs broker margin statement |
| Collateral reconciliation | Posted collateral vs collateral statement |
| Valuation reconciliation | Internal valuation vs vendor/dealer/custodian |
| Lifecycle reconciliation | Expiry/exercise/reset events vs broker/dealer notices |
| P&L reconciliation | Daily P&L vs broker/custodian statements |

## 8. Common production incidents

| Incident | Likely cause | Fix/control |
|---|---|---|
| Option still active after expiry | Missing expiry event | Expiry sweep and exception queue |
| Wrong exposure for option | Contract multiplier missing/wrong | Contract master validation |
| Futures MV near zero but report shows no exposure | Reporting uses MV only | Use notional and delta-equivalent exposure |
| FX forward settlement wrong | Buy/sell leg direction reversed | Dual-currency leg validation |
| NDF cash settlement wrong | Fixing formula/source mismatch | Currency-pair convention control |
| Swap cashflow wrong | Day count/reset/fixing issue | Schedule and fixing reconciliation |
| Margin treated as loss | Classification error | Collateral/P&L classification rules |
| OTC valuation jump | Curve/vol/model/source change | Valuation explain and source audit |
| Mandate breach missed | Derivative not included in exposure | Risk-equivalent exposure integration |
| Physical delivery surprise | Expiry/delivery alerts missing | Delivery window controls |

## 9. QA test scenarios

### Options

1. Buy call, price rises, sell close and realize gain.
2. Buy put, expires worthless, realize premium loss.
3. Sell covered call, expiry OTM, premium retained.
4. Sell covered call, assignment creates equity sale.
5. Short naked call blocked by mandate rule.
6. Stock split adjusts strike and contract size.
7. Special dividend adjusts option strike.
8. Option missing underlying price is flagged.
9. 0DTE option exposure spikes due to gamma.
10. Option physically settles with fractional cash adjustment.

### Futures

1. Open long index future, initial margin posted.
2. Daily variation margin gain/loss booked.
3. Close future before expiry.
4. Contract rolls from front month to next month.
5. Physical commodity future approaching delivery triggers alert.
6. Futures notional exceeds mandate and is blocked.
7. Variation margin reconciles with broker statement.

### FX forwards/NDF

1. Open deliverable FX forward, no upfront cash.
2. Daily MTM valuation using forward curve.
3. Mature forward and exchange two currencies.
4. Close forward early and realize P&L.
5. Roll forward and preserve realized P&L on old contract.
6. NDF fixes and settles net cash.
7. Wrong fixing source triggers exception.

### Swaps/CDS

1. IRS opens with zero market value and non-zero DV01.
2. Floating reset updates projected cashflow.
3. Swap coupon net payment books correctly.
4. Partial termination reduces notional.
5. Collateral call posts cash without P&L impact.
6. CDS premium is paid periodically.
7. Credit event triggers protection payment/recovery workflow.
8. Counterparty valuation differs beyond tolerance.

## 10. API design considerations

Suggested endpoints:

| Endpoint | Purpose |
|---|---|
| `GET /derivatives/contracts/{id}` | Contract terms |
| `GET /portfolios/{id}/derivative-positions` | Current derivative positions |
| `GET /portfolios/{id}/derivative-exposures` | Notional, delta, DV01, CS01, vega exposure |
| `GET /portfolios/{id}/margin-collateral` | Margin/collateral usage |
| `GET /derivatives/{id}/lifecycle-events` | Expiry, reset, exercise, margin events |
| `POST /derivatives/lifecycle-events` | Ingest lifecycle events |
| `GET /portfolios/{id}/derivative-pnl` | P&L and P&L explain |
| `GET /portfolios/{id}/mandate-checks` | Limit checks including derivatives |

## 11. Implementation principles

- Use decimal precision for all monetary, price, rate, notional and Greek values.
- Store product terms independently of transactions.
- Store lifecycle events independently of transactions.
- Do not use market value as exposure.
- Support multi-leg transactions.
- Support contract-level and underlying-level risk views.
- Maintain valuation source and model audit trail.
- Classify collateral separately from investment P&L.
- Reconcile with broker/dealer/custodian statements daily.
- Keep enough history to explain any client report number.

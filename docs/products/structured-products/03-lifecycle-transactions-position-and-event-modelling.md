# 03 — Lifecycle, Transactions, Position and Event Modelling

## 1. Core principle

Structured products require separation between:

| Object | Meaning |
|---|---|
| Product terms | Contractual rules from termsheet/product master |
| Lifecycle events | Observations, fixings, barrier breaches, coupon determinations, autocall triggers |
| Transactions | Accounting/economic postings such as buy, sell, coupon, redemption, conversion |
| Positions | What the client legally holds after transactions |
| Valuations | Daily/intraday mark-to-market or estimated value |
| Analytics | Look-through exposures, scenario P&L, risk, Greeks and suitability metrics |

Do not create a transaction for every observation. Create a lifecycle event. Create a transaction only when there is an economic posting.

## 2. Generic lifecycle

| Stage | Description | Main system object |
|---|---|---|
| Product creation | Issuer/arranger creates termsheet | Instrument/product master |
| Offering | Product offered to eligible clients | Offer/order book |
| Subscription/order | Client places order | Order |
| Allocation/confirmation | Order accepted and allocated | Trade confirmation |
| Settlement | Cash paid and product received | Transaction + position |
| Holding period | Product marked and monitored | Valuation + lifecycle schedule |
| Observation/fixing | Underlying checked | Lifecycle event |
| Coupon determination | Coupon entitlement determined | Lifecycle event |
| Coupon payment | Coupon paid | Transaction |
| Barrier event | Barrier touched or observed | Lifecycle event + status update |
| Autocall/knock-out | Early redemption triggered | Lifecycle event |
| Early redemption settlement | Product closes early | Transaction |
| Secondary sale/early exit | Client exits before maturity | Transaction |
| Final fixing | Maturity payoff determined | Lifecycle event |
| Maturity settlement | Cash/security/alternate currency delivered | Transaction(s) |
| Default/credit event | Issuer/reference/counterparty event | Lifecycle event + loss/recovery transaction |
| Closure | Position fully closed | Position status |

## 3. Recommended transaction types

Use transaction types based on economic effect, not product marketing name.

| Transaction type | Use |
|---|---|
| STRUCTURED_SUBSCRIPTION | Primary market subscription |
| STRUCTURED_SECONDARY_BUY | Secondary market purchase |
| STRUCTURED_SECONDARY_SELL | Secondary market sale |
| STRUCTURED_TRANSFER_IN | Transfer in from another account/custodian |
| STRUCTURED_TRANSFER_OUT | Transfer out |
| STRUCTURED_COUPON_PAYMENT | Coupon/income paid |
| STRUCTURED_COUPON_ACCRUAL | Optional accrual for unconditional income |
| STRUCTURED_AUTOCALL_REDEMPTION | Early redemption due to autocall |
| STRUCTURED_KNOCKOUT_REDEMPTION | Redemption/termination due to knock-out |
| STRUCTURED_MATURITY_CASH_REDEMPTION | Cash redemption at maturity |
| STRUCTURED_PHYSICAL_REDEMPTION_OUT | Structured product closed into delivered asset |
| STRUCTURED_PHYSICAL_DELIVERY_IN | Delivered equity/bond/commodity/fund/security received |
| STRUCTURED_ALTERNATE_CCY_REDEMPTION | Redemption in alternate currency |
| STRUCTURED_PARTIAL_REDEMPTION | Partial redemption/paydown |
| STRUCTURED_ISSUER_CALL_REDEMPTION | Issuer calls product |
| STRUCTURED_INVESTOR_PUT_REDEMPTION | Investor exercises put/early redemption right |
| STRUCTURED_CREDIT_EVENT_WRITEDOWN | Loss/write-down from reference credit event |
| STRUCTURED_RECOVERY_PAYMENT | Recovery payment after credit event |
| STRUCTURED_DEFAULT_WRITEOFF | Issuer/counterparty default write-off |
| STRUCTURED_SETTLEMENT_ADJUSTMENT | Cash-in-lieu, rounding, settlement correction |
| STRUCTURED_FEE | Product/custody/platform fee |
| STRUCTURED_TAX_WITHHOLDING | Tax withheld |
| STRUCTURED_REVERSAL | Cancellation/reversal of previous transaction |

For wrapper-specific systems, prefixes may differ:

- NOTE_* for structured notes;
- DEPOSIT_* for structured deposits;
- CERTIFICATE_* for structured certificates;
- WARRANT_* for structured warrants;
- OTC_DERIVATIVE_* for OTC contracts.

But a canonical transaction model should map them into common economic effects.

## 4. Transaction event separation

| Event | Transaction? | Reason |
|---|---|---|
| Product issued | No client transaction unless purchased | Product setup only |
| Client order entered | No | Order is not accounting posting |
| Order allocated | Maybe not | Confirmation stage |
| Settlement completed | Yes | Cash and position move |
| Underlying observed | No | Event only |
| Barrier touched | Usually no | Status changes, no cash yet |
| Coupon condition satisfied | No initially | Entitlement event; transaction when paid |
| Coupon paid | Yes | Cash income received |
| Autocall triggered | No initially | Event; redemption transaction on settlement |
| Product matures | Yes when settled | Close position and book cash/security |
| Product valued lower | No | Valuation, not transaction |
| Issuer default confirmed | Yes when loss booked | Write-off/loss posting |

## 5. Position modelling

### 5.1 Common position fields

| Field | Meaning |
|---|---|
| account_id | Client portfolio/account |
| instrument_id | Structured product instrument |
| wrapper_type | NOTE / DEPOSIT / CERTIFICATE / WARRANT / FUND / OTC |
| product_family | Yield enhancement, participation, protected, leveraged, etc. |
| quantity_units | Units/certificates/contracts where applicable |
| nominal_amount | Notional/principal amount |
| current_notional | Outstanding notional after partial redemption |
| investment_currency | Product currency |
| settlement_currency | Currency settled or expected |
| cost_amount | Purchase/subscription cost |
| average_cost | Average cost per unit or % of par |
| market_value | Latest valuation |
| accrued_income | Accrued or confirmed income where applicable |
| unrealized_pnl | Market value minus cost basis |
| realized_pnl | Realized on sale/redemption/write-off |
| lifecycle_status | Active, knocked-in, autocall-pending, matured, defaulted, closed |
| next_observation_date | Next monitoring date |
| next_coupon_date | Next coupon/payment date |
| next_call_date | Next call/autocall date |
| barrier_status | Not touched, touched, breached, not applicable |
| principal_protection_type | None, conditional, full-at-maturity |
| issuer_id | Issuer/counterparty |
| valuation_source | Exchange, issuer, vendor, model |
| liquidity_classification | Daily, issuer bid, restricted, illiquid |

### 5.2 Actual position versus look-through exposure

| Concept | Example |
|---|---|
| Actual position | Client owns USD 100,000 autocallable note |
| Underlying reference | Apple, Microsoft, Nvidia |
| Look-through risk | Equity downside, worst-of basket, issuer risk, volatility risk |
| Actual delivered position | Apple shares received after physical settlement |

A client holding a structured deposit linked to USD/SGD should not be shown as holding an FX option. However, risk analytics should show FX exposure and conversion risk.

## 6. Lifecycle event model

Structured products need event records independent of transactions.

| Field | Description |
|---|---|
| lifecycle_event_id | Unique event ID |
| instrument_id | Product instrument |
| account_id | Optional if event is product-level or account-specific |
| event_type | Observation, fixing, coupon determination, barrier breach, autocall, final fixing |
| event_date | Date event is measured |
| effective_date | Contractual effective date |
| payment_date | Payment/settlement date if applicable |
| underlying_id | Underlying observed |
| observed_value | Observed market level |
| reference_value | Initial fixing/strike/barrier/autocall level |
| condition | Human-readable or coded condition |
| result | Passed, failed, triggered, not triggered |
| event_status | Pending, confirmed, cancelled, corrected |
| source_system | Issuer, exchange, lifecycle engine, custodian, market data |
| generated_transaction_id | Nullable link to transaction |
| audit_hash/version | Audit and replay support |

## 7. Product-specific lifecycle patterns

### 7.1 Principal-protected product

| Event | Transaction | Position impact |
|---|---|---|
| Subscription | STRUCTURED_SUBSCRIPTION | Increase position |
| Valuation | None | Market value changes |
| Final fixing | Lifecycle event | Determine payoff |
| Maturity | STRUCTURED_MATURITY_CASH_REDEMPTION | Close position |

If sold early:

| Event | Transaction | Result |
|---|---|---|
| Early exit | STRUCTURED_SECONDARY_SELL | Realized gain/loss; principal protection may not apply |

### 7.2 Yield enhancement / reverse convertible

| Event | Transaction | Position impact |
|---|---|---|
| Subscription | STRUCTURED_SUBSCRIPTION | Increase position |
| Coupon paid | STRUCTURED_COUPON_PAYMENT | Cash income |
| Barrier observed/breached | Lifecycle event | Status update |
| Maturity safe | STRUCTURED_MATURITY_CASH_REDEMPTION | Close product; principal cash |
| Maturity physical | STRUCTURED_PHYSICAL_REDEMPTION_OUT + STRUCTURED_PHYSICAL_DELIVERY_IN | Close product; create delivered asset |

### 7.3 Autocallable

| Event | Transaction | Position impact |
|---|---|---|
| Observation | Lifecycle event | Check coupon/autocall |
| Coupon condition met | Lifecycle event | Entitlement |
| Coupon paid | STRUCTURED_COUPON_PAYMENT | Cash income |
| Autocall condition met | Lifecycle event | Status = autocall pending |
| Autocall settlement | STRUCTURED_AUTOCALL_REDEMPTION | Close product |
| No autocall | None | Product continues |

### 7.4 Structured deposit

| Event | Transaction | Position impact |
|---|---|---|
| Placement | STRUCTURED_SUBSCRIPTION or STRUCTURED_DEPOSIT_PLACEMENT | Increase deposit position |
| Early withdrawal | STRUCTURED_SECONDARY_SELL / EARLY_WITHDRAWAL | Close/reduce; may have penalty |
| Final fixing | Lifecycle event | Determine interest/return |
| Maturity | STRUCTURED_MATURITY_CASH_REDEMPTION | Close and pay principal/return |

### 7.5 Warrant/certificate

| Event | Transaction | Position impact |
|---|---|---|
| Exchange buy | STRUCTURED_SECONDARY_BUY | Increase units |
| Exchange sell | STRUCTURED_SECONDARY_SELL | Reduce units |
| Knock-out/expiry worthless | STRUCTURED_KNOCKOUT_REDEMPTION or STRUCTURED_DEFAULT_WRITEOFF-like expiry | Reduce to zero |
| Cash settlement at expiry | STRUCTURED_MATURITY_CASH_REDEMPTION | Close with cash value |

### 7.6 Accumulator/decumulator

Accumulator lifecycle requires periodic generated trades.

| Event | Transaction | Position impact |
|---|---|---|
| Contract inception | OTC/structured contract open | Create contract position |
| Periodic observation | Lifecycle event | Determine quantity |
| Periodic accumulation | STRUCTURED_PHYSICAL_DELIVERY_IN or underlying BUY | Increase underlying or cash-settled exposure |
| Knock-out | Lifecycle event + termination | Close contract |
| Final settlement | Contract close transaction | Close remaining exposure |

For decumulator, periodic observations generate sells or cash-settled reductions.

## 8. Physical delivery modelling

When structured product settles into underlying securities, model it as a transaction group.

Example: equity-linked product delivers shares.

| Transaction | Instrument | Quantity/nominal | Cash |
|---|---|---:|---:|
| STRUCTURED_PHYSICAL_REDEMPTION_OUT | Structured product | -100,000 notional | 0 |
| STRUCTURED_PHYSICAL_DELIVERY_IN | Equity shares | +500 shares | 0 |
| STRUCTURED_SETTLEMENT_ADJUSTMENT | Cash | 0 | Cash-in-lieu if needed |

Cost basis policy must be explicit:

| Policy | Description |
|---|---|
| Carryover basis | Delivered asset inherits structured product carrying value |
| Fair value basis | Delivered asset booked at fair value and structured product realizes loss/gain |
| Tax basis | Jurisdiction-specific basis from tax engine/custodian |

## 9. Alternate currency redemption

For DCI or FX-linked products, redemption may be in alternate currency.

Transaction group:

| Transaction | Instrument/cash | Effect |
|---|---|---|
| STRUCTURED_ALTERNATE_CCY_REDEMPTION | Product | Close product |
| CASH_CREDIT | Alternate currency cash | Create cash balance |
| FX_PNL_DERIVED | Analytics | Calculate base-currency gain/loss |

Do not book an FX conversion trade unless there is an actual FX trade. The contractual redemption itself creates alternate currency cash.

## 10. Performance treatment

| Event | Performance classification |
|---|---|
| Client funds subscription from outside portfolio | External inflow |
| Purchase using existing portfolio cash | Internal investment transaction |
| Coupon paid into portfolio | Investment income |
| Coupon withdrawn by client | Income first, then external outflow |
| Redemption cash remains in portfolio | Internal disposal/redemption |
| Product converts into shares | Internal transformation |
| Issuer default/write-off | Investment loss |
| Fee/tax | Expense/tax classification based on reporting policy |
| Alternate currency redemption | Investment result plus FX exposure |

## 11. Reversal and correction model

Structured products often face corrections due to revised issuer data, late corporate-action-like events, amended fixings, wrong barrier status or incorrect cash posting.

Recommended approach:

- never overwrite settled transactions without audit trail;
- post reversal transaction linked to original;
- post corrected transaction;
- version lifecycle events;
- recalculate positions, P&L, performance and reporting snapshots;
- retain old and new valuation source metadata;
- flag client-report restatement if material.

## 12. Minimum transaction payload

```json
{
  "transactionId": "TXN-001",
  "transactionGroupId": "GRP-001",
  "accountId": "ACC-123",
  "instrumentId": "SP-456",
  "transactionType": "STRUCTURED_COUPON_PAYMENT",
  "tradeDate": "2026-06-27",
  "valueDate": "2026-07-02",
  "bookingDate": "2026-07-02",
  "quantityDelta": "0",
  "nominalDelta": "0",
  "price": null,
  "grossAmount": "1500.00",
  "fees": "0.00",
  "tax": "150.00",
  "netAmount": "1350.00",
  "cashCurrency": "USD",
  "incomeAmount": "1500.00",
  "performanceClassification": "INVESTMENT_INCOME",
  "lifecycleEventId": "EVT-COUPON-001",
  "sourceSystem": "ISSUER_CA_FEED"
}
```

## 13. Minimum lifecycle event payload

```json
{
  "lifecycleEventId": "EVT-AUTOCALL-001",
  "instrumentId": "SP-456",
  "eventType": "AUTOCALL_OBSERVATION",
  "eventDate": "2026-09-30",
  "paymentDate": "2026-10-07",
  "underlyingResults": [
    {"underlyingId": "AAPL", "observedValue": "225.00", "conditionValue": "200.00", "result": "PASSED"},
    {"underlyingId": "MSFT", "observedValue": "430.00", "conditionValue": "400.00", "result": "PASSED"}
  ],
  "overallResult": "TRIGGERED",
  "eventStatus": "CONFIRMED",
  "generatedTransactionExpected": true,
  "sourceSystem": "LIFECYCLE_ENGINE"
}
```

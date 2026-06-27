# 06 - Data Model: Order, Trade, Transaction, Position, Ledger and Lineage

## 1. Purpose

This file proposes a canonical data model for wealth trade-lifecycle platforms. It is designed to work across products while allowing product-specific extensions.

The model separates intent, execution, settlement, accounting, positions and reporting lineage.

## 2. Core object hierarchy

```text
Proposal
  -> Order
      -> Execution / Fill
          -> Trade
              -> Allocation
                  -> SettlementInstruction
                      -> TransactionGroup
                          -> TransactionLeg
                              -> Position / Cash / Ledger impact
```

Corporate actions, income events and lifecycle events also generate transaction groups:

```text
CorporateActionEvent
  -> Entitlement / Election
      -> TransactionGroup
          -> TransactionLeg
```

## 3. Proposal

| Field | Meaning |
|---|---|
| proposal_id | Unique proposal |
| proposal_type | ADVISORY / DPM_REBALANCE / MODEL_UPDATE / CLIENT_DIRECTED_REVIEW |
| client_id | Client |
| portfolio_id | Portfolio |
| mandate_id | Mandate context |
| base_currency | Reporting currency |
| rationale | Investment rationale |
| expected_impact | Expected allocation/risk/performance impact |
| created_by | Advisor/PM/system |
| status | Draft/pending/approved/rejected/executed |
| suitability_summary | Overall suitability result |
| disclosure_pack_id | Documents provided |
| version | Proposal version |

## 4. Order

| Field | Meaning |
|---|---|
| order_id | Unique order |
| proposal_id | Source proposal, nullable |
| parent_order_id | Linked/block order |
| order_origin | CLIENT / ADVISORY / DPM / SYSTEMATIC / CORPORATE_ACTION |
| account_id | Account |
| instrument_id | Instrument |
| side | Buy/sell/subscribe/redeem/switch/elect/exercise |
| quantity_type | Units/nominal/amount/percentage/all |
| requested_quantity | Quantity |
| requested_amount | Amount |
| order_currency | Order currency |
| limit_price | Limit price |
| time_in_force | Validity |
| status | Order status |
| pre_trade_check_snapshot_id | Check evidence |
| client_consent_id | Consent evidence |
| created_at/submitted_at | Timing |

## 5. Execution / Fill

| Field | Meaning |
|---|---|
| execution_id | Unique execution |
| order_id | Parent order |
| execution_venue | Venue/counterparty |
| broker_execution_id | External ID |
| execution_timestamp | Time |
| executed_quantity | Quantity |
| executed_price | Price |
| execution_currency | Currency |
| commission | Commission |
| fees | Fees |
| status | Valid/cancelled/corrected |

## 6. Trade

| Field | Meaning |
|---|---|
| trade_id | Unique trade |
| order_id | Parent order |
| execution_ids | Linked fills |
| trade_date | Trade date |
| settlement_date | Intended settlement date |
| instrument_id | Instrument |
| side | Buy/sell/etc. |
| quantity | Executed quantity |
| nominal | Bond/note/loan nominal |
| price | Price |
| price_type | Clean/dirty/NAV/percentage/per-unit |
| gross_amount | Gross consideration |
| accrued_interest | Bond/note accrued |
| fees | Fees |
| taxes | Taxes |
| net_amount | Settlement amount |
| trade_currency | Trade currency |
| settlement_currency | Settlement currency |
| counterparty_id | Broker/dealer |
| confirmation_status | Unconfirmed/confirmed/mismatched |
| booking_status | Captured/booked/corrected |

## 7. Allocation

| Field | Meaning |
|---|---|
| allocation_id | Unique allocation |
| trade_id | Parent trade |
| account_id | Allocated account |
| allocation_method | Pro-rata/manual/model-based |
| allocated_quantity | Quantity |
| allocated_amount | Amount |
| average_price | Price |
| allocation_status | Pending/confirmed/corrected |
| allocation_reason | Rationale |

## 8. Settlement instruction

| Field | Meaning |
|---|---|
| settlement_instruction_id | Unique instruction |
| trade_id/allocation_id | Source |
| custodian_account_id | Custody account |
| place_of_settlement | CSD/ICSD/market |
| counterparty_ssi_id | Counterparty SSI |
| own_ssi_id | Own SSI |
| settlement_quantity | Quantity |
| settlement_amount | Amount |
| settlement_currency | Currency |
| intended_settlement_date | ISD |
| actual_settlement_date | Actual date |
| status | Instructed/matched/settled/failed |
| fail_reason | If failed |

## 9. Transaction group

A transaction group represents a business event with multiple accounting legs.

Examples:

- equity buy
- bond coupon
- FX spot settlement
- fund subscription
- corporate action split
- note physical redemption
- private market capital call

| Field | Meaning |
|---|---|
| transaction_group_id | Unique group |
| source_event_type | TRADE / CORPORATE_ACTION / INCOME / FEE / FX / LOAN / POLICY |
| source_event_id | Source object |
| account_id | Account |
| event_date | Economic event date |
| value_date | Value date |
| booking_date | Booking date |
| status | Posted/reversed/corrected |
| reversal_group_id | Reversal if any |
| performance_classification | Income/capital/external/internal/fee/tax |
| lineage_id | End-to-end lineage |

## 10. Transaction leg

| Field | Meaning |
|---|---|
| transaction_leg_id | Unique leg |
| transaction_group_id | Parent group |
| leg_type | SECURITY / CASH / FEE / TAX / ACCRUAL / FX / MARGIN |
| instrument_id | Security/instrument/cash currency |
| quantity_delta | Security units |
| nominal_delta | Bond/note/loan nominal |
| cash_delta | Cash amount |
| currency | Currency |
| price | Price used |
| fx_rate | Translation rate |
| debit_credit | Debit/credit |
| cost_basis_delta | Cost basis impact |
| realized_pnl | Realized P&L |
| income_amount | Income impact |
| tax_amount | Tax impact |
| performance_treatment | External/internal/income/expense |

## 11. Position

Position should usually be derived from transaction legs, not manually maintained.

| Field | Meaning |
|---|---|
| position_id | Unique position |
| account_id | Account |
| instrument_id | Instrument |
| basis | TRADE_DATE / SETTLEMENT_DATE / AVAILABLE / COLLATERAL_ELIGIBLE |
| quantity | Units |
| nominal | Nominal |
| cost_basis | Cost |
| accrued_income | Accrued income |
| market_price | Price |
| market_value | Value |
| unrealized_pnl | Unrealized P&L |
| position_date | Date |
| source_version | Calculation version |

## 12. Cash balance

| Field | Meaning |
|---|---|
| account_id | Account |
| currency | Currency |
| basis | LEDGER / SETTLED / TRADE_DATE / AVAILABLE / PROJECTED |
| balance | Amount |
| blocked_amount | Reserved amount |
| overdraft_amount | Negative/funded amount |
| value_date | Value date |
| calculation_timestamp | Timestamp |

## 13. Ledger account model

A richer platform may use ledger accounts:

| Ledger account | Example |
|---|---|
| Cash asset | Client cash by currency |
| Security asset | Holdings by instrument |
| Receivable | Pending sale proceeds, dividends receivable |
| Payable | Pending purchase payable, fees payable |
| Income | Dividend/coupon income |
| Expense | Fees/taxes/interest expense |
| Realized gain/loss | Disposal P&L |
| Unrealized gain/loss | Valuation P&L |
| Accrued interest | Bond/note accrual |
| Margin/collateral | Margin posted/received |

Ledger modelling improves auditability but requires strong accounting design.

## 14. Lineage model

Every result should be traceable.

| Lineage dimension | Example |
|---|---|
| business lineage | Proposal -> order -> trade -> transaction |
| operational lineage | Trade -> settlement instruction -> custodian statement |
| data lineage | Source feed -> normalized event -> calculation output |
| calculation lineage | Input positions/prices/FX -> performance/risk result |
| reporting lineage | Report section -> metric -> calculation run -> source data |

Lineage identifiers should be carried through APIs and events.

## 15. Event model

Use events for state changes:

| Event | Meaning |
|---|---|
| OrderSubmitted | Order sent |
| OrderAccepted | Accepted by broker/OMS |
| ExecutionReceived | Fill received |
| TradeCaptured | Trade record created |
| AllocationConfirmed | Allocated to accounts |
| SettlementInstructionSent | Instruction sent |
| SettlementMatched | Matched externally |
| SettlementFailed | Failed to settle |
| SettlementCompleted | Settled |
| CorporateActionAnnounced | Event announced |
| EntitlementCalculated | Eligible entitlement calculated |
| ElectionSubmitted | Voluntary election sent |
| CorporateActionBooked | Resulting transactions booked |
| ReconciliationBreakDetected | Break created |
| CorrectionBooked | Reversal/rebook posted |
| PositionRecalculated | Position updated |
| ReportGenerated | Report produced |

Events should be idempotent and versioned.

## 16. Versioning and correction

Versioned entities:

- instrument static
- product governance/eligibility
- client suitability profile
- mandate rules
- order amendments
- trade corrections
- settlement instructions
- corporate action terms
- price/valuation data
- FX rates
- performance calculation runs
- client reports

Corrections should preserve prior versions and clearly identify current effective version.

## 17. Product extension pattern

Use a common core and extensions.

```text
Instrument
  -> product-specific contract terms
Order
  -> product-specific order details
Trade
  -> product-specific trade economics
TransactionGroup
  -> product-specific transaction legs
Position
  -> product-specific attributes
LifecycleEvent
  -> product-specific event terms
```

Examples:

| Product | Extension |
|---|---|
| Bond | accrued interest, yield, nominal, clean/dirty price |
| Fund | NAV date, dealing date, units/amount, equalisation |
| Note | observation schedule, barrier, autocall, physical settlement |
| Option | expiry, strike, premium, exercise style, Greeks |
| FX forward | value date, far/near legs, NDF fixing |
| Private market | commitment, unfunded, capital call, distribution |
| Loan | drawdown, repayment, interest accrual, collateral |

## 18. API design hints

Core APIs should expose:

- `GET /orders/{id}`
- `GET /trades/{id}`
- `GET /transactions?sourceEventId=...`
- `GET /positions?basis=TRADE_DATE|SETTLEMENT_DATE|AVAILABLE`
- `GET /cash-balances?basis=...`
- `GET /settlements?status=FAILED`
- `GET /corporate-actions/{id}`
- `GET /reconciliation-breaks?...`
- `GET /lineage/{id}`

Do not force downstream systems to infer lifecycle from free-text transaction descriptions.

## 19. Data quality rules

| Rule | Reason |
|---|---|
| Every transaction group must have source event | Lineage |
| Every security leg must reference instrument | Position derivation |
| Every cash leg must have currency and value date | Cash availability |
| Every correction must reference reversed/corrected object | Audit |
| Every position must specify basis | Avoid ambiguity |
| Every order must preserve origin | Advisory/DPM controls |
| Every manual override must have reason/user/timestamp | Governance |
| Every settlement instruction must reference SSI version | Fail analysis |
| Every report should reference calculation run | Reproducibility |

## 20. Implementation recommendation

A canonical model should be strict enough for reconciliation and flexible enough for product extensions. Avoid hundreds of nullable fields in one transaction table. Use a core transaction model plus typed attributes and product-specific extension tables/documents.

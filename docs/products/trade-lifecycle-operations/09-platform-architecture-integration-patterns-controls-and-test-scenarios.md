# 09 - Platform Architecture, Integration Patterns, Controls and Test Scenarios

## 1. Purpose

This file translates the trade-lifecycle knowledge into platform architecture, engineering patterns, controls and QA scenarios.

It is intentionally implementation-aware and suitable for enterprise wealth platforms.

## 2. Reference architecture

```text
Advisor / PM UI
  -> Proposal and Suitability Services
  -> Mandate and Buying-Power Services
  -> Order Management Service
  -> Execution / Broker / Fund / Dealer Adapters
  -> Trade Capture Service
  -> Settlement and Custody Services
  -> Transaction / Ledger Service
  -> Position and Cash Services
  -> Corporate Action Service
  -> Reconciliation Service
  -> Performance / Risk / Reporting Services
```

Supporting services:

- instrument master
- client/account master
- mandate/rules engine
- product governance/APU service
- market data/price service
- FX rate service
- calendar service
- notification service
- audit/lineage service

## 3. Integration patterns

| Pattern | Use |
|---|---|
| Synchronous API | Pre-trade checks, order capture, real-time status |
| Event streaming | Trade status updates, settlement events, corporate actions |
| Batch files | Custodian EOD positions/cash/transactions |
| Message queues | Reliable processing of booking/reconciliation events |
| Webhooks/callbacks | Broker/fund-platform status updates |
| Managed file transfer | Custodian statements, fund NAVs, corporate action files |
| Manual upload with controls | Exception-only fallback |
| Event sourcing | Reconstruct lifecycle and audit history |

## 4. Idempotency

Critical operations requiring idempotency:

- order submission
- execution capture
- trade booking
- settlement status update
- custodian transaction import
- corporate-action event import
- entitlement booking
- cash/position transaction booking
- reconciliation result creation
- report generation

Recommended idempotency keys:

| Event | Key example |
|---|---|
| Execution | broker_execution_id + venue + trade date |
| Trade | source_trade_id + account + instrument + side |
| Settlement | settlement_instruction_id + status timestamp/version |
| Custodian transaction | custodian_txn_id + account + value date |
| Corporate action event | source_event_id + event type + version |
| Entitlement | event_id + account_id + option + payable date |
| Report | report_type + account + period + data_version |

## 5. Event versioning

Many lifecycle events are revised.

Examples:

- corporate action announced then updated
- fund NAV corrected
- trade cancelled/rebooked
- settlement fail later settled
- custodian statement restated
- client tax status updated
- instrument classification changed

Store event versions and effective versions. Do not assume first feed is final.

## 6. State management

Avoid deriving states only from latest transaction rows.

Maintain explicit status history for:

- orders
- executions
- trades
- allocations
- settlement instructions
- corporate actions
- elections
- reconciliation breaks
- client reports

Status history fields:

| Field | Meaning |
|---|---|
| entity_id | Order/trade/etc. |
| from_status | Previous status |
| to_status | New status |
| changed_at | Timestamp |
| changed_by | System/user/source |
| source_message_id | Source event |
| reason_code | Business reason |
| comment | Optional explanation |

## 7. Calendar service

Use a central calendar service for:

- market holidays
- currency holidays
- exchange trading sessions
- settlement cycles
- fund dealing days
- corporate-action deadlines
- timezone cut-offs
- daylight-saving changes
- early market close
- product-specific holidays

Incorrect calendar logic causes trade-date, settlement, coupon, FX and reporting errors.

## 8. Data quality controls

| Control | Example |
|---|---|
| Required fields | Trade must have instrument, account, side, date, quantity/amount |
| Referential integrity | Instrument/account/custodian must exist |
| Product convention validation | Bond nominal, option contract size, fund units precision |
| Price tolerance | Execution price within market tolerance |
| FX tolerance | FX rate source/date/time valid |
| Settlement date validation | Calculated date matches market/product calendar |
| Duplicate detection | Same external trade not processed twice |
| Lifecycle consistency | Cannot settle cancelled trade |
| Position availability | Cannot sell blocked/pledged quantity unless allowed |
| Cash availability | Cannot buy without funding/credit approval |

## 9. Recalculation and restatement

Corrections may require recalculation of:

- cash balances
- positions
- cost basis
- realized P&L
- unrealized P&L
- performance returns
- attribution
- risk exposure
- mandate breaches
- client reports
- tax lots

Recalculation engine should support:

- impacted account identification
- impacted date range
- dependency graph
- dry run and compare
- approval for material restatement
- audit of before/after metrics

## 10. Observability

Operational metrics:

- order submission latency
- execution feed latency
- trade booking latency
- settlement instruction timeliness
- settlement fail rate
- reconciliation break count/ageing
- corporate-action events pending election
- stale price count
- NAV delay count
- failed idempotency/dedup events
- queue backlog
- report generation success/failure

Technical metrics:

- API latency
- event processing lag
- retry count
- dead-letter queue count
- database lock/contention
- batch runtime
- failed validations
- manual override count

## 11. Audit and entitlement controls

Sensitive actions requiring maker-checker or elevated permission:

- manual trade creation
- trade cancellation/rebook
- settlement status override
- price override
- corporate action event override
- manual entitlement booking
- reconciliation break forced close
- report restatement
- mandate breach override
- suitability override
- client consent override

## 12. Test scenario catalogue

### Order and execution

| Scenario | Expected result |
|---|---|
| Market order fully filled | Order filled, trade booked, settlement pending |
| Limit order partially filled then expires | Partial trade booked, residual expired |
| Cancel request after partial fill | Filled quantity remains, residual cancelled |
| Broker duplicate execution message | No duplicate trade |
| Order fails suitability | Order blocked, no execution |
| DPM block allocation partial fill | Fair allocation according to policy |

### Settlement

| Scenario | Expected result |
|---|---|
| Equity buy settles normally | Cash/security posted, status settled |
| Settlement instruction unmatched | Break created, settlement pending |
| Partial settlement | Partial position/cash update, residual fail |
| Failed sell due to insufficient securities | Fail recorded, sale proceeds unavailable per policy |
| Wrong SSI corrected | New instruction version, audit retained |
| Settlement after fail | Actual settlement date updated, fail closed |

### Corporate actions

| Scenario | Expected result |
|---|---|
| Cash dividend | Gross/net income, withholding, cash posted |
| 2-for-1 split | Quantity doubled, unit cost adjusted, no artificial return |
| Rights issue exercised | Rights removed, cash paid, new shares added |
| Rights lapse | Rights removed, possible loss |
| Spin-off | New security created, cost basis allocated |
| Corporate action revised | Old event version superseded, bookings corrected if needed |

### Funds

| Scenario | Expected result |
|---|---|
| Subscription before cut-off | NAV date assigned, units booked after confirmation |
| Subscription after cut-off | Next dealing date assigned |
| Redemption by all units | Units removed, cash booked when settled |
| Fund distribution reinvested | Income and new units or direct unit increase per policy |
| NAV correction | Valuation/performance recalculation triggered |

### Bonds

| Scenario | Expected result |
|---|---|
| Bond buy clean price plus accrued | Accrued interest cash leg recorded |
| Coupon payment | Income posted to cash |
| Partial redemption | Nominal reduced, cash received |
| Callable bond redeemed | Position closed/reduced, realized P&L calculated |
| Default write-down | Loss posted, status defaulted |

### Notes / structured products

| Scenario | Expected result |
|---|---|
| Conditional coupon met | Coupon entitlement then payment booked |
| Autocall trigger | Lifecycle event, redemption on settlement date |
| Knock-in physical settlement | Note out, underlying security in, cash-in-lieu handled |
| Issuer quote missing | Valuation stale flag |
| Barrier revised/corrected | Lifecycle event versioned, reporting updated |

### Derivatives and FX

| Scenario | Expected result |
|---|---|
| Option expires worthless | Position closed, premium P&L retained |
| Option exercised | Underlying transaction generated |
| Future variation margin | Daily cash P&L booked |
| FX forward settles | Two currency cash legs booked |
| NDF fixes | Net cash settlement booked |
| Margin call | Collateral/cash movement recorded |

### Reconciliation

| Scenario | Expected result |
|---|---|
| Custodian position mismatch | Position break created |
| Cash statement missing dividend | Cash/income break created |
| Duplicate custodian transaction | Duplicate identified and quarantined |
| Price stale | Valuation exception and reporting flag |
| Break corrected after report generation | Restatement decision required |

## 13. Non-functional requirements

| Requirement | Guidance |
|---|---|
| Availability | Order/status APIs require high availability; reports can have batch windows |
| Latency | Pre-trade checks should be low-latency; settlement/recon may be batch/intraday |
| Scalability | Event and reconciliation processing should scale by account/product/date |
| Resilience | Retry, DLQ, idempotency and replay support are mandatory |
| Auditability | Immutable records for executed/booked events |
| Security | Account/client data access by role and legal entity |
| Privacy | Beneficial owner and tax data protected |
| Explainability | Trace report figures to source transactions and prices |

## 14. Production-support runbook themes

Runbooks should cover:

- order stuck in submitted state
- execution feed duplicate/missing
- trade booked with wrong account
- settlement file delayed
- settlement fail spike
- corporate action missed/revised
- custodian cash/position feed mismatch
- price/NAV feed missing
- performance restatement
- client report incorrect after correction
- reconciliation backlog

## 15. Architecture anti-patterns

| Anti-pattern | Risk |
|---|---|
| One table for all states | Cannot distinguish order/trade/settlement/accounting |
| No idempotency | Duplicate bookings |
| No event versions | Cannot process corrections safely |
| No lineage | Impossible to explain report numbers |
| Direct position updates | Audit and reconciliation failures |
| Settlement cycle hardcoded | Market/product errors |
| Manual overrides without workflow | Control weakness |
| Corporate action handled as free text | Cost/P&L/reporting errors |
| No recalculation engine | Corrections do not flow to analytics |

## 16. Implementation checklist

- canonical lifecycle model defined
- status models separated
- trade-date/settlement-date/available basis explicit
- settlement calendar service implemented
- SSI management implemented
- idempotency keys defined
- corporate action event model implemented
- reconciliation engine implemented
- exception workflow implemented
- price/NAV staleness controls implemented
- performance recalculation linked to corrections
- audit lineage available from report to source
- product-specific test pack maintained

# 01 - Trade Lifecycle Fundamentals and Operating Model

## 1. What is the trade lifecycle?

The trade lifecycle is the full journey of an investment decision from intent to final accounting, custody, performance and reporting impact.

In a private-banking or wealth-management platform, the lifecycle is broader than exchange execution. It includes advisory suitability, mandate checks, buying power, product eligibility, client consent, order handling, execution, confirmation, settlement, reconciliation, corporate actions and client reporting.

A simplified lifecycle is:

```text
Idea / Advice
  -> Proposal
  -> Suitability and Mandate Checks
  -> Order Capture
  -> Order Validation
  -> Routing / Execution
  -> Trade Capture
  -> Allocation
  -> Confirmation / Affirmation
  -> Clearing
  -> Settlement
  -> Custody Safekeeping
  -> Cash / Position / Ledger Booking
  -> Reconciliation
  -> Corporate Actions and Income Events
  -> Performance / Risk / Reporting
```

## 2. Why this matters for wealth platforms

Product knowledge explains how an instrument behaves. Trade-lifecycle knowledge explains how that instrument becomes a real client holding and how it affects cash, performance, reporting, risk and advisory controls.

Without a strong lifecycle model, platforms suffer from:

- positions that do not reconcile with custodians
- cash balances that differ between front-office and back-office systems
- unsettled trades incorrectly included in buying power
- incorrect performance cashflow classification
- corporate actions applied twice or not at all
- cost basis not adjusted after reorganisations
- failed-trade risk not visible to advisors
- stale order statuses and incomplete audit trail
- mismatch between proposal, order, execution and final transaction
- poor explainability in client reports

## 3. Major lifecycle domains

| Domain | Purpose | Example objects |
|---|---|---|
| Advisory and proposal | Decide what client should do | Proposal, recommendation, model deviation, rationale |
| Pre-trade control | Check eligibility and constraints before order | Suitability result, mandate breach, buying-power result |
| Order management | Capture and manage instruction | Order, order version, order leg, order status |
| Execution | Fill the order in market or with counterparty | Execution, fill, venue, broker, price |
| Trade capture | Convert executions into bookable trade records | Trade, trade leg, allocation, confirmation |
| Clearing | Net/novate/prepare trades for settlement where applicable | Clearing instruction, CCP status, margin |
| Settlement | Exchange cash and securities | Settlement instruction, SSI, DVP/PVP status |
| Custody | Safekeep assets and record ownership | Custody account, sub-account, depot position |
| Asset servicing | Process income and corporate actions | Dividend, coupon, split, rights, tender, maturity |
| Reconciliation | Compare internal records with external records | Position break, cash break, transaction break |
| Reporting and analytics | Reflect accurate investment state | Performance, risk, exposure, client report |

## 4. Front office, middle office and back office

| Area | Primary concerns |
|---|---|
| Front office | Client advice, suitability, proposals, orders, execution status, client experience |
| Middle office | Trade enrichment, allocation, confirmation, exception management, risk, controls |
| Back office | Settlement, cash/security movements, custody, accounting, corporate actions, reconciliation |
| Product control / valuation | Prices, valuation, P&L, independent price verification |
| Reporting / analytics | Positions, performance, risk, attribution, client statements |

In modern platforms, these boundaries are increasingly event-driven and integrated, but the control responsibilities remain distinct.

## 5. Trade date, value date, settlement date and booking date

| Date | Meaning | Why it matters |
|---|---|---|
| Trade date | Date trade is agreed/executed | Performance event timing, market exposure start, regulatory record |
| Value date | Date cash/security value is effective | Cash availability, FX, interest, settlement |
| Settlement date | Date securities/cash actually settle | Custody position and available cash |
| Booking date | Date platform records the event | Accounting/reconciliation and operational audit |
| Contractual settlement date | Intended settlement date | Fail detection and ageing |
| Actual settlement date | Date settlement completed | Penalty, overdraft, exposure, client reporting |

Do not collapse these dates into one field. They answer different business questions.

## 6. Contractual state versus operational state

A trade may be legally executed but not settled. A coupon may be announced but not paid. A corporate action may be elected but not allocated. A note may be autocall-triggered but not redeemed.

Platform design should therefore separate:

| Concept | Example |
|---|---|
| Contractual/economic event | Client bought 1,000 shares on trade date |
| Operational status | Trade awaiting settlement instruction |
| Accounting transaction | Cash/security entries posted on settlement |
| Analytical treatment | Exposure starts on trade date or settlement date depending policy |

## 7. Position basis: trade-date, settlement-date and available basis

A platform often needs multiple views of the same holding:

| Position basis | Meaning | Used by |
|---|---|---|
| Trade-date position | Includes executed trades even if unsettled | Performance, market exposure, advisory view |
| Settlement-date position | Includes only settled securities | Custody, regulatory ownership, some reporting |
| Available-to-sell position | Settled plus allowed pending inflows minus blocked quantities | Order validation, buying power |
| Collateral-eligible position | Haircut-adjusted eligible settled collateral | Lombard / margin systems |
| Reporting position | Business-defined position basis for statements | Client reporting |

A strong platform should explicitly label which basis is used in every API/report.

## 8. Cash basis: ledger, available, settled, projected and blocked

Cash is not one number.

| Cash basis | Meaning |
|---|---|
| Ledger cash | Posted cash balance |
| Settled cash | Cash that has actually settled |
| Trade-date cash | Includes unsettled trade cash movements |
| Available cash | Cash that can be used for new orders after blocks and buffers |
| Projected cash | Cash after expected future settlements/income/calls |
| Blocked cash | Reserved for open orders, fees, margin, capital calls or pending settlement |
| Overdraft cash | Negative cash funded by credit line or overdraft facility |

For buying power, cash availability must consider settlement cycle, pending orders, FX conversion, credit lines, fees, margin, and product-specific liquidity rules.

## 9. Lifecycle state machine

A generic trade lifecycle state machine may look like:

| State | Meaning |
|---|---|
| Draft | Not yet submitted |
| Proposed | Created as part of advisory proposal |
| Pending approval | Awaiting client/manager/compliance approval |
| Rejected | Failed approval or validation |
| Submitted | Sent for order handling |
| Accepted | Accepted by OMS/broker/counterparty |
| Partially filled | Some quantity executed |
| Filled | Fully executed |
| Cancel requested | Cancellation requested but not confirmed |
| Cancelled | Order cancelled |
| Expired | Order validity expired |
| Allocated | Filled quantity allocated to accounts |
| Confirmed | Trade confirmation matched/agreed |
| Instructed | Settlement instruction sent |
| Settled | Cash/security settlement completed |
| Failed | Failed to settle on intended date |
| Corrected | Amended or cancelled/rebooked |

Do not use only one status for all stages. A robust model separates order status, execution status, trade status, settlement status, accounting status and reconciliation status.

## 10. Principal parties

| Party | Role |
|---|---|
| Client / account holder | Ultimate investor or beneficial owner |
| Advisor / RM / PM | Provides advice or manages account |
| Portfolio manager | Makes DPM or model decisions |
| Product issuer | Issues note, bond, fund, certificate, insurance policy, etc. |
| Broker / dealer | Executes or intermediates trades |
| Exchange / venue | Trading venue |
| Counterparty | OTC trade counterparty |
| Clearing broker | Provides clearing access |
| CCP | Central counterparty for cleared trades |
| Custodian | Safekeeps assets and settles transactions |
| CSD / depository | Central securities depository |
| Transfer agent | Maintains fund/shareholder register |
| Fund administrator | Calculates NAV and processes subscriptions/redemptions |
| Corporate action agent | Communicates and processes event details |
| Paying agent | Distributes coupon/dividend/redemption proceeds |

## 11. Control principles

| Principle | Meaning |
|---|---|
| Event lineage | Every downstream position/cash/reporting result should link back to source events |
| Idempotency | Replays and duplicate messages must not double-book trades or cash |
| State explicitness | Trade, settlement, accounting and reconciliation states should be separate |
| Immutable audit | Corrections should reverse and rebook; do not silently overwrite executed records |
| Source hierarchy | Define golden sources for orders, trades, positions, cash, prices and corporate actions |
| Basis transparency | Every balance should specify trade-date, settlement-date or availability basis |
| Product-specific rules | Funds, notes, derivatives, FX and private markets need special lifecycle treatment |
| Exception ownership | Every break should have owner, severity, SLA, ageing and resolution evidence |
| Reconciliation-first design | APIs should expose enough identifiers to reconcile with custodians and source systems |
| Reporting explainability | Client reports must explain why numbers changed |

## 12. Common lifecycle anti-patterns

| Anti-pattern | Problem |
|---|---|
| Single transaction table with no lifecycle links | Cannot trace order to settlement or report |
| Updating positions directly | Breaks audit and explainability |
| Treating all cash as available cash | Creates overdraft and failed trade risk |
| Treating unsettled trades as settled holdings | Incorrect custody and collateral treatment |
| Hardcoding settlement cycle | Breaks by market/product/venue/date |
| Treating corporate actions as price changes only | Cost basis, income and quantity become wrong |
| Booking observations as transactions | Notes/derivatives become noisy and hard to reconcile |
| Ignoring cancelled/corrected events | Duplicate or stale records persist |
| No event versioning | Cannot handle restatements and late corrections |
| No source priority | Conflicting records cannot be resolved consistently |

## 13. Enterprise platform view

A strong wealth platform should support:

- canonical order/trade/transaction model
- product-specific lifecycle extensions
- trade-date and settlement-date views
- projected and available cash views
- event-sourced or lineage-rich transaction processing
- configurable settlement calendars
- corporate-action event processing
- reconciliation and exception workflow
- pricing and valuation lineage
- performance cashflow classification
- audit-ready client/advisor reporting

The platform should answer:

1. What did the client intend to do?
2. Who approved it?
3. Was it suitable and mandate-compliant?
4. What was executed?
5. What settled?
6. What failed or changed?
7. How did it affect cash, positions, performance, risk and reporting?
8. Can the full story be reconstructed later?

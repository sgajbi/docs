# 11. Worked Examples and Implementation Patterns

## Purpose

This file turns trade-lifecycle operating concepts into concrete examples for advisory, DPM, analytics, reporting, reconciliation, QA and implementation work.

## Example 1. Equity order with partial fills and allocation

### Scenario

A portfolio manager submits an order to buy 10,000 shares of a listed equity for three discretionary accounts. The order receives partial fills across two prices.

| Account | Target allocation | Filled quantity |
|---|---:|---:|
| Account A | 50% | 5,000 |
| Account B | 30% | 3,000 |
| Account C | 20% | 2,000 |

| Fill | Quantity | Price |
|---|---:|---:|
| Fill 1 | 6,000 | 24.10 |
| Fill 2 | 4,000 | 24.20 |

### Calculation

```text

gross consideration = (6,000 x 24.10) + (4,000 x 24.20)
gross consideration = 144,600 + 96,800
gross consideration = 241,400

average execution price = 241,400 / 10,000
average execution price = 24.14

```

### Correct object separation

| Object | What it stores |
|---|---|
| Order | Original intent to buy 10,000 shares and allocation policy. |
| Execution | Fill-level market facts: quantity, price, time, venue and broker. |
| Trade | Bookable deal after fills are aggregated or allocated. |
| Allocation | Account-level split of executed quantity and consideration. |
| Transaction | Account postings that change security position and cash. |
| Position | Resulting holding after trade-date or settlement-date treatment. |

### QA assertions

| Test | Expected result |
|---|---|
| Fill quantity is less than order quantity | Order remains partially filled or done-for-day based on state. |
| Allocation percentages do not sum to 100% | Allocation is rejected. |
| Average price is displayed | Weighted average price is used, not simple average. |
| Account C has insufficient mandate room | Allocation is blocked or reallocated through governed workflow. |

## Example 2. Bond purchase with accrued interest

### Scenario

A client buys a bond with a clean price of 101.25 and accrued interest of 1.35 per 100 nominal.

| Attribute | Value |
|---|---:|
| Nominal | 200,000 |
| Clean price | 101.25 |
| Accrued interest | 1.35 |
| Commission | 150 |

### Calculation

```text

clean consideration = nominal x clean price / 100
clean consideration = 200,000 x 101.25 / 100 = 202,500

accrued interest = nominal x accrued interest / 100
accrued interest = 200,000 x 1.35 / 100 = 2,700

cash settlement amount = clean consideration + accrued interest + commission
cash settlement amount = 202,500 + 2,700 + 150 = 205,350

```

### Reporting treatment

| Report area | Treatment |
|---|---|
| Position market value | Usually based on clean or dirty valuation policy, clearly labeled. |
| Cash movement | Full settlement amount including accrued interest and commission. |
| Performance | Accrued interest purchase component should not be treated as investment gain. |
| Income | Future coupon income should not double count purchased accrued interest. |

### QA assertions

| Test | Expected result |
|---|---|
| Trade captures clean price only | Settlement calculation still includes accrued interest. |
| Accrued interest missing | Trade is incomplete for booking or settlement. |
| Commission omitted from cash ledger | Cash reconciliation breaks. |
| Report mixes clean and dirty values without label | Reporting QA fails. |

## Example 3. Settlement fail and position basis

### Scenario

A client sells 1,000 shares on trade date, but settlement fails because the custodian rejects the settlement instruction.

### State timeline

| Date | State | Position view |
|---|---|---|
| Trade date | Sell executed | Trade-date position reduced by 1,000. |
| Intended settlement date | Settlement fail | Settlement-date position still includes the shares. |
| Repair date | SSI corrected | Fail is pending repair. |
| Actual settlement date | Settlement completes | Settlement-date position reduced by 1,000. |

### Platform impact

| Area | Correct behavior |
|---|---|
| Advisory exposure | May use trade-date view, with pending-settlement warning. |
| Custody statement | Uses settlement-date view until settlement completes. |
| Available position | Should not allow duplicate sale if shares are already blocked by pending sell. |
| Cash availability | Sale proceeds should not become settled cash until settlement completes. |
| Reporting | Should identify settlement fail if material or client-visible. |

### QA assertions

| Test | Expected result |
|---|---|
| Trade-date report is generated | Position reflects executed sell with pending-settlement status. |
| Custody report is generated before repair | Settled position still includes the shares. |
| User attempts second sell of same quantity | Available position blocks duplicate sale. |
| Settlement completes late | Cash and settlement-date position update on actual settlement date. |

## Example 4. Fund order cut-off and NAV date

### Scenario

A client submits a fund subscription after the dealing cut-off. The fund is daily dealing, but late orders receive the next dealing date's NAV.

| Attribute | Value |
|---|---|
| Order timestamp | 15:45 |
| Dealing cut-off | 15:00 |
| Submitted order date | Monday |
| Effective dealing date | Tuesday |
| NAV used | Tuesday NAV |
| Settlement date | Based on fund terms from Tuesday dealing date |

### Common implementation mistake

The platform books the order using Monday's NAV because the order was submitted on Monday. That misstates units, cash settlement, performance start date and client explanation.

### Correct treatment

| Object | Date basis |
|---|---|
| Order | Monday submission timestamp |
| Dealing event | Tuesday dealing date |
| Price/NAV | Tuesday NAV |
| Unit allocation | Calculated from Tuesday NAV |
| Settlement | Terms measured from Tuesday dealing date |
| Performance | Exposure starts according to booked units and valuation policy |

### QA assertions

| Test | Expected result |
|---|---|
| Order submitted before cut-off | Same-day dealing date if fund calendar allows. |
| Order submitted after cut-off | Next valid dealing date. |
| Market holiday on next day | Next valid fund dealing date, not next calendar day. |
| NAV file arrives late | Order remains pending price with clear status. |

## Example 5. Corporate action entitlement revision

### Scenario

A company announces a cash dividend. The entitlement is calculated, then revised after a late position correction.

### Event model

```text

Announcement
  -> entitlement calculation
  -> entitlement confirmation
  -> payment instruction
  -> cash transaction
  -> later correction or reversal if entitlement changes

```

### Correct platform behavior

The platform should not overwrite the original dividend transaction silently. It should create a reversal or adjustment with lineage to the revised entitlement.

| Artifact | Required lineage |
|---|---|
| Corporate-action event | Event identifier, terms, dates and source. |
| Entitlement | Position basis, eligible quantity, withholding treatment. |
| Cash transaction | Payment amount, currency, tax, value date. |
| Revision | Reason, source event, old amount, new amount, adjustment transaction. |
| Report | Correct income and cash with restatement note where needed. |

### QA assertions

| Test | Expected result |
|---|---|
| Entitlement is revised upward | Additional cash transaction is booked with lineage. |
| Entitlement is revised downward | Reversal or negative adjustment is booked with lineage. |
| Client report was already issued | Restatement workflow flags impacted report period. |
| Tax withholding changed | Net and gross income are recalculated consistently. |

## Example 6. Lifecycle data contract

### Minimum links

| Link | Why it matters |
|---|---|
| proposal_id -> order_id | Connects recommendation and consent to execution. |
| order_id -> execution_id | Connects intent to market fill. |
| execution_id -> trade_id | Connects fill facts to bookable trade. |
| trade_id -> settlement_instruction_id | Connects trade to settlement status and fail repair. |
| trade_id -> transaction_id | Connects trade to account posting. |
| transaction_id -> position_lot_id | Connects posting to holdings and cost basis. |
| transaction_id -> performance_event_id | Connects economic event to return calculation. |
| corporate_action_event_id -> transaction_id | Connects asset servicing to cash/security postings. |
| source_event_id -> report_line_id | Connects report numbers back to operational facts. |

### Design principle

Every material number in a client report, portfolio review, mandate breach, buying-power calculation or reconciliation break should be traceable to source lifecycle events. A platform that can only show current positions but cannot explain the lifecycle path is weak for operations, QA, audit and client servicing.

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

## Example 7. Post-Trade Allocation Correction

### Scenario

A block trade for 10,000 shares was allocated 60% to Account A and 40% to Account B. Operations later identifies that the approved allocation should have been 50%/50%.

| Account | Original allocation | Corrected allocation | Quantity delta |
|---|---:|---:|---:|
| Account A | 6,000 | 5,000 | -1,000 |
| Account B | 4,000 | 5,000 | +1,000 |

### Correct workflow

| Step | Treatment |
|---|---|
| Detect error | Link exception to order, execution, trade, allocation and approval evidence. |
| Freeze downstream impact | Identify impacted positions, cash, tax lots, performance and reports. |
| Correct allocation | Create adjustment or cancel/rebook workflow according to accounting policy. |
| Recalculate | Update cost basis, cash consideration, fees, tax and performance for both accounts. |
| Publish | Record maker/checker sign-off and report restatement decision. |

### QA assertions

| Test | Expected result |
|---|---|
| Corrected allocation changes account quantities | Position, cash, lots and performance update with lineage. |
| Client report was already issued | Impact assessment decides restatement or next-period disclosure. |
| Mandate breach appears after correction | Breach is evaluated against corrected account exposure. |
| Correction lacks approval | Allocation correction remains blocked. |

## Example 8. FX Settlement Linked To Security Trade

### Scenario

A USD account buys a EUR security. Funding requires an FX trade that must settle before or on the security settlement date.

| Attribute | Value |
|---|---:|
| EUR security purchase | 100,000 |
| EUR/USD FX rate | 1.0800 |
| USD funding required | 108,000 |
| Security settlement date | T+2 |
| FX value date | T+2 |

### Correct workflow

```text
security order -> funding check -> linked FX order -> FX confirmation -> cash reservation -> security settlement -> position and cash posting
```

### Failure states

| Failure | Required behavior |
|---|---|
| FX trade missing | Security trade is blocked, prefunded, bridged or escalated by policy. |
| FX settles late | EUR cash remains unavailable; linked security settlement is at risk. |
| FX rate changes before execution | Buying-power estimate and actual cash posting retain separate rates. |
| One FX leg fails | Cash is restricted and linked settlement workflow is exceptioned. |

## Example 9. Income Reversal After Entitlement Error

### Scenario

A coupon payment of 5,000 was booked to a client account. Custodian later confirms the account was not entitled because the bond was sold before record date.

| Attribute | Value |
|---|---:|
| Original gross coupon | 5,000 |
| Withholding tax | 500 |
| Net cash credited | 4,500 |
| Correct entitlement | 0 |

### Correct reversal

| Posting | Treatment |
|---|---|
| Gross income reversal | Reverse 5,000 income with source reason. |
| Tax reversal | Reverse or reclaim 500 withholding according to custodian/tax source. |
| Net cash reversal | Debit 4,500 or create receivable if cash is unavailable. |
| Report update | Mark impacted statement/income report for correction review. |

### QA assertions

| Test | Expected result |
|---|---|
| Cash is insufficient for reversal | Receivable/exception workflow opens. |
| Tax cannot be reversed immediately | Tax reclaim or pending tax adjustment is tracked. |
| Performance period is closed | Performance restatement decision is recorded. |
| Original income is audited | Original and reversal remain visible with lineage. |

## Example 10. Trade Cancellation After Booking

### Scenario

A trade is booked and reported on trade date, but the broker cancels it before settlement because the execution was erroneous.

### Correct cancellation workflow

| Artifact | Required action |
|---|---|
| Order | Preserve original order and cancellation reason. |
| Execution | Mark cancelled or busted with broker evidence. |
| Trade | Reverse booked trade or mark cancelled according to book-of-record policy. |
| Cash reservation | Release pending cash reservation. |
| Position | Remove trade-date exposure or create reversal entry. |
| Performance | Reverse any provisional performance impact. |
| Report | Flag impacted report output if already published. |

### QA assertions

| Test | Expected result |
|---|---|
| Trade is cancelled before settlement | No settled cash or settled position remains. |
| Trade was included in a report | Report correction workflow identifies impacted line items. |
| Fees were already posted | Fee reversal or broker adjustment is linked to cancellation. |
| User tries to delete trade | Deletion is blocked; cancellation is versioned. |

## Example 11. Fee And Tax Posting After Trade Settlement

### Scenario

An equity purchase settles, then stamp duty and exchange fees arrive in a separate file after settlement.

| Attribute | Value |
|---|---:|
| Gross consideration | 50,000 |
| Broker commission | 75 |
| Exchange fee | 10 |
| Stamp duty | 100 |
| Total cash cost | 50,185 |

### Correct treatment

```text
total cash cost = gross consideration + commission + exchange fee + stamp duty
total cash cost = 50,000 + 75 + 10 + 100 = 50,185
```

| Area | Treatment |
|---|---|
| Cash ledger | Fees/taxes post as separate legs or linked adjustments. |
| Cost basis | Inclusion depends on configured accounting/tax policy. |
| Performance | Fees reduce return according to methodology. |
| Reconciliation | Late fee file creates expected adjustment, not unexplained cash break. |

## Example 12. Performance Restatement From Trade Correction

### Scenario

A trade price was corrected from 24.10 to 24.00 after month-end performance was published.

| Attribute | Original | Corrected |
|---|---:|---:|
| Quantity | 10,000 | 10,000 |
| Trade price | 24.10 | 24.00 |
| Consideration | 241,000 | 240,000 |

### Impact

```text
cash difference = original consideration - corrected consideration
cash difference = 241,000 - 240,000 = 1,000
```

Correct workflow:

- store corrected trade price and source reason;
- post cash/position/lot adjustment according to correction policy;
- identify impacted performance periods, reports, mandate checks and fees;
- decide whether to restate, annotate or carry forward based on materiality;
- preserve original performance output and corrected output.

## Example 13. Reconciliation Break Closure

### Scenario

Internal cash shows USD 1,000,000 while custodian cash shows USD 997,500. The difference is traced to a missing fee and a pending FX cash leg.

| Break component | Amount |
|---|---:|
| Missing custody fee | -500 |
| Pending FX settlement leg | -2,000 |
| Total break | -2,500 |

### Closure workflow

| Step | Treatment |
|---|---|
| Detect | Identify account, currency, date, source balances and tolerance breach. |
| Classify | Split break into known pending item, missing posting, timing difference or unknown. |
| Resolve | Book missing fee and link pending FX leg to expected settlement. |
| Evidence | Attach custodian statement, fee file and FX confirmation. |
| Close | Close break only after balances reconcile or approved exception remains. |

### QA assertions

| Test | Expected result |
|---|---|
| Break is within tolerance but client-visible | Materiality policy decides whether closure is allowed. |
| Break has multiple causes | Closure stores component-level explanation. |
| Pending item passes expected date | Break reopens or escalates. |
| Manual adjustment is used | Adjustment requires reason, approver and source evidence. |

## Example 14. Multi-Market Allocation With Local Charges

### Scenario

A portfolio manager places one approved block order for 12,000 shares of the same issuer. Liquidity is sourced across two trading venues with different currencies, fees and settlement calendars.

| Market | Quantity | Local price | Currency | Local fees and taxes |
|---|---:|---:|---|---:|
| Market A | 7,000 | 18.20 | USD | 145 |
| Market B | 5,000 | 24.70 | SGD | 110 |

| Account | Approved allocation | Filled quantity |
|---|---:|---:|
| Account A | 50% | 6,000 |
| Account B | 30% | 3,600 |
| Account C | 20% | 2,400 |

### Correct workflow

| Step | Treatment |
|---|---|
| Capture fills | Preserve venue, currency, price, timestamp, broker and local settlement cycle for each fill. |
| Normalize economics | Convert reporting-currency analytics separately from source local-currency cash settlement. |
| Allocate fairly | Allocate quantities, consideration, fees and taxes by approved method, with rounding controls. |
| Validate eligibility | Check market, custody, tax, mandate and restricted-list eligibility at account level. |
| Book lineage | Link each account allocation back to each execution slice and settlement instruction. |

### QA assertions

| Test | Expected result |
|---|---|
| One market has a different settlement date | Account allocation stores market-specific pending settlement legs. |
| Local fees are posted only at parent order level | Allocation fails until account-level fee/tax treatment is derived or sourced. |
| One account is ineligible for Market B | Allocation is blocked, rebalanced or escalated under governed workflow. |
| Average price is displayed | Weighted price is labelled by market/currency basis, not presented as a single unsupported all-in price. |

## Example 15. Settlement Netting Across Same Value Date

### Scenario

The same account has three USD equity trades settling with the same custodian and broker on the same value date.

| Trade | Direction | Settlement amount |
|---|---|---:|
| Trade 1 | Buy | -80,000 |
| Trade 2 | Sell | 52,000 |
| Trade 3 | Sell | 21,500 |

### Calculation

```text
net settlement cash = sum(sell proceeds) - sum(buy consideration)
net settlement cash = 52,000 + 21,500 - 80,000
net settlement cash = -6,500
```

### Correct platform behavior

| Area | Treatment |
|---|---|
| Trade economics | Keep all three trades gross for position, lot, tax and performance. |
| Settlement instruction | Create or receive one net cash settlement instruction if market and custodian rules allow. |
| Reconciliation | Reconcile gross internal trade legs to net external cash movement. |
| Reporting | Do not hide gross buys and sells merely because cash settled net. |

### QA assertions

| Test | Expected result |
|---|---|
| Net cash equals external statement | Break closes with component-level trade evidence. |
| One gross trade fails | Netting group is reopened or partially repaired according to source status. |
| Trades have different value dates | They are not netted unless source explicitly confirms a valid netting arrangement. |
| Fees arrive separately | Net cash and fee/tax postings remain independently traceable. |

## Example 16. Custody Account Transfer Without Sale

### Scenario

A client moves 3,000 shares from one custody account to another account under the same reporting perimeter. The transfer is free-of-payment and should not be treated as a sale or contribution.

| Attribute | Value |
|---|---|
| Source custody account | Account A |
| Target custody account | Account B |
| Security quantity | 3,000 |
| Cash consideration | 0 |
| Transfer type | Free-of-payment internal custody transfer |

### Correct treatment

| Area | Treatment |
|---|---|
| Source account | Position decreases by 3,000 with transfer-out classification. |
| Target account | Position increases by 3,000 with transfer-in classification. |
| Cost basis | Original lot basis and acquisition date are preserved if policy and source support it. |
| Performance | Treated as internal transfer within the reporting perimeter, not investment gain or external cashflow. |
| Reconciliation | Source and target custody confirmations must both be matched. |

### QA assertions

| Test | Expected result |
|---|---|
| Transfer is posted as sell/buy | QA fails because realized P&L and turnover would be overstated. |
| Target leg arrives before source leg | Temporary in-transit state is visible and reconciled. |
| Lot details are missing | Transfer remains supportable for position but cost-basis portability is flagged. |
| Reporting perimeter changes | Classification is re-evaluated as internal transfer, contribution or withdrawal. |

## Example 17. Securities Lending Recall Before Sale

### Scenario

An account owns 10,000 shares, of which 4,000 are on loan. The manager sells 6,000 shares. Operations must recall enough lent shares before settlement.

| Attribute | Value |
|---|---:|
| Total position | 10,000 |
| Lent quantity | 4,000 |
| Unlent quantity | 6,000 |
| Sale quantity | 6,000 |
| Recall required | 0 if only unlent shares are sold; more if lending source restricts availability |

### Correct workflow

| Step | Treatment |
|---|---|
| Availability check | Distinguish total position from available deliverable position. |
| Recall decision | Determine whether recall is required based on settlement date, lending agreement and inventory source. |
| Recall instruction | Link recall to sale trade, loan identifier, counterparty and expected return date. |
| Settlement monitor | Escalate if recall does not complete before security delivery deadline. |
| Income treatment | Track manufactured dividends or lending fees separately from ordinary dividends. |

### QA assertions

| Test | Expected result |
|---|---|
| Sale quantity exceeds deliverable inventory | Trade is blocked, recalled or escalated before settlement fail. |
| Recall completes late | Settlement fail workflow opens with loan and sale lineage. |
| Corporate action occurs during loan | Manufactured income or entitlement treatment is explicitly sourced. |
| Lending fee is received | Fee is classified separately from investment dividend income. |

## Example 18. Failed Corporate-Action Election

### Scenario

A voluntary tender offer allows cash election, stock election or no action. The advisor submits a cash election, but the custodian rejects it because the instruction missed the market deadline.

| Attribute | Value |
|---|---|
| Event | Voluntary tender offer |
| Client instruction | Cash election |
| Custodian status | Rejected |
| Rejection reason | After market deadline |
| Default outcome | No action or default option per event terms |

### Correct treatment

| Area | Treatment |
|---|---|
| Election record | Preserve requested election, timestamp, channel, user and client consent evidence. |
| Source status | Store custodian acceptance, rejection or pending status separately from internal instruction. |
| Entitlement | Apply only accepted election or default terms to cash/security postings. |
| Client service | Trigger exception workflow and explanation when requested outcome cannot be delivered. |
| Reporting | Show final confirmed outcome, not the failed requested election as if accepted. |

### QA assertions

| Test | Expected result |
|---|---|
| Internal election is submitted but source rejects | No elected cash/security posting is created from the rejected instruction. |
| Default option applies | Entitlement is booked from default terms with rejected-election lineage. |
| Advisor changes instruction after deadline | Workflow blocks or records late unsupported status. |
| Client report is produced during pending state | Report labels election as pending or unsupported, not confirmed. |

## Example 19. Nostro Cash Break From Value-Date Mismatch

### Scenario

Internal cash expects a EUR 250,000 credit from an FX settlement on Tuesday. The bank nostro statement shows the credit on Wednesday due to a correspondent-bank delay.

| Item | Amount | Value date |
|---|---:|---|
| Internal expected FX credit | 250,000 | Tuesday |
| Nostro statement credit | 250,000 | Wednesday |
| Break on Tuesday | 250,000 | Tuesday |

### Correct workflow

| Step | Treatment |
|---|---|
| Detect | Compare expected cash ledger to nostro statement by account, currency and value date. |
| Classify | Mark as timing break if source confirms matched amount with different value date. |
| Restrict | Do not make cash available for downstream settlement until availability policy allows. |
| Resolve | Close break when Wednesday statement confirms receipt or escalate if unmatched. |
| Evidence | Store statement line, FX confirmation and correspondent-bank reference. |

### QA assertions

| Test | Expected result |
|---|---|
| Amount matches but value date differs | Break is classified as timing, not silently ignored. |
| Client cash availability is requested Tuesday | Availability uses governed policy and pending status. |
| Credit does not arrive Wednesday | Break escalates from timing to unresolved cash break. |
| Manual cash adjustment is entered | Adjustment requires evidence and does not replace source reconciliation. |

## Example 20. Omnibus Position With Internal Sub-Account Allocation

### Scenario

A custodian reports one omnibus position of 100,000 fund units. The wealth platform maintains internal sub-account allocations for three clients.

| Client sub-account | Internal units |
|---|---:|
| Client A | 40,000 |
| Client B | 35,000 |
| Client C | 25,000 |
| Total internal allocation | 100,000 |

### Control model

| Control | Treatment |
|---|---|
| Omnibus reconciliation | Sum internal sub-account units to external custodian omnibus position. |
| Privacy | Do not expose other sub-account holdings in client-facing views. |
| Orders | Allocate subscriptions/redemptions to sub-accounts before or after transfer-agent confirmation according to source model. |
| Fees and distributions | Allocate cash, units, taxes and fees based on confirmed sub-account entitlement. |
| Break handling | Identify whether break belongs to external omnibus position, internal allocation, price/NAV or pending order. |

### QA assertions

| Test | Expected result |
|---|---|
| Internal sum differs from custodian omnibus | Reconciliation break opens at allocation layer. |
| Client report is generated | Only the client's sub-account holding is visible. |
| Distribution is posted at omnibus level | Allocation to sub-accounts uses entitlement evidence and rounding policy. |
| One sub-account order is rejected | Omnibus and internal allocation states remain reconcilable. |

## Example 21. Market Claim After Record-Date Trade

### Scenario

An equity dividend has an ex-date before trade date, but settlement and custodian processing cause the cash to be paid to the wrong party. A market claim is required to move entitlement from seller to buyer or buyer to seller according to market rules.

| Attribute | Value |
|---|---|
| Corporate action | Cash dividend |
| Dividend per share | 0.80 |
| Affected quantity | 5,000 |
| Claim amount | 4,000 before tax and fees |

### Calculation

```text
claim amount = affected quantity x dividend per share
claim amount = 5,000 x 0.80
claim amount = 4,000
```

### Correct workflow

| Step | Treatment |
|---|---|
| Identify entitlement mismatch | Compare trade date, ex-date, record date, settlement date and custody entitlement. |
| Raise or receive claim | Create market-claim receivable/payable with counterparty, custodian and event lineage. |
| Book cash movement | Post claim cash only when confirmed or according to controlled accrual policy. |
| Update income reporting | Attribute income to the economically entitled account, with correction notes when needed. |
| Reconcile | Close claim only after cash/security movement and source confirmation match. |

### QA assertions

| Test | Expected result |
|---|---|
| Dividend is paid to wrong account | Market-claim workflow creates receivable/payable rather than manual income overwrite. |
| Claim is disputed | Claim remains pending with ageing and escalation. |
| Tax differs from original dividend | Gross, tax and net claim components are stored separately. |
| Report was already issued | Restatement or next-period correction decision is recorded. |

## Example 22. Intraday Settlement Cut-Off Miss

### Scenario

A same-day securities settlement instruction is ready internally, but the custodian cut-off is missed because affirmation completed late. The trade remains economically valid, but settlement risk and client cash availability must reflect the missed cut-off.

| Attribute | Value |
|---|---|
| Trade value | 750,000 |
| Market settlement date | T+0 |
| Custodian cut-off | 14:00 |
| Affirmation completed | 14:18 |
| Revised settlement expectation | T+1 or manual exception |

### Correct workflow

| Step | Treatment |
|---|---|
| Detect cut-off miss | Compare market, custodian, currency and instruction cut-offs with actual affirmation timestamp. |
| Update settlement state | Mark instruction as missed cut-off or pending exception, not settled. |
| Cash availability | Keep settlement cash reserved or projected according to policy. |
| Escalation | Notify operations, advisor and liquidity owner when client impact is material. |
| Evidence | Preserve affirmation timestamp, custodian status, cut-off calendar and repair instruction. |

### QA assertions

| Test | Expected result |
|---|---|
| Affirmation completes after cut-off | Settlement instruction is not marked settled. |
| Cash is requested for other use | Reserved settlement cash remains restricted until settlement state resolves. |
| Manual exception is approved | Exception includes authority, timestamp and custodian reference. |
| Report is generated | Trade shows pending or delayed settlement state with reason. |

## Example 23. Partial Custody Transfer

### Scenario

A client transfers a multi-asset custody portfolio to a new custodian. Equities transfer on time, but bonds and funds remain pending. Reporting must show transferred, pending and failed legs without treating the transfer as trading activity.

| Asset class | Quantity/value | Transfer status |
|---|---:|---|
| Equities | 1,200,000 | Completed |
| Bonds | 850,000 | Pending lot detail |
| Funds | 400,000 | Rejected by transfer agent |

### Transfer progress

```text
total_transfer_value = 1,200,000 + 850,000 + 400,000 = 2,450,000
completed_transfer_ratio = 1,200,000 / 2,450,000 = 48.98%
pending_or_failed_value = 1,250,000
```

### Correct workflow

| Step | Treatment |
|---|---|
| Position transfer | Move only completed custody legs into confirmed target account positions. |
| Pending legs | Keep pending assets in in-transfer or source-custody state. |
| Rejected legs | Route fund transfer-agent rejection to repair workflow. |
| Tax lots | Preserve lot portability state separately by asset class and custodian file. |
| Reporting | Show transfer progress and exceptions without booking sells or buys. |

### QA assertions

| Test | Expected result |
|---|---|
| Only equity leg completes | Equity moves while bonds/funds remain pending or rejected. |
| Fund transfer is rejected | No target fund position is created without transfer-agent confirmation. |
| Bond lot file is pending | Position may be visible but tax-lot reporting is incomplete. |
| Performance report is generated | Internal transfer does not create external cashflow or realized P&L. |

## Example 24. Buy-In Exposure After Settlement Fail

### Scenario

A short-delivered equity sale fails to settle. The market may initiate buy-in after the fail window. Operations must measure buy-in exposure and client impact without prematurely booking final loss.

| Attribute | Value |
|---|---:|
| Failed delivery quantity | 8,000 |
| Original sale price | 42.00 |
| Current market buy-in price | 44.25 |
| Potential buy-in fee | 1,500 |

### Exposure estimate

```text
price_exposure = 8,000 x (44.25 - 42.00) = 18,000
estimated_buy_in_exposure = 18,000 + 1,500 = 19,500
```

### Correct workflow

| Step | Treatment |
|---|---|
| Fail monitoring | Track failed quantity, age, market, broker and buy-in eligibility date. |
| Exposure estimate | Estimate potential buy-in cost separately from realized cash settlement. |
| Client impact | Escalate when buy-in risk affects cash, performance or client commitments. |
| Resolution | Close exposure only when delivery, buy-in, cancellation or compensation is source-confirmed. |
| Reporting | Label buy-in amount as estimate until final market notice is received. |

### QA assertions

| Test | Expected result |
|---|---|
| Fail reaches buy-in window | Buy-in exposure workflow opens. |
| Market price changes | Estimated exposure recalculates without booking realized loss. |
| Buy-in executes | Final cost posts with market notice and settlement evidence. |
| Delivery completes before buy-in | Exposure closes without buy-in loss. |

## Example 25. Tax-Lot Transfer Portability

### Scenario

A custodian transfer includes position quantity immediately, but acquisition dates and cost basis arrive in a later tax-lot file. The platform must support position continuity while keeping tax reporting incomplete until lots reconcile.

| Lot state | Quantity | Cost basis status |
|---|---:|---|
| Position transferred | 5,000 | n/a |
| Lots received | 3,500 | Complete |
| Lots pending | 1,500 | Missing |

### Reconciliation

```text
lot_quantity_break = transferred_quantity - received_lot_quantity
lot_quantity_break = 5,000 - 3,500 = 1,500
lot_coverage = 3,500 / 5,000 = 70.00%
```

### Correct workflow

| Step | Treatment |
|---|---|
| Position continuity | Show transferred position from custody confirmation. |
| Tax status | Mark tax-lot basis as partial until lots reconcile to full quantity. |
| Sale before completion | Block or label realized gain/loss according to jurisdiction and policy. |
| Evidence | Preserve old custodian lot ids, acquisition dates, cost basis and transfer date. |
| Reconciliation | Close portability exception when lot quantity and position quantity match. |

### QA assertions

| Test | Expected result |
|---|---|
| Position arrives before lots | Holding is visible but tax-lot reporting is partial. |
| Sale occurs with missing lots | Realized tax result is provisional or blocked by policy. |
| Final lot file arrives | Lot coverage reaches 100% and exception closes. |
| Lot quantity exceeds position | Reconciliation break opens rather than silently trimming lots. |

## Example 26. Cross-Custodian SSI Migration

### Scenario

A client migrates settlement instructions from Custodian A to Custodian B. Trades booked during the transition must use the correct standing settlement instruction by market, currency and effective date.

| Attribute | Value |
|---|---|
| Old SSI effective until | 2026-06-30 |
| New SSI effective from | 2026-07-01 |
| Trade date | 2026-06-28 |
| Settlement date | 2026-07-02 |
| Market rule | Settlement-date SSI applies |

### Correct workflow

| Step | Treatment |
|---|---|
| Effective dating | Resolve SSI by market rule, account, currency and settlement date. |
| Instruction generation | Use new SSI when settlement-date rule applies after migration date. |
| Repair | Detect mismatched SSI before settlement fail when possible. |
| Evidence | Preserve old SSI, new SSI, approval, market rule and instruction version. |
| Reporting | Show SSI migration as operational state, not a change in product exposure. |

### QA assertions

| Test | Expected result |
|---|---|
| Settlement date falls after migration | New SSI is used where settlement-date rule applies. |
| Trade-date rule applies in a market | Old SSI remains valid for trades before effective date. |
| SSI approval is missing | Instruction generation is blocked or routed to repair. |
| Custodian rejects instruction | Repair workflow preserves rejected and corrected SSI versions. |

## Example 27. Operational Incident Communication

### Scenario

A settlement processing incident delays confirmations for multiple clients. Operations must communicate status, impact and remediation without exposing unrelated client data or making unsupported completion claims.

| Attribute | Value |
|---|---:|
| Impacted trades | 42 |
| Impacted clients | 18 |
| Confirmed settled | 31 |
| Pending confirmation | 11 |
| Material client cash impact | 3 clients |

### Incident metrics

```text
settled_confirmation_ratio = 31 / 42 = 73.81%
pending_confirmation_ratio = 11 / 42 = 26.19%
material_cash_impact_rate = 3 / 18 = 16.67%
```

### Correct workflow

| Step | Treatment |
|---|---|
| Incident scope | Track impacted trades, clients, markets, custodians and status. |
| Communication | Send role-appropriate updates to advisors, operations and affected clients where required. |
| Confidentiality | Do not include other client names, holdings or trade details in client communication. |
| Evidence | Preserve incident id, timeline, status snapshots, approvals and remediation actions. |
| Closure | Close only after settlement confirmations, reconciliation and client-impact review complete. |

### QA assertions

| Test | Expected result |
|---|---|
| Incident update is generated | Message includes status, impact and next update time without unsupported finality. |
| Client-specific notice is needed | Notice includes only authorized client-specific information. |
| Pending confirmations remain | Incident cannot be closed as fully resolved. |
| Post-incident review completes | Root cause, corrective actions and evidence are linked to affected trades. |

## Example 28. Trade Affirmation Mismatch Repair

### Scenario

A bond trade is confirmed by the broker, but the affirmation file from the custodian shows a different accrued interest amount. The platform must prevent settlement from being treated as clean until the mismatch is resolved or approved as an exception.

| Field | Internal trade | Custodian affirmation | Difference |
|---|---:|---:|---:|
| Nominal | 2,000,000 | 2,000,000 | 0 |
| Clean consideration | 1,984,000 | 1,984,000 | 0 |
| Accrued interest | 18,750 | 19,125 | 375 |
| Total settlement cash | 2,002,750 | 2,003,125 | 375 |

### Mismatch amount

```text
affirmation_cash_break = custodian_total_settlement_cash - internal_total_settlement_cash
affirmation_cash_break = 2,003,125 - 2,002,750 = 375
```

### Correct workflow

| Step | Treatment |
|---|---|
| Match attempt | Compare trade id, account, nominal, price, accrued interest, settlement date and counterparty. |
| Break classification | Classify as accrued-interest mismatch, not price movement or fee adjustment. |
| Repair | Route to trade support with source day-count, settlement calendar and broker/custodian evidence. |
| Settlement gating | Keep settlement state pending or exception-approved until repair completes. |
| Reporting | Show pending affirmation state with break reason and materiality. |

### QA assertions

| Test | Expected result |
|---|---|
| Accrued interest differs | Affirmation break opens with amount and source fields. |
| Broker confirms internal value | Repair records broker evidence and corrected custodian response. |
| Exception is approved | Settlement proceeds with approval lineage and unresolved-break label where required. |
| Report is generated before repair | Trade is not shown as fully affirmed. |

## Example 29. Corporate-Action Election Escalation

### Scenario

A voluntary tender offer requires client election before the custodian deadline. The advisor submits the instruction late, leaving insufficient time for operations to validate, approve and submit the election.

| Attribute | Value |
|---|---|
| Client eligible quantity | 10,000 shares |
| Client elected quantity | 7,500 shares |
| Internal instruction cut-off | 14:00 |
| Advisor submission time | 14:35 |
| Custodian deadline | 16:00 |

### Deadline breach

```text
internal_cutoff_miss_minutes = 35
remaining_minutes_to_custodian_deadline = 85
election_submission_risk = late_internal_instruction and custodian_deadline_not_yet_passed
```

### Correct workflow

| Step | Treatment |
|---|---|
| Eligibility check | Validate eligible position, election quantity and client authority. |
| Late instruction | Escalate to operations and supervisor with cut-off breach reason. |
| Submission decision | Submit only if authority, validation and custodian acceptance can still be evidenced. |
| Failure handling | If not submitted, preserve late instruction, failed election state and client impact analysis. |
| Reporting | Show election status separately from underlying holding until event completion. |

### QA assertions

| Test | Expected result |
|---|---|
| Advisor submits after internal cut-off | Escalation workflow opens automatically. |
| Custodian deadline has passed | Election is rejected or marked missed with evidence. |
| Custodian accepts late instruction | Accepted election carries late-submission approval lineage. |
| Quantity exceeds eligibility | Workflow blocks over-election or routes it to repair. |

## Example 30. Failed FX Settlement Compensation

### Scenario

An FX spot trade linked to a securities purchase fails to settle because the counterparty misses the value-date payment. The client incurs replacement funding cost while the securities settlement cash remains reserved.

| Attribute | Value |
|---|---:|
| Failed FX receive amount | 1,500,000 |
| Original value date | T+2 |
| Replacement funding rate | 5.40% |
| Contractual compensation rate | 3.90% |
| Delay days | 3 |
| Day-count basis | 360 |

### Compensation gap

```text
replacement_funding_cost = 1,500,000 x 5.40% x 3 / 360 = 675.00
contractual_compensation = 1,500,000 x 3.90% x 3 / 360 = 487.50
unrecovered_cost = 675.00 - 487.50 = 187.50
```

### Correct workflow

| Step | Treatment |
|---|---|
| Fail recognition | Track failed currency, value date, counterparty, linked security trade and cash reservation. |
| Replacement funding | Estimate funding cost separately from final compensation receivable. |
| Claim workflow | File counterparty claim with rate, day count, value date and evidence. |
| Cash treatment | Do not release reserved settlement cash until security settlement and FX repair resolve. |
| Reporting | Label compensation as receivable, disputed or received based on source state. |

### QA assertions

| Test | Expected result |
|---|---|
| FX receive fails on value date | Linked security funding state becomes exceptioned. |
| Funding cost exceeds compensation | Unrecovered cost is measurable and separately reported. |
| Counterparty pays claim | Receivable closes against confirmed cash. |
| Claim is disputed | Compensation remains non-cash and dispute evidence is retained. |

## Example 31. Omnibus Allocation Rounding Dispute

### Scenario

A fund order settles at omnibus level, but client-level allocation creates a residual because units must be rounded to three decimals. The residual cannot be hidden in one client account without an approved allocation policy.

| Client | Target cash allocation | NAV | Rounded units |
|---|---:|---:|---:|
| Client A | 100,000.00 | 12.3456 | 8,100.052 |
| Client B | 75,000.00 | 12.3456 | 6,075.039 |
| Client C | 25,000.00 | 12.3456 | 2,025.013 |
| Omnibus confirmation | 200,000.00 | 12.3456 | 16,200.105 |

### Rounding break

```text
sum_client_units = 8,100.052 + 6,075.039 + 2,025.013 = 16,200.104
omnibus_unit_break = 16,200.105 - 16,200.104 = 0.001
cash_equivalent_break = 0.001 x 12.3456 = 0.0123456
```

### Correct workflow

| Step | Treatment |
|---|---|
| Allocation calculation | Apply configured rounding precision and residual policy. |
| Break handling | Track residual units and cash equivalent instead of forcing hidden adjustment. |
| Dispute | Route disputed residuals to operations with omnibus confirmation and allocation run evidence. |
| Client fairness | Allocate residual only under approved pro-rata, largest-remainder or house-account policy. |
| Reporting | Reconcile client holdings to omnibus custody after residual handling. |

### QA assertions

| Test | Expected result |
|---|---|
| Rounded client units do not equal omnibus units | Rounding break is visible with residual quantity. |
| No residual policy exists | Allocation cannot be marked final. |
| Policy assigns residual to house account | Client allocations remain policy-consistent and auditable. |
| NAV correction arrives | Allocation and residual calculation re-run with version lineage. |

## Example 32. Settlement Penalty Pass-Through

### Scenario

A market settlement fail generates a penalty. Operations must determine whether the penalty is absorbed, passed to a client, charged to a broker or allocated across multiple contributing causes.

| Cause | Responsible party | Penalty amount |
|---|---|---:|
| Late client funding | Client | 320 |
| Broker confirmation delay | Broker | 180 |
| Internal SSI repair delay | Internal operations | 95 |
| Total penalty | Mixed | 595 |

### Pass-through amount

```text
client_pass_through = 320
broker_claim = 180
internal_absorb = 95
total_penalty = 320 + 180 + 95 = 595
```

### Correct workflow

| Step | Treatment |
|---|---|
| Penalty ingestion | Capture market penalty id, fail id, settlement date, amount and currency. |
| Cause attribution | Link penalty to funding, confirmation, SSI, custody or market-infrastructure cause. |
| Approval | Require policy and approval before client pass-through. |
| Recovery | Track broker or custodian claim separately from client charge and internal absorption. |
| Reporting | Show penalty as cost, claim, waived amount or absorbed amount according to final decision. |

### QA assertions

| Test | Expected result |
|---|---|
| Penalty has mixed causes | Allocation records client, broker and internal components separately. |
| Client charge lacks approval | Pass-through is blocked. |
| Broker accepts claim | Broker receivable closes when cash is received. |
| Penalty is waived | Waiver evidence is retained and no client charge is booked. |

## Example 33. Post-Incident Control Attestation

### Scenario

After a settlement incident, management requires evidence that corrective controls were implemented, tested and attested before the incident can be closed for governance reporting.

| Control action | Owner | Evidence state |
|---|---|---|
| Cut-off monitoring alert | Operations technology | Implemented |
| SSI repair dashboard | Operations | Tested |
| Daily fail ageing review | Settlement team | Attested |
| Client-impact review checklist | Client service | Pending |

### Attestation coverage

```text
completed_control_actions = 3
required_control_actions = 4
control_attestation_coverage = 3 / 4 = 75%
open_actions = 1
```

### Correct workflow

| Step | Treatment |
|---|---|
| Incident closure | Require linked root cause, impacted trades, remediation actions and control owners. |
| Evidence | Preserve test result, reviewer, timestamp and exception decision for each action. |
| Attestation | Allow closure only when mandatory actions are implemented or formally risk-accepted. |
| Knowledge management | Link lessons learned to runbooks, reconciliation checks and monitoring dashboards. |
| Reporting | Separate operational recovery from governance closure when controls remain open. |

### QA assertions

| Test | Expected result |
|---|---|
| One mandatory control remains pending | Governance closure is blocked or risk-accepted with approval. |
| Evidence is uploaded without reviewer | Attestation remains incomplete. |
| Control test fails | Incident stays open for corrective action. |
| All mandatory actions pass | Incident closure includes control evidence and durable runbook links. |

# 02 — Fund Lifecycle, Transactions and Position Modelling

## 1. Why Fund Lifecycle Matters

Funds have a different lifecycle from equities, bonds and notes.

For listed equities and ETFs, the client usually buys and sells immediately in the market at a known or near-known price.

For many mutual funds and unit trusts, the client places an order before a cut-off time and receives the next available NAV only later. This is known as **forward pricing**.

For hedge funds and private market funds, lifecycle can be even more complex: notice periods, lock-ups, gates, estimated NAVs, final NAV adjustments, capital calls, distributions and delayed settlement.

Platform design must therefore distinguish:

```text
Order date
Trade/dealing date
NAV date
Price confirmation date
Settlement date
Booking date
Performance effective date
```

These dates may not be the same.

---

## 2. Generic Fund Lifecycle

| Stage | Business Event | System Object | Transaction? |
|---|---|---|---|
| Product setup | Fund/share class onboarded | Instrument master, fund terms | No |
| Client order | Subscription/redemption/switch order placed | Order | No, until confirmed/booked |
| Eligibility check | Suitability, jurisdiction, minimum, risk profile | Advisory/order validation | No |
| Cut-off | Order accepted for dealing cycle | Order status update | No |
| Dealing | Fund processes order for NAV date | Dealing event | No or pending transaction |
| NAV publication | NAV confirmed by fund administrator | Price/NAV record | No |
| Allocation | Units or cash amount confirmed | Confirmation | Usually yes |
| Settlement | Cash paid/received | Cash/security transaction | Yes |
| Holding period | NAV changes daily/periodically | Valuation records | No |
| Distribution | Income/capital paid or reinvested | Distribution event | Yes |
| Corporate action | Split, merger, liquidation, class conversion | Corporate action event | Yes if position/cash changes |
| Redemption | Investor exits fully/partially | Redemption transaction | Yes |
| Closure | Position quantity becomes zero | Position status | No separate transaction needed |

---

## 3. Key Fund Dates

| Date | Meaning | Why It Matters |
|---|---|---|
| Order date | Client places order | Advisor/client timeline |
| Cut-off datetime | Latest time to submit for a dealing cycle | Determines eligible NAV date |
| Trade/dealing date | Fund processes order | Economic exposure start/end date |
| NAV date | NAV used for pricing | Determines units/cash amount |
| Price confirmation date | NAV and units confirmed | Booking accuracy |
| Settlement date | Cash and units settle | Cash availability and failed-settlement controls |
| Booking date | Recorded in platform/accounting system | Operational audit |
| Value date | Cash value date | Cash ledger and interest calculations |

Common issue:

```text
A fund order entered today may not become a confirmed transaction until NAV is available tomorrow or later.
```

---

## 4. Order Types

| Order Type | Meaning |
|---|---|
| Amount-based subscription | Client invests a cash amount; units are calculated after NAV. |
| Unit-based subscription | Client requests a number of units; cash amount is calculated after NAV. Less common for mutual funds. |
| Full redemption | Client redeems entire position. |
| Partial redemption by units | Client redeems specified number of units. |
| Partial redemption by amount | Client requests target cash amount; units calculated after NAV. |
| Switch by amount | Redeem one fund and subscribe into another by amount. |
| Switch by units | Redeem units from one fund and subscribe into another. |
| Regular savings plan | Repeating subscription schedule. |
| Regular withdrawal plan | Repeating redemption/distribution withdrawal. |
| Dividend reinvestment | Distribution automatically reinvested. |

---

## 5. Recommended Transaction Types

A robust fund platform should use a small, controlled transaction taxonomy.

| Transaction Type | Use |
|---|---|
| FUND_SUBSCRIPTION | Primary subscription into open-ended fund/unit trust. |
| FUND_REDEMPTION | Redemption from open-ended fund/unit trust. |
| FUND_SECONDARY_BUY | Exchange/secondary-market buy, common for ETF/closed-end fund. |
| FUND_SECONDARY_SELL | Exchange/secondary-market sell. |
| FUND_SWITCH_OUT | Redemption leg of fund switch. |
| FUND_SWITCH_IN | Subscription leg of fund switch. |
| FUND_TRANSFER_IN | Transfer of fund units into portfolio. |
| FUND_TRANSFER_OUT | Transfer of fund units out of portfolio. |
| FUND_DISTRIBUTION_CASH | Distribution paid in cash. |
| FUND_DISTRIBUTION_REINVEST | Distribution reinvested into more units. |
| FUND_REINVESTMENT_BUY | Units purchased using reinvested distribution. |
| FUND_RETURN_OF_CAPITAL | Distribution classified as return of capital. |
| FUND_CAPITAL_GAIN_DISTRIBUTION | Capital-gain distribution. |
| FUND_TAX_WITHHOLDING | Tax withheld from distribution/redemption. |
| FUND_FEE | Explicit platform/advisory/custody fee related to fund. |
| FUND_LOAD_CHARGE | Front-end/back-end/sales charge when explicit. |
| FUND_REBATE | Trailer/platform rebate credited to client. |
| FUND_UNIT_SPLIT | Unit split/consolidation. |
| FUND_MERGER_OUT | Old fund/share class removed due to merger. |
| FUND_MERGER_IN | New fund/share class received due to merger. |
| FUND_CLASS_CONVERSION_OUT | Old share class removed. |
| FUND_CLASS_CONVERSION_IN | New share class received. |
| FUND_LIQUIDATION_REDEMPTION | Fund liquidation cash payment. |
| FUND_SIDE_POCKET_TRANSFER | Transfer to side-pocket or restricted class. |
| FUND_GATE_PARTIAL_REDEMPTION | Partial redemption due to gate. |
| FUND_NAV_ADJUSTMENT | Correction due to NAV restatement, if booked as transaction. |
| FUND_CORRECTION_REVERSAL | Reversal/correction of incorrect transaction. |
| PRIVATE_FUND_COMMITMENT | Investor commitment to private fund. |
| PRIVATE_FUND_CAPITAL_CALL | Capital called and paid. |
| PRIVATE_FUND_DISTRIBUTION | Cash/security distribution from private fund. |
| PRIVATE_FUND_RECALLABLE_DISTRIBUTION | Distribution that may be recalled. |
| PRIVATE_FUND_COMMITMENT_TRANSFER | Transfer/sale of fund interest. |

Do not create a new transaction type for every fund category. Use the economic effect.

---

## 6. Fund Subscription Lifecycle

### 6.1 Amount-Based Subscription Example

Client invests SGD 100,000 into a unit trust.

| Item | Value |
|---|---:|
| Subscription amount | SGD 100,000 |
| Sales charge | 2% |
| Net investment amount | SGD 98,000 |
| NAV | SGD 10.00 |
| Units allocated | 9,800 units |

Possible transaction group:

| Transaction Type | Quantity Delta | Cash Impact | Notes |
|---|---:|---:|---|
| FUND_SUBSCRIPTION | +9,800 units | -98,000 | Net amount invested into fund. |
| FUND_LOAD_CHARGE | 0 | -2,000 | Explicit sales charge if modelled separately. |

Alternative modelling:

```text
Gross cash out = 100,000
Fee embedded in transaction = 2,000
Net investment amount = 98,000
Units = 9,800
```

Both approaches are acceptable if consistent.

### 6.2 Subscription States

| Status | Meaning |
|---|---|
| ENTERED | Order captured. |
| VALIDATED | Suitability, eligibility and cash checks passed. |
| SUBMITTED | Sent to distributor/transfer agent. |
| ACCEPTED | Accepted before cut-off. |
| DEALING_PENDING | Waiting for NAV/dealing. |
| CONFIRMED | NAV and units confirmed. |
| SETTLED | Cash and units settled. |
| FAILED | Rejected/failed. |
| CANCELLED | Cancelled before processing. |
| REVERSED | Operational reversal after booking. |

---

## 7. Fund Redemption Lifecycle

### 7.1 Unit-Based Redemption Example

Client redeems 5,000 units.

| Item | Value |
|---|---:|
| Units redeemed | 5,000 |
| NAV | SGD 11.00 |
| Gross redemption | SGD 55,000 |
| Redemption fee | SGD 500 |
| Net proceeds | SGD 54,500 |

Transaction group:

| Transaction Type | Quantity Delta | Cash Impact | Notes |
|---|---:|---:|---|
| FUND_REDEMPTION | -5,000 units | +55,000 | Gross redemption proceeds. |
| FUND_FEE or FUND_LOAD_CHARGE | 0 | -500 | Redemption/deferred sales charge. |

### 7.2 Amount-Based Redemption

Client requests SGD 20,000 cash. NAV is unknown until dealing.

If NAV = SGD 10.50:

```text
Units redeemed = requested amount / NAV
               = 20,000 / 10.50
               = 1,904.761905 units
```

Need decimal precision and rounding policy.

Platform controls:

- Do not allow redemption amount greater than estimated available value unless overdraft/shortfall handling exists.
- Support full-redemption flag to avoid dust balances.
- Support pending redemption orders reducing available units.
- Support redemption gate and partial fill.

---

## 8. Switch Lifecycle

A **fund switch** is a linked redemption from one fund and subscription into another fund.

Example:

Client switches SGD 50,000 from Fund A to Fund B.

Transaction group:

| Leg | Transaction Type | Instrument | Quantity | Cash |
|---|---|---|---:|---:|
| 1 | FUND_SWITCH_OUT | Fund A | -units based on Fund A NAV | +cash or internal switch amount |
| 2 | FUND_SWITCH_IN | Fund B | +units based on Fund B NAV | -cash or internal switch amount |

Important modelling choice:

| Model | Description | Pros | Cons |
|---|---|---|---|
| Explicit cash bridge | Switch-out creates cash, switch-in consumes cash. | Transparent accounting. | May show temporary cash exposure. |
| Internal switch bridge | Use internal settlement account, not client-visible cash. | Cleaner client reporting. | More complex reconciliation. |
| Direct linked transaction | Link switch-out and switch-in without cash postings. | Simple display. | Harder for ledger and audit. |

Recommended approach:

```text
Use linked transaction_group_id and explicit or internal cash bridge.
Keep switch-out and switch-in as separate transaction legs.
```

### Switch Timing Risk

Fund A and Fund B may have different dealing dates and settlement dates.

| Risk | Example |
|---|---|
| Out-of-market risk | Fund A redeemed before Fund B subscribed. |
| Overlap risk | Fund B subscribed before Fund A proceeds settle. |
| FX risk | Fund A and Fund B currencies differ. |
| NAV timing risk | Different NAV dates create execution uncertainty. |

---

## 9. Distribution Lifecycle

A fund distribution may represent:

- Dividend income
- Interest income
- Capital gains
- Return of capital
- Equalisation
- Mixed distribution
- Taxable/non-taxable components depending on jurisdiction

### 9.1 Cash Distribution

| Transaction Type | Position Impact | Cash Impact |
|---|---|---:|
| FUND_DISTRIBUTION_CASH | No unit change | Cash in |
| FUND_TAX_WITHHOLDING | No unit change | Cash out |

Example:

| Item | Value |
|---|---:|
| Units held | 10,000 |
| Distribution per unit | SGD 0.05 |
| Gross distribution | SGD 500 |
| Tax withholding | SGD 50 |
| Net cash | SGD 450 |

### 9.2 Reinvested Distribution

If distribution is reinvested:

| Transaction Type | Position Impact | Cash Impact |
|---|---|---:|
| FUND_DISTRIBUTION_REINVEST | No direct unit change or temporary cash | Income entitlement |
| FUND_REINVESTMENT_BUY | Units increase | Cash consumed internally |

Example:

| Item | Value |
|---|---:|
| Gross distribution | SGD 500 |
| Reinvestment NAV | SGD 10 |
| New units | 50 |

Position increases by 50 units.

Important performance point:

```text
Distribution is investment income.
Reinvestment is an internal purchase funded by that income.
It should not be treated as external client contribution.
```

---

## 10. Accumulating Share Class Lifecycle

Accumulating share classes usually do not pay cash distributions. Income is retained in the fund and reflected in NAV.

| Event | Transaction? |
|---|---|
| Fund receives dividends/coupons internally | No client transaction |
| NAV increases from retained income | Valuation change |
| Client sells/redeems fund | Redemption transaction |

Do not create artificial distribution transactions for accumulating classes unless the fund administrator reports a formal reinvested distribution event.

---

## 11. Fund Transfers

Transfers happen when units move between accounts or custodians.

| Transaction Type | Use |
|---|---|
| FUND_TRANSFER_IN | Units received into account. |
| FUND_TRANSFER_OUT | Units moved out. |

Key modelling fields:

| Field | Meaning |
|---|---|
| cost_basis_transfer_flag | Whether original cost is carried over. |
| acquisition_date | Original purchase date, if known. |
| transfer_value | Market value at transfer date. |
| source_custodian | Transfer-out custodian. |
| target_custodian | Transfer-in custodian. |
| tax_lot_id | If tax lots are tracked. |

Performance treatment:

- If transfer is from outside the portfolio, it is usually an external inflow/outflow for TWR.
- If transfer is between portfolios in the same performance composite, treatment depends on performance policy.

---

## 12. Fund Corporate Actions

Fund corporate actions include:

| Corporate Action | Impact |
|---|---|
| Unit split | Units increase, NAV decreases proportionally. |
| Unit consolidation | Units decrease, NAV increases proportionally. |
| Fund merger | Old fund units converted into new fund units. |
| Share-class conversion | One class converted into another class. |
| Fund liquidation | Fund units redeemed into cash. |
| Name change | Instrument master update, usually no transaction. |
| ISIN change | Instrument identifier update, maybe no economic change. |
| Distribution policy change | Product master update. |
| Suspension | Orders blocked, valuation may become stale. |
| Side pocket | Illiquid assets separated into restricted class. |

### 12.1 Unit Split Example

Before split:

| Units | NAV | Market Value |
|---:|---:|---:|
| 1,000 | 100 | 100,000 |

2-for-1 split:

| Units | NAV | Market Value |
|---:|---:|---:|
| 2,000 | 50 | 100,000 |

Transaction:

| Transaction Type | Quantity Delta | Cash Impact |
|---|---:|---:|
| FUND_UNIT_SPLIT | +1,000 | 0 |

No performance gain/loss should be created.

### 12.2 Fund Merger Example

Fund A merges into Fund B.

| Item | Value |
|---|---:|
| Fund A units | 1,000 |
| Fund A NAV | 10 |
| Value | 10,000 |
| Fund B NAV | 20 |
| Fund B units received | 500 |

Transaction group:

| Transaction Type | Instrument | Quantity | Cash |
|---|---|---:|---:|
| FUND_MERGER_OUT | Fund A | -1,000 | 0 |
| FUND_MERGER_IN | Fund B | +500 | 0 |

This is usually an internal transformation, not a client cashflow.

---

## 13. Redemption Gates, Suspensions and Side Pockets

### 13.1 Redemption Gate

A redemption gate limits how much can be redeemed on a dealing date.

Example:

Client requests redemption of 100,000 units. Fund applies 40% gate.

| Requested | Accepted | Deferred |
|---:|---:|---:|
| 100,000 | 40,000 | 60,000 |

Transactions:

| Transaction Type | Units |
|---|---:|
| FUND_GATE_PARTIAL_REDEMPTION | -40,000 |

Deferred balance remains as pending or resubmitted order.

### 13.2 Fund Suspension

A fund may suspend subscriptions/redemptions and NAV publication.

Platform should support:

- Dealing status = SUSPENDED
- Valuation status = STALE / ESTIMATED / SUSPENDED
- Order blocking or manual approval
- Disclosure in reporting
- Risk and concentration flags

### 13.3 Side Pocket

A side pocket separates illiquid or distressed assets from the main fund.

Transaction group:

| Transaction Type | Instrument | Quantity |
|---|---|---:|
| FUND_SIDE_POCKET_TRANSFER | Main fund | -portion |
| FUND_SIDE_POCKET_TRANSFER | Side-pocket class | +portion |

Side-pocket units may have restricted liquidity and delayed valuation.

---

## 14. Private Fund Lifecycle

Private funds need separate lifecycle modelling.

### 14.1 Commitment

Investor commits USD 1,000,000.

| Transaction Type | Cash Impact | Position Impact |
|---|---:|---|
| PRIVATE_FUND_COMMITMENT | 0 | Commitment recorded |

No cash moves at commitment unless upfront fee is charged.

### 14.2 Capital Call

Fund calls 20% of commitment.

| Item | Value |
|---|---:|
| Commitment | 1,000,000 |
| Capital call | 200,000 |
| Paid-in capital after call | 200,000 |
| Unfunded commitment | 800,000 |

Transaction:

| Transaction Type | Cash Impact | Position Impact |
|---|---:|---|
| PRIVATE_FUND_CAPITAL_CALL | -200,000 | Paid-in capital increases |

### 14.3 Distribution

Fund distributes USD 50,000.

| Transaction Type | Cash Impact | Position Impact |
|---|---:|---|
| PRIVATE_FUND_DISTRIBUTION | +50,000 | Distribution received increases |

Classification matters:

- Return of capital
- Realized gain
- Income
- Recallable distribution

### 14.4 NAV Update

Quarterly NAV is received.

| Event | Transaction? |
|---|---|
| NAV update only | No |
| Valuation record update | Yes, valuation record, not transaction |

---

## 15. Position Model for Open-Ended Funds

| Field | Meaning |
|---|---|
| account_id | Portfolio/account. |
| instrument_id | Fund share-class instrument. |
| quantity_units | Units/shares held. |
| average_cost | Average cost per unit. |
| cost_amount | Total cost basis. |
| market_nav | Latest NAV or valuation price. |
| market_value | Units × NAV. |
| valuation_currency | Share-class currency. |
| accrued_income | Usually not used unless distribution entitlement accrued. |
| pending_subscription_units | Units expected but not confirmed. |
| pending_redemption_units | Units blocked for pending redemption. |
| available_units | Units available to sell/redeem. |
| distribution_option | Cash / reinvest / accumulate. |
| tax_lots | Optional lot-level tracking. |
| valuation_status | Final / estimated / stale / suspended. |

Formula:

```text
Market Value = Confirmed Units × NAV
```

If bid/offer pricing applies:

```text
Market Value = Confirmed Units × Valuation Price
Valuation Price may be bid, mid, offer, NAV or adjusted NAV depending on policy.
```

---

## 16. Position Model for ETFs and Listed Closed-End Funds

| Field | Meaning |
|---|---|
| quantity_shares | Exchange-traded shares held. |
| exchange_price | Market price. |
| nav_per_share | Fund NAV, analytical. |
| premium_discount_pct | Market price vs NAV. |
| market_value | Quantity × exchange price. |
| trading_currency | Exchange trading currency. |
| fund_base_currency | Fund accounting/base currency. |
| distribution_income | Dividends/distributions. |

ETF/closed-end fund position is operationally closer to equity, but product taxonomy should still identify it as a fund/ETF/closed-end fund.

---

## 17. Position Model for Private Funds

| Field | Meaning |
|---|---|
| commitment_amount | Total committed capital. |
| paid_in_capital | Capital called and paid. |
| unfunded_commitment | Remaining commitment. |
| recallable_distribution | Distributions that may be recalled. |
| nav | Latest reported NAV/carrying value. |
| distribution_received | Cumulative distributions. |
| realized_gain_loss | Realized result if reported. |
| vintage_year | Fund vintage. |
| fund_life_stage | Investment period / harvest / extension / liquidation. |
| valuation_date | NAV date. |
| report_received_date | Date NAV was received. |
| liquidity_status | Locked / secondary possible / liquidating. |

Private funds should not be valued only by units × daily NAV unless the actual product provides units and daily/periodic NAV in that form.

---

## 18. Pending Orders and Available Quantity

Funds need availability calculation because orders can remain pending.

```text
Confirmed Units
- Pending Redemption Units
- Blocked/Pledged Units
- Lien/Collateral Units
= Available Units
```

For amount-based redemptions, estimate units using latest NAV and apply buffer:

```text
Estimated Redemption Units = Requested Cash Amount / Latest NAV
```

Add controls for NAV movement, fees and minimum holding rules.

---

## 19. Tax Lot Modelling

Tax lot tracking may be required depending on jurisdiction and reporting needs.

Tax lot fields:

| Field | Meaning |
|---|---|
| lot_id | Purchase lot identifier. |
| acquisition_date | Original purchase date. |
| units | Units in lot. |
| cost_per_unit | Cost basis. |
| cost_currency | Currency of cost. |
| source_transaction_id | Subscription/buy transaction. |
| disposal_method | FIFO / LIFO / average cost / specific ID. |

Portfolio performance may use average cost while tax reporting may require lot-level cost basis. Do not mix the two unless policy says so.

---

## 20. Performance Treatment

| Event | Performance Classification |
|---|---|
| Subscription funded by existing portfolio cash | Internal investment transaction. |
| Subscription funded by new client cash | External inflow followed by investment. |
| Redemption proceeds retained in portfolio | Internal disposal. |
| Redemption proceeds withdrawn by client | Internal disposal followed by external outflow. |
| Cash distribution retained in portfolio | Investment income. |
| Distribution reinvested | Income + internal purchase. |
| Return of capital | Capital repayment; reduces cost basis depending on policy. |
| Fund switch | Internal reallocation, not external cashflow. |
| Fund transfer in from outside | Usually external inflow. |
| Fund transfer between accounts | Depends on performance entity policy. |
| Unit split | No performance impact. |
| Fund merger/class conversion | Internal transformation. |
| NAV restatement | Correct historical valuation/performance if material. |
| Capital call | External inflow followed by private fund investment, or cash out from portfolio if prefunded. |
| Private fund distribution | Income/realization/return of capital depending on notice. |

---

## 21. Summary Lifecycle Model

```text
Order captures intent.
Lifecycle/dealing event determines NAV date and eligibility.
Confirmation determines units/cash.
Transaction posts economic impact.
Position updates confirmed holdings.
Valuation updates market value.
Performance classifies whether cashflow is external or investment activity.
```

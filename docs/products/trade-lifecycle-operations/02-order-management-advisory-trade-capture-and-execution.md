# 02 - Order Management, Advisory Trade Capture and Execution

## 1. Purpose

Order management is the controlled transformation of investment intent into executable instructions.

In wealth management, orders may originate from:

- client-directed execution-only instruction
- advisory recommendation
- DPM portfolio-manager decision
- model portfolio rebalance
- systematic investment plan
- fund subscription/redemption
- note subscription
- bond order
- FX conversion
- derivatives hedge
- corporate-action election
- capital-call funding instruction

The order-management layer must preserve the business context behind the trade, not just the execution details.

## 2. From idea to order

A typical advisory/DPM order path:

```text
Investment idea
  -> portfolio analysis
  -> recommendation / rebalance proposal
  -> product eligibility and APU check
  -> client suitability / appropriateness check
  -> mandate and restriction check
  -> liquidity / buying-power check
  -> fee and disclosure generation
  -> approval / client consent
  -> order creation
  -> order routing
```

For DPM, client consent may be replaced by mandate authority, but audit still needs decision rationale, model alignment, mandate version and pre-trade checks.

## 3. Order versus trade versus transaction

| Object | Meaning | Example |
|---|---|---|
| Order | Client/PM instruction to buy/sell/subscribe/redeem | Buy 1,000 AAPL limit 190 |
| Execution / fill | Market/counterparty execution result | 600 AAPL filled at 189.70, 400 at 189.80 |
| Trade | Bookable executed deal, possibly aggregated from fills | Buy 1,000 AAPL avg 189.74 |
| Allocation | Split of block trade across accounts | 400 to account A, 600 to account B |
| Transaction | Accounting/economic posting to cash/position/ledger | +1,000 shares, -cash, fees |

A common platform mistake is to use the transaction as the order and trade record. That loses important control data.

## 4. Order types

| Order type | Use | Platform considerations |
|---|---|---|
| Market order | Immediate execution at available market price | Slippage, price-band checks, client risk warning |
| Limit order | Execute at price or better | Expiry, partial fills, residual quantity |
| Stop order | Trigger once stop price reached | Trigger logic and suitability restrictions |
| Stop-limit order | Trigger then limit | Complex client explanation |
| Good-for-day | Valid only for trading day | Expiry event needed |
| Good-till-cancelled | Remains active until cancelled/expired | Ageing, periodic review |
| Fill-or-kill | Fill all immediately or cancel | Execution status clarity |
| Immediate-or-cancel | Fill available quantity, cancel rest | Partial fill handling |
| At-the-close / at-the-open | Market auction order | Venue-specific calendar/cut-off |
| RFQ | Request quote from dealer | Quote capture, expiry, best execution evidence |
| Subscription order | Buy fund/note/IPO/placement | Cut-off, NAV date, allocation uncertainty |
| Redemption order | Redeem fund/private market units | Notice period, gates, NAV lag |
| Switch order | Redeem one fund and subscribe another | Linked legs and settlement timing |
| Corporate action election | Instruction for rights/tender/conversion | Election deadline, entitlement validation |

## 5. Product-specific order handling

| Product | Order handling specifics |
|---|---|
| Listed equities / ETFs / REITs | Exchange venue, order book, limit/market, partial fills, T+ settlement |
| Bonds | RFQ/dealer quote, price/yield negotiation, accrued interest, minimum denomination |
| Funds / unit trusts | Subscription/redemption by amount or units, dealing cut-off, forward NAV, settlement cycle |
| Structured notes | Primary subscription, termsheet acceptance, issuer allocation, secondary bid if exit |
| Derivatives | Strategy legs, margin, notional exposure, expiry/exercise, collateral |
| FX spot/forward | Currency pair, value date, notional, rate, PVP/settlement risk, NDF fixing |
| Private markets | Commitment, capital call, transfer, redemption/secondary, subscription docs |
| Insurance / annuities | Policy application, premium payment, underwriting, cooling-off, surrender |
| Loans / credit lines | Drawdown, repayment, collateral pledge, LTV/buying-power check |
| Corporate actions | Election, default option, entitlement, oversubscription, deadline |

## 6. Key order fields

| Field | Description |
|---|---|
| order_id | Unique order identifier |
| parent_order_id | For linked/block/strategy orders |
| proposal_id | Advisory or rebalance proposal that generated the order |
| client_id / account_id | Client and booking account |
| portfolio_id | Portfolio context |
| mandate_id | DPM/advisory mandate context |
| order_origin | CLIENT_DIRECTED / ADVISORY / DPM / SYSTEMATIC / CORPORATE_ACTION |
| side | BUY / SELL / SUBSCRIBE / REDEEM / SWITCH / EXERCISE / ELECT |
| instrument_id | Target instrument |
| quantity_type | UNITS / NOMINAL / AMOUNT / PERCENTAGE / ALL |
| order_quantity | Requested quantity |
| order_amount | Requested amount |
| limit_price | Optional limit |
| stop_price | Optional stop |
| order_currency | Order currency |
| settlement_currency | Settlement currency |
| time_in_force | DAY / GTC / IOC / FOK / GTD |
| expiry_datetime | Expiry time |
| execution_venue | Exchange/dealer/fund platform |
| broker_id | Broker/dealer |
| advisor_id / pm_id | Responsible person |
| suitability_check_id | Link to check result |
| mandate_check_id | Link to restriction result |
| buying_power_check_id | Link to liquidity result |
| disclosure_pack_id | Terms, KID, PHS, risk disclosure |
| client_consent_id | Consent evidence |
| status | Current order status |
| status_reason | Rejection/cancel/fail reason |
| created_at / submitted_at | Timing lineage |

## 7. Pre-trade checks

| Check | Purpose |
|---|---|
| Product eligibility | Product is approved and allowed for booking centre/client segment |
| Client suitability | Product risk, complexity, horizon, liquidity and concentration fit client profile |
| Appropriateness | Client has knowledge/experience for complex product where applicable |
| Mandate compliance | Order fits mandate asset allocation, restrictions, leverage, issuer limits |
| Buying power | Sufficient cash/credit/collateral, including unsettled obligations |
| Concentration | Avoid excessive issuer/security/sector/country/product exposure |
| Currency suitability | FX exposure, settlement currency and client base currency understood |
| Minimum denomination | Bond/note/fund minimums satisfied |
| Marketability | Selling quantity available and not blocked/pledged/restricted |
| Short-sale control | Prevent unintended shorts unless allowed |
| Sanctions/restrictions | Client/product/market restrictions satisfied |
| Disclosure | Required documents and warnings delivered |
| Best execution / fair allocation | Required execution policy satisfied |

## 8. Order status model

Recommended order status model:

| Status | Meaning |
|---|---|
| DRAFT | Created but not submitted |
| PENDING_APPROVAL | Awaiting client/manager/compliance approval |
| APPROVED | Approved but not routed |
| VALIDATION_FAILED | Failed pre-trade check |
| SUBMITTED | Sent to OMS/broker/fund platform |
| ACCEPTED | Accepted by execution venue/counterparty |
| REJECTED | Rejected externally |
| PARTIALLY_FILLED | Partially executed |
| FILLED | Fully executed |
| CANCEL_REQUESTED | Cancel pending |
| CANCELLED | Cancel confirmed |
| EXPIRED | Validity expired |
| ALLOCATED | Fills allocated to accounts |
| BOOKED | Trade booked |
| SETTLEMENT_PENDING | Awaiting settlement |
| SETTLED | Settled |
| FAILED_SETTLEMENT | Settlement failed |
| CORRECTED | Corrected by cancel/rebook/reversal |

Avoid using `COMPLETE` without specifying whether execution, booking or settlement completed.

## 9. Partial fills

Partial fills are common in listed securities and some dealer markets.

Example:

| Event | Quantity | Price | Status |
|---|---:|---:|---|
| Order submitted | 1,000 | Limit 100 | Accepted |
| Fill 1 | 400 | 99.90 | Partially filled |
| Fill 2 | 300 | 100.00 | Partially filled |
| Cancel rest | 300 | - | Cancelled residual |

Platform requirements:

- retain all fill prices and times
- compute average execution price
- track residual quantity
- book trades based on fill allocation policy
- reflect partially filled exposure and cash reserve
- support cancellation of remaining quantity
- maintain client disclosure if original order not fully filled

## 10. Block orders and allocation

In DPM or advisory campaigns, one block trade may be allocated to multiple portfolios.

Key allocation principles:

- define allocation before execution where possible
- support pro-rata allocation
- support model-based allocation
- prevent cherry-picking
- record allocation rationale
- preserve fill-to-account lineage
- handle partial fill fairness
- handle account-level restrictions after execution

Fields:

| Field | Meaning |
|---|---|
| block_order_id | Parent order |
| allocation_id | Allocation record |
| allocation_method | PRE_TRADE_PRO_RATA / POST_TRADE_FAIR_ALLOCATION / MANUAL_APPROVED |
| account_id | Target account |
| target_quantity | Intended account quantity |
| allocated_quantity | Final account quantity |
| average_price | Allocated price |
| allocation_reason | Rationale / exception reason |

## 11. Execution capture

Execution record should include:

| Field | Meaning |
|---|---|
| execution_id | Unique fill/execution ID |
| order_id | Parent order |
| venue | Exchange/dealer/platform |
| broker_execution_id | External execution ID |
| execution_timestamp | Timestamp with timezone |
| executed_quantity | Fill quantity |
| executed_price | Fill price |
| execution_currency | Price currency |
| commission | Commission/fee |
| market_fee | Exchange/clearing fee |
| accrued_interest | Bonds/notes |
| settlement_amount | Net cash amount |
| counterparty_id | Dealer/counterparty |
| liquidity_indicator | Maker/taker/RFQ/etc. |

## 12. Trade capture

Trade capture transforms execution into a bookable trade.

Trade capture should enrich:

- instrument static data
- account and custodian details
- settlement instructions
- price and currency details
- tax and fee treatment
- accrued interest
- trade date and settlement date
- product-specific lifecycle terms
- confirmation details
- regulatory reporting identifiers where applicable

Trade capture should validate:

- instrument exists and is active
- client/account can hold product
- currency and market are supported
- quantity/nominal conventions are valid
- settlement calendar is valid
- custodian and SSI exist
- price is within tolerance
- fees are within policy
- duplicate trade detection passes

## 13. Cancel, amend and correct

Order/trade corrections must preserve audit.

| Action | Use |
|---|---|
| Cancel order | Before execution or for remaining quantity |
| Amend order | Change price/quantity/expiry before execution where allowed |
| Cancel/rebook trade | Correct executed trade details after booking |
| Reversal transaction | Reverse previously posted transaction |
| Adjustment transaction | Post difference where full reversal not appropriate |

Never silently overwrite an executed trade or posted transaction without audit trail.

## 14. Advisory-specific order evidence

For advisory orders, retain:

- recommendation rationale
- product risk explanation
- alternatives considered
- suitability result
- client acknowledgement
- disclosure documents delivered
- fees/charges disclosed
- concentration impact
- expected portfolio impact
- client approval timestamp and channel

## 15. DPM-specific order evidence

For DPM orders, retain:

- mandate version
- model portfolio version
- rebalance reason
- drift before/after
- investment committee decision, if applicable
- restriction checks
- best execution / fair allocation evidence
- post-trade mandate result
- client reporting explanation where material

## 16. Implementation guidance

Keep order management separate from transaction booking.

Recommended separation:

| Service/module | Responsibility |
|---|---|
| Proposal service | Recommendations and client proposal workflow |
| Suitability service | Client/product suitability and appropriateness |
| Mandate service | DPM/advisory constraints and target allocations |
| Buying-power service | Cash/collateral/credit availability |
| OMS | Order state, routing, execution capture |
| Trade capture service | Enrichment, validation, bookable trades |
| Transaction service | Accounting-grade postings |
| Position service | Derived/current positions |
| Reconciliation service | Break detection and workflow |

This gives clear ownership and avoids mixing advisory workflow with back-office accounting logic.

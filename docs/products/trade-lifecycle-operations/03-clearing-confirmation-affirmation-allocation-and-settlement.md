# 03 - Clearing, Confirmation, Affirmation, Allocation and Settlement

## 1. Purpose

Post-trade processing converts an executed trade into a legally agreed, operationally enriched and settled transaction.

The post-trade lifecycle is where many production issues arise: incorrect settlement instructions, mismatched confirmations, insufficient cash/securities, wrong custodian account, late allocations, failed trades, FX funding issues and corporate-action complications on unsettled trades.

## 2. Post-trade flow

```text
Execution / Fill
  -> trade capture
  -> enrichment
  -> allocation
  -> confirmation
  -> affirmation / matching
  -> clearing, where applicable
  -> settlement instruction
  -> settlement matching
  -> settlement completion or fail
  -> cash/security booking
  -> reconciliation
```

## 3. Confirmation versus affirmation versus matching

| Term | Meaning |
|---|---|
| Confirmation | Formal trade details sent by broker/counterparty to confirm economics |
| Affirmation | Investment manager/client/custodian agrees confirmation details |
| Matching | Operational comparison of settlement instruction details between parties |
| Enrichment | Adding static, SSI, tax, account, custodian and settlement data |

A trade can be executed but unmatched, confirmed but unsettled, or settled but later subject to correction.

## 4. Trade enrichment

Trade enrichment adds operational details needed for settlement and accounting.

Key enrichment data:

| Data | Examples |
|---|---|
| Account data | client account, portfolio, custody account, sub-account |
| Instrument data | ISIN, CUSIP, sedol, market, denomination, settlement method |
| Settlement data | CSD, custodian, SSI, counterparty SSI, settlement currency |
| Cash data | settlement amount, fees, taxes, FX funding account |
| Product data | accrued interest, coupon, maturity, fund NAV/dealing date |
| Regulatory data | transaction reporting flags, short-sale flag, LEI where needed |
| Calendar data | market holidays, settlement cycle, cut-off |

## 5. Allocation

Allocation assigns executed quantity/amount to one or more client accounts.

Allocation scenarios:

| Scenario | Example |
|---|---|
| Single-account order | Client buys 1,000 shares |
| Block order | PM buys 100,000 shares across 50 accounts |
| Model rebalance | Multiple accounts adjusted to target weights |
| Partial fill | Only 60% of block order filled |
| Minimum-lot constraints | Allocation rounded to board lots or denominations |
| Account exclusion | Account fails suitability/mandate check and is excluded |

Allocation fields should include method, timestamp, user/system, before/after quantities, and reason for manual override.

## 6. Clearing

Clearing is the process of preparing obligations for settlement. In some markets, a central counterparty interposes itself between buyer and seller. In OTC or bilateral markets, the counterparties may settle directly.

| Product/market | Clearing treatment |
|---|---|
| Listed equities | Often exchange/CCP cleared depending market |
| Listed futures/options | Cleared through clearing house; margin required |
| OTC derivatives | May be bilateral or centrally cleared depending product/regulation |
| Bonds | Often bilateral/ICSD/CSD settlement; market-specific clearing |
| Funds | Typically processed by transfer agent/platform, not exchange clearing |
| FX spot/forward | Bilateral or CLS/PVP where eligible |

Clearing introduces statuses such as submitted, accepted, novated, margin required, cleared, rejected and failed.

## 7. Settlement

Settlement is final exchange of securities and cash, or cash-for-cash in FX.

| Settlement type | Meaning |
|---|---|
| DVP | Delivery versus payment: securities and cash exchanged together |
| FOP | Free of payment: securities move without cash movement |
| PVP | Payment versus payment: cash payments exchanged together, common FX risk-mitigation concept |
| RVP | Receive versus payment |
| DAP | Deliver against payment |
| Bilateral settlement | Direct settlement between counterparties/custodians |
| CSD settlement | Settlement through central securities depository |
| ICSD settlement | International CSD settlement, e.g., Eurobond markets |

DVP reduces principal risk because delivery and payment are linked.

## 8. Settlement cycle

Settlement cycle defines the intended number of business days from trade date to settlement date.

Examples:

| Market/product | Typical cycle concept |
|---|---|
| U.S. equities/corporate bonds/ETFs after May 2024 | T+1 for many regular-way securities |
| Singapore SGX ready-market securities | T+2 under SGX rulebook |
| Mutual funds/unit trusts | Product-specific, often dealing date plus one or more days |
| FX spot | Often T+2 for many major pairs, but currency-pair specific |
| FX same-day/tom/next | Special value-date conventions |
| Private markets | Notice and cashflow schedule based, not standard T+ cycle |
| Structured notes | Primary issue settlement based on termsheet |
| Bonds | Market/currency/security-specific |

Do not hardcode settlement cycle. Store it by market, instrument, product type, venue and effective date.

## 9. Settlement instruction

A settlement instruction should include:

| Field | Meaning |
|---|---|
| settlement_instruction_id | Unique instruction |
| trade_id | Linked trade |
| account_id | Client/portfolio account |
| custodian_account_id | Custody account |
| counterparty_id | Broker/dealer/counterparty |
| CSD/ICSD | Settlement depository |
| place_of_settlement | Market infrastructure |
| security_id | ISIN/local ID |
| quantity/nominal | Settlement quantity |
| settlement_amount | Cash amount |
| settlement_currency | Cash currency |
| intended_settlement_date | Contractual settlement date |
| actual_settlement_date | Actual settlement completion |
| SSI version | Standing settlement instruction version |
| status | Matched, pending, settled, failed, cancelled |
| fail_reason | Insufficient securities, cash, unmatched, market issue |

## 10. Standing settlement instructions, SSIs

SSIs define where securities and cash should settle.

Strong SSI management is critical in accelerated settlement environments.

SSI attributes:

- account/custodian identifier
- market/CSD
- currency
- security type
- cash correspondent bank
- BIC/IBAN/account details where applicable
- effective dates
- approval status
- source and version
- client/account applicability

SSI issues are a common root cause of settlement failures.

## 11. Settlement statuses

| Status | Meaning |
|---|---|
| NOT_INSTRUCTED | Settlement instruction not sent |
| INSTRUCTED | Instruction sent |
| MATCHED | Counterparty/custodian instruction matched |
| UNMATCHED | Details do not match |
| PENDING | Awaiting settlement date or resources |
| PARTIALLY_SETTLED | Partial quantity/amount settled |
| SETTLED | Fully settled |
| FAILED | Did not settle on intended date |
| CANCELLED | Settlement cancelled |
| REVERSED | Settlement reversed/corrected |
| BUY_IN_PENDING | Market buy-in may occur |
| CLAIM_PENDING | Market claim/compensation pending |

Settlement status should not be mixed with order status.

## 12. Settlement fails

A settlement fail occurs when a trade does not settle on intended settlement date.

Common reasons:

| Reason | Example |
|---|---|
| Insufficient securities | Seller does not have deliverable position |
| Insufficient cash | Buyer does not fund account |
| SSI mismatch | Wrong custodian/account details |
| Trade detail mismatch | Quantity, amount, date or security differs |
| Market/custodian cut-off missed | Instruction too late |
| Restricted security | Legal/market restriction |
| Corporate action complication | Security under event, blocked or transformed |
| FX funding issue | Settlement currency not available |
| System outage | Custodian/broker/market infrastructure issue |

Fail handling should include ageing, owner, reason, cash/security impact, penalty/buy-in risk and client/advisor visibility.

## 13. Partial settlement

Some markets support partial settlement.

Example:

| Trade | Intended | Settled | Remaining |
|---|---:|---:|---:|
| Buy bond nominal | 1,000,000 | 600,000 | 400,000 |

Platform requirements:

- store original intended quantity
- store settled quantity and remaining quantity
- update position/cash based on actual settlement policy
- track fail on residual
- apply corporate actions proportionally where applicable
- reconcile with custodian at partial level

## 14. Settlement and performance

Performance treatment depends on policy:

| View | Treatment |
|---|---|
| Trade-date accounting | Market exposure starts at trade date |
| Settlement-date accounting | Position starts when settled |
| Performance TWR | Often trade-date based for investment decision attribution |
| Custody reporting | Settlement-date based |
| Buying power | Depends on settled/available/projection policy |
| Collateral | Usually requires settled and eligible assets |

A robust platform should allow multiple basis views and clearly label them.

## 15. Settlement and buying power

Buying power must consider:

- current settled cash
- pending buy settlements
- pending sell settlements
- blocked cash for open orders
- unsettled income
- FX conversions required
- credit line availability
- collateral eligibility and haircut
- margin requirements
- settlement-fail risk
- product-specific settlement cycles

Example:

```text
Available Cash
= Settled Cash
+ eligible pending sale proceeds
+ eligible credit availability
- pending purchase settlements
- open-order reservations
- fees/taxes/buffers
```

Eligibility of pending sale proceeds is a business/risk decision, not a universal rule.

## 16. Settlement calendars

Settlement date calculation requires:

- trade date
- product type
- market/venue
- exchange holidays
- currency holidays
- settlement system holidays
- cut-off time
- non-standard settlement terms
- daylight saving/timezone handling
- effective-date changes in market rules

Use a settlement-calendar service rather than ad hoc date arithmetic.

## 17. Settlement transactions

For a settled equity buy, transaction legs might be:

| Leg | Instrument | Quantity | Cash |
|---|---|---:|---:|
| Security leg | Equity | +1,000 | 0 |
| Cash leg | Cash | 0 | -gross consideration |
| Fee leg | Cash | 0 | -commission |
| Tax leg | Cash | 0 | -stamp duty / tax |

For a bond buy:

| Leg | Instrument | Quantity/nominal | Cash |
|---|---|---:|---:|
| Security leg | Bond | +1,000,000 nominal | 0 |
| Principal cash | Cash | 0 | -clean consideration |
| Accrued interest | Cash | 0 | -accrued interest |
| Fees/tax | Cash | 0 | -fees/tax |

## 18. Controls and reconciliation

Key controls:

- trade capture completeness
- duplicate trade detection
- price/amount tolerance checks
- allocation fairness check
- confirmation matching
- SSI validation
- settlement instruction timeliness
- unsettled trade ageing
- failed trade reason coding
- custodian position/cash reconciliation
- exception workflow and SLA
- audit of corrections

## 19. Platform recommendations

- Store intended and actual settlement dates separately.
- Store settlement status separately from trade/order status.
- Maintain instruction-level records and status history.
- Support partial settlement.
- Support fail ageing and reason taxonomy.
- Make settlement basis explicit in position and cash APIs.
- Do not update positions directly from orders; derive from booked trades/settlements based on chosen basis.
- Use idempotent settlement event processing.
- Preserve cancelled and corrected instructions.

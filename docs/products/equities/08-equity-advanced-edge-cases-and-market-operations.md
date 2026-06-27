# 08 — Equity Advanced Edge Cases and Market Operations

## 1. Purpose

This file captures equity cases that often appear in private banking, portfolio analytics, migration, reconciliation and production support. These cases are usually not visible in beginner product explanations but are important for a bank-grade platform.

## 2. Multi-listing and security identity

The same economic company exposure can appear as local ordinary shares, ADR/GDR, secondary listing, different share class, foreign/local line, currency-specific line, or partly fungible/non-fungible line.

Do not rely only on ticker. Use:

```text
Issuer ID
Instrument ID
Listing ID
Quote ID
Custody security ID
External identifiers: ISIN, CUSIP, SEDOL, exchange code
Corporate action mapping
Fungibility/conversion rule
```

## 3. ADR / GDR edge cases

| Event | Platform issue |
|---|---|
| ADR ratio change | Adjust underlying equivalent exposure and price interpretation |
| ADR dividend | Convert local dividend to ADR currency and deduct fees/tax |
| Depositary fee | Book as fee expense, not dividend |
| ADR termination | Convert to local shares, sell, or process cash according to event |
| Local market corporate action | Map to ADR event and entitlement |
| Unsponsored ADR | Data quality and event coverage may be weaker |
| Asynchronous trading | ADR and local share prices move in different time zones |

Formula:

```text
Underlying Equivalent Quantity = DR Quantity × DR Ratio
```

## 4. Odd lots and fractional shares

Some markets trade in board lots. Corporate actions may create odd lots or fractional shares.

Impacts:

- order routing;
- execution price;
- liquidity;
- custody processing;
- client reporting;
- mandate rebalancing;
- fractional cash-in-lieu.

Recommended fields:

```text
position.quantity = precise ledger quantity
position.board_lot_quantity = floor(quantity / board_lot_size)
position.odd_lot_quantity = quantity mod board_lot_size
fractional_allowed = true/false
rounding_policy = floor/round/cash-in-lieu
```

## 5. Trading halt, suspension and delisting

| Status | Meaning | Recommended treatment |
|---|---|---|
| Trading halt | Temporary stop | Use last price with stale flag; block new trade if required |
| Suspension | Longer stop | Move to exception/fair-value monitoring |
| Delisting pending | Exchange listing ends | Monitor tender/cash offer/OTC migration |
| Delisted | No active exchange listing | Fair value or write-down process |
| Bankruptcy | Equity may be worthless | Impair/writeoff and possible liquidation proceeds |

Reporting should clearly flag valuation uncertainty and liquidity restriction.

## 6. IPO and placement processing

Lifecycle:

```text
Offer announcement
  -> client indication/subscription
  -> cash blocking
  -> allocation/allotment
  -> refund unallocated cash
  -> share credit
  -> listing/trading start
```

| Event | Transaction/state |
|---|---|
| Cash blocked | Cash block, not final investment |
| Subscription | Pending order |
| Allocation | EQ_IPO_ALLOTMENT |
| Refund | CASH_REFUND |
| Shares credited | EQ_IPO_SUBSCRIPTION |
| First pricing | Market price after listing |

## 7. Share buybacks

A company buyback may affect shareholders indirectly by reducing shares outstanding. Tender buybacks can require client election.

| Type | Platform treatment |
|---|---|
| Open-market buyback | No direct client transaction |
| Tender offer | Voluntary corporate action; client election |
| Mandatory squeeze-out | Mandatory reorganization/cash consideration |

## 8. Securities lending by long holders

| Dimension | Treatment |
|---|---|
| Economic exposure | Often retained |
| Available quantity | Lent shares may not be immediately sellable |
| Voting | May be lost unless recalled |
| Dividend | Manufactured dividend may replace ordinary dividend |
| Income | Lending fee income |
| Collateral | Cash/non-cash collateral may be held |

Transactions: `EQ_SECURITIES_LENDING_OUT`, `EQ_SECURITIES_LENDING_RETURN`, `EQ_SECURITIES_LENDING_FEE`, `MANUFACTURED_DIVIDEND_RECEIVED`.

## 9. Short selling

Lifecycle:

```text
Locate / borrow
  -> short sell
  -> maintain margin
  -> pay borrow fee
  -> pay manufactured dividend if dividend occurs
  -> buy to cover
  -> return borrowed shares
```

Risk: loss can exceed initial proceeds, borrow can be recalled, dividend expenses reduce return, corporate actions can create complex obligations, and margin calls can force liquidation.

## 10. Margin and collateral

Equities may be collateral-eligible with haircuts/LTV.

| Attribute | Example |
|---|---|
| Market value | SGD 100,000 |
| LTV | 60% |
| Collateral value | SGD 60,000 |
| Haircut | 40% |
| Concentration haircut | Additional if single issuer high |
| Liquidity haircut | Additional if low liquidity/suspended |
| Currency haircut | Additional if non-base currency |

Buying power engines should use collateral value, not only market value.

## 11. Restricted lists and watchlists

A stock can be restricted due to compliance, sanctions, insider/research blackout, issuer relationship, product governance, ESG exclusion, market access, or corporate action uncertainty.

| Status | Action |
|---|---|
| Allowed | Normal processing |
| Watchlist | Warning and monitoring |
| Restricted buy | Block buy, allow sell |
| Restricted all | Block buy/sell except approval |
| Prohibited | Force review/exit if mandated |

## 12. Corporate action cancellation and correction

The platform must support event versioning, cancellation, reversal of generated transactions, replacement transactions, recalculation of positions, lots, valuations and performance, and a full audit trail.

Never overwrite processed corporate action terms without lineage.

## 13. Data migration issues

| Issue | Impact |
|---|---|
| Missing historical cost | P&L incomplete |
| Missing lots | Realized P&L and tax-lot selection affected |
| Aggregated position only | Cannot reconstruct historical performance |
| Missing corporate action history | Cost basis may be wrong |
| Different identifiers | Duplicate positions or missed mapping |
| Local vs ADR lines | Wrong exposure |
| Wrong FX rates | Base currency P&L mismatch |
| Position as-of-date mismatch | Reconciliation breaks |

Migration strategy:

1. Load opening positions with source and valuation date.
2. Load known cost basis separately from performance start value.
3. Preserve legacy identifiers.
4. Flag unknown cost separately.
5. Do not invent historical corporate actions if not available; document limitation.
6. Reconcile against custodian and statements.

## 14. Stress scenarios for risk and advisory

| Scenario | Impact |
|---|---|
| Market crash -20% | Equity sleeve drawdown |
| Sector shock -30% | Sector concentration loss |
| Single-name collapse -50% | Idiosyncratic risk |
| Currency depreciation -10% | Base-currency return |
| Dividend cut | Income shortfall |
| Trading halt | Liquidity and valuation uncertainty |
| Rights issue | Cash funding and dilution |
| Margin haircut increase | Buying power pressure |
| Short squeeze | Short loss and margin call |
| Corporate action error | Reconciliation/client reporting risk |

## 15. Production support triage

When an equity position/report looks wrong, check in this order:

1. Is the security identifier/listing correct?
2. Is the price current and from the right market/currency?
3. Is FX correct?
4. Are trade date and settlement date handled correctly?
5. Are all transactions present?
6. Did any corporate action occur?
7. Was cost basis adjusted?
8. Was dividend tax posted?
9. Are transfers classified correctly?
10. Is position reconciled to custodian?
11. Did any reversal/correction/backdated event occur?
12. Are benchmark/classification mappings correct?

## 16. Advanced definition of done

Equity capability is mature when it handles ordinary buys/sells, trade-date and settlement-date positions, multi-currency valuation, dividends with tax, all common corporate actions, cost-basis adjustments, ADR/GDR ratio and fee events, shorts and lending if in scope, margin/collateral haircuts if in scope, stale/suspended/delisted securities, mandate/suitability restrictions, performance and attribution continuity, reconciliation and reprocessing.

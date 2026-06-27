# 03 — Forwards, Futures, FX and Commodity Derivatives

## 1. Forward vs future

A **forward** and a **future** are both contracts to buy or sell an underlying at a future date at a pre-agreed price. The difference is market structure.

| Feature | Forward | Future |
|---|---|---|
| Trading | OTC bilateral | Exchange-traded |
| Terms | Customized | Standardized |
| Counterparty | Dealer/client | Clearing house via broker |
| Margin | Bilateral collateral, if applicable | Initial and variation margin |
| Valuation | Model / forward curve / counterparty quote | Exchange settlement price |
| Settlement | Usually at maturity | Daily mark-to-market |
| Examples | FX forward, NDF, equity forward | S&P futures, Treasury futures, oil futures |

## 2. Futures basics

A futures contract has:

| Field | Meaning |
|---|---|
| Underlying | Index, bond, commodity, rate, FX |
| Contract size / multiplier | Converts price movement into P&L |
| Expiry month | Contract maturity |
| Tick size | Minimum price movement |
| Tick value | Cash value of one tick |
| Initial margin | Collateral required to open position |
| Maintenance margin | Minimum margin level |
| Variation margin | Daily mark-to-market cash movement |
| Settlement type | Cash or physical |

Example: equity index future

```text
Notional Exposure = Futures Price × Contract Multiplier × Number of Contracts
```

If index future is 5,000, multiplier is USD 50, and client buys 4 contracts:

```text
Notional = 5,000 × 50 × 4 = USD 1,000,000
```

The client may post far less than USD 1,000,000 as margin, but risk exposure is still about USD 1,000,000.

## 3. Futures lifecycle

| Stage | Event | Transaction? |
|---|---|---|
| Open trade | Buy/sell future | FUTURE_BUY_OPEN / FUTURE_SELL_OPEN |
| Initial margin | Margin posted | FUTURE_INITIAL_MARGIN_POSTED |
| Daily MTM | Exchange settlement price changes | FUTURE_VARIATION_MARGIN |
| Margin call | Additional collateral needed | FUTURE_MARGIN_CALL_POSTED |
| Close-out | Offset opposite trade | FUTURE_SELL_CLOSE / FUTURE_BUY_CLOSE |
| Expiry cash settlement | Final cash settlement | FUTURE_FINAL_CASH_SETTLEMENT |
| Physical delivery | Deliver/receive underlying | FUTURE_PHYSICAL_DELIVERY_IN/OUT |

## 4. Futures transaction types

| Transaction type | Use |
|---|---|
| FUTURE_BUY_OPEN | Long futures position opened |
| FUTURE_SELL_OPEN | Short futures position opened |
| FUTURE_SELL_CLOSE | Long futures closed |
| FUTURE_BUY_CLOSE | Short futures closed |
| FUTURE_INITIAL_MARGIN_POSTED | Initial margin posted |
| FUTURE_INITIAL_MARGIN_RETURNED | Initial margin returned |
| FUTURE_VARIATION_MARGIN_GAIN | Daily MTM cash gain |
| FUTURE_VARIATION_MARGIN_LOSS | Daily MTM cash loss |
| FUTURE_MARGIN_CALL_POSTED | Additional margin posted |
| FUTURE_MARGIN_RELEASED | Excess margin returned |
| FUTURE_FINAL_CASH_SETTLEMENT | Final settlement at expiry |
| FUTURE_PHYSICAL_DELIVERY_IN | Underlying received |
| FUTURE_PHYSICAL_DELIVERY_OUT | Underlying delivered |
| FUTURE_FEE | Exchange/broker/clearing fee |
| FUTURE_REVERSAL_CORRECTION | Cancel/correct |

## 5. Futures position model

| Field | Meaning |
|---|---|
| account_id | Portfolio/account |
| contract_id | Futures contract |
| underlying_id | Index/bond/commodity/FX underlying |
| position_side | LONG / SHORT |
| contract_quantity | Signed quantity |
| contract_multiplier | Contract multiplier |
| trade_price | Entry price |
| settlement_price | Latest exchange settlement price |
| notional_exposure | Price × multiplier × contracts |
| unrealized_pnl | Depending accounting model, before daily VM |
| cumulative_variation_margin | Sum of daily VM |
| initial_margin_posted | Collateral posted |
| lifecycle_status | ACTIVE / CLOSED / EXPIRED / DELIVERED |

## 6. FX forwards

An FX forward is an agreement to exchange two currencies on a future date at a locked rate.

Example:

Client agrees to sell USD 1,000,000 and buy SGD at 1.3500 in 3 months.

| Field | Example |
|---|---|
| buy_currency | SGD |
| buy_amount | 1,350,000 |
| sell_currency | USD |
| sell_amount | 1,000,000 |
| forward_rate | 1.3500 |
| maturity_date | 3 months |
| settlement_type | Deliverable |

FX forwards are common in wealth management for:

- hedging foreign-currency holdings;
- hedging bond/fund/equity currency risk;
- tactical currency views;
- DPM overlay management;
- fund share-class hedging.

## 7. FX forward lifecycle

| Event | Transaction type | Position/cash impact |
|---|---|---|
| Trade | FX_FORWARD_OPEN | Creates forward position, usually no upfront cash |
| MTM valuation | No transaction | Valuation changes |
| Early close | FX_FORWARD_CLOSE | Realized FX forward P&L |
| Roll | FX_FORWARD_CLOSE + FX_FORWARD_OPEN | Old forward closed, new opened |
| Maturity deliverable | FX_FORWARD_SETTLEMENT | Buy/sell currencies settle |
| Maturity NDF | NDF_CASH_SETTLEMENT | Net cash amount in settlement currency |

## 8. Non-deliverable forward, NDF

An NDF is a cash-settled FX forward, often used when a currency is restricted or not freely deliverable offshore.

| Feature | Deliverable FX forward | NDF |
|---|---|---|
| Principal exchange | Yes | No |
| Settlement | Both currencies delivered | Net cash difference |
| Fixing | May not be central | Official fixing source is critical |
| Use case | Standard currency hedge | Restricted currency exposure |

NDF payoff:

```text
Settlement Amount = Notional × (Fixing Rate - Contract Rate) / Fixing Rate
```

Exact formula depends on market convention and currency pair.

## 9. Commodity futures

Commodity futures may be cash-settled or physically deliverable.

| Commodity | Special platform concerns |
|---|---|
| Oil | Expiry roll, physical delivery avoidance, storage/logistics |
| Gold | Contract unit, delivery location, currency |
| Agricultural | Seasonality, delivery grades, expiry months |
| Metals | Warehouse/location quality |

Wealth portfolios usually should not accidentally go into physical delivery. Platforms need expiry alerts and auto-close/roll controls where appropriate.

## 10. Bond and rate futures

Bond futures require additional modelling:

| Concept | Meaning |
|---|---|
| Cheapest-to-deliver | Deliverable bond that minimizes seller cost |
| Conversion factor | Adjusts deliverable bond value |
| Basis | Difference between cash bond and futures-implied value |
| DV01 | Interest-rate sensitivity |

For DPM, bond futures may be used to adjust duration quickly without trading physical bonds.

## 11. Performance and reporting treatment

| Event | Treatment |
|---|---|
| Initial margin | Collateral movement, not investment loss |
| Variation margin gain/loss | Investment P&L and cash movement |
| Futures close-out | Realized P&L |
| FX forward MTM | Unrealized FX derivative P&L |
| FX forward settlement | Cash currency conversion + realized P&L |
| Roll | Realized P&L on old contract + new exposure |

Client reporting should clearly separate:

- market value;
- notional exposure;
- realized and unrealized P&L;
- margin/collateral used;
- hedge vs active exposure;
- underlying exposure contribution.

## 12. Advisory and mandate considerations

| Product | Main advisory concern |
|---|---|
| Futures | Leverage, margin calls, expiry, basis risk |
| FX forwards | Currency suitability, settlement liquidity, rollover risk |
| NDFs | Fixing risk, restricted currency risk, OTC counterparty |
| Commodity futures | Volatility, expiry roll, physical delivery risk |
| Bond futures | Duration risk, basis risk, leverage |

For mandates, controls should include:

- permitted derivative families;
- maximum gross and net notional;
- maximum delta-equivalent exposure;
- margin utilization limits;
- allowed underlyings/currencies;
- expiry concentration limits;
- requirement to close physically delivered futures before delivery window;
- hedge-purpose tagging.

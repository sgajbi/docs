# 04 - FX Spot, Forwards, Swaps, NDFs, Lifecycle and Position Modelling

## 1. Why FX matters in wealth platforms

FX is not only a trading product. It is a portfolio infrastructure capability.

FX is used for:

- converting settlement currency for securities trades;
- funding withdrawals in client-preferred currency;
- investing in foreign currency assets;
- hedging foreign currency exposure;
- reporting multi-currency portfolios in base currency;
- measuring realised and unrealised FX P&L;
- managing cash shortfalls and overdrafts;
- supporting structured products and dual currency products;
- executing DPM currency overlays.

## 2. Currency concepts

| Term | Meaning |
|---|---|
| Base currency | Currency in which portfolio/reporting performance is measured |
| Local currency | Currency of a security/market/account |
| Settlement currency | Currency in which trade settles |
| Trade currency | Currency used for transaction price/consideration |
| Cash currency | Currency of cash ledger movement |
| Quote currency | Currency used to quote a currency pair |
| Functional currency | Accounting/reporting currency for entity/account |
| Hedged currency | Currency exposure being hedged |

## 3. Currency pair convention

An FX quote expresses how much of one currency is exchanged for another.

Example:

```text
EUR/USD = 1.1000
```

This usually means 1 EUR = 1.1000 USD.

| Pair | Base currency | Quote currency | Meaning |
|---|---|---|---|
| EUR/USD | EUR | USD | USD per 1 EUR |
| USD/SGD | USD | SGD | SGD per 1 USD |
| USD/JPY | USD | JPY | JPY per 1 USD |

Platforms should store pair convention explicitly. Many FX errors arise from inverted rates.

## 4. FX spot

FX spot is an agreement to exchange one currency for another at a spot rate for near-term settlement/value date.

### Example

Client buys USD 100,000 and sells SGD at USD/SGD 1.3500.

| Leg | Currency | Amount |
|---|---|---:|
| Buy leg | USD | +100,000 |
| Sell leg | SGD | -135,000 |

Transaction group:

| Transaction type | Currency | Cash amount |
|---|---|---:|
| FX_SPOT_BUY_LEG | USD | +100,000 |
| FX_SPOT_SELL_LEG | SGD | -135,000 |

## 5. FX spot lifecycle

| Stage | Event | System object | Transaction? |
|---|---|---|---|
| Quote | Rate quoted | Quote | No |
| Order | Client accepts/places FX order | Order | No accounting movement |
| Trade | FX trade executed | Trade | Pending cash legs |
| Confirmation | Counterparty/custodian confirms | Trade confirmation | No new transaction if already booked |
| Settlement/value date | Cash legs settle | Cash ledger entries | Yes |
| Cancellation/correction | Trade amended/cancelled | Reversal/rebook | Yes if booked |

## 6. FX forward

An FX forward is an agreement today to exchange currencies on a future value date at an agreed forward rate.

### Example

Client agrees to sell EUR 1,000,000 and buy USD at 1.1200 in 3 months.

| Item | Value |
|---|---:|
| Sell currency | EUR |
| Sell amount | 1,000,000 |
| Buy currency | USD |
| Forward rate | 1.1200 |
| Buy amount | USD 1,120,000 |
| Value date | 3 months later |

### Why use FX forwards?

| Use case | Example |
|---|---|
| Hedge foreign assets | SGD client hedges USD equity exposure |
| Fund known future payment | Client needs EUR in 3 months |
| DPM overlay | Mandate hedges non-base currency exposure |
| Lock exchange rate | Avoid uncertainty before settlement/capital call |

## 7. FX forward position model

Unlike spot, a forward remains an open derivative/contract until maturity.

| Field | Description |
|---|---|
| fx_contract_id | Unique contract ID |
| contract_type | FORWARD |
| buy_currency | Currency to receive |
| buy_amount | Amount to receive |
| sell_currency | Currency to deliver |
| sell_amount | Amount to deliver |
| agreed_rate | Forward rate |
| trade_date | Contract trade date |
| value_date | Future settlement date |
| counterparty | Dealer/bank |
| mtm_value | Mark-to-market value |
| mtm_currency | Valuation currency |
| collateral_required | If collateralised |
| hedge_designation | Hedging purpose, if applicable |
| lifecycle_status | Open/settled/cancelled/rolled/matured |

## 8. FX forward transaction types

| Lifecycle | Transaction type |
|---|---|
| Opening trade | FX_FORWARD_OPEN |
| Daily valuation | Valuation record, no cash transaction |
| Collateral/margin movement | COLLATERAL_POST / COLLATERAL_RETURN |
| Close-out before maturity | FX_FORWARD_CLOSE |
| Roll | FX_FORWARD_CLOSE + FX_FORWARD_OPEN |
| Maturity settlement buy leg | FX_FORWARD_SETTLEMENT_BUY_LEG |
| Maturity settlement sell leg | FX_FORWARD_SETTLEMENT_SELL_LEG |
| Realised P&L settlement | FX_FORWARD_REALIZED_PNL if cash-settled separately |
| Cancellation/correction | FX_FORWARD_REVERSAL |

## 9. FX forward valuation

A simple forward MTM compares the contracted forward rate with the current market forward rate for the remaining tenor.

Conceptually:

```text
Forward MTM approx. Present Value of difference between contracted cashflows and current market forward cashflows
```

For platform purposes, MTM can come from:

- counterparty valuation;
- market data/valuation vendor;
- internal model using spot, forward points and discount curves.

## 10. FX swap

An FX swap combines a near-leg exchange and a far-leg reverse exchange.

Example:

| Leg | Date | Action |
|---|---|---|
| Near leg | Today/T+2 | Buy USD, sell SGD |
| Far leg | 1 month | Sell USD, buy SGD |

FX swaps are commonly used for liquidity and funding, not necessarily for taking directional FX risk.

### Modelling approach

Represent the swap as a linked transaction group:

```text
FX_SWAP
  +-- Near leg: FX_SPOT-style cash legs
  +-- Far leg: FX_FORWARD-style future cash legs
```

## 11. Non-deliverable forward, NDF

An NDF is a forward contract where the restricted/non-deliverable currency is not physically exchanged. Instead, the difference between the contracted rate and fixing rate is cash-settled, usually in a deliverable currency such as USD.

Example:

| Item | Value |
|---|---:|
| Reference pair | USD/INR |
| Notional | USD 1,000,000 |
| NDF rate | 84.00 |
| Fixing rate | 85.00 |
| Settlement currency | USD |

Only the net difference is exchanged. No INR principal is delivered.

## 12. NDF lifecycle

| Stage | Event | Transaction? |
|---|---|---|
| Trade | NDF contract opened | FX_NDF_OPEN |
| Daily MTM | Valuation | No cash unless collateral |
| Fixing | Official fixing rate determined | Lifecycle event |
| Settlement | Net cash amount paid/received | FX_NDF_CASH_SETTLEMENT |
| Close/roll | Contract closed and new one opened | FX_NDF_CLOSE / FX_NDF_OPEN |

## 13. FX settlement risk

FX settlement risk arises because two currency legs may not settle at exactly the same time or through the same system. A party may pay one currency but fail to receive the other. Payment-versus-payment settlement mechanisms such as CLS are designed to reduce this risk for eligible currencies and participants.

Platform controls should track:

- value dates;
- settlement cutoffs;
- eligible currencies;
- counterparty settlement status;
- failed/mismatched legs;
- partial settlement;
- holiday calendars;
- time-zone differences;
- CLS/non-CLS treatment where relevant.

## 14. FX holidays and value dates

FX settlement dates depend on currency holiday calendars. A trade involving two currencies requires both currencies to be open for settlement.

Common errors:

| Error | Impact |
|---|---|
| Ignoring currency holiday | Incorrect value date |
| Using security settlement calendar for FX | Wrong cash projection |
| Mis-handling USD/CAD or special pairs | Wrong settlement date |
| Ignoring time zone/cutoff | Settlement missed or delayed |

## 15. FX P&L treatment

FX P&L can arise from several sources:

| Source | Example |
|---|---|
| Cash translation | USD cash held by SGD portfolio changes in SGD value |
| Security local price | Apple price changes in USD |
| Currency movement | USD/SGD changes while Apple local price unchanged |
| FX trade spread | Client converts at rate different from mid-market |
| Forward MTM | Forward contract gains/loses value |
| Hedge ineffectiveness | Hedge does not perfectly offset asset FX exposure |

## 16. FX in performance attribution

A useful decomposition for foreign assets:

```text
Base-currency return approx. Local asset return + Currency return + Cross term
```

Example:

| Component | Return |
|---|---:|
| US equity local return | +5.00% |
| USD/SGD currency return | -2.00% |
| Cross term | -0.10% |
| SGD return | about +2.90% |

For precise calculations, use geometric linking rather than simple addition.

## 17. FX exposure model

| Exposure type | Description |
|---|---|
| Cash FX exposure | Foreign currency cash balance |
| Asset FX exposure | Security/fund/alternative asset currency exposure |
| Liability FX exposure | Loans/overdrafts in foreign currency |
| Derivative FX exposure | Forward/swap/NDF/options exposure |
| Hedged exposure | Gross exposure offset by hedges |
| Net currency exposure | Final exposure after offsets |

## 18. Transaction grouping examples

### FX spot conversion

| Transaction group | Transaction | Currency | Amount |
|---|---|---|---:|
| FX-001 | FX_SPOT_BUY_LEG | USD | +100,000 |
| FX-001 | FX_SPOT_SELL_LEG | SGD | -135,000 |

### FX forward maturity

| Transaction group | Transaction | Currency | Amount |
|---|---|---|---:|
| FWD-001 | FX_FORWARD_SETTLEMENT_BUY_LEG | USD | +1,000,000 |
| FWD-001 | FX_FORWARD_SETTLEMENT_SELL_LEG | SGD | -1,360,000 |

### NDF cash settlement

| Transaction group | Transaction | Currency | Amount |
|---|---|---|---:|
| NDF-001 | FX_NDF_CASH_SETTLEMENT | USD | +12,500 |

## 19. Advisory considerations

Before recommending FX conversion or hedging, advisors should understand:

- client base currency;
- natural future liabilities and spending currency;
- investment horizon;
- risk tolerance;
- cost/spread of conversion;
- need for hedge versus willingness to accept FX volatility;
- product complexity if using forwards/options;
- collateral/margin implications;
- tax/accounting considerations;
- suitability for leveraged FX products.

## 20. Interview-ready explanation

FX is both a product and a portfolio infrastructure capability. Spot FX converts cash today or near-term. FX forwards lock a future exchange rate and remain open derivative contracts until maturity. FX swaps combine a near-leg and far-leg and are often used for funding/liquidity. NDFs are cash-settled forwards used where currencies are restricted or non-deliverable. A wealth platform must model FX as multi-leg, value-date-sensitive transactions, track open forward exposure and MTM, and separate accounting cash movement from performance currency translation and advisory FX exposure.

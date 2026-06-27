# 05 - Lifecycle, Transactions, Position and Event Modelling

## 1. Design principle

The same commodity exposure can appear as very different product positions. Therefore, transaction and position modelling should be driven by legal wrapper and economic event, not by the word “commodity.”

Use a canonical model with:

- instrument/security master;
- underlying commodity master;
- position table;
- transaction table;
- transaction legs;
- lifecycle events;
- valuation records;
- look-through exposure records;
- custody/collateral records where relevant.

## 2. Common transaction vocabulary

| Transaction type | Use |
|---|---|
| COMMODITY_BUY | Generic physical/spot commodity purchase |
| COMMODITY_SELL | Generic physical/spot commodity sale |
| METAL_BUY | Precious metal purchase |
| METAL_SELL | Precious metal sale |
| METAL_STORAGE_FEE | Storage/custody fee |
| METAL_TRANSFER_IN | Metal transferred into custody |
| METAL_TRANSFER_OUT | Metal transferred out |
| METAL_COLLATERAL_PLEDGE | Pledge metal as collateral |
| METAL_COLLATERAL_RELEASE | Release metal pledge |
| ETP_BUY | Buy ETF/ETC/ETN/certificate |
| ETP_SELL | Sell ETF/ETC/ETN/certificate |
| ETP_DISTRIBUTION | Distribution from ETP/fund if applicable |
| FUTURE_OPEN | Open futures position |
| FUTURE_CLOSE | Close futures position |
| FUTURE_VARIATION_MARGIN | Daily mark-to-market cash movement |
| FUTURE_INITIAL_MARGIN_POST | Post initial margin |
| FUTURE_MARGIN_RETURN | Margin returned |
| FUTURE_EXPIRY_SETTLEMENT | Expiry cash/physical settlement |
| FUTURE_ROLL_CLOSE | Close near contract for roll |
| FUTURE_ROLL_OPEN | Open next contract for roll |
| OPTION_BUY_PREMIUM | Pay premium to buy option |
| OPTION_SELL_PREMIUM | Receive premium for written option |
| OPTION_EXERCISE | Exercise option |
| OPTION_ASSIGNMENT | Assignment on written option |
| OPTION_EXPIRY | Option expires worthless |
| SWAP_RESET_PAYMENT | Periodic swap settlement |
| SWAP_TERMINATION | Close/terminate swap |
| STRUCTURED_PRODUCT_SUBSCRIPTION | Buy commodity-linked note/product |
| STRUCTURED_PRODUCT_COUPON | Coupon/conditional coupon paid |
| STRUCTURED_PRODUCT_REDEMPTION | Product redeemed/matured |
| COLLATERAL_MOVEMENT | Generic collateral movement |
| FEE | Fee not otherwise classified |
| TAX | Tax/withholding/stamp duty |
| REVERSAL_CORRECTION | Cancel/correct previous transaction |

## 3. Lifecycle by wrapper

### Physical/allocated metal lifecycle

| Stage | Event | Transaction? |
|---|---|---|
| Order | Client buys metal | Order only |
| Execution | Price fixed | METAL_BUY |
| Settlement | Cash paid, metal credited | Settlement status update |
| Custody | Storage and insurance ongoing | METAL_STORAGE_FEE periodically |
| Revaluation | Daily price update | Valuation record, no transaction |
| Sale | Metal sold | METAL_SELL |
| Delivery | Client requests physical withdrawal | METAL_TRANSFER_OUT / delivery fee |
| Pledge | Metal pledged to credit line | METAL_COLLATERAL_PLEDGE lifecycle/encumbrance |

### Unallocated metal account lifecycle

| Stage | Event | Transaction? |
|---|---|---|
| Buy | Metal claim credited | METAL_ACCOUNT_CREDIT / METAL_BUY |
| Sell | Claim reduced | METAL_ACCOUNT_DEBIT / METAL_SELL |
| Conversion | Allocated to physical or converted to cash | METAL_CONVERSION |
| Provider event | Provider downgrade/default | Risk event / write-down if required |

### Commodity ETF/ETC/ETN lifecycle

| Stage | Event | Transaction? |
|---|---|---|
| Buy | Exchange/security purchase | ETP_BUY |
| Holding | Price/NAV changes | Valuation record |
| Distribution | Income/distribution if any | ETP_DISTRIBUTION |
| Sell | Exchange/security sale | ETP_SELL |
| Issuer call/redemption | ETC/ETN redeemed | ETP_REDEMPTION |
| Split/reverse split | Unit adjustment | Corporate action event/transaction |

### Futures lifecycle

| Stage | Event | Transaction? |
|---|---|---|
| Open | Buy/sell futures contract | FUTURE_OPEN |
| Daily MTM | Settlement price changes | FUTURE_VARIATION_MARGIN |
| Margin | Collateral posted/returned | FUTURE_INITIAL_MARGIN_POST / RETURN |
| Roll | Close old, open new | FUTURE_ROLL_CLOSE + FUTURE_ROLL_OPEN |
| Expiry | Cash or physical settlement | FUTURE_EXPIRY_SETTLEMENT |
| Close | Position closed before expiry | FUTURE_CLOSE |

### Commodity-linked structured product lifecycle

| Stage | Event | Transaction? |
|---|---|---|
| Subscription | Client buys product | STRUCTURED_PRODUCT_SUBSCRIPTION |
| Observation | Barrier/coupon/autocall check | Lifecycle event only |
| Coupon | Cash coupon paid | STRUCTURED_PRODUCT_COUPON |
| Autocall | Early redemption condition met | Lifecycle event first |
| Redemption | Cash paid / note closed | STRUCTURED_PRODUCT_REDEMPTION |
| Maturity | Final payoff | Redemption transaction |

## 4. Position models

### Commodity holding position

| Field | Description |
|---|---|
| account_id | Portfolio/account |
| instrument_id | Metal/commodity instrument |
| commodity_id | XAU/WTI/Copper etc. |
| holding_type | PHYSICAL / ALLOCATED_ACCOUNT / UNALLOCATED_ACCOUNT |
| quantity | Quantity in base unit |
| unit_of_measure | oz, grams, barrels, tonnes |
| purity_or_grade | Precious metal fineness or commodity grade |
| location | Vault/warehouse/location |
| custodian_id | Custodian/provider |
| provider_credit_exposure | For unallocated/provider claim |
| market_value | Quantity × price × FX |
| pledged_quantity | Encumbered quantity |
| available_quantity | Free quantity |
| storage_fee_accrual | Optional |

### ETP position

| Field | Description |
|---|---|
| account_id | Portfolio/account |
| instrument_id | ETF/ETC/ETN identifier |
| units | Units/shares held |
| average_cost | Cost per unit |
| market_price | Exchange price |
| nav | NAV/iNAV if available |
| market_value | Units × market price |
| wrapper_type | ETF/ETC/ETN/certificate |
| underlying_exposure | Derived look-through |
| issuer_exposure | Relevant for ETN/certificate |

### Futures position

| Field | Description |
|---|---|
| account_id | Portfolio/account |
| contract_id | Futures contract |
| long_short | Long/short |
| contracts | Number of contracts |
| contract_size | Quantity per contract |
| notional_exposure | contracts × size × price |
| settlement_price | Daily settlement |
| unrealized_mtm | Current MTM |
| variation_margin_to_date | Cash settled gains/losses |
| initial_margin_posted | Collateral posted |
| expiry_date | Contract expiry |
| roll_policy | Roll before expiry |

## 5. Event model

Lifecycle events should not always generate transactions immediately.

| Event | Transaction? | Comment |
|---|---|---|
| Price update | No | Valuation only |
| Futures settlement price published | Yes for variation margin | Daily P&L cash posting |
| Futures contract nearing expiry | No | Alert/workflow event |
| Futures roll decision | Yes when executed | Close/open transactions |
| Commodity ETF NAV published | No | Valuation/reference data |
| Structured product barrier observation | No | Lifecycle event; transaction later if coupon/redemption paid |
| Physical delivery request | Event first | Transaction when delivery confirmed |
| Collateral pledge | Usually encumbrance event | Not sale; position remains owned |

## 6. Cost basis and realized P&L

| Product | Cost basis logic |
|---|---|
| Physical metal | Average/FIFO/specific lot depending jurisdiction and platform policy |
| Metal account | Average/FIFO by metal/provider/account |
| ETF/ETC/ETN | Security cost basis like listed securities |
| Futures | Realized daily via variation margin in some accounting models |
| Options | Premium capitalized and realized on sale/exercise/expiry |
| Structured product | Note/product cost basis; coupon income separate |

## 7. Performance classification

| Transaction | Classification |
|---|---|
| External cash used to buy commodity | External contribution then investment transaction |
| Buy from existing portfolio cash | Internal investment transaction |
| Sell proceeds retained in portfolio | Internal disposal |
| Sell proceeds withdrawn | Internal disposal plus external withdrawal |
| Storage fee paid from portfolio | Expense reducing performance if charged to portfolio |
| Futures variation margin | Investment P&L cash settlement |
| Margin collateral movement | Usually not performance cashflow if internal collateral movement |
| Commodity-linked note coupon | Income if coupon is investment income |
| Physical delivery out to client | External withdrawal if leaving portfolio |

## 8. Common platform mistakes

| Mistake | Correct approach |
|---|---|
| Treating XAU as cash | XAU is commodity exposure, not risk-free cash |
| Ignoring unallocated provider risk | Model provider as counterparty/issuer exposure |
| Modelling futures exposure only as cash | Need notional, margin, MTM and roll lifecycle |
| Treating futures collateral movement as P&L | Collateral movement is not return; variation margin is P&L |
| Reporting oil ETF as spot oil | Need disclose futures/roll-based exposure if applicable |
| Ignoring storage fees | Fees affect net return |
| Creating physical commodity positions for structured notes | Structured notes remain note positions; commodity is underlying exposure |

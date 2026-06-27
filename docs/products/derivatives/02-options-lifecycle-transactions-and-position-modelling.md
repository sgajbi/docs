# 02 — Options Lifecycle, Transactions and Position Modelling

## 1. What is an option?

An **option** gives the buyer a right, but not an obligation, to buy or sell an underlying asset at a pre-agreed price within a specified period or on a specified date.

| Option type | Buyer right | Buyer view |
|---|---|---|
| Call option | Right to buy underlying | Bullish / wants upside |
| Put option | Right to sell underlying | Bearish / wants protection |

The seller/writer receives premium but takes the obligation if the buyer exercises.

| Role | Cash at trade | Future obligation |
|---|---|---|
| Option buyer | Pays premium | Has right, no obligation |
| Option seller/writer | Receives premium | Has obligation if exercised/assigned |

## 2. Key option terms

| Term | Meaning |
|---|---|
| Underlying | Security/index/FX/rate/commodity linked to option |
| Strike | Price at which buyer can buy/sell |
| Expiry | Date option expires |
| Style | American, European, Bermudan |
| Premium | Price paid by buyer to seller |
| Contract size | Number of underlying units controlled |
| Moneyness | In-the-money, at-the-money, out-of-the-money |
| Intrinsic value | Immediate exercise value |
| Time value | Value above intrinsic value |
| Implied volatility | Market-implied future volatility |
| Settlement type | Cash or physical |
| Exercise type | Automatic, manual, early exercise allowed |

## 3. Vanilla option payoff

### Long call

```text
Payoff at expiry = max(Underlying Price - Strike, 0) × Contract Size
Profit = Payoff - Premium Paid
```

### Long put

```text
Payoff at expiry = max(Strike - Underlying Price, 0) × Contract Size
Profit = Payoff - Premium Paid
```

### Short call / short put

The writer receives premium but has the opposite payoff. Short naked options can create very large or unlimited losses depending on product.

## 4. Common option strategies

| Strategy | Legs | Use | Key risk |
|---|---|---|---|
| Protective put | Long equity + long put | Downside protection | Premium cost |
| Covered call | Long equity + short call | Income generation | Upside capped |
| Collar | Long equity + long put + short call | Protect downside, reduce cost | Upside limited |
| Cash-secured put | Cash + short put | Buy stock at lower effective level / income | Stock can fall below strike |
| Bull call spread | Long call + short higher strike call | Limited upside view | Upside capped |
| Bear put spread | Long put + short lower strike put | Limited downside view | Protection capped |
| Straddle | Long call + long put same strike | Volatility view | Premium loss if no move |
| Strangle | Long OTM call + long OTM put | Cheaper volatility view | Needs larger move |

## 5. Option lifecycle

| Stage | Event | Platform object | Transaction? |
|---|---|---|---|
| Product setup | Contract listed or OTC terms agreed | Instrument/contract master | No |
| Order | Client places order | Order | No |
| Trade | Option bought/sold | Trade + premium transaction | Yes |
| Holding | MTM changes daily | Valuation records | No |
| Margin | Short option margin changes | Margin/collateral event | Yes if cash/collateral moves |
| Corporate action adjustment | Strike/contract size adjusted | Contract adjustment event | Usually no cash transaction |
| Exercise | Buyer exercises | Exercise event | Yes, if settlement occurs |
| Assignment | Writer assigned | Assignment event | Yes, if settlement occurs |
| Expiry ITM cash settled | Option settles in cash | Expiry settlement transaction | Yes |
| Expiry OTM | Option expires worthless | Expiry close transaction | Yes, close position; no cash |
| Close-out | Client sells/buys back option | Closing trade | Yes |

## 6. Recommended option transaction types

| Transaction type | Use |
|---|---|
| OPTION_BUY_OPEN | Buy option to open long position |
| OPTION_SELL_OPEN | Sell/write option to open short position |
| OPTION_BUY_CLOSE | Buy option to close short position |
| OPTION_SELL_CLOSE | Sell option to close long position |
| OPTION_PREMIUM_PAID | Cash premium paid by buyer |
| OPTION_PREMIUM_RECEIVED | Cash premium received by writer |
| OPTION_EXERCISE | Long option exercised |
| OPTION_ASSIGNMENT | Short option assigned |
| OPTION_EXPIRY_WORTHLESS | Option expires out-of-the-money |
| OPTION_CASH_SETTLEMENT | Cash settlement at expiry/exercise |
| OPTION_PHYSICAL_DELIVERY_IN | Underlying received due to exercise/assignment |
| OPTION_PHYSICAL_DELIVERY_OUT | Underlying delivered due to exercise/assignment |
| OPTION_MARGIN_POSTED | Margin cash/collateral posted |
| OPTION_MARGIN_RETURNED | Margin released |
| OPTION_FEE | Brokerage/exchange/clearing fee |
| OPTION_TAX_WITHHOLDING | Tax if applicable |
| OPTION_CONTRACT_ADJUSTMENT | Non-cash lifecycle adjustment for split/merger etc. |
| OPTION_REVERSAL_CORRECTION | Cancel/correct transaction |

## 7. Position modelling

### Listed option position

| Field | Meaning |
|---|---|
| account_id | Portfolio/account |
| option_instrument_id | Unique option contract |
| underlying_id | Underlying security/index |
| option_type | CALL / PUT |
| position_side | LONG / SHORT |
| contract_quantity | Number of contracts, signed |
| contract_size | Underlying units per contract |
| strike | Exercise price |
| expiry_date | Expiry |
| exercise_style | American / European / Bermudan |
| settlement_type | CASH / PHYSICAL |
| premium_cost | Premium paid/received |
| market_price | Current option price |
| market_value | Contracts × contract size × price × side |
| delta | Option delta |
| delta_equivalent_exposure | Underlying price × contract size × contracts × delta |
| margin_requirement | Margin required for short position |
| lifecycle_status | ACTIVE / EXERCISED / ASSIGNED / EXPIRED / CLOSED |

### OTC option position

Additional fields are needed:

| Field | Meaning |
|---|---|
| counterparty_id | Dealer/counterparty |
| legal_agreement_id | ISDA/CSA or local agreement |
| confirmation_id | Trade confirmation |
| valuation_agent | Counterparty / independent / internal |
| collateral_agreement | Margin/collateral terms |
| premium_settlement_date | Premium value date |
| exercise_notice_rules | How and when exercise must be instructed |
| settlement_fixing_source | FX/index/equity fixing source |

## 8. Example: covered call

Client owns 1,000 Apple shares and sells 10 call option contracts, each controlling 100 shares.

| Item | Value |
|---|---:|
| Apple shares | 1,000 |
| Call contracts sold | 10 |
| Contract size | 100 |
| Strike | USD 220 |
| Premium received | USD 3 per share |
| Total premium | USD 3,000 |

Transactions:

| Transaction | Instrument | Quantity | Cash |
|---|---|---:|---:|
| OPTION_SELL_OPEN | Apple call | -10 contracts | 0 |
| OPTION_PREMIUM_RECEIVED | Cash | 0 | +3,000 |

At expiry:

| Scenario | Outcome |
|---|---|
| Apple below 220 | Option expires worthless; client keeps premium |
| Apple above 220 | Client may be assigned and shares sold at 220 |

If assigned:

| Transaction | Instrument | Quantity | Cash |
|---|---|---:|---:|
| OPTION_ASSIGNMENT | Apple call | +10 contracts close | 0 |
| EQUITY_SELL_DELIVERY | Apple shares | -1,000 shares | +220,000 |

## 9. Example: protective put

Client owns an equity portfolio worth USD 1,000,000 and buys index puts.

| Component | Purpose |
|---|---|
| Long equity | Upside exposure |
| Long put | Downside protection |
| Premium paid | Cost of insurance |

Performance reporting should show:

- equity price return;
- option valuation/P&L;
- premium cost;
- net portfolio return after hedge;
- downside protection effect during stress periods.

## 10. Corporate actions and option adjustments

Options may be adjusted for:

| Corporate action | Possible option adjustment |
|---|---|
| Stock split | Strike and contract size adjusted |
| Special dividend | Strike adjusted |
| Merger | Deliverable may change to cash/new shares/basket |
| Spin-off | Deliverable becomes package of securities |
| Rights issue | Strike/deliverable adjusted depending rules |

Important implementation point:

```text
Do not treat every option adjustment as a new trade.
Represent it as a contract adjustment lifecycle event with full audit history.
```

## 11. Performance treatment

| Event | Performance treatment |
|---|---|
| Premium paid for long option | Investment cost / internal portfolio activity |
| Premium received for written option | Investment income or option premium P&L depending policy |
| MTM change | Unrealized investment P&L |
| Close-out | Realized investment P&L |
| Expiry worthless | Realized loss for long option; realized gain for short option |
| Exercise into underlying | Internal conversion / acquisition or disposal |
| Assignment | Internal option settlement + underlying sale/purchase |
| Margin posted | Usually collateral movement, not performance loss |
| Variation margin on listed futures-style options | P&L/cash movement depending exchange model |

## 12. Advisory perspective

Options can be useful for hedging and income strategies, but suitability depends heavily on client sophistication and strategy.

| Strategy | Advisory question |
|---|---|
| Long call | Can client lose full premium? |
| Long put | Is premium cost acceptable for protection? |
| Covered call | Is client comfortable selling upside? |
| Naked short call | Is unlimited loss risk prohibited? |
| Short put | Is client willing and able to own underlying if assigned? |
| Option spread | Does client understand both legs and max loss/gain? |
| 0DTE option | Is high gamma/expiry risk appropriate? |

For mandates, controls should distinguish hedging options from speculative leverage.

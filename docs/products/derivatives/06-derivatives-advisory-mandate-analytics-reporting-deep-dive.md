# 06 — Derivatives Advisory, Mandate, Analytics and Reporting Deep Dive

## 1. Why derivatives matter in advisory and DPM

Derivatives are not only trading instruments. In wealth management they are used to shape portfolio outcomes:

| Objective | Derivative usage |
|---|---|
| Protect downside | Protective puts, collars, index hedges |
| Generate income | Covered calls, put-writing strategies |
| Manage currency risk | FX forwards, FX options, share-class hedging |
| Adjust duration | Bond futures, interest-rate swaps |
| Manage equity beta | Equity index futures/options |
| Manage credit risk | CDS or credit index hedges |
| Equitize cash | Futures to maintain market exposure while holding cash |
| Rebalance efficiently | Futures/forwards before physical trades settle |
| Access markets | Swaps/forwards where direct access is difficult |

The same derivative can be prudent in one mandate and inappropriate in another.

## 2. Advisory suitability framework

Before recommending or approving derivative usage, assess:

| Area | Key questions |
|---|---|
| Client classification | Retail, accredited, professional, ECP? |
| Product knowledge | Does client understand derivative mechanics? |
| Risk profile | Can client bear loss, margin calls and volatility? |
| Objective | Hedge, income, leverage, tactical or speculative? |
| Time horizon | Does expiry/maturity match the investment horizon? |
| Liquidity | Can client meet margin or settlement obligations? |
| Underlying familiarity | Does client understand underlying asset/currency/rate? |
| Worst-case loss | Is max loss limited or potentially unlimited? |
| Complexity | Is payoff linear or non-linear/path-dependent? |
| Concentration | Does derivative create hidden concentration? |
| Counterparty risk | Is OTC counterparty acceptable? |
| Reporting clarity | Can advisor explain exposure and P&L? |

## 3. Product-level advisory questions

| Product | Advisory focus |
|---|---|
| Long option | Full premium can be lost |
| Short option | Obligation, assignment risk, potentially large loss |
| Covered call | Income but upside capped |
| Protective put | Hedge cost and expiry risk |
| Futures | Leverage and margin call risk |
| FX forward | Settlement liquidity and rollover risk |
| NDF | Fixing and restricted currency risk |
| Interest-rate swap | DV01, curve risk, early termination cost |
| Cross-currency swap | FX/rate/basis/collateral risk |
| TRS/equity swap | Synthetic exposure, leverage, counterparty risk |
| CDS | Reference credit event, recovery and documentation risk |

## 4. Mandate rule framework

A DPM/advisory mandate should not simply say “derivatives allowed” or “not allowed”. It should define permitted purpose and exposure limits.

| Rule dimension | Example |
|---|---|
| Permitted product family | Listed futures allowed; OTC exotics prohibited |
| Purpose | Hedging only / efficient portfolio management / tactical overlay |
| Underlying eligibility | Major indices only, G10 FX only, investment-grade rates only |
| Gross notional limit | Gross derivative notional <= 100% NAV |
| Net exposure limit | Net equity exposure 40–70% NAV |
| Delta-equivalent limit | Options counted by delta-equivalent exposure |
| DV01 limit | Rate derivatives by duration/DV01 |
| CS01 limit | Credit derivatives by spread sensitivity |
| Vega limit | Option volatility exposure cap |
| Leverage limit | Derivative leverage not to exceed mandate threshold |
| Margin utilization | Margin posted <= x% portfolio NAV |
| Counterparty limit | OTC exposure by counterparty group |
| Liquidity limit | No illiquid OTC derivatives beyond x% NAV |
| Expiry concentration | No excessive exposure expiring same date |
| Short option rules | Naked short options prohibited or separately approved |
| Physical delivery rule | Futures/options must be closed before delivery unless authorized |

## 5. Advisory vs discretionary treatment

| Context | Treatment |
|---|---|
| Execution-only | Client instruction, but platform still checks product eligibility and regulatory requirements |
| Advisory | Suitability, product explanation, risk disclosure, rationale required |
| DPM | Mandate constraints, investment committee rules and model/risk oversight apply |
| Structured product manufacturing | Derivatives are hedge/manufacturing legs, not necessarily client-visible positions |
| Portfolio overlay | Derivatives should be tagged to hedge or exposure objective |

## 6. Analytics views required for derivatives

A portfolio analytics platform should provide multiple views:

| View | Purpose |
|---|---|
| Accounting position | Legal/client holding |
| Market value | Current MTM |
| Notional exposure | Contract face exposure |
| Risk-equivalent exposure | Delta, DV01, CS01, vega |
| Look-through exposure | Underlying asset/currency/rate/credit exposure |
| Margin/collateral view | Liquidity and collateral usage |
| P&L view | Realized/unrealized and P&L explanation |
| Stress view | Downside under scenarios |
| Mandate view | Limit usage and breach monitoring |
| Client-reporting view | Simplified explanation and next lifecycle event |

## 7. Exposure reporting examples

### Equity option

| Measure | Example |
|---|---:|
| Market value | USD 20,000 |
| Notional | USD 500,000 |
| Delta | 0.45 |
| Delta-equivalent exposure | USD 225,000 |
| Vega | USD 1,200 per 1 vol point |
| Expiry | 3 months |

### Interest-rate swap

| Measure | Example |
|---|---:|
| Market value | USD -150,000 |
| Notional | USD 10,000,000 |
| DV01 | USD 4,500 per bp |
| Pay/receive | Pay fixed |
| Maturity | 5 years |

### FX forward

| Measure | Example |
|---|---:|
| Market value | SGD 12,000 |
| Buy currency | SGD |
| Sell currency | USD |
| Notional | USD 1,000,000 |
| Forward rate | 1.3500 |
| Maturity | 2 months |

## 8. Performance treatment

Derivatives require careful TWR/performance classification.

| Event | Performance classification |
|---|---|
| Option premium paid/received | Internal investment transaction, not external flow if paid from portfolio cash |
| Futures variation margin | Investment P&L cash settlement |
| Initial margin | Collateral movement, not P&L |
| OTC collateral posted | Collateral movement, not P&L |
| Swap coupon/payment | Investment P&L or income/expense by policy |
| FX forward settlement | Currency conversion + realized derivative P&L |
| Exercise/assignment | Internal conversion into underlying/security/cash |
| Expiry worthless | Realized investment gain/loss |
| Client adds cash for margin call | External inflow |
| Client withdraws excess margin | External outflow |

## 9. Reporting to clients and advisors

Client reporting should avoid hiding derivatives in technical tables only. It should answer:

1. What derivative do we hold?
2. What is it linked to?
3. Why do we hold it?
4. What is the current value?
5. What is the exposure if markets move?
6. What is the maximum or stressed loss?
7. What margin/collateral is required?
8. What are the next important dates?
9. Is it hedging or adding risk?
10. How has it contributed to performance?

## 10. Portfolio review wording examples

### Hedge explanation

“The portfolio holds index put options to reduce downside equity risk over the next three months. The option premium reduces current return, but the position is expected to offset part of the equity loss if the market falls sharply before expiry.”

### Income strategy explanation

“The portfolio has sold covered call options against existing equity holdings. This generated option premium income, but part of the equity upside is capped if the shares rise above the strike price.”

### FX hedge explanation

“The portfolio uses FX forwards to reduce USD exposure relative to the client’s SGD reference currency. The forward contracts may generate gains or losses depending on currency movements, but their purpose is to reduce total portfolio currency volatility.”

### Duration overlay explanation

“Bond futures are used to adjust portfolio duration efficiently. The futures exposure changes interest-rate sensitivity without requiring immediate trading in all underlying bonds.”

## 11. Mandate breach examples

| Scenario | Possible breach |
|---|---|
| Short call not backed by underlying | Naked option breach |
| Futures notional exceeds NAV limit | Leverage breach |
| FX forward increases non-reference currency exposure | Currency mandate breach |
| Swap creates high DV01 | Duration/rate risk breach |
| CDS selling protection on high-yield issuer | Credit quality breach |
| OTC derivative with unapproved dealer | Counterparty breach |
| Option position expires into physical delivery | Operational/mandate breach |

## 12. Advisor/product-owner questions

For every derivative capability, ask:

- Is this derivative allowed for this client segment?
- Is it advisory, DPM or execution-only?
- Does the product create leverage or only hedge exposure?
- Which exposure measure controls the mandate?
- How is margin funded?
- What happens on expiry/exercise/assignment?
- What happens if the client cannot meet margin call?
- What is the fallback valuation source?
- How does this show in client reporting?
- How is P&L explained?
- What should happen during migration if open derivatives exist?

## 13. Common advisory mistakes

| Mistake | Better framing |
|---|---|
| Looking only at premium | Look at notional and worst-case loss |
| Calling covered call “free income” | It sells upside and may trigger sale |
| Treating margin as loss | Margin is collateral; VM may be P&L |
| Ignoring expiry | Derivative risk changes sharply near expiry |
| Reporting only market value | Report exposure and Greeks too |
| Treating FX forwards as simple cash trades | They carry forward MTM and settlement risk |
| Treating all derivatives as speculative | Some are risk-reducing hedges |

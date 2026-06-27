# 08 — Derivatives Glossary, Practitioner Reference and Product Combinations

## 1. Core glossary

| Term | Meaning |
|---|---|
| Derivative | Contract whose value depends on an underlying/reference |
| Underlying | Asset, rate, index, FX, commodity or credit entity driving value |
| Notional | Reference amount used to calculate payoff/exposure |
| Market value / MTM | Current value of derivative contract |
| Long | Position benefits from certain favorable movement / owns right |
| Short | Position has obligation or benefits from opposite movement |
| Premium | Price paid/received for option |
| Strike | Exercise price of option |
| Expiry | Date option/future expires |
| Maturity | End date of forward/swap/CDS |
| Exercise | Option buyer uses right |
| Assignment | Option writer is required to perform |
| Initial margin | Collateral posted at start/for open risk |
| Variation margin | MTM gain/loss cash settlement |
| Collateral | Assets posted to reduce counterparty exposure |
| Delta | Sensitivity to underlying movement |
| Gamma | Sensitivity of delta |
| Vega | Sensitivity to implied volatility |
| Theta | Time decay |
| Rho | Sensitivity to interest rates |
| DV01/PV01 | Rate sensitivity for 1 bp move |
| CS01 | Credit spread sensitivity for 1 bp move |
| Basis risk | Hedge and hedged exposure do not move perfectly together |
| Counterparty risk | Risk that OTC counterparty fails |
| Cleared derivative | Derivative cleared by clearing house |
| OTC derivative | Bilateral/custom derivative |
| ISDA | Standard OTC derivative legal framework |
| CSA | Collateral support annex/agreement |

## 2. Product reference table

| Product | Client position | Upfront cash | Main lifecycle | Main risk |
|---|---|---|---|---|
| Long call | Right to buy | Premium paid | Expiry/exercise | Premium loss, market risk |
| Short call | Obligation to sell | Premium received | Assignment/expiry | Large/unlimited loss if naked |
| Long put | Right to sell | Premium paid | Expiry/exercise | Premium loss |
| Short put | Obligation to buy | Premium received | Assignment/expiry | Downside if underlying falls |
| Future | Obligation via exchange | Margin | Daily VM, expiry | Leverage/margin |
| FX forward | Exchange currencies future date | Usually zero | Maturity/roll | FX/counterparty/liquidity |
| NDF | Cash-settled FX forward | Usually zero | Fixing/settlement | Fixing and FX risk |
| IRS | Exchange fixed/floating | Usually zero | Resets/payments | Rate/DV01 |
| CCS | Exchange cashflows in two currencies | Sometimes principal exchange | Resets/payments | FX/rate/basis |
| TRS | Receive/pay total return | Collateral/margin | Resets/payments | Synthetic exposure/leverage |
| CDS | Buy/sell credit protection | Premium/coupon | Credit event | Default/recovery/counterparty |

## 3. Product combinations

| Combination | Product created |
|---|---|
| Bond + call option | Principal-protected note / equity participation note |
| Bond + short put | Reverse convertible / equity-linked note |
| Bond + barrier option | Barrier note / FCN |
| Bond + autocall feature | Autocallable note / Phoenix note |
| Deposit + short FX option | Dual currency investment/note |
| Bond + CDS-like credit exposure | Credit-linked note |
| Equity + short call | Covered call strategy |
| Equity + long put | Protective put strategy |
| Equity + long put + short call | Collar |
| Cash + futures | Cash equitization overlay |
| Bond portfolio + bond futures | Duration overlay |
| Foreign assets + FX forwards | Currency hedge |
| Portfolio + index puts | Downside hedge |
| Rates exposure + IRS | Duration/rate hedge |
| Credit portfolio + CDS index | Credit hedge |

## 4. Practitioner explanation

“Derivatives are contracts whose value depends on an underlying asset, rate, currency, index, commodity or credit event. They are used for hedging, efficient exposure, income generation and sometimes leverage or speculation. From a platform perspective, derivatives are different from normal securities because market value can be small while notional exposure is large. Therefore the system must model contract terms, underlyings, lifecycle events, margin and collateral, valuation, Greeks, notional exposure, delta-equivalent exposure, DV01, CS01 and counterparty risk. Transactions should capture economic postings such as premium, margin, variation margin, cash settlement, exercise, assignment and swap payments. Lifecycle events such as expiry, reset, fixing or credit event should be stored separately from transactions. In advisory and DPM, the key is to understand purpose, suitability, mandate limits, leverage, liquidity, margin calls, worst-case loss and reporting transparency.”

## 5. Explaining derivatives to a non-technical business user

“A derivative is not usually the asset itself. It is a contract linked to the asset. For example, instead of buying an equity index directly, a portfolio can use a futures contract linked to that index. This may be efficient, but it creates exposure larger than the cash initially paid. That is why reporting should show not only market value, but also notional exposure, risk sensitivity, margin and purpose.”

## 6. Advisory-friendly explanations

### Option

“An option gives the buyer a right and the seller an obligation. The buyer pays premium for that right. A long option has limited loss to the premium paid, but a written option can create large obligations if the market moves against the writer.”

### Future

“A future creates market exposure through an exchange-traded contract. The client posts margin rather than paying the full notional. Gains and losses are settled daily through variation margin.”

### FX forward

“An FX forward locks an exchange rate for a future date. It can reduce currency risk, but it also creates settlement and mark-to-market risk if currency moves.”

### Swap

“A swap exchanges cashflows, such as fixed interest against floating interest. The contract may start with little or no value, but it can create significant sensitivity to rates, FX or credit.”

### CDS

“A CDS transfers credit default risk. The protection buyer pays premium; the protection seller receives premium but must pay if the reference entity experiences a credit event.”

## 7. Practitioner Questions and Answers

### Q1. Why is notional not the same as market value?

Market value is the current fair value of the derivative. Notional is the reference amount used to calculate payoff and exposure. A swap may have near-zero market value on day one but a notional of USD 10 million and meaningful DV01.

### Q2. Why can derivatives be risky even with small upfront cost?

Because they can create leverage. The upfront premium or margin may be small compared with the underlying exposure. Losses can exceed initial cash paid for some products, especially futures and written options.

### Q3. How should a platform model option exercise?

Exercise should be a lifecycle event that generates settlement transactions. For physical settlement, close the option contract and create or deliver the underlying security/cash according to contract terms.

### Q4. How should margin be treated in performance?

Initial margin and collateral postings are generally collateral movements, not investment losses. Variation margin may represent realized daily P&L for exchange-traded futures. Classification must be product- and accounting-policy-specific.

### Q5. What is the difference between hedge and speculation?

A hedge reduces an existing portfolio risk. Speculation adds risk to profit from market movement. The same product can be hedge or speculation depending on the portfolio context and purpose tag.

### Q6. Why are derivatives difficult for client reporting?

Because market value alone is insufficient. Reports need exposure, sensitivity, margin, purpose, P&L contribution, underlying look-through and next lifecycle event.

## 8. Platform design reference table

| Requirement | Good design |
|---|---|
| Product terms | Contract master with product-specific extensions |
| Underlying exposure | Separate look-through/risk exposure table |
| Lifecycle | Event table independent from transactions |
| Transactions | Multi-leg accounting/economic postings |
| Valuation | Source hierarchy and model audit trail |
| Risk | Store Greeks/sensitivities by risk factor |
| Margin | Separate collateral ledger/classification |
| Mandates | Use notional and risk-equivalent exposure |
| Reporting | Show market value + exposure + purpose |
| Reconciliation | Broker/dealer/custodian comparison daily |

## 9. Minimum data required by product

| Product | Minimum critical fields |
|---|---|
| Option | Underlying, call/put, strike, expiry, style, contract size, premium, settlement type |
| Future | Underlying, contract month, multiplier, tick size, settlement price, margin |
| FX forward | Buy/sell currencies, amounts, forward rate, maturity, settlement type |
| NDF | Currency pair, notional, contract rate, fixing source/date, settlement currency |
| IRS | Notional, fixed rate, floating index, reset/payment schedule, day count, maturity |
| CCS | Currency legs, notionals, FX rate, rate terms, principal exchange, basis spread |
| TRS | Reference asset, notional, financing leg, reset schedule, dividend treatment |
| CDS | Reference entity, notional, premium, maturity, credit events, recovery/settlement terms |

## 10. Key takeaway

The most important derivative platform principle is:

```text
Do not confuse cash invested, market value, notional exposure and risk exposure.
```

A high-quality wealth platform must represent all four clearly:

1. cash invested or margin posted;
2. market value / MTM;
3. notional exposure;
4. risk-equivalent exposure such as delta, DV01, CS01, vega and stress loss.

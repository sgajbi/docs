# 07 — Glossary, Practitioner Reference and Review Checklists

## 1. One-minute explanation

A structured product is an investment wrapper whose return is determined by a formula linked to one or more underlyings. It can be issued as a note, deposit, certificate, warrant, fund or OTC contract. The client normally holds the wrapper, not the underlying directly. The payoff is built using debt/deposit components and derivatives such as options, forwards, swaps or credit derivatives. Structured products can provide enhanced income, principal protection, market participation, hedging or leverage, but they introduce complexity, issuer/counterparty risk, liquidity risk, path dependency and sometimes principal loss.

## 2. Practitioner-ready platform answer

“From a wealth platform perspective, structured products should be modelled using a common wrapper-plus-payoff architecture. The legal position represents what the client actually holds, such as a note, deposit, certificate or warrant. The payoff model stores underlyings, strikes, barriers, coupon rules, observation dates, settlement terms and protection features. Lifecycle events such as observations, barrier breaches, coupon determinations and autocall triggers should be separated from accounting transactions. Transactions should only represent economic postings such as subscription, coupon, redemption, sale, physical delivery, alternate-currency redemption, write-down or recovery. For analytics and reporting, the platform needs both legal allocation and economic look-through exposure, including issuer risk, underlying exposure, liquidity, barrier distance, scenario loss and mandate concentration.”

## 3. Structured product versus related products

| Product | Difference |
|---|---|
| Bond | Plain debt with coupon/principal, usually no complex payoff formula |
| Note | Debt wrapper; structured note is one type of structured product |
| Deposit | Bank placement; structured deposit adds market-linked return |
| Equity | Direct ownership; structured product may reference equity without ownership |
| Fund | Pooled vehicle with units/NAV; structured fund may embed option strategy |
| Derivative | Contract whose value derives from underlying; structured product embeds derivatives in wrapper |
| Certificate | Often exchange-traded structured wrapper |
| Warrant | Listed leveraged option-like product |

## 4. Key terms

| Term | Meaning |
|---|---|
| Wrapper | Legal form of product held by client |
| Underlying | Asset/index/rate/FX/credit reference driving payoff |
| Strike | Reference price/rate/level |
| Initial fixing | Starting level used for payoff comparison |
| Final fixing | Level used for final payoff |
| Barrier | Threshold that changes payoff if touched/observed |
| Knock-in | Event that activates downside or feature |
| Knock-out | Event that terminates or deactivates product |
| Autocall | Early redemption if condition is met |
| Participation rate | Percentage of underlying return passed to investor |
| Cap | Maximum return |
| Floor | Minimum return |
| Buffer | First layer of loss absorbed before client loses principal |
| Principal protection | Contractual protection at maturity, subject to terms and issuer/counterparty risk |
| Conditional protection | Protection only if condition holds, such as barrier not breached |
| Coupon barrier | Level required for coupon payment |
| Memory coupon | Missed coupon may be paid later if condition is met |
| Worst-of | Payoff depends on worst-performing basket component |
| Best-of | Payoff depends on best-performing component |
| Digital payoff | Fixed payoff if condition met |
| Physical settlement | Underlying asset delivered instead of cash |
| Alternate currency redemption | Principal repaid in another currency |
| Issuer bid | Price issuer may offer to buy back product |
| Indicative value | Estimated value, not necessarily executable |
| Delta | Sensitivity to underlying price |
| Vega | Sensitivity to volatility |
| Barrier distance | Distance between current level and barrier |

## 5. Product family reference

| Product family | Main client objective | Main risk |
|---|---|---|
| Principal-protected | Protect principal at maturity with upside | Issuer risk, limited upside, early-exit loss |
| Yield enhancement | Higher income | Downside risk, conditional coupon, liquidity |
| Autocallable | Income with early redemption | Path dependency, reinvestment, downside barrier |
| Reverse convertible | High coupon | Equity downside/physical delivery |
| DCI | Enhanced FX yield | Alternate currency repayment |
| CLN | Credit spread income | Reference credit event and issuer risk |
| Certificate | Market exposure via listed wrapper | Issuer, liquidity, payoff complexity |
| Warrant | Leveraged exposure | Total loss of premium, expiry, volatility |
| Accumulator | Build position at target level | Forced leveraged buying in falling market |
| Decumulator | Sell position at target level | Forced selling in rising market/opportunity loss |
| Buffered product | Defined downside buffer | Loss beyond buffer, cap, issuer risk |
| Leveraged certificate | Tactical leveraged exposure | Rapid losses, path dependency |

## 6. Common mistakes

| Mistake | Correct view |
|---|---|
| High coupon means safe income | High coupon usually compensates for hidden risk |
| Principal protected means risk-free | Issuer risk and early-exit risk remain |
| Product linked to equity means client owns equity | Usually no, unless physical settlement occurs |
| Listed means liquid | Listing does not guarantee deep liquidity |
| Deposit means no investment risk | Structured deposits can have market-linked risk and early withdrawal cost |
| Coupon is always earned | Conditional coupons may not be paid |
| Worst-of basket is diversified | Worst-of can increase downside risk |
| Barrier not breached today means safe | Risk remains until observation/maturity ends |
| Issuer price equals fair value | Issuer price may include margins and bid/offer effects |
| Legal allocation is enough | Need economic look-through exposure |

## 7. Questions to ask when reviewing a structured product

### Product design

1. What is the wrapper?
2. Who is the issuer/counterparty?
3. What is the underlying or basket?
4. What is the payoff formula?
5. Is principal protected, conditional or at risk?
6. What are the coupon conditions?
7. Are there barriers, autocall or memory features?
8. Is settlement cash, physical or alternate currency?
9. What happens in the worst case?
10. Is there early termination?

### Advisory

1. Does the client understand the product?
2. Is it suitable for the risk profile?
3. Does it meet the investment objective?
4. Can the client hold to maturity?
5. Is the client comfortable with issuer risk?
6. Does the client understand liquidity limitations?
7. Does it create concentration?
8. Is the product allowed by mandate?
9. Is the delivered asset allowed if physical settlement occurs?
10. Has disclosure been recorded?

### Platform

1. Is product master complete?
2. Are lifecycle schedules loaded?
3. Are underlyings mapped to security master?
4. Are price/fixing sources configured?
5. Is valuation source hierarchy set?
6. Is event processing automated or manual?
7. Are transaction types mapped correctly?
8. Is look-through exposure available?
9. Are mandate rules connected?
10. Are reconciliation and audit trails complete?

## 8. Reporting phrases

### Simple structured product description

“This product is a structured investment whose return depends on the performance of the referenced underlying. The client holds the structured product issued by the issuer, not the underlying asset directly.”

### Principal protection explanation

“Principal protection applies only according to product terms, usually at maturity, and remains subject to issuer/counterparty credit risk. If the product is sold before maturity, the market value may be below principal.”

### Yield enhancement explanation

“The enhanced coupon is compensation for accepting defined risks such as underlying downside, barrier risk, liquidity risk and issuer risk. The coupon should not be viewed as equivalent to a risk-free or plain bond coupon.”

### Worst-of explanation

“In a worst-of structure, the final outcome can be determined by the weakest-performing underlying in the basket, even if the other underlyings perform well.”

### Autocall explanation

“The product may redeem early if the autocall condition is met. This can shorten the holding period and create reinvestment risk. If the condition is not met, the product continues and downside risk may remain.”

### Liquidity explanation

“The product may not have an active secondary market. Any early exit may depend on issuer bid pricing and may occur at a value below the investment amount.”

## 9. Product modelling reference

| Product | Position | Key event | Key transaction |
|---|---|---|---|
| Structured note | Nominal/units | Observation, coupon, autocall, maturity | Subscription, coupon, redemption, sale |
| Structured deposit | Principal | Fixing, maturity | Placement, redemption, early withdrawal |
| Certificate | Units | Knock-out, issuer call, expiry | Buy, sell, redemption |
| Warrant | Units | Expiry, exercise, knock-out | Buy, sell, exercise/expiry |
| Accumulator | Contract + delivered underlying | Periodic fixing, knock-out | Periodic buy/delivery, settlement |
| DCI | Principal | FX fixing | Alternate currency redemption |
| CLN | Nominal | Credit event | Write-down, recovery, redemption |

## 10. Analytics reference

| Analytics | Purpose |
|---|---|
| Legal allocation | What client legally holds |
| Economic look-through | Actual risk exposure |
| Issuer concentration | Credit exposure to issuers |
| Underlying concentration | Hidden exposure to same names/indices |
| Barrier distance | Near-trigger risk |
| Autocall schedule | Reinvestment and cashflow planning |
| Scenario analysis | Good/base/bad outcome explanation |
| Delta-equivalent exposure | Risk-adjusted market exposure |
| Worst-case loss | Suitability and disclosure |
| Liquidity score | Exit risk |
| Income reliability | Fixed versus conditional income |
| Maturity ladder | Future cashflow planning |

## 11. Practitioner answer: why structured products are hard in wealth platforms

“Structured products are hard because the legal holding, economic exposure and lifecycle are different. A client may legally hold one note or certificate, but economically the product may contain equity, FX, credit, rates, volatility, barrier and issuer risk. The platform must support termsheet-level product data, lifecycle events, observation schedules, payoff-specific transaction processing, issuer and underlying look-through exposure, valuation source hierarchy, scenario analytics and suitability controls. The most important architecture decision is to separate product terms, lifecycle events, accounting transactions, legal positions and risk analytics.”

## 12. Practitioner answer: how to model common products with one model

“I would use a common structured product contract with wrapper type, product family, underlyings, payoff legs, coupon schedule, barrier terms, call terms, settlement terms, protection terms and valuation policy. Optional extensions cover credit, FX, rates, leverage or physical settlement. The position model remains common: units or notional, cost, market value, accrued or confirmed income, lifecycle status and valuation source. Transactions remain economic: buy, sell, coupon, redemption, physical delivery, alternate-currency redemption, write-down and recovery.”

## 13. Study sequence after this pack

Recommended next packs:

1. Private markets and alternatives.
2. FX and money-market products.
3. Lending, Lombard and collateral products.
4. Insurance and investment-linked policies.
5. Commodities and precious metals.
6. Real estate investment products.
7. Portfolio mandates and advisory frameworks as a standalone pack.

# 05 — Advisory, Mandate, Analytics and Reporting Deep Dive

## 1. Why structured products matter in advisory and DPM

Structured products can be attractive to clients because they convert a market view into a defined payoff. They are also challenging because the stated coupon or headline protection may hide meaningful complexity.

For advisory, DPM and portfolio analytics, the key is not only product approval. The key is ongoing portfolio fit:

- Is the product suitable for the client?
- Is it consistent with the mandate?
- Does it create hidden concentration?
- Does it change risk profile under stress?
- Is income sustainable or conditional?
- Can the client exit if liquidity is needed?
- Is the payoff clearly reportable and explainable?

## 2. Advisory conversation framework

A structured product recommendation should be explainable through seven lenses.

| Lens | Question |
|---|---|
| Objective | What is the client trying to achieve: income, protection, participation, hedging, conversion? |
| Market view | What view does the product express? Bullish, range-bound, bearish, volatility, rates, credit, FX? |
| Payoff | What happens in good, neutral and bad scenarios? |
| Risk | What can go wrong? What is maximum loss? Is there issuer/counterparty risk? |
| Liquidity | Can the client exit before maturity? At what cost? |
| Portfolio fit | Does it diversify or concentrate risk? |
| Client understanding | Can the client explain the payoff and downside in simple terms? |

## 3. Suitability and product governance

Structured products often require enhanced suitability controls.

| Control | Description |
|---|---|
| Client risk profile | Product risk must not exceed client risk tolerance unless exception approved |
| Knowledge and experience | Client must understand derivatives/structured payoff where required |
| Complexity assessment | Product classified by complexity, leverage, principal risk and liquidity |
| Concentration limit | Limit exposure by issuer, underlying, product family and payoff type |
| Scenario disclosure | Show adverse scenarios and maximum loss |
| Liquidity check | Match tenor/lock-up to client liquidity needs |
| Currency check | Ensure client understands product/settlement/base currency mismatch |
| Tenor check | Match maturity to investment horizon |
| Income classification | Distinguish fixed, conditional, memory and discretionary income |
| Disclosure acknowledgement | Ensure termsheet and risk disclosures are provided and recorded |

## 4. Mandate treatment

In discretionary or advisory mandates, structured products need clear eligibility rules.

| Mandate dimension | Structured product rule examples |
|---|---|
| Asset-class eligibility | Allowed only for certain mandates or risk bands |
| Product complexity | Autocallables/worst-of/leverage may be restricted |
| Principal risk | Principal-at-risk products limited or prohibited in conservative mandates |
| Issuer concentration | Max exposure per issuer/guarantor |
| Underlying concentration | Look-through exposure counts toward equity/single-name limits |
| Liquidity | Maximum illiquid/issuer-bid-only exposure |
| Tenor | Maximum maturity or weighted average maturity |
| Currency | Product and settlement currency limits |
| Leverage | Leverage products prohibited or tightly limited |
| Physical settlement | Mandate must allow receiving delivered asset |
| Alternatives bucket | Some structured products may consume alternatives/structured allocation |
| Hedging bucket | Protective structures may be treated differently from yield enhancement |

## 5. Mandate classification examples

| Product | Legal bucket | Economic bucket | Mandate treatment |
|---|---|---|---|
| Principal-protected S&P 500 note | Structured note | Equity participation + issuer credit | May count partly to equity and structured allocation |
| Worst-of FCN on tech stocks | Structured note | Equity downside + volatility + issuer risk | Count to structured, equity, single-name concentration |
| DCI USD/SGD | Structured deposit/note | FX conversion risk | Count to FX exposure and structured allocation |
| Credit-linked note | Structured note | Reference credit + issuer credit | Count to credit concentration and structured allocation |
| Leveraged certificate | Certificate | Leveraged equity/index exposure | Usually restricted, count to leverage limit |
| Accumulator on single stock | OTC structured contract | Forward equity accumulation | Count to equity commitment and single-name exposure |

## 6. Portfolio analytics treatment

Structured products require both position-level and look-through analytics.

### 6.1 Legal allocation

Legal allocation answers:

> What does the client hold?

Example:

| Holding type | Allocation |
|---|---:|
| Equities | 35% |
| Bonds | 30% |
| Funds | 20% |
| Structured products | 10% |
| Cash | 5% |

### 6.2 Economic exposure

Economic exposure answers:

> What risks is the client actually taking?

Example:

| Risk exposure | Allocation / exposure |
|---|---:|
| Direct equities | 35% |
| Equity-linked structured downside | 7% |
| Issuer credit exposure | 10% |
| FX conversion exposure | 3% |
| Cash/rates exposure | 5% |

Both views are needed. Legal allocation supports accounting and holdings reporting. Economic exposure supports advisory, risk and mandate monitoring.

## 7. Contribution and attribution

Structured product return can come from multiple sources:

| Return source | Example |
|---|---|
| Coupon/income | Fixed coupon, conditional coupon, memory coupon |
| Price movement | Mark-to-market change |
| Underlying exposure | Equity/index/FX/rate movement |
| Volatility | Change in option value |
| Time decay | Theta effect |
| Credit spread | Issuer/reference spread movement |
| FX translation | Product or underlying currency movement |
| Liquidity spread | Bid/offer or market-making changes |
| Event payoff | Autocall, knock-in, knock-out, maturity payoff |

For performance reporting, a simple contribution by asset class may not fully explain the result. A richer attribution can show:

- income contribution;
- mark-to-market contribution;
- realized redemption result;
- FX contribution;
- issuer/credit spread contribution;
- residual/non-allocable effect.

## 8. Income reporting

Structured product income must be carefully classified.

| Income type | Reporting treatment |
|---|---|
| Fixed unconditional coupon | Income when accrued/paid based on policy |
| Floating coupon | Income after rate fixing/accrual based on policy |
| Conditional coupon | Do not show as earned until condition met/confirmed |
| Memory coupon | Track potential/missed amount separately; income only when paid/confirmed |
| Digital payoff | Usually payoff/income based on terms and accounting classification |
| Redemption gain | Realized gain/loss or payoff return, not ordinary coupon unless classified |
| Accrued estimate | Should be clearly marked if estimated/modelled |

Client reporting should avoid showing “yield” as guaranteed if coupons are conditional.

## 9. Risk reporting

Structured product risk reports should include:

| Report item | Why needed |
|---|---|
| Issuer exposure | Client depends on issuer/counterparty |
| Underlying exposure | Shows hidden equity/FX/rate/credit risk |
| Barrier distance | Highlights near-trigger risk |
| Autocall probability/next date | Shows reinvestment and early redemption risk |
| Worst-of underlying | Identifies weakest risk driver |
| Maximum loss | Core risk disclosure |
| Liquidity status | Exit risk |
| Maturity ladder | Cashflow planning |
| Currency exposure | Base currency impact |
| Product complexity | Governance and suitability |
| Concentration by product type | Avoid overuse of FCNs/autocallables/DCIs |

## 10. Reporting layout for client statements

Minimum client-facing fields:

| Field | Example |
|---|---|
| Product name | 1Y Worst-of Autocallable Note on AAPL/MSFT/NVDA |
| Issuer | Bank X |
| Currency | USD |
| Notional/units | USD 100,000 |
| Cost | USD 100,000 |
| Market value | USD 98,250 |
| Unrealized P&L | -USD 1,750 |
| Maturity | 2027-06-27 |
| Next observation | 2026-09-27 |
| Coupon | 12% p.a., conditional memory |
| Underlyings | AAPL, MSFT, NVDA |
| Worst performer | NVDA at 74% of initial |
| Barrier | 60% knock-in, not breached |
| Liquidity | Issuer bid only |
| Principal risk | Principal at risk |

## 11. Advisor dashboard

An advisor dashboard should be more detailed than client reporting.

Useful dashboard sections:

- product summary;
- suitability flags;
- issuer exposure;
- underlying look-through;
- next lifecycle events;
- coupon status;
- barrier distances;
- scenario analysis;
- client concentration;
- maturity/expected call schedule;
- exit price and bid/ask spread;
- term sheet and disclosure documents;
- exception or stale valuation flags.

## 12. Portfolio review narrative examples

### Yield enhancement narrative

“The portfolio holds several yield-enhancement structured products. These contribute to income but introduce conditional downside exposure to the referenced equities and issuer credit exposure to the issuing banks. Coupons should not be treated as equivalent to bond coupons because payment and principal protection depend on product terms and market conditions.”

### Principal protection narrative

“The principal-protected products provide defined participation in market upside with protection at maturity, subject to issuer risk and the client holding the products until maturity. Their secondary market value may fall below principal before maturity.”

### Concentration narrative

“Although structured products represent 8% of legal holdings, look-through analysis shows higher effective exposure to US technology equities because several products reference the same names. This should be considered alongside direct equity and fund exposures.”

### Liquidity narrative

“Several structured products are not exchange-traded and rely on issuer bid pricing. These positions may be less liquid during stressed markets, and early exit prices may differ materially from theoretical value.”

## 13. Advisory suitability questions

Before recommending:

1. Does the client understand the product is not the same as direct ownership of the underlying?
2. Does the client understand issuer/counterparty risk?
3. Is principal at risk? If yes, how much can be lost?
4. Are coupons guaranteed, conditional or memory-based?
5. What happens if the underlying falls sharply?
6. What happens if the product autocalls early?
7. What happens if the product does not autocall?
8. Can the client hold to maturity?
9. Is there a reliable secondary market?
10. Is the product consistent with the client's risk profile and knowledge?
11. Does it breach issuer, underlying, product or liquidity concentration limits?
12. Is physical settlement possible and allowed in the mandate?
13. Is alternate currency repayment possible and acceptable?
14. Are fees, bid/offer spread and valuation method understood?
15. Does the product fit the client's overall portfolio rather than only looking attractive standalone?

## 14. Mandate checks for platform implementation

| Check | Example rule |
|---|---|
| Product eligibility | Structured products allowed only for Balanced and Growth mandates |
| Complexity | Very high complexity requires senior approval |
| Principal-at-risk | Max 10% of portfolio |
| Issuer concentration | Max 5% per issuer |
| Underlying single-name | Max 10% total direct + look-through exposure |
| Basket sector | Max 25% technology sector look-through |
| FX conversion risk | Max 15% unhedged non-base currency exposure |
| Liquidity | Max 20% issuer-bid-only instruments |
| Tenor | Max 5Y final maturity; max 3Y weighted average |
| Physical delivery | Only if delivered assets are mandate-eligible |
| Leverage | Leverage products prohibited unless explicit mandate |
| CLN/reference credit | Reference entity must meet credit-quality threshold |

## 15. Red flags

| Red flag | Why it matters |
|---|---|
| Client only focuses on coupon | May not understand downside risk |
| Multiple products on same underlying | Hidden concentration |
| Worst-of structures in conservative portfolio | Downside can be severe |
| Principal protection marketed without issuer risk explanation | Misleading risk perception |
| Product cannot be valued independently | Valuation/control concern |
| Stale prices used in reporting | Misleading portfolio value |
| Conditional coupon shown as guaranteed income | Reporting/advisory issue |
| Physical delivery asset not mandate-eligible | Operational/mandate breach |
| FX alternate currency not suitable | Currency mismatch |
| High commissions/embedded margins | Conflict and fair-value concern |

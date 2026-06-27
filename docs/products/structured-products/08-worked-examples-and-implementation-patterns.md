# 08 - Worked Examples and Implementation Patterns

Use these examples to translate structured-product terms into payoff behavior, lifecycle events, valuation, reporting, suitability, mandate controls, and implementation requirements.

## 1. Structured Product Modelling Layers

| Layer | What it represents | Example |
|---|---|---|
| Legal holding | What the client actually owns | structured note, structured deposit, certificate, warrant, structured fund unit |
| Embedded economics | Terms that drive payoff | coupon condition, barrier, participation, cap, buffer, leverage |
| Underlying reference | Asset, index, rate, FX pair, fund, basket, or credit entity | equity basket, index, FX rate, commodity, issuer credit |
| Lifecycle state | Observations, coupons, autocalls, maturity, delivery | barrier breached, coupon missed, autocall pending |
| Analytical exposure | Look-through risk view | issuer exposure, underlying equity exposure, FX exposure, delta |
| Actual delivered position | New holding created only by legal settlement | shares delivered after physical settlement |

Core rule:

```text
Do not create underlying holdings until the product legally delivers them.
```

## 2. Autocallable Worst-Of Note Path

Scenario:

- Notional: 100,000.
- Underlyings: three equities.
- Quarterly autocall if worst-of performance >= 100% of initial level.
- Conditional coupon: 2% per quarter if worst-of performance >= 70%.
- Knock-in barrier: 60% observed at maturity.
- Maturity: 1 year.

Observation path:

| Date | Worst-of level | Coupon result | Autocall result | Lifecycle state |
|---|---:|---|---|---|
| Q1 | 92% | Coupon paid | No autocall | Active |
| Q2 | 75% | Coupon paid | No autocall | Active |
| Q3 | 68% | Coupon missed | No autocall | Active |
| Q4 | 58% | Coupon missed | No autocall | Knock-in breached at maturity |

Potential maturity result:

```text
cash redemption or physical delivery depends on term sheet settlement rule
loss is linked to worst-performing underlying if knock-in breached
```

Implementation controls:

- store initial fixing per underlying;
- store each observation date and result;
- compute worst-of exactly from source levels;
- separate coupon determination from cash payment;
- book no coupon cashflow until payment date;
- create delivered shares only if settlement terms require physical delivery.

## 3. Memory Coupon Example

Scenario:

- Quarterly coupon: 1.50%.
- Memory feature: missed coupons accumulate and pay later if condition is met.
- Notional: 200,000.

Path:

| Quarter | Condition met? | Coupon cashflow |
|---|---|---:|
| Q1 | Yes | 3,000 |
| Q2 | No | 0 |
| Q3 | No | 0 |
| Q4 | Yes | 9,000 |

Calculation:

```text
quarterly coupon = 200,000 x 1.50% = 3,000
Q4 payment = Q2 missed + Q3 missed + Q4 current = 9,000
```

Data model implication:

- missed coupon balance is lifecycle state, not a cash receivable unless policy says accrued income is recognized;
- client reporting should distinguish paid coupon, missed conditional coupon, and memory coupon potential;
- stale observation data must block coupon determination.

## 4. Principal-Protected Participation Product

Scenario:

- Notional: 100,000.
- Principal protection: 100% at maturity, subject to issuer credit.
- Underlying index participation: 80%.
- Cap: 20%.
- Index return at maturity: 25%.

Payoff:

```text
participation return = 25% x 80% = 20%
cap applies at 20%
redemption = 100,000 x (1 + 20%) = 120,000
```

If index return is -15%:

```text
redemption = 100,000, assuming issuer does not default and terms hold to maturity
```

Advisory explanation:

- principal protection is issuer-dependent and usually applies only at maturity;
- early exit can be below principal;
- upside may be capped;
- dividends may be excluded from index return;
- liquidity and bid/offer spread can materially affect realizable value.

## 5. Buffer Product Downside Example

Scenario:

- Notional: 100,000.
- Buffer: first 10% of downside absorbed.
- Participation in downside beyond buffer: 1-for-1.
- Underlying return at maturity: -35%.

Payoff:

```text
loss = max(0, absolute underlying loss - buffer)
loss = max(0, 35% - 10%) = 25%
redemption = 100,000 x (1 - 25%) = 75,000
```

Reporting:

- show payoff scenario table before trade;
- show current barrier/buffer distance after trade if model inputs are available;
- avoid describing buffer as full capital protection;
- show issuer credit and underlying downside as separate risk types.

## 6. Dual Currency Investment

Scenario:

- Base currency investment: USD 100,000.
- Alternate currency: SGD.
- Strike: 1.3500 USD/SGD.
- Coupon: 4% for one month.
- At maturity, spot is 1.3600.

If terms convert into SGD when USD weakens beyond strike, platform must:

- determine settlement currency from term-sheet rule;
- book either USD cash redemption plus coupon or SGD alternate-currency redemption plus coupon;
- avoid creating an artificial FX trade unless actual conversion is instructed;
- show FX outcome, strike, spot, coupon, and settlement currency in the report.

Implementation edge cases:

- strike direction must be unambiguous;
- settlement currency and product currency must be separate fields;
- alternate-currency cash may create mandate or liquidity issues;
- early unwind should use issuer bid, not maturity payoff formula.

## 7. Accumulator And Decumulator

Accumulator behavior:

- client buys an underlying periodically at a pre-agreed strike;
- quantity may double if spot is below strike depending on terms;
- product may knock out when spot breaches an upper barrier.

Decumulator behavior:

- client sells an underlying periodically at a pre-agreed strike;
- quantity may double if spot is above strike depending on terms;
- product may knock out based on barrier terms.

Platform model:

| Requirement | Why it matters |
|---|---|
| Schedule of fixing dates | Determines periodic obligations |
| Quantity formula | Determines delivered or sold units |
| Knock-out rule | Stops future obligations |
| Underlying custody readiness | Physical settlement may create or consume holdings |
| Mandate check | Future obligations can exceed current cash or holdings |
| Reporting label | Client must see remaining obligation, not just current market value |

QA assertion:

An accumulator should not be treated as a simple option or simple equity order. It is a structured obligation with repeated observations and potential physical settlement.

## 8. Structured Certificate Or Warrant

Scenario:

- Client holds a listed leveraged certificate.
- Underlying index moves +2%.
- Leverage factor: 5x.
- Expected certificate move before fees/spreads: about +10%.

Controls:

- distinguish listed certificate price from theoretical leveraged return;
- apply issuer or exchange price as market value source;
- show issuer/provider risk and liquidity risk;
- capture stop-loss, reset, or knock-out features where applicable;
- avoid reporting the underlying index as a direct holding.

## 9. Valuation And Scenario Controls

Structured-product valuation should store:

| Input | Examples |
|---|---|
| Product terms | wrapper, payoff formula, barriers, coupons, maturity, settlement type |
| Underlying state | spot, volatility, dividends, rates, correlation, credit spreads |
| Lifecycle state | observations, barrier status, autocall state, coupon memory |
| Source hierarchy | issuer quote, exchange price, independent vendor, model |
| Quality flags | stale, manual, estimated, indicative, disputed, missing |

Scenario outputs should clearly label:

- issuer quote versus model estimate;
- held-to-maturity payoff versus early-exit valuation;
- stress scenario versus forecast;
- source-complete versus support-limited scenario.

## 10. Advisory, DPM And Mandate Controls

| Control | Required treatment |
|---|---|
| Product approval | Must be in approved product universe for the booking entity and client segment |
| Complexity | Client knowledge and experience must support structured payoff |
| Concentration | Check issuer, product family, underlying, sector, currency, and maturity bucket |
| Liquidity | Secondary market, lock-up, bid/offer, early unwind, and observation schedule |
| Worst-case outcome | Explain payoff under adverse underlying and issuer default scenarios |
| Physical delivery | Confirm delivered asset is allowed, custody-enabled, and mandate-compliant |
| DPM mandate | Check allowed product type, max complexity, issuer limits, underlying exposure, and maturity |
| Reporting readiness | Ensure terms, observations, price source, and risk labels are present |

## 11. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Legal position | product id, issuer, wrapper, notional/units, maturity, market value | full payoff component decomposition |
| Underlying exposure | referenced underlyings and simple look-through tags | scenario-sensitive delta and correlation exposure |
| Lifecycle state | coupon, autocall, maturity, delivery when issuer/custodian events are sourced | automated observation scheduler and payoff engine |
| Valuation | issuer/vendor quote with date and quality flag | independent structured-product model valuation |
| Reporting | position, price, maturity, coupon/redemption cashflows, risk labels | client-facing payoff simulator with suitability controls |
| Physical delivery | delivered asset booking from custodian event | pre-maturity custody and mandate readiness workflow |

## 12. Regression Test Pack

Minimum release-gate scenarios:

1. Autocallable pays coupon but does not autocall.
2. Autocallable autocalls and closes position.
3. Memory coupon misses two observations and later pays accumulated coupon.
4. Knock-in at maturity creates cash loss or physical delivery according to terms.
5. Principal-protected product pays upside participation at maturity.
6. Buffer product calculates downside beyond buffer.
7. Dual currency product settles in alternate currency.
8. Accumulator generates scheduled physical settlement obligation.
9. Listed certificate uses market price while showing underlying exposure separately.
10. Stale issuer quote blocks client-ready valuation or labels degraded state.
11. Missing observation result blocks coupon/autocall determination.
12. Underlying corporate action adjustment preserves audit trail.

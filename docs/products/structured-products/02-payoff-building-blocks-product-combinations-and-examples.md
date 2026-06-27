# 02 — Payoff Building Blocks, Product Combinations and Examples

## 1. Why payoff building blocks matter

Structured products look diverse, but most are built from a repeatable set of building blocks:

- debt or deposit component;
- option component;
- forward component;
- swap component;
- credit derivative component;
- payoff rule;
- observation schedule;
- settlement rule;
- issuer/counterparty economics;
- fees and distribution margins.

For platform design, these blocks should be captured as terms, not as separate accounting positions unless the client legally holds them.

## 2. Core building blocks

| Building block | What it does | Common products |
|---|---|---|
| Zero-coupon bond | Provides maturity principal repayment | Principal-protected notes/deposits |
| Fixed coupon leg | Pays fixed income | Fixed coupon structured products |
| Floating coupon leg | Pays rate-linked income | Floating-rate structured products |
| Long call option | Provides upside participation | Participation products, PPNs |
| Short put option | Funds coupon by selling downside protection | Reverse convertibles, FCNs |
| Short call option | Caps upside or generates premium | Capped participation, covered-call structures |
| Barrier option | Changes payoff when threshold is touched/observed | Knock-in, knock-out, bonus, turbo |
| Digital option | Pays fixed amount if condition is met | Digital coupon, binary payoff |
| Autocall feature | Redeems early if condition met | Autocallables, phoenix notes |
| Memory feature | Accumulates missed coupon entitlement | Memory coupon notes |
| Participation rate | Defines exposure percentage | Participation notes/certificates |
| Cap | Limits upside | Capped notes/certificates |
| Floor | Minimum return or coupon | Protected products, rate notes |
| Buffer | Absorbs first layer of loss | Buffered notes/funds |
| Leverage factor | Magnifies return/loss | Warrants, leveraged certificates |
| Worst-of rule | Uses worst-performing underlying | Worst-of notes/certificates |
| Best-of/rainbow rule | Uses best/weighted performer | Rainbow products |
| FX conversion option | Allows repayment in alternate currency | DCI, FX-linked deposit |
| Credit protection sale | Transfers reference credit risk to investor | CLN, credit-linked products |

## 3. Debt/deposit plus option decomposition

A simple principal-protected product can be decomposed as:

```text
Principal-protected product
= Zero-coupon debt/deposit component
+ Long option on underlying
- Fees / structuring margin
```

A reverse convertible can be decomposed as:

```text
Reverse convertible / FCN
= Debt component
+ Coupon funded by investor selling downside option
+ Barrier and settlement rules
```

A dual-currency investment can be decomposed as:

```text
DCI
= Short-term deposit-like funding component
+ Investor sells FX option
+ Enhanced coupon
+ Alternate currency settlement rule
```

A credit-linked product can be decomposed as:

```text
CLN / credit-linked product
= Funding note/deposit
+ Investor sells credit protection on reference entity
+ Coupon spread compensation
```

## 4. Payoff dimensions

Structured products vary across several dimensions.

| Dimension | Choices |
|---|---|
| Principal treatment | Full protection, partial protection, conditional protection, principal-at-risk |
| Income treatment | Fixed, floating, conditional, memory, digital, range accrual, no coupon |
| Upside treatment | Full participation, partial participation, capped, leveraged, no upside |
| Downside treatment | Full exposure, buffer, barrier, knock-in, conditional loss, inverse exposure |
| Observation | Continuous, daily close, monthly, quarterly, final-only, average price |
| Basket logic | Single, weighted basket, worst-of, best-of, rainbow, dispersion |
| Settlement | Cash, physical delivery, alternate currency, issuer choice, investor choice |
| Early termination | Autocall, issuer call, investor put, stop-loss, knock-out |
| Liquidity | Exchange traded, issuer bid, OTC bilateral, non-transferable |

## 5. Common payoff families

### 5.1 Yield enhancement product

Purpose: earn enhanced income by taking conditional downside risk.

Typical structure:

- investor receives coupon;
- payoff linked to equity/index/FX/rate/credit;
- principal may be reduced or settled into underlying if adverse condition occurs.

Products:

- fixed coupon note;
- reverse convertible;
- dual currency investment;
- range accrual note;
- credit-linked note;
- autocallable yield note.

Key business message:

> Higher yield is usually compensation for selling optionality or accepting credit, FX, equity, volatility or liquidity risk.

### 5.2 Capital-protected product

Purpose: preserve principal at maturity while offering market-linked upside.

Typical structure:

- principal protection at maturity, subject to issuer/counterparty risk;
- upside through participation in underlying;
- may have cap, averaging or participation less than 100%;
- early exit may not preserve principal.

Products:

- principal-protected note;
- capital-protected structured deposit;
- protected certificate;
- index-linked deposit.

Key business message:

> Principal protection is usually at maturity only and depends on the issuer or deposit/product terms.

### 5.3 Participation product

Purpose: provide exposure to a market without directly buying the asset.

Typical structure:

- tracks underlying return with participation rate;
- may include cap, floor, buffer or leverage;
- may be note, certificate, fund or ETN.

Products:

- participation note;
- tracker certificate;
- outperformance certificate;
- structured ETF/fund strategy;
- market-linked certificate.

### 5.4 Barrier/buffer product

Purpose: provide conditional downside protection or enhanced terms until a threshold is breached.

Examples:

- knock-in barrier: downside activates if underlying breaches threshold;
- knock-out barrier: product terminates if threshold reached;
- buffer: product absorbs first layer of loss;
- bonus certificate: bonus payment if barrier not breached.

Key platform issue:

Barriers must be represented in event schedules and lifecycle status, not buried only in free-text product terms.

### 5.5 Autocallable product

Purpose: pay income and redeem early if underlying performs above defined level.

Typical features:

- observation dates;
- autocall barrier;
- coupon barrier;
- memory coupons;
- knock-in barrier;
- final downside settlement.

Path dependency is critical. Two products with identical final underlying levels can have different results depending on observation path.

### 5.6 Leveraged product

Purpose: provide magnified exposure to market movement.

Products:

- structured warrants;
- turbos;
- leveraged certificates;
- daily leveraged certificates;
- leveraged notes.

Key risks:

- losses can be rapid;
- stop-loss or knock-out can make value zero;
- daily leverage causes compounding path dependency;
- not appropriate for all clients or mandates.

### 5.7 Inverse product

Purpose: gain when underlying falls, or hedge long exposure.

Products:

- put warrants;
- inverse certificates;
- bear certificates;
- inverse ETFs/funds with structured strategy.

Risk:

- can lose when market rises;
- daily reset inverse products can perform unexpectedly over longer periods.

### 5.8 Accumulator and decumulator

Purpose:

- accumulator: acquire asset at discount or target level;
- decumulator: dispose asset at target level.

Typical accumulator terms:

| Term | Meaning |
|---|---|
| Strike | Price at which client buys |
| Knock-out | Level where structure terminates, usually favourable to client |
| Leverage ratio | More units bought if price moves against client |
| Observation frequency | Daily/weekly/monthly accumulation |
| Settlement | Periodic shares/cash/FX settlement |

Main risk:

> The product may terminate when favourable and continue with larger exposure when adverse.

## 6. Worked examples

### Example 1: Principal-protected index participation

| Term | Value |
|---|---:|
| Investment | USD 100,000 |
| Tenor | 5 years |
| Principal protection | 100% at maturity |
| Underlying | S&P 500 |
| Participation | 70% upside |
| Cap | None |

Scenario:

| Index return at maturity | Client payoff |
|---:|---:|
| +40% | 100,000 + 70% × 40,000 = 128,000 |
| +10% | 107,000 |
| 0% | 100,000 |
| -30% | 100,000, assuming issuer performs |

Platform treatment:

- position: structured note/deposit/certificate, not direct index;
- valuation: debt component plus long call option;
- reporting: show protection at maturity, issuer risk, upside participation and early-exit risk.

### Example 2: Reverse convertible / fixed coupon product

| Term | Value |
|---|---:|
| Investment | USD 100,000 |
| Underlying | Apple |
| Strike | USD 200 |
| Knock-in barrier | 70%, USD 140 |
| Coupon | 12% p.a. |
| Tenor | 6 months |
| Settlement | Cash or physical shares |

Possible outcomes:

| Apple final/barrier result | Outcome |
|---|---|
| Barrier not breached or final above strike | Principal + coupon |
| Barrier breached and final below strike | Shares delivered or cash loss based on formula |

If physical settlement occurs:

```text
Shares delivered = Notional / Strike
```

For USD 100,000 notional and USD 200 strike:

```text
Shares delivered = 500 Apple shares
```

### Example 3: Dual-currency investment

| Term | Value |
|---|---:|
| Investment currency | USD |
| Alternate currency | SGD |
| Notional | USD 100,000 |
| Tenor | 1 month |
| Conversion rate | 1.35 |
| Coupon | Enhanced |

Outcomes:

| FX fixing | Outcome |
|---|---|
| No conversion | Client receives USD principal + coupon |
| Conversion triggered | Client receives SGD at pre-agreed conversion rate |

Platform treatment:

- close original currency structured product;
- create cash in delivered currency;
- report FX risk, conversion outcome and base-currency P&L.

### Example 4: Autocallable worst-of product

| Term | Value |
|---|---:|
| Basket | Apple, Microsoft, Nvidia |
| Autocall level | 100% |
| Coupon barrier | 70% |
| Knock-in barrier | 60% |
| Observation | Quarterly |
| Tenor | 3 years |

Lifecycle:

1. On each observation date, check each underlying.
2. If all underlyings are above autocall level, product redeems early.
3. If coupon condition is met, coupon is paid.
4. If condition is not met, coupon may be missed or stored as memory depending on terms.
5. At maturity, if knock-in/downside condition applies, final repayment is based on worst-performing underlying.

Platform issue:

Worst-of basket requires every observation level to be stored by underlying, not only the final result.

### Example 5: Accumulator

| Term | Value |
|---|---:|
| Underlying | DBS share |
| Strike | SGD 40 |
| Spot at inception | SGD 42 |
| Knock-out | SGD 46 |
| Accumulation frequency | Weekly |
| Normal quantity | 1,000 shares/week |
| Leverage quantity | 2,000 shares/week if spot below strike |

Lifecycle:

| Market condition | Result |
|---|---|
| Spot above knock-out | Product terminates |
| Spot between strike and knock-out | Client buys normal quantity at strike |
| Spot below strike | Client buys leveraged quantity at strike |

Risk:

The client may accumulate more when the stock falls and stop accumulating when the stock rises.

## 7. Product combinations

Structured products are often described using multiple labels. For example:

| Label combination | Meaning |
|---|---|
| Principal-protected equity-linked note | Note wrapper + principal protection + equity option payoff |
| Worst-of autocallable FCN | Note wrapper + basket + autocall + coupon + downside barrier |
| Capital-protected structured deposit | Deposit wrapper + principal protection + market-linked return |
| Leveraged index certificate | Certificate wrapper + index exposure + leverage |
| FX-linked DCI | Deposit/note-like wrapper + FX option + alternate currency redemption |
| Credit-linked deposit | Deposit wrapper + credit event risk |
| Buffered participation note | Note wrapper + market upside + first-loss buffer |
| Daily leveraged certificate | Certificate wrapper + daily reset leverage |

For product taxonomy, separate the dimensions:

```text
Wrapper + Underlying + Payoff Objective + Risk Feature + Settlement Feature
```

Example:

```text
Unlisted note + worst-of equity basket + enhanced coupon + knock-in barrier + physical settlement
```

## 8. Payoff modelling checklist

Every structured product should answer these questions:

1. What is the legal wrapper?
2. What is the investment currency?
3. What is the underlying or basket?
4. What is the initial fixing or strike?
5. Is principal protected, conditional or at risk?
6. Is there a coupon? Is it fixed, floating or conditional?
7. Are coupons memory or non-memory?
8. Are there barriers? What type and observation style?
9. Is there autocall, issuer call or investor put?
10. Is settlement cash, physical or alternate currency?
11. Are there caps, floors, participation rates or buffers?
12. Is the payoff path-dependent?
13. What is the worst-case client outcome?
14. Who is the issuer/counterparty?
15. What is the valuation source?
16. Can the client exit before maturity?
17. How should look-through exposure be reported?

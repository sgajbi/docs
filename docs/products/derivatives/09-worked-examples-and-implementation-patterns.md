# 09 - Worked Examples and Implementation Patterns

Use these examples when designing derivative product support, reviewing advisory workflows, testing analytics, or explaining derivative behavior to product, operations, QA, reporting, and engineering teams.

## 1. Exposure Lens By Product Type

Market value is rarely enough for derivatives. Store and report multiple exposure lenses.

| Product | Legal position | Cashflow behavior | Primary exposure lens | Risk lens | Reporting warning |
|---|---|---|---|---|---|
| Listed equity option | Contract quantity | Premium, exercise/assignment, expiry | Delta-equivalent equity exposure | Delta, gamma, vega, theta | Small premium can create large directional exposure. |
| Index future | Contract quantity | Initial margin, daily variation margin, close/expiry | Notional exposure | Delta, margin, stress loss | Market value may be near zero while exposure is large. |
| FX forward | Forward contract | Two currency settlement legs or early close P&L | Bought/sold currency notional | FX sensitivity, settlement risk | No upfront cash does not mean no exposure. |
| NDF | Non-deliverable forward | Net cash settlement in settlement currency | Reference currency notional | Fixing and basis risk | Do not create deliverable cash legs. |
| Interest-rate swap | OTC contract | Fixed/floating net payments, collateral | DV01 and notional | Curve, reset, counterparty | Notional is not loss amount. |
| CDS | Credit derivative contract | Premium leg, protection leg after credit event | Reference entity notional | CS01, jump-to-default | Credit event lifecycle must drive settlement. |

## 2. Listed Call Option Purchase

Scenario:

- Client buys 10 call option contracts.
- Underlying share price: 100.
- Strike: 105.
- Premium: 4 per share.
- Contract size: 100 shares.
- Total premium: 10 x 100 x 4 = 4,000.

Transactions:

| Event | Posting |
|---|---|
| Trade | Option contract position +10 |
| Settlement | Cash debit 4,000 plus fees/taxes |
| Daily valuation | Option market value and unrealized P&L |
| Sale to close | Option position -10, cash credit sale proceeds |
| Expiry worthless | Position closed, realized premium loss |
| Exercise | Option closed, underlying shares created, strike cash paid |

Analytics:

| Metric | Treatment |
|---|---|
| Market value | Contracts x contract size x option price |
| Cost | Premium plus transaction costs, according to accounting policy |
| P&L | Change in option value plus realized close/exercise outcome |
| Delta-equivalent exposure | Contracts x contract size x option delta x underlying price |
| Contribution | Use option return and weight; explain leverage effect |
| Risk | Include delta, gamma, vega, theta, concentration, and expiry risk |

Implementation requirements:

- contract size and multiplier must be source-owned;
- underlying identifier must reconcile to market data;
- expiry calendar must drive expired-state handling;
- exercise/assignment must create actual underlying positions only when legally delivered;
- mandate checks should use exposure, not premium paid.

## 3. Covered Call Assignment

Scenario:

- Portfolio holds 1,000 shares.
- Client sells 10 covered call contracts.
- Contract size: 100.
- Strike: 55.
- At expiry, underlying closes above strike and assignment occurs.

Correct lifecycle:

```text
short call assignment -> close option -> sell 1,000 shares at strike -> book sale cash -> realize equity P&L
```

Do not:

- leave the short option open after assignment;
- book only option P&L and ignore the equity sale;
- sell more shares than are covered by the portfolio;
- treat the assignment as a voluntary market sale without assignment evidence.

QA assertions:

| Assertion | Expected result |
|---|---|
| Option status | Closed by assignment |
| Equity quantity | Reduced by 1,000 shares |
| Sale price | Strike price, not market close |
| Premium | Retained and included according to performance policy |
| Mandate | Covered-call strategy remains within approved option policy |

## 4. Futures Variation Margin Versus Investment P&L

Scenario:

- Long 5 index futures.
- Multiplier: 50.
- Index level rises from 4,000 to 4,020.
- Daily variation margin gain: 5 x 50 x (4,020 - 4,000) = 5,000.

Cash behavior:

| Posting | Meaning |
|---|---|
| Initial margin | Collateral or restricted cash, not investment cost |
| Variation margin gain/loss | Daily realized futures P&L cash movement |
| Close trade | Contract closed; final variation margin settles economics |

Reporting treatment:

- show futures notional exposure separately from market value;
- classify variation margin as futures P&L, not external cashflow;
- show collateral/margin separately from portfolio performance unless local policy says otherwise;
- preserve broker statement reconciliation for every daily variation margin posting.

Implementation boundary:

Generic cash and transaction models can store margin postings. Full futures support requires contract multipliers, expiry, broker margin statements, variation margin classification, roll handling, and notional exposure reporting.

## 5. FX Forward Hedge

Scenario:

- Portfolio holds EUR assets.
- Client sells EUR 1,000,000 forward against USD.
- Forward rate: 1.0900.
- Maturity: 3 months.

At maturity:

| Leg | Amount |
|---|---:|
| Deliver EUR | -1,000,000 |
| Receive USD | +1,090,000 |

Key controls:

- store bought currency and sold currency explicitly;
- do not infer direction from currency-pair display alone;
- value using forward points, spot, curve inputs, and valuation date;
- report hedge exposure against EUR asset exposure;
- classify realized forward P&L separately from underlying asset return unless hedge accounting or reporting policy links them.

Common edge cases:

- early close realizes P&L before maturity;
- roll creates a closing forward plus a new forward;
- split settlements require multiple cash legs;
- restricted currency markets may require NDF treatment instead of deliverable settlement.

## 6. NDF Fixing And Net Settlement

Scenario:

- Client buys USD 1,000,000 versus INR through an NDF.
- Contract rate: 83.00.
- Fixing rate: 84.00.
- Settlement currency: USD.

Simplified settlement:

```text
cash settlement = notional x (fixing rate - contract rate) / fixing rate
cash settlement = 1,000,000 x (84.00 - 83.00) / 84.00 = 11,904.76 USD
```

Platform treatment:

- book one net settlement cashflow;
- do not create INR cash;
- store fixing source, fixing date, settlement date, and formula;
- expose supportability if fixing source is missing or disputed.

QA scenarios:

| Scenario | Expected behavior |
|---|---|
| Missing fixing | lifecycle event pending; settlement not booked |
| Wrong fixing source | exception raised; no silent settlement |
| Reversed direction | sign check fails against trade direction |
| Settlement currency mismatch | cash posting blocked until corrected |

## 7. Interest-Rate Swap Reset And Payment

Scenario:

- Pay fixed 3.00%, receive floating.
- Notional: 10,000,000.
- Floating reset for period: 3.40%.
- Day-count fraction: 0.25.

Simplified net payment:

```text
fixed leg = 10,000,000 x 3.00% x 0.25 = 75,000
floating leg = 10,000,000 x 3.40% x 0.25 = 85,000
net receive = 10,000
```

Required data:

- notional schedule;
- fixed rate and day count;
- floating index, reset date, fixing source, spread, and day count;
- payment calendar;
- collateral agreement reference if applicable;
- valuation curve and discounting policy.

Reporting:

- show market value separately from accrued/reset cashflows;
- show DV01 and curve exposure for risk;
- disclose if valuation is dealer-only, model-only, stale, or disputed;
- avoid reporting notional as market value or as maximum loss.

## 8. CDS Credit Event

Scenario:

- Client buys protection on reference entity.
- Notional: 5,000,000.
- Credit event occurs.
- Auction recovery price: 40%.

Simplified protection payment:

```text
loss amount = notional x (1 - recovery price)
loss amount = 5,000,000 x (1 - 40%) = 3,000,000
```

Lifecycle:

```text
credit event notice -> event validation -> premium stop/accrual treatment -> settlement determination -> protection payment -> contract close
```

Implementation controls:

- credit event source must be authoritative;
- reference obligation and settlement protocol must be captured;
- premium accrual cut-off must be policy-driven;
- cash settlement must reconcile to dealer/custodian notice;
- client reporting must explain reference entity, notional, recovery basis, and settlement status.

## 9. Multi-Leg Option Strategy: Bull Call Spread

Scenario:

- Client buys 10 call contracts with strike 100 for premium 8.
- Client sells 10 call contracts with strike 110 for premium 3.
- Contract size: 100 shares.
- Net premium paid: 10 x 100 x (8 - 3) = 5,000.

Payoff at expiry:

| Underlying at expiry | Long call payoff | Short call payoff | Net payoff before premium | Net P&L after premium |
|---:|---:|---:|---:|---:|
| 95 | 0 | 0 | 0 | -5,000 |
| 105 | 5,000 | 0 | 5,000 | 0 |
| 115 | 15,000 | -5,000 | 10,000 | 5,000 |

Implementation treatment:

- store each option leg separately with its own contract id, direction, strike, expiry, premium, Greek set, lifecycle state, and exercise/assignment outcome;
- store strategy grouping as an analytical relationship, not as a replacement for legal contract positions;
- calculate maximum loss, maximum gain and breakeven at strategy level;
- apply mandate checks to both individual legs and combined strategy exposure.

QA assertions:

| Assertion | Expected result |
|---|---|
| One leg is missing | Strategy-level payoff is unavailable or labelled incomplete. |
| Expiry closes both legs | Long and short contracts close with correct intrinsic value or assignment behavior. |
| Client report shows exposure | Report includes leg-level holdings and strategy-level payoff explanation. |
| Mandate allows long calls but not short calls | Strategy fails unless covered spread permission exists. |

## 10. Hedge Effectiveness For An FX Forward Overlay

Scenario:

- Portfolio holds EUR assets worth EUR 1,000,000.
- Client sells EUR 800,000 forward against USD.
- EUR asset value falls by USD 40,000 from FX movement.
- FX forward gains USD 32,000 over the same period.

Simplified effectiveness:

```text
hedge ratio = hedged notional / exposure notional
hedge ratio = 800,000 / 1,000,000 = 80%

offset ratio = hedge gain / hedged exposure loss
offset ratio = 32,000 / 40,000 = 80%
```

Reporting treatment:

| Area | Correct treatment |
|---|---|
| Exposure | Show gross EUR exposure, hedged notional and residual unhedged exposure. |
| Performance | Separate asset local return, FX translation effect and hedge P&L. |
| Mandate | Validate hedge ratio, permitted instruments, tenor and counterparty. |
| Advisory | Explain that hedge reduces currency exposure but can also reduce upside. |
| QA | Match hedge result to forward valuation source and underlying exposure source. |

## 11. OTC Novation And Compression

Scenario:

An OTC swap with Counterparty A is novated to Counterparty B. Later, two offsetting swaps are compressed into one smaller residual contract.

Correct lifecycle:

```text
original contract -> novation consent -> close old counterparty exposure -> open replacement contract -> preserve valuation and collateral lineage

offsetting contracts -> compression proposal -> source confirmation -> close replaced contracts -> create residual contract or zero position
```

Implementation treatment:

| Event | Required data |
|---|---|
| Novation | old counterparty, new counterparty, consent date, effective date, transfer value, collateral treatment. |
| Compression | original contracts, compression group id, replacement notional, termination cashflow, effective date. |
| Reporting | contract lineage, counterparty exposure before/after, realized termination amount and residual exposure. |
| Risk | counterparty, DV01, notional and collateral exposure recalculate from new contract set. |

QA assertions:

| Assertion | Expected result |
|---|---|
| Novation lacks consent evidence | Contract owner cannot be changed automatically. |
| Collateral balance transfers | Collateral movement is linked to novation event. |
| Compression closes contracts | Old contracts are closed with lineage to replacement or termination. |
| Risk report runs after event | Counterparty and notional exposure do not double count old and new contracts. |

## 12. Central Clearing And Variation Margin

Scenario:

An OTC interest-rate swap is centrally cleared. Initial margin is posted to the clearing broker, and variation margin settles daily.

| Attribute | Value |
|---|---:|
| Cleared swap notional | 20,000,000 |
| Initial margin posted | 450,000 |
| Daily mark-to-market change | 18,000 |
| Variation margin received | 18,000 |

Correct treatment:

| Item | Treatment |
|---|---|
| Cleared contract | Legal contract remains a derivative position with clearing broker/CCP references. |
| Initial margin | Restricted collateral, not investment cost. |
| Variation margin | Daily cash settlement of derivative P&L according to clearing rules. |
| Counterparty exposure | Counterparty risk shifts from bilateral dealer exposure to clearing structure. |
| Reporting | Show notional, market value, margin, collateral, cleared status and source dates separately. |

QA assertions:

| Assertion | Expected result |
|---|---|
| Clearing broker margin file is stale | Margin/collateral report is stale or blocked. |
| Variation margin posts | Cash changes and derivative P&L classification reconciles to clearing statement. |
| Initial margin changes | Collateral restriction updates without treating it as realized loss. |
| Contract moves from bilateral to cleared | Counterparty and CSA/clearing metadata update with lineage. |

## 13. Collateral Optimization Under A CSA

Scenario:

A bilateral OTC portfolio requires collateral. Eligible collateral includes USD cash and government bonds with haircuts.

| Collateral type | Available market value | Haircut | Lending value |
|---|---:|---:|---:|
| USD cash | 300,000 | 0% | 300,000 |
| Government bonds | 500,000 | 5% | 475,000 |

Collateral requirement: 620,000.

Simplified calculation:

```text
total eligible lending value = 300,000 + 475,000 = 775,000
surplus after requirement = 775,000 - 620,000 = 155,000
```

Implementation treatment:

- eligible collateral rules come from the CSA or margin agreement;
- haircut, currency, concentration, substitution and settlement timing must be source-backed;
- optimization should preserve liquidity buffers and mandate restrictions;
- reporting should show pledged collateral, free collateral and substitution candidates separately.

QA assertions:

| Assertion | Expected result |
|---|---|
| Bond price is stale | Bond collateral lending value is stale or excluded. |
| Currency haircut applies | Lending value uses currency-adjusted haircut. |
| Collateral substitution is requested | Old pledge is released only after replacement collateral is accepted. |
| Client liquidity need conflicts | Optimization respects cash buffer and mandate priority. |

## 14. Portfolio Greeks Aggregation

Scenario:

A portfolio holds option positions on the same underlying. Risk reporting needs aggregate delta, gamma and vega.

| Position | Contracts | Multiplier | Delta | Gamma | Vega |
|---|---:|---:|---:|---:|---:|
| Long call | 20 | 100 | 0.55 | 0.030 | 0.12 |
| Short put | -10 | 100 | -0.35 | 0.025 | 0.10 |

Simplified aggregation:

```text
aggregate delta units = sum(contracts x multiplier x delta)
long call delta = 20 x 100 x 0.55 = 1,100
short put delta = -10 x 100 x -0.35 = 350
aggregate delta units = 1,450

aggregate gamma units = 20 x 100 x 0.030 + (-10) x 100 x 0.025 = 35
aggregate vega units = 20 x 100 x 0.12 + (-10) x 100 x 0.10 = 140
```

Correct treatment:

Aggregate Greeks require consistent underlying, valuation date, model source, multiplier and sign convention. Reports should show stale or partial coverage when any position lacks current sensitivities.

QA assertions:

| Assertion | Expected result |
|---|---|
| One leg has stale Greeks | Aggregate Greek is stale, partial or blocked by policy. |
| Underlyings differ | Aggregation is grouped by underlying and risk factor. |
| Multiplier changes after corporate action | Greek aggregation uses adjusted contract multiplier. |
| Client report shows exposure | Delta-equivalent exposure is labelled separately from market value. |

## 15. Advisory And Mandate Checklist

Before using derivatives in advisory or DPM workflows, check:

| Dimension | Required question |
|---|---|
| Purpose | Hedge, income, exposure, transition management, liquidity, or speculation? |
| Product knowledge | Does the client or mandate allow derivative complexity? |
| Loss profile | Is maximum loss, margin exposure, or gap risk explainable? |
| Exposure | Which notional, delta, DV01, CS01, or stress exposure applies? |
| Liquidity | Can the position be closed, exercised, assigned, rolled, or terminated? |
| Collateral | What margin or collateral calls can occur? |
| Authority | Who may approve trade, exercise, close-out, collateral movement, or unwind? |
| Reporting | Which client/advisor labels and warnings are required? |

## 16. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Contract position | contract id, notional, quantity, underlying, expiry, counterparty, market value, leg grouping where sourced | advanced strategy decomposition and scenario replay |
| Exposure | notional, delta-equivalent, DV01/CS01 where supplied | central cross-product exposure engine and strategy-level attribution |
| Margin/collateral | broker/dealer margin postings and collateral balances | full margin simulation, optimization and substitution workflow |
| Valuation | vendor/dealer/exchange values with source timestamps | independent model valuation with explain and calibration |
| Lifecycle events | expiry, exercise, assignment, reset, fixing, settlement, novation, compression and clearing when sourced | automated OTC confirmation matching and event generation |
| Reporting | market value, exposure, source date, stale/unsupported labels, hedge overlay split where sourced | advanced hedge-effectiveness reporting and multi-factor attribution |

## 17. Regression Test Pack

Minimum release-gate scenarios:

1. Buy option, sell to close, verify realized P&L.
2. Buy option, expire worthless, verify premium loss.
3. Short covered call assignment creates underlying sale.
4. Futures variation margin posts daily and reconciles.
5. Futures notional exposure appears even when market value is near zero.
6. Deliverable FX forward settles two cash legs.
7. NDF settles one net cashflow with correct sign.
8. Swap reset and net payment use correct fixing and day count.
9. Collateral call posts outside investment P&L.
10. CDS credit event produces controlled settlement workflow.
11. Stale valuation blocks client-ready reporting or labels degraded state.
12. Missing Greek/exposure input prevents unsupported mandate conclusion.
13. Bull call spread reports leg-level holdings and strategy payoff without losing legal contract positions.
14. FX hedge effectiveness separates underlying FX loss, hedge P&L and residual exposure.
15. OTC novation/compression preserves contract lineage and prevents double-counted counterparty exposure.
16. Cleared swap margin distinguishes initial margin from daily variation margin.
17. Collateral optimization applies CSA eligibility, haircut, currency and liquidity constraints.
18. Portfolio Greeks aggregate only compatible, current sensitivities and label partial coverage.

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

## 15. Variance Swap Realized Variance Settlement

Scenario:

- Client enters a long variance swap on an equity index.
- Variance notional: USD 50,000 per variance point.
- Strike volatility: 20%.
- Realized volatility over the observation period: 24%.

Simplified settlement:

```text
strike variance = 20%^2 = 400 variance points
realized variance = 24%^2 = 576 variance points
variance difference = 576 - 400 = 176 variance points
settlement = 176 x 50,000 = 8,800,000
```

Implementation treatment:

- store volatility strike, variance strike, observation calendar and realized-variance methodology explicitly;
- use sourced fixings and observation dates, not a single end-date price move;
- cap or floor settlement only when the confirmation includes that term;
- report variance exposure separately from simple delta exposure;
- preserve valuation model, realized-variance file and counterparty confirmation lineage.

QA assertions:

| Assertion | Expected result |
|---|---|
| Observation file is missing | Settlement remains pending or support-limited. |
| Volatility is used instead of variance | Calculation QA fails because variance swaps settle on squared volatility terms. |
| Cap applies | Settlement respects source-confirmed cap. |
| Client report shows index exposure | Report labels volatility/variance exposure separately from directional equity exposure. |

## 16. Barrier Option Knock-Out

Scenario:

- Client holds a long up-and-out call option.
- Strike: 100.
- Knock-out barrier: 120.
- Underlying trades at 121 during the observation window.

Lifecycle:

```text
barrier observation -> barrier breached -> option terminates or rebate applies -> position closes with source event lineage
```

Correct treatment:

| Area | Treatment |
|---|---|
| Observation | Use source-owned intraday or close-based observation rule from the confirmation. |
| Position | Close the option only when breach is confirmed under product terms. |
| Rebate | Book rebate cash only if terms include a rebate. |
| Reporting | Show barrier event, date, source and resulting position state. |
| Risk | Remove future option Greeks after confirmed knock-out. |

QA assertions:

| Assertion | Expected result |
|---|---|
| Barrier source is missing | Knock-out is not inferred from incomplete price data. |
| Barrier is close-only | Intraday high does not trigger unless terms support intraday observation. |
| Rebate exists | Rebate cashflow is separate from option market value. |
| Barrier not breached | Option remains live until expiry or another lifecycle event. |

## 17. Cross-Currency Swap Notional Exchange

Scenario:

- Client enters a USD/EUR cross-currency swap.
- Initial exchange: pay USD 1,100,000 and receive EUR 1,000,000.
- Final exchange at maturity reverses notionals.
- Periodic coupons are exchanged during the life of the swap.

Cashflow pattern:

```text
trade date initial exchange -> periodic USD/EUR coupon exchanges -> maturity final notional exchange
```

Implementation treatment:

- store both currency notionals, exchange rates, reset rules and final exchange terms;
- distinguish notional exchanges from coupon payments and MTM valuation;
- report FX exposure, rate exposure and counterparty exposure separately;
- avoid creating permanent cash balances from projected future notional exchanges;
- link collateral and CSA currency eligibility to the swap.

QA assertions:

| Assertion | Expected result |
|---|---|
| Initial exchange posts | Cash legs settle in both currencies and contract remains open. |
| Final exchange is projected | Future cashflows are projected, not booked as settled cash. |
| FX rate source is stale | Valuation and exposure are degraded or blocked. |
| One leg is missing | Swap cashflow schedule is incomplete and client-ready reporting is blocked. |

## 18. Option Exercise Window And Missed Exercise

Scenario:

- Client holds an American-style equity call option.
- Option is in the money before expiry.
- Exercise instruction deadline is 15:00 on expiry date.
- Advisor submits instruction after the deadline.

Workflow:

```text
monitor exercise value -> validate mandate and cash/shares -> capture instruction before deadline -> submit to broker/exchange -> confirm exercise or expiry outcome
```

Correct treatment:

| State | Treatment |
|---|---|
| Before deadline | Exercise instruction can be accepted if controls pass. |
| After deadline | Instruction is rejected or escalated according to broker policy. |
| Exercise confirmed | Option closes and underlying trade/cash settlement is created. |
| Exercise missed | Option expires or is handled by automatic-exercise rules if applicable. |

QA assertions:

| Assertion | Expected result |
|---|---|
| Deadline is missing | Exercise workflow is blocked or urgent exception is raised. |
| Cash required for exercise is unavailable | Exercise is blocked unless financing is approved. |
| Automatic exercise applies | Outcome follows exchange/broker rule with source evidence. |
| Advisor submits late instruction | No underlying position is created without accepted exercise confirmation. |

## 19. Collateral Dispute Resolution

Scenario:

- Counterparty margin call: USD 750,000.
- Internal valuation supportable collateral requirement: USD 710,000.
- Dispute threshold: USD 25,000.

Calculation:

```text
dispute amount = 750,000 - 710,000 = 40,000
threshold breach = 40,000 > 25,000
```

Correct workflow:

```text
receive margin call -> compare valuation -> identify disputed amount -> post agreed amount if policy allows -> open dispute workflow -> resolve or escalate
```

Implementation treatment:

- store call amount, agreed amount, disputed amount, threshold, due date and resolution state;
- avoid treating disputed collateral as freely available;
- preserve valuation source and counterparty statement;
- report liquidity impact, collateral restriction and unresolved dispute status separately.

QA assertions:

| Assertion | Expected result |
|---|---|
| Dispute exceeds threshold | Dispute workflow opens with required approval. |
| Agreed amount is posted | Cash/collateral movement is linked to margin call. |
| Disputed amount later resolves | Additional collateral or return posts with lineage. |
| Counterparty file is stale | Margin-call status is stale or exceptioned. |

## 20. Clearing-Broker Default Portability

Scenario:

- Client has cleared derivatives through a clearing broker.
- Clearing broker defaults.
- Positions may be ported to another clearing broker or closed out under clearing rules.

Required controls:

| Area | Treatment |
|---|---|
| Position inventory | Freeze affected cleared contracts for event review. |
| Margin | Reconcile initial margin, variation margin and excess collateral. |
| Portability | Track target broker, consent, deadline and acceptance. |
| Close-out | If porting fails, process close-out valuation and cash settlement. |
| Reporting | Label event state, counterparty structure and collateral uncertainty. |

QA assertions:

| Assertion | Expected result |
|---|---|
| Broker default notice arrives | Affected positions and collateral are identified by clearing broker. |
| Porting is accepted | Contract metadata updates with lineage and no duplicate position. |
| Porting fails | Close-out workflow applies and collateral recovery remains tracked. |
| Margin file is unavailable | Collateral reporting is degraded and escalation remains open. |

## 21. Strategy-Level Attribution For Option Spread

Scenario:

- Client holds a bull call spread.
- Long call gains USD 12,000.
- Short call loses USD 5,000.
- Net premium amortization and fees reduce performance by USD 800.

Attribution:

```text
gross leg contribution = 12,000 - 5,000 = 7,000
strategy net contribution = 7,000 - 800 = 6,200
```

Reporting treatment:

| View | Treatment |
|---|---|
| Legal position | Show each option leg separately. |
| Strategy view | Show grouped payoff, maximum loss/gain and net contribution. |
| Risk | Aggregate delta/gamma/vega only when sensitivities are compatible and current. |
| Attribution | Separate long-leg gain, short-leg loss, premium/fees, FX and residual effects. |

QA assertions:

| Assertion | Expected result |
|---|---|
| One leg is missing from group | Strategy attribution is incomplete and labelled partial. |
| Leg valuation dates differ | Attribution is stale or blocked by policy. |
| Strategy group changes | Attribution preserves effective-dated group membership. |
| Client report hides short leg | Reporting QA fails because legal exposure is incomplete. |

## 22. SIMM-Style Margin Input Completeness

Scenario:

- An uncleared OTC derivative portfolio requires margin analytics.
- The platform receives sensitivity files for rates and FX, but the equity vega file is missing.
- The CSA requires exposure by netting set, product class, risk bucket and currency.

Input completeness check:

| Required input | Example source | Why it matters |
|---|---|---|
| Netting set id | CSA / margin agreement | Determines permitted netting and threshold application. |
| Product class | Trade confirmation / risk system | Separates rates, credit, equity, FX and commodity margin inputs. |
| Risk-factor sensitivities | Risk engine | Drives delta, vega and curvature-style margin components. |
| Currency and bucket | Market-data / risk taxonomy | Supports currency conversion, concentration and bucket treatment. |
| Threshold and independent amount | CSA / credit support annex | Determines call amount after agreed contractual offsets. |
| Valuation date and model version | Risk engine / margin engine | Supports auditability and stale-input controls. |

Correct treatment:

- do not calculate or display a final margin estimate when mandatory sensitivity inputs are incomplete;
- show which risk class is partial, stale or missing;
- keep broker/counterparty margin calls separate from internal estimated margin;
- preserve source files so operations can reconcile disputes and explain differences.

QA assertions:

| Assertion | Expected result |
|---|---|
| Equity vega file is missing | Margin estimate is labelled partial or blocked according to policy. |
| Netting set is absent | Trades cannot be netted for margin analytics. |
| Threshold is stale | Call amount is not client-ready and requires margin agreement review. |
| Counterparty call differs | Difference is routed to dispute workflow, not silently overwritten. |

## 23. Volatility-Surface Shock For Option Portfolio

Scenario:

- Portfolio holds listed and OTC equity options on the same index.
- Current vega exposure is USD 140 per volatility point.
- Risk asks for a +3 volatility-point shock.

Approximate shock impact:

```text
estimated_value_change = aggregate_vega x volatility_shock
estimated_value_change = 140 x 3 = 420
```

Implementation treatment:

- group shock results by underlying, expiry bucket, option type and model source;
- distinguish parallel volatility shocks from surface-specific skew, smile or term-structure shocks;
- use current vega only when valuation date, multiplier, underlying and model source are compatible;
- label results as approximate sensitivity, not a full revaluation, unless the pricing model reruns.

QA assertions:

| Assertion | Expected result |
|---|---|
| One OTC option lacks current vega | Shock result is partial or blocked. |
| Surface source is stale | Scenario output carries stale-input warning. |
| Long and short options offset | Net vega reflects sign convention and leg direction. |
| User requests full revaluation | Platform requires model rerun evidence, not only vega approximation. |

## 24. Callable Swap Exercise And Breakage Review

Scenario:

- Client owns a callable receive-fixed interest-rate swap.
- Next call date is 2026-09-30.
- Current swap MTM is USD 320,000 in the client's favor.
- Estimated break funding and administrative cost is USD 25,000.

Simplified economic review:

```text
net_exercise_value = current_mtm - break_funding_cost - fees
net_exercise_value = 320,000 - 25,000 = 295,000
```

Correct workflow:

```text
identify call date -> validate exercise window -> obtain dealer valuation -> calculate breakage -> approve instruction -> confirm exercise or leave contract live
```

Implementation treatment:

- store call schedule, notice deadline, exercise authority and dealer confirmation;
- separate valuation MTM from breakage, fees and actual termination cash settlement;
- block late or unauthorized exercise instructions;
- preserve "not exercised" decisions with rationale when the call window passes.

QA assertions:

| Assertion | Expected result |
|---|---|
| Notice deadline is missing | Exercise workflow is blocked or escalated. |
| Dealer valuation is stale | Exercise economics are not client-ready. |
| Exercise is confirmed | Swap closes and termination cashflow posts with lineage. |
| Call window expires | Contract remains live and audit trail records missed/declined action. |

## 25. Equity Swap Dividend Adjustment

Scenario:

- Client receives total return on an equity basket through an equity swap.
- Expected dividend in the model: USD 80,000.
- Actual dividend confirmed by corporate-action source: USD 92,000.
- Financing leg is booked separately.

Dividend adjustment:

```text
dividend_adjustment = actual_dividend - expected_dividend
dividend_adjustment = 92,000 - 80,000 = 12,000
```

Correct treatment:

- link dividend adjustment to the reference basket and observation period;
- keep dividend return, price return and financing leg distinct;
- do not book physical share income unless the client legally holds the shares;
- reconcile the adjustment to dealer statement and corporate-action source.

QA assertions:

| Assertion | Expected result |
|---|---|
| Actual dividend source is missing | Adjustment remains estimated or pending. |
| Financing leg is netted in source file | Report still separates total return and financing components where possible. |
| Basket membership changed mid-period | Dividend attribution uses effective-dated basket composition. |
| Client report labels dividend as share income | Reporting QA fails because exposure is synthetic. |

## 26. Futures Delivery Notice And Physical-Delivery Control

Scenario:

- Portfolio is long 20 deliverable bond futures.
- First notice day is approaching.
- The strategy is intended for exposure management, not physical delivery.

Control workflow:

```text
monitor first notice day -> identify open deliverable positions -> validate roll/close instruction -> confirm no unintended delivery obligation -> process delivery only with source notice
```

Implementation treatment:

- store last trade date, first notice day, delivery window and contract deliverability terms;
- raise escalation before delivery-risk dates when positions remain open;
- distinguish closing/rolling futures from accepting or making delivery;
- do not create bond positions or warehouse/custody movements without delivery notice evidence.

QA assertions:

| Assertion | Expected result |
|---|---|
| Long position remains open near first notice day | Delivery-risk alert opens. |
| Roll trade executes | Old future closes and new contract opens with lineage. |
| Delivery notice is received | Physical settlement workflow starts with eligible deliverable source. |
| No delivery notice exists | Platform does not infer a physical bond position. |

## 27. Prime-Broker Give-Up And Clearing Acceptance

Scenario:

- Advisor executes an option trade with an executing broker.
- The trade is given up to a prime broker / clearing broker.
- Clearing broker rejects the give-up because account mapping is wrong.

Correct workflow:

```text
execution fill -> allocation -> give-up submission -> clearing acceptance or rejection -> repair -> final booking and reconciliation
```

Implementation treatment:

- store executing broker, clearing broker, account, give-up id, acceptance state and rejection reason;
- keep execution economics visible even when clearing acceptance is pending;
- block client-ready settled position reporting until clearing acceptance and custody reconciliation pass;
- preserve repair history instead of overwriting rejected allocations.

QA assertions:

| Assertion | Expected result |
|---|---|
| Give-up is accepted | Trade books to clearing broker account with execution lineage. |
| Give-up is rejected | Position remains pending/exceptioned and does not settle silently. |
| Account mapping changes | Repair event records old and corrected account mapping. |
| Duplicate acceptance arrives | Idempotency prevents duplicate contract position. |

## 28. Early Termination Amount And Break Funding

Scenario:

- Client terminates an OTC derivative before maturity.
- Dealer close-out MTM: USD 180,000 payable to client.
- Accrued net coupon receivable: USD 15,000.
- Break funding charge: USD 22,000.

Simplified termination amount:

```text
termination_cash = close_out_mtm + accrued_receivable - break_funding_charge
termination_cash = 180,000 + 15,000 - 22,000 = 173,000
```

Correct treatment:

- distinguish close-out valuation, accrued cashflows, break funding, fees and taxes;
- require source confirmation before closing the legal contract;
- preserve whether termination is full or partial;
- update exposure, collateral and performance only after effective termination state is confirmed.

QA assertions:

| Assertion | Expected result |
|---|---|
| Dealer close-out statement is missing | Termination cashflow is not final. |
| Partial termination applies | Notional reduces and residual contract remains active. |
| Accrued coupon is already paid | Cashflow is not double counted in termination amount. |
| Collateral remains pledged | Collateral release workflow stays open after contract close. |

## 29. Close-Out Netting Set And Independent Amount Dispute

Scenario:

- Three OTC derivative trades sit under the same master agreement and CSA.
- Trade MTMs: +250,000, -90,000 and +40,000.
- Posted collateral: USD 120,000.
- Independent amount required by counterparty: USD 75,000, disputed internally.

Simplified net exposure:

```text
net_mtm = 250,000 - 90,000 + 40,000 = 200,000
collateral_adjusted_exposure = net_mtm - posted_collateral
collateral_adjusted_exposure = 200,000 - 120,000 = 80,000
```

Correct treatment:

- net only trades that share the same enforceable netting set and legal agreement;
- track independent amount separately from variation margin and posted collateral;
- preserve disputed independent amount state with reason, owner, due date and resolution;
- report gross MTM, net MTM, collateral, independent amount and residual exposure separately.

QA assertions:

| Assertion | Expected result |
|---|---|
| One trade lacks netting-set id | It is excluded from netted exposure or blocks the calculation. |
| Independent amount is disputed | Dispute state is shown separately from agreed collateral. |
| Posted collateral exceeds net MTM | Residual exposure can become overcollateralized but is not treated as free cash until released. |
| Master agreement changes | Netting set and historical exposure are effective-dated. |

## 30. CVA/DVA/FVA Input Ownership And Valuation Adjustment

Scenario:

- OTC swap clean value is USD 2,000,000 receivable by the client.
- Credit valuation adjustment (CVA): USD 45,000.
- Debit valuation adjustment (DVA): USD 12,000.
- Funding valuation adjustment (FVA): USD 18,000.

Simplified adjusted value:

```text
adjusted_value = clean_value - CVA + DVA - FVA
adjusted_value = 2,000,000 - 45,000 + 12,000 - 18,000 = 1,949,000
```

Correct treatment:

- keep clean value, CVA, DVA, FVA and final adjusted value as separate measures;
- identify the owner and timestamp for exposure profiles, counterparty curves, own-credit inputs, funding curves and collateral assumptions;
- label whether adjustments are dealer-provided, vendor-provided or independently modelled;
- avoid mixing front-office explain values with official accounting or client-reporting values without source policy.

QA assertions:

| Assertion | Expected result |
|---|---|
| CVA input file is stale | Adjusted valuation is blocked or labelled stale. |
| DVA is unavailable | Clean value remains available but adjusted value is incomplete. |
| Funding curve changes | FVA component is recalculated with model/version lineage. |
| Client report uses adjusted value | Report shows the valuation basis and source timestamp. |

## 31. Compression Tear-Up Allocation

Scenario:

- Portfolio compression tears up three economically offsetting OTC trades.
- Net portfolio tear-up P&L is USD 24,000 gain.
- Allocation weights by agreed risk reduction: 50%, 30%, 20%.

Simplified allocation:

```text
trade_a_allocation = 24,000 x 50% = 12,000
trade_b_allocation = 24,000 x 30% = 7,200
trade_c_allocation = 24,000 x 20% = 4,800
```

Correct treatment:

- preserve old contract ids, new compressed state and tear-up event id;
- allocate residual P&L by documented rule, not by arbitrary trade order;
- avoid double counting notional reduction as realized performance;
- keep counterparty, netting-set and legal agreement lineage after compression.

QA assertions:

| Assertion | Expected result |
|---|---|
| Allocation weights do not sum to 100% | Compression allocation is rejected. |
| One trade is outside netting set | It is excluded from the tear-up batch. |
| Compression event is replayed | Idempotency prevents duplicate P&L and duplicate notional reduction. |
| Residual P&L is immaterial | Event still records allocation policy and lineage. |

## 32. Cross-Currency Collateral Optionality

Scenario:

- USD collateral requirement: USD 1,000,000.
- Client proposes EUR 1,000,000 collateral.
- EUR/USD rate: 1.10.
- Base collateral haircut: 3%.
- FX mismatch haircut: 5%.

Simplified lending value:

```text
lending_value_usd = collateral_amount_eur x fx_rate x (1 - base_haircut - fx_mismatch_haircut)
lending_value_usd = 1,000,000 x 1.10 x (1 - 0.03 - 0.05) = 1,012,000
```

Correct treatment:

- verify the collateral currency is eligible under the CSA;
- apply both asset haircut and currency mismatch haircut where required;
- report surplus/shortfall in the exposure currency;
- preserve collateral substitution, FX rate source, haircut source and valuation timestamp.

QA assertions:

| Assertion | Expected result |
|---|---|
| EUR is not CSA-eligible | Collateral proposal is rejected even if value is sufficient. |
| FX rate is stale | Lending value is stale or blocked. |
| Haircut rules change | Historical collateral value remains reproducible by effective date. |
| Requirement currency changes | Surplus/shortfall is recalculated in the correct currency. |

## 33. Lifecycle Event Replay After Corrected Reset

Scenario:

- Interest-rate swap reset was processed with a 5.12% fixing.
- Correct fixing is 5.18%.
- Floating notional is USD 50,000,000.
- Accrual factor is 90/360 = 0.25.

Simplified correction:

```text
cashflow_delta = notional x (correct_rate - original_rate) x accrual_factor
cashflow_delta = 50,000,000 x (0.0518 - 0.0512) x 0.25 = 7,500
```

Correct treatment:

- replay lifecycle events from source facts instead of overwriting the final cashflow silently;
- preserve original fixing, corrected fixing, correction reason and approval state;
- post adjustment cashflow separately when the original cashflow has already settled;
- recalculate valuation, accrual, performance and reporting impact from effective event history.

QA assertions:

| Assertion | Expected result |
|---|---|
| Corrected fixing arrives before settlement | Pending cashflow is updated with audit trail. |
| Corrected fixing arrives after settlement | Adjustment cashflow is created. |
| Reset source is unsupported | Replay is blocked until source evidence is available. |
| Replay runs twice | No duplicate adjustment cashflow is created. |

## 34. Barrier Observation Dispute

Scenario:

- Up-and-out option barrier level is 110.00.
- Dealer observation file shows high price of 110.05.
- Independent market-data source shows high price of 109.95.
- Observation rule requires official exchange fixing, not intraday dealer high.

Decision treatment:

- do not knock out the option until the governing observation source and rule confirm breach;
- preserve dealer value, independent value, observation rule, dispute owner and resolution state;
- separate disputed lifecycle state from valuation estimate;
- block client-ready lifecycle reporting when barrier state is unresolved.

QA assertions:

| Assertion | Expected result |
|---|---|
| Dealer and official source disagree | Barrier state becomes disputed, not final. |
| Observation rule is missing | Barrier processing is blocked. |
| Dispute resolves as no breach | Option remains active with dispute history. |
| Dispute resolves as breach | Knock-out event records breach source, timestamp and approval lineage. |

## 35. Uncleared Margin Phase-In And SIMM Completeness

Scenario:

- Counterparty becomes subject to uncleared margin rules after threshold monitoring.
- SIMM-style initial margin requires sensitivities, product class, bucket, currency, risk weights, netting set and threshold.
- Delta sensitivity file is present; vega and curvature files are missing.

Correct treatment:

- determine applicability by legal entity, jurisdiction, average aggregate notional amount and effective date;
- distinguish regulatory initial margin from house margin and independent amount;
- block unsupported initial-margin estimates when mandatory risk classes are missing;
- preserve phase-in date, threshold, model version, sensitivity files and netting-set evidence.

QA assertions:

| Assertion | Expected result |
|---|---|
| Phase-in date has not arrived | Regulatory initial margin is not applied prematurely. |
| Sensitivity files are incomplete | Estimate is blocked or labelled partial. |
| Netting-set id is missing | Margin cannot be netted across trades. |
| Threshold changes | Margin requirement is effective-dated and reproducible. |

## 36. Exchange Position Limit Control

Scenario:

- Mandate holds 950 contracts in an exchange-traded futures contract.
- Exchange limit is 1,000 contracts.
- Proposed order adds 75 contracts.

Simplified control:

```text
projected_position = current_contracts + proposed_order_contracts
projected_position = 950 + 75 = 1,025
excess = 1,025 - 1,000 = 25
```

Correct treatment:

- check position limits before order release and after fills;
- aggregate across accounts, sub-accounts or beneficial owner where the rule requires it;
- distinguish hard exchange limits from internal concentration limits;
- preserve alert, override, reduction or exemption evidence.

QA assertions:

| Assertion | Expected result |
|---|---|
| Projected position exceeds exchange limit | Order is blocked or routed for approved exception. |
| Partial fill breaches limit | Post-trade alert and remediation workflow open. |
| Accounts share beneficial owner | Positions aggregate for limit check. |
| Limit file is stale | Order release is blocked or labelled unsupported by policy. |

## 37. Cleared OTC Porting After Clearing Broker Default

Scenario:

- Clearing broker defaults while client has cleared swaps.
- Initial margin: USD 800,000.
- Variation margin receivable: USD 120,000.
- Excess collateral: USD 50,000.
- Backup clearing broker accepts 70% of contracts for porting.

Operational treatment:

- identify affected contracts, margin, collateral and pending settlements;
- split contracts into ported, pending and close-out buckets;
- update clearing broker metadata only after receiving acceptance evidence;
- preserve margin recovery, collateral transfer and close-out lineage.

QA assertions:

| Assertion | Expected result |
|---|---|
| Backup broker accepts a contract | Contract is ported with old and new clearing-broker lineage. |
| Backup broker rejects a contract | Contract remains in close-out or liquidation workflow. |
| Margin transfer is partial | Residual claim remains open and separately reported. |
| Acceptance file is replayed | Porting state is idempotent. |

## 38. Valuation Model-Change Approval

Scenario:

- A structured option valuation model changes from version 3.1 to 3.2.
- Clean MTM changes from USD 420,000 to USD 438,000.
- Model change impact is USD 18,000.

Correct treatment:

- separate market movement from model-change impact;
- require approval evidence for model version, calibration inputs and effective date;
- keep old and new valuation explain available for audit and client/reporting questions;
- avoid backfilling historical values unless policy explicitly requires restatement.

QA assertions:

| Assertion | Expected result |
|---|---|
| Model version changes without approval | Valuation is blocked or marked pending approval. |
| Calibration inputs are missing | Model-change impact cannot be finalized. |
| Historical report is regenerated | It uses the valuation version effective at the report date unless restated. |
| New model materially changes MTM | Impact is surfaced for review and performance explain. |

## 39. Wrong-Way Risk In Counterparty Exposure

Scenario:

- Client has an OTC equity put option with Bank A.
- Option MTM receivable from Bank A is USD 1,800,000.
- Underlying equity is issued by Bank A's parent group.
- Stress scenario assumes the issuer equity falls 35% and Bank A credit spread widens materially.

Correct treatment:

- identify wrong-way risk when counterparty exposure increases as the counterparty or related issuer weakens;
- preserve counterparty, issuer relationship, group linkage, stress scenario and exposure profile;
- separate option market exposure from counterparty credit exposure;
- escalate collateral, counterparty limit and advisory review when wrong-way risk breaches policy.

QA assertions:

| Assertion | Expected result |
|---|---|
| Counterparty and underlying issuer are related | Wrong-way risk flag is raised. |
| Relationship mapping is missing | Counterparty risk report is source-limited. |
| MTM receivable increases in stress | Counterparty exposure and collateral need update. |
| Client report is generated | Market payoff and counterparty concentration are explainable separately. |

## 40. Collateral Interest And Settlement Fail

Scenario:

- Counterparty posts USD 2,000,000 cash collateral.
- Collateral remuneration rate is 4.80%.
- Interest period is 30 days.
- Interest payment fails on due date because settlement instructions are invalid.

Simplified interest:

```text
collateral_interest = 2,000,000 x 4.80% x 30 / 360 = 8,000
```

Correct treatment:

- accrue collateral interest separately from derivative MTM and investment income;
- track due, paid, failed and repaired settlement states;
- preserve collateral agreement terms, rate source, day count, settlement instruction and repair evidence;
- do not mark collateral interest as received cash until settlement confirmation arrives.

QA assertions:

| Assertion | Expected result |
|---|---|
| Interest due date arrives | Receivable/payable is created according to CSA terms. |
| Settlement fails | Cash does not post; failed settlement workflow opens. |
| SSI is repaired | Payment can settle with original due-date lineage. |
| Report is generated before repair | Interest is shown as receivable or failed, not settled cash. |

## 41. Fallback-Rate Transition For Swap Reset

Scenario:

- A legacy swap references a discontinued rate.
- Contract fallback replaces it with the compounded risk-free rate plus spread adjustment.
- Legacy fixing would have been 5.10%.
- Fallback compounded rate is 4.85%.
- Spread adjustment is 0.26%.
- Notional is USD 25,000,000.
- Accrual factor is 90/360 = 0.25.

Fallback cashflow:

```text
fallback_rate = 4.85% + 0.26% = 5.11%
cashflow = 25,000,000 x 5.11% x 0.25 = 319,375
```

Correct treatment:

- preserve fallback trigger, contract amendment or protocol adherence evidence;
- distinguish legacy benchmark, fallback benchmark, spread adjustment and compounding method;
- recalculate cashflows and valuation from the effective fallback date;
- label unsupported fallback data rather than using stale discontinued fixings.

QA assertions:

| Assertion | Expected result |
|---|---|
| Legacy rate is discontinued | Reset uses source-backed fallback method. |
| Spread adjustment is missing | Cashflow is blocked or labelled incomplete. |
| Protocol adherence differs by counterparty | Fallback treatment is contract-specific. |
| Historical report is rerun | Uses the benchmark effective on the report date. |

## 42. Option Lifecycle Amendment

Scenario:

- A client and dealer amend an OTC option before expiry.
- Original strike is 100.
- Amended strike is 95.
- Amendment fee is USD 12,000 payable by the client.
- Contract id remains the same.

Correct treatment:

- preserve the original contract and amendment history instead of creating an unrelated new option;
- update strike, effective date, fee, valuation impact and approval evidence;
- treat amendment fee as lifecycle cashflow, not premium for a new opening trade unless legal terms say so;
- recalculate Greeks, payoff diagram, suitability and reporting labels from amended terms.

QA assertions:

| Assertion | Expected result |
|---|---|
| Amendment is confirmed | Contract terms version from effective date. |
| Amendment fee settles | Fee posts as lifecycle cashflow with amendment reference. |
| Advisor report is generated | Payoff uses amended strike and shows amendment history where required. |
| Amendment source is missing | Contract remains pending amendment and original terms stay effective. |

## 43. Cross-Clearinghouse Portability

Scenario:

- A cleared futures portfolio is migrated from Clearinghouse A to Clearinghouse B after market infrastructure change.
- Open contracts: 300.
- Initial margin at A: USD 450,000.
- Required initial margin at B: USD 520,000.
- Porting acceptance covers all contracts.

Margin top-up:

```text
margin_top_up = 520,000 - 450,000 = 70,000
```

Correct treatment:

- preserve old clearinghouse, new clearinghouse, porting date, acceptance file and contract mapping;
- update margin methodology and account references only after acceptance evidence;
- separate porting from trade close/open so performance and realized P&L are not distorted;
- calculate margin top-up or release from the target clearinghouse requirement.

QA assertions:

| Assertion | Expected result |
|---|---|
| Porting acceptance is complete | Contracts retain economic continuity with new clearinghouse lineage. |
| Target margin exceeds source margin | Top-up requirement opens before port completion. |
| Contract mapping is incomplete | Unmapped contracts remain in exception state. |
| Report spans port date | No false realized P&L or position close/open appears. |

## 44. Valuation Model Backtesting Exception

Scenario:

- A derivatives valuation model is backtested against independent exit quotes.
- Model value is USD 1,250,000.
- Independent quote is USD 1,180,000.
- Tolerance is USD 50,000.

Backtest break:

```text
valuation_difference = 1,250,000 - 1,180,000 = 70,000
tolerance_excess = 70,000 - 50,000 = 20,000
```

Correct treatment:

- keep model value, independent quote, source timestamp, tolerance and breach owner;
- open model review when backtest variance breaches threshold;
- avoid changing client valuation until approved valuation policy or model correction is applied;
- preserve whether breach is caused by model, input, liquidity, quote quality or product terms.

QA assertions:

| Assertion | Expected result |
|---|---|
| Difference exceeds tolerance | Backtesting exception opens. |
| Independent quote is stale | Backtest result is source-limited. |
| Model correction is approved | Valuation changes from effective approval date. |
| Report is produced during review | Model value is labelled with valuation-review state where required. |

## 45. Hedge-Accounting Evidence Pack

Scenario:

- A swap is designated as a hedge of a fixed-rate bond position.
- Hedged item DV01 is USD 42,000.
- Swap DV01 is USD -39,500.
- Policy requires hedge ratio between 80% and 125%.

Hedge ratio:

```text
hedge_ratio = abs(-39,500) / 42,000 = 94.05%
```

Correct treatment:

- preserve hedged item, hedge instrument, designation date, risk component, hedge objective and effectiveness method;
- report accounting hedge evidence separately from economic risk overlay reporting;
- keep prospective and retrospective effectiveness evidence with source data and approval;
- de-designate or escalate when hedge ratio or documentation falls outside policy.

QA assertions:

| Assertion | Expected result |
|---|---|
| Hedge ratio is within band | Hedge remains eligible if documentation is complete. |
| Designation document is missing | Hedge-accounting status is incomplete even if economics offset. |
| Hedged item is sold | De-designation or redesignation workflow opens. |
| Effectiveness test fails | Accounting treatment and reporting state update with evidence. |

## 46. Client-Reporting Explain Pack For Structured Derivative

Scenario:

- A client holds an autocallable option package.
- Current MTM is -USD 85,000.
- Delta exposure is USD 620,000.
- Worst downside scenario is linked to a 35% fall in the worst-performing equity.
- Next observation date is in 21 days.

Correct treatment:

- explain MTM, exposure, payoff condition, observation date and downside risk separately;
- avoid using notional alone as the client-facing exposure measure;
- preserve payoff diagram/version, underlying prices, barrier levels, observation calendar and valuation source;
- flag stale valuation, missing Greeks or unsupported scenario output before client-ready reporting.

QA assertions:

| Assertion | Expected result |
|---|---|
| MTM is negative but payoff may recover | Report explains conditional payoff and observation mechanics. |
| Delta is missing | Exposure explain is partial or blocked. |
| Observation calendar is stale | Next-event reporting is source-limited. |
| Scenario output is generated | Worst-case narrative links to source-backed payoff terms. |

## 47. Autocall Observation Correction

Scenario:

- A structured derivative has an autocall observation date.
- The first source file marks the underlying basket above the autocall level.
- A corrected exchange close later shows one underlying below the required threshold.
- Autocall amount would have been USD 1,000,000 principal plus USD 32,000 coupon.

Observation state:

```text
initial_autocall_state = triggered
corrected_autocall_state = not_triggered
cashflow_reversal = 1,032,000 if initial payment was posted
```

Correct treatment:

- preserve original observation, corrected observation, source timestamps and correction reason;
- reverse or block autocall cashflows only when corrected governing source evidence is accepted;
- keep the contract alive when corrected terms show no autocall occurred;
- update client reports, lifecycle state, next observation date and coupon accrual treatment.

QA assertions:

| Assertion | Expected result |
|---|---|
| Corrected observation invalidates autocall | Contract remains open and autocall cashflow is reversed or blocked. |
| Payment already settled | Correction workflow creates controlled reversal, not manual deletion. |
| Corrected source is not governing | Observation remains disputed or source-limited. |
| Client report spans correction date | Report shows corrected lifecycle state with audit lineage. |

## 48. Commodity Futures Delivery Alternative

Scenario:

- A client holds a deliverable commodity futures contract close to first notice date.
- The mandate permits cash exposure but not physical delivery.
- Futures notional is 100 contracts x 1,000 barrels.
- Last trade price is USD 78.20.
- Broker offers roll, cash close-out or exchange-for-physical alternative.

Exposure:

```text
deliverable_quantity = 100 x 1,000 = 100,000 barrels
notional_value = 100,000 x 78.20 = 7,820,000
```

Correct treatment:

- identify physical-delivery risk before first notice date;
- block unintended delivery when mandate, custody or operational capability does not support it;
- preserve broker notice, available alternatives, roll/close-out instruction and execution state;
- separate futures P&L from operational delivery-avoidance workflow.

QA assertions:

| Assertion | Expected result |
|---|---|
| Contract approaches delivery window | Delivery-risk workflow opens before notice deadline. |
| Mandate disallows physical delivery | Delivery instruction is blocked; roll or close-out is required. |
| Broker alternative is selected | Lifecycle records roll, close-out or EFP with source evidence. |
| Deadline is missed | Exception escalates with delivery, custody and client-impact state. |

## 49. FX Option Premium Settlement

Scenario:

- A client buys a EUR/USD option.
- Premium is USD 45,000, payable T+2.
- Trade date valuation includes the option position, but cash is not settled until premium payment date.

Premium state:

```text
trade_date_option_position = open
premium_payable = 45,000
available_cash_impact_before_settlement = reserve_or_payable, not settled_cash_debit
settled_cash_debit_on_pay_date = 45,000
```

Correct treatment:

- create option position from trade confirmation;
- reserve or recognize premium payable according to accounting policy before settlement;
- debit cash only on settlement date when payment is confirmed;
- report premium separately from option MTM and FX exposure.

QA assertions:

| Assertion | Expected result |
|---|---|
| Option trade is confirmed | Option position opens with premium payable state. |
| Premium has not settled | Cash is reserved/payable, not silently removed from settled cash. |
| Premium settlement fails | Option remains open but cash/payment exception is visible. |
| Client report is generated | Premium, MTM and exposure are labelled separately. |

## 50. Swap Portfolio Novation Wave

Scenario:

- A portfolio of swaps is novated from Dealer A to Dealer B.
- 18 swaps are in scope.
- 16 swaps are accepted by Dealer B.
- 2 swaps are rejected due to missing consent.
- Clean MTM of accepted swaps is USD 2,400,000.

Novation coverage:

```text
accepted_ratio = 16 / 18 = 88.89%
rejected_count = 2
accepted_clean_mtm = 2,400,000
```

Correct treatment:

- preserve old counterparty, new counterparty, novation consent, effective date and contract mapping;
- update counterparty exposure only for accepted swaps;
- keep rejected swaps with original counterparty until consent or close-out occurs;
- avoid booking false terminations or new trades when legal continuity is preserved.

QA assertions:

| Assertion | Expected result |
|---|---|
| Some swaps are rejected | Accepted and rejected populations are separated. |
| Accepted swaps have mapping | Counterparty changes with novation lineage and no false realized P&L. |
| Consent is missing | Swap remains with original dealer and exception state. |
| Exposure report runs after wave | Dealer A and Dealer B exposures reconcile to accepted/rejected state. |

## 51. Collateral Substitution Dispute

Scenario:

- A counterparty requests to substitute pledged collateral under a CSA.
- Existing collateral is USD cash of 1,000,000.
- Proposed collateral is a government bond with market value 1,050,000.
- CSA haircut for the bond is 8%.
- Required collateral value is 1,000,000.

Eligibility value:

```text
eligible_bond_value = 1,050,000 x (1 - 8%) = 966,000
substitution_shortfall = 1,000,000 - 966,000 = 34,000
```

Correct treatment:

- evaluate substitution against CSA eligibility, haircut, concentration and currency rules;
- reject, resize or dispute substitution when eligible value is below requirement;
- preserve request, valuation source, haircut policy, agreed/disputed state and settlement instructions;
- avoid releasing existing collateral until replacement collateral is accepted and settled.

QA assertions:

| Assertion | Expected result |
|---|---|
| Proposed collateral is insufficient after haircut | Substitution is disputed or requires top-up. |
| Collateral source price is stale | Substitution remains pending/source-limited. |
| Replacement settles successfully | Existing collateral releases only after accepted replacement. |
| Counterparty disputes haircut | Dispute state tracks agreed and disputed collateral values. |

## 52. XVA Sensitivity Explain Pack

Scenario:

- An OTC derivatives portfolio has clean value and valuation adjustments.
- Clean MTM is USD 3,200,000.
- CVA is -USD 140,000.
- DVA is USD 45,000.
- FVA is -USD 30,000.
- A credit-spread stress increases CVA loss by USD 25,000.

Adjusted value:

```text
adjusted_value = 3,200,000 - 140,000 + 45,000 - 30,000 = 3,075,000
stressed_adjusted_value = 3,075,000 - 25,000 = 3,050,000
```

Correct treatment:

- separate clean MTM, CVA, DVA, FVA and stress sensitivities;
- preserve model version, counterparty credit inputs, funding inputs and netting-set scope;
- explain XVA movement separately from underlying market movement;
- label sensitivities approximate when driven by scenario or first-order estimates.

QA assertions:

| Assertion | Expected result |
|---|---|
| XVA components are sourced | Adjusted value reconciles to clean value plus valuation adjustments. |
| Credit-spread stress is applied | CVA sensitivity is shown separately from clean MTM. |
| Netting-set scope changes | XVA is recalculated or labelled not comparable. |
| Client report is produced | XVA explain pack separates valuation components and methodology version. |

## 53. Structured-Product Greek Restatement

Scenario:

- A structured-product desk restates Greeks for a note with embedded derivative exposure.
- Previous delta was 320,000 delta-equivalent notional.
- Restated delta is 410,000 delta-equivalent notional.
- Vega changes from 18,500 to 22,000 per volatility point.
- The market value is unchanged because the official valuation file is not restated.

Restatement impact:

```text
delta_change = 410,000 - 320,000 = 90,000
vega_change = 22,000 - 18,500 = 3,500
```

Correct treatment:

- preserve original and restated Greek files, methodology version, effective date and correction reason;
- update exposure, mandate, hedge and risk analytics from the restated Greeks;
- do not change accounting valuation unless the official price or valuation file is also restated;
- label historical risk reports as restated when prior exposure was materially wrong.

QA assertions:

| Assertion | Expected result |
|---|---|
| Greek file is restated | Risk analytics version the old and new sensitivities. |
| Valuation file is unchanged | Market value remains unchanged while exposure changes. |
| Mandate exposure limit is breached by restatement | Breach workflow opens with restatement lineage. |
| Client report spans restatement date | Report labels exposure correction without inventing trading activity. |

## 54. Intraday Futures Margin Spike

Scenario:

- A futures position has a sharp intraday price move.
- Opening initial margin is USD 650,000.
- Intraday margin add-on is USD 280,000.
- Available cash is USD 810,000.
- The clearing broker requires same-day funding by 14:00.

Liquidity impact:

```text
total_margin_need = 650,000 + 280,000 = 930,000
cash_shortfall = 930,000 - 810,000 = 120,000
```

Correct treatment:

- separate initial margin, intraday add-on, variation margin and settled cash movement;
- trigger liquidity escalation when same-day margin exceeds available cash;
- preserve clearing-broker call, timestamp, deadline, covered contracts and funding resolution;
- do not classify margin funding as realized investment loss unless variation margin settlement confirms P&L.

QA assertions:

| Assertion | Expected result |
|---|---|
| Intraday margin add-on arrives | Same-day funding need recalculates from broker call. |
| Cash is insufficient | Margin shortfall workflow opens before deadline. |
| Variation margin also posts | Funding and P&L movements remain separately labelled. |
| Broker call is later revised | Margin workflow versions original and revised calls. |

## 55. Uncleared Margin Threshold Reset

Scenario:

- A bilateral CSA has a threshold that resets after a ratings downgrade.
- Previous threshold is USD 1,000,000.
- Revised threshold is USD 250,000.
- Current exposure is USD 1,350,000.
- Posted collateral is USD 400,000.

Collateral requirement:

```text
required_collateral_before = max(0, 1,350,000 - 1,000,000) = 350,000
required_collateral_after = max(0, 1,350,000 - 250,000) = 1,100,000
additional_collateral_needed = 1,100,000 - 400,000 = 700,000
```

Correct treatment:

- preserve rating trigger, CSA threshold schedule, effective date, exposure source and posted collateral;
- recalculate collateral calls from the revised threshold only from the source-backed effective date;
- separate threshold-driven collateral increase from market MTM movement;
- keep agreed, disputed and pending collateral amounts visible.

QA assertions:

| Assertion | Expected result |
|---|---|
| Rating trigger is effective | Threshold updates and collateral requirement recalculates. |
| Trigger evidence is missing | Margin call remains source-limited or uses prior threshold by policy. |
| Exposure changes same day | Threshold and exposure impacts are separately explainable. |
| Counterparty disputes threshold | Dispute state tracks agreed and disputed call amounts. |

## 56. Swap Rate-Fixing Dispute

Scenario:

- A floating-rate swap coupon uses a published fixing.
- Internal source captured 4.72%.
- Dealer confirmation uses 4.68%.
- Notional is USD 20,000,000.
- Accrual factor is 0.25.

Disputed cashflow delta:

```text
cashflow_internal = 20,000,000 x 4.72% x 0.25 = 236,000
cashflow_dealer = 20,000,000 x 4.68% x 0.25 = 234,000
cashflow_dispute = 2,000
```

Correct treatment:

- preserve internal fixing source, dealer fixing, publication timestamp, convention and dispute owner;
- keep cashflow provisional or disputed until governing source is resolved;
- do not silently overwrite the fixing when source hierarchy is explicit;
- post adjustment cashflow if dispute resolves after settlement.

QA assertions:

| Assertion | Expected result |
|---|---|
| Dealer fixing differs from internal fixing | Dispute workflow opens with calculated delta. |
| Governing source confirms internal fixing | Dealer amount is challenged or adjusted. |
| Payment already settled | Adjustment cashflow posts instead of overwriting history. |
| Report is generated during dispute | Cashflow is labelled disputed or provisional. |

## 57. Portfolio Compression Rejection

Scenario:

- A portfolio compression cycle proposes to tear up 24 offsetting swap trades.
- 20 trades are accepted by all counterparties.
- 4 trades are rejected because legal consent is missing.
- Proposed notional reduction is USD 180,000,000.
- Accepted notional reduction is USD 145,000,000.

Compression result:

```text
acceptance_ratio = 20 / 24 = 83.33%
rejected_trades = 4
rejected_notional = 180,000,000 - 145,000,000 = 35,000,000
```

Correct treatment:

- tear up only accepted trades with source-backed consent and effective date;
- keep rejected trades live with original lifecycle, valuation and counterparty exposure;
- preserve compression run id, candidate trades, acceptance status, residual notional and P&L allocation;
- avoid applying proposed notional reduction before final accepted population is confirmed.

QA assertions:

| Assertion | Expected result |
|---|---|
| Some trades are rejected | Accepted and rejected populations remain separate. |
| Compression proposal is not final | No notional is reduced from proposal-only file. |
| Accepted trades tear up | Live portfolio and exposure reduce only by accepted notional. |
| P&L allocation is incomplete | Compression cannot finalize until allocation evidence is complete. |

## 58. Derivative Tax-Lot Close-Out Reporting

Scenario:

- A client closes part of an option position held in multiple tax lots.
- Lot A has 40 contracts with premium cost USD 120,000.
- Lot B has 60 contracts with premium cost USD 210,000.
- The client closes 50 contracts for proceeds of USD 190,000.
- Tax policy uses FIFO.

FIFO close-out:

```text
closed_lot_a_contracts = 40
closed_lot_b_contracts = 10
lot_b_cost_per_contract = 210,000 / 60 = 3,500
closed_cost = 120,000 + 10 x 3,500 = 155,000
realized_gain = 190,000 - 155,000 = 35,000
remaining_lot_b_contracts = 50
```

Correct treatment:

- preserve derivative lots, acquisition dates, premium costs, close-out quantity, proceeds and tax method;
- calculate realized gain/loss by configured lot-selection policy;
- keep remaining lots open with residual quantity and cost basis;
- separate tax-lot reporting from risk exposure reduction and trade lifecycle state.

QA assertions:

| Assertion | Expected result |
|---|---|
| Partial close-out occurs | Closed and remaining derivative lots reconcile to original contracts. |
| FIFO policy applies | Realized gain uses oldest lots first. |
| Lot history is missing | Realized tax result is provisional or blocked by policy. |
| Client report is generated | Trade P&L, realized tax result and remaining exposure are separately visible. |

## 59. Collateral Rehypothecation Restriction

Scenario:

- An OTC derivative CSA permits cash collateral but prohibits rehypothecation for segregated client assets.
- The dealer requests permission to reuse posted collateral.
- Posted collateral is USD 1,200,000.
- Rehypothecation-eligible amount under current legal agreement is USD 0.

Restriction check:

```text
restricted_collateral = posted_collateral - rehypothecation_eligible_amount
restricted_collateral = 1,200,000 - 0 = 1,200,000
```

Correct treatment:

- preserve CSA terms, segregation status, collateral account, legal owner and rehypothecation flag;
- block collateral reuse when the governing agreement or client asset rule prohibits it;
- separate collateral eligibility for margin from collateral eligibility for reuse;
- report restriction state to operations, treasury and advisor workflows without exposing unrelated client terms.

QA assertions:

| Assertion | Expected result |
|---|---|
| CSA prohibits reuse | Rehypothecation amount is zero and reuse workflow is blocked. |
| Collateral is eligible for margin | Margin eligibility does not override reuse restriction. |
| Legal terms change | Reuse status updates only from source-backed effective date. |
| Operations report is generated | Posted, restricted and reusable collateral are separately visible. |

## 60. FX Barrier Digital Payout Dispute

Scenario:

- A digital FX option pays USD 250,000 if EUR/USD trades at or above the barrier during the observation window.
- Client source shows a high of 1.1052.
- Dealer source shows a high of 1.1048.
- Barrier is 1.1050.

Payout state:

```text
client_source_triggered = 1.1052 >= 1.1050 = true
dealer_source_triggered = 1.1048 >= 1.1050 = false
disputed_payout = 250,000
```

Correct treatment:

- keep the payout disputed until the governing observation source and timestamp are resolved;
- preserve option terms, barrier level, observation window, source hierarchy and tick data;
- do not book final digital payout from non-governing price evidence;
- show provisional, disputed and final payout states separately.

QA assertions:

| Assertion | Expected result |
|---|---|
| Sources disagree around barrier | Payout remains disputed or provisional. |
| Governing source confirms barrier hit | Digital payout is booked from confirmed source. |
| Governing source confirms no hit | Option expires without payout and dispute evidence is retained. |
| Client report is generated during dispute | Payoff state is labelled disputed, not final. |

## 61. Option Auto-Exercise Threshold

Scenario:

- A listed call option is in the money by 0.04 at expiry.
- Exchange auto-exercise threshold is 0.01.
- Client has a do-not-exercise instruction for half the contracts.
- Position is 20 contracts, contract size is 100.

Exercise quantity:

```text
auto_exercise_contracts_before_instruction = 20
do_not_exercise_contracts = 10
final_exercised_contracts = 20 - 10 = 10
delivered_shares = 10 x 100 = 1,000
```

Correct treatment:

- apply exchange auto-exercise rules, client instructions, account eligibility and funding checks together;
- preserve expiry price, threshold rule, do-not-exercise instruction, timestamp and clearing confirmation;
- create underlying shares and strike cash only for final exercised contracts;
- expire non-exercised contracts with clear reason and audit trail.

QA assertions:

| Assertion | Expected result |
|---|---|
| Option is above auto-exercise threshold | Auto-exercise workflow opens. |
| Valid do-not-exercise instruction exists | Final exercise quantity excludes instructed contracts. |
| Funding is insufficient | Exercise is blocked or exceptioned by policy. |
| Expiry report is generated | Exercised, not-exercised and expired quantities reconcile to opening contracts. |

## 62. Cleared Margin Model Migration

Scenario:

- A clearing house migrates from one margin model version to another.
- Initial margin increases from USD 820,000 to USD 970,000 for the same cleared swap portfolio.
- Market MTM is unchanged.

Model impact:

```text
margin_model_delta = 970,000 - 820,000 = 150,000
margin_model_delta_percentage = 150,000 / 820,000 = 18.29%
```

Correct treatment:

- preserve old model version, new model version, effective date, clearing house notice and portfolio population;
- separate model-driven margin change from market MTM movement and new trade activity;
- update liquidity projections and margin dashboards from the migration effective date;
- keep backtesting, approval and client explanation evidence for the methodology change.

QA assertions:

| Assertion | Expected result |
|---|---|
| Margin model changes | Initial margin changes with model-version lineage. |
| MTM is unchanged | P&L does not change because margin methodology changed. |
| Effective date is future-dated | Current margin remains on old model until effective date. |
| Liquidity report is generated | Additional margin need is shown as model migration impact. |

## 63. Swaption Exercise Settlement

Scenario:

- A physically settled payer swaption is exercised.
- Exercise creates an underlying interest-rate swap rather than a cash-only payout.
- Swaption notional is USD 15,000,000.
- Exercise fee is USD 12,000.
- Swap start date is two business days after exercise.

Settlement workflow:

```text
exercise confirmed -> close swaption -> create underlying swap -> post exercise fee -> initialize swap schedule
```

Correct treatment:

- validate exercise date, notice deadline, settlement method and swap terms before creating the underlying swap;
- close the option position and create the swap only after exercise confirmation;
- preserve swaption confirmation, exercise notice, resulting swap identifier, fee and schedule;
- update DV01, counterparty exposure, collateral and future cashflows from the new swap.

QA assertions:

| Assertion | Expected result |
|---|---|
| Swaption is physically settled | Underlying swap is created after confirmed exercise. |
| Exercise notice is late | Exercise is blocked or disputed according to terms. |
| Cash-settled swaption is exercised | Cash settlement posts instead of creating a swap. |
| Risk report is generated | Swaption exposure closes and swap DV01/exposure opens. |

## 64. Total Return Swap Corporate-Action Basket Adjustment

Scenario:

- A total return swap references an equity basket.
- One basket component completes a 2-for-1 stock split and another pays a special dividend.
- The swap economics must adjust basket quantity and dividend return without creating direct client holdings.

| Component | Event | Prior quantity | Adjustment |
|---|---|---:|---|
| Equity A | 2-for-1 split | 50,000 | New reference quantity 100,000 |
| Equity B | Special dividend | 40,000 | Dividend adjustment 80,000 |

Dividend adjustment:

```text
special_dividend_adjustment = 40,000 x 2.00 = 80,000
```

Correct treatment:

- preserve basket composition, corporate-action terms, effective date and dealer calculation notice;
- adjust reference basket economics without creating physical equity positions for the client;
- separate synthetic dividend return, price return, financing leg and reset cashflow;
- update exposure, attribution and client explanation from effective-dated basket terms.

QA assertions:

| Assertion | Expected result |
|---|---|
| Basket component splits | Reference quantity adjusts without creating client shares. |
| Special dividend is confirmed | Synthetic dividend adjustment posts according to swap terms. |
| Dealer notice is missing | Basket adjustment remains provisional or source-limited. |
| Attribution report is generated | Price return, dividend adjustment and financing leg are separate. |

## 65. Volatility-Control Overlay Rebalancing

Scenario:

- A portfolio uses a volatility-control overlay to keep equity exposure near a 10% target realized volatility.
- Current equity overlay notional is USD 20,000,000.
- The latest source-backed realized volatility is 16%.
- The mandate caps overlay notional at USD 22,000,000 and floors it at USD 8,000,000.

Rebalanced notional:

```text
raw_rebalanced_notional = 20,000,000 x 10% / 16% = 12,500,000
rebalanced_notional = min(max(12,500,000, 8,000,000), 22,000,000) = 12,500,000
notional_reduction = 20,000,000 - 12,500,000 = 7,500,000
```

Correct treatment:

- preserve target volatility, realized-volatility source, lookback window, cap/floor limits and rebalance timestamp;
- separate overlay notional change from underlying portfolio transactions and client cashflows;
- update delta-equivalent exposure, margin forecasts, mandate checks and scenario reports from the effective rebalance date;
- label exposure as provisional when volatility source, rebalance rule or execution confirmation is missing.

QA assertions:

| Assertion | Expected result |
|---|---|
| Realized volatility is above target | Overlay notional is reduced according to the control rule. |
| Calculated notional breaches floor/cap | Floor or cap is applied with clear explanation. |
| Rebalance confirmation is missing | Exposure is marked pending or source-limited. |
| Client report is generated | Underlying portfolio return and overlay exposure change are separate. |

## 66. Knock-In Recovery Basket

Scenario:

- A structured derivative references a worst-of equity basket.
- Initial worst-underlying level is 100.
- A knock-in barrier is observed during the term.
- At maturity, the final worst-underlying level is 72.
- Nominal is USD 1,000,000.

Recovery value:

```text
recovery_ratio = final_worst_underlying / initial_worst_underlying
recovery_ratio = 72 / 100 = 72%
cash_recovery = 1,000,000 x 72% = 720,000
capital_loss = 1,000,000 - 720,000 = 280,000
```

Correct treatment:

- preserve the observation calendar, barrier level, governing source, basket constituents and worst-of selection;
- process recovery from final source-backed basket levels, not from average basket performance or the best component;
- keep issuer calculation notice, payoff condition and settlement method attached to the lifecycle event;
- label or block final recovery when barrier observation or final fixing is disputed.

QA assertions:

| Assertion | Expected result |
|---|---|
| Knock-in barrier is confirmed | Downside payoff is activated for maturity calculation. |
| Final worst performer is identified | Recovery uses the worst-underlying ratio. |
| Barrier source is disputed | Final payoff remains provisional or source-limited. |
| Reporting explains outcome | Barrier event, final worst level, recovery and loss are visible. |

## 67. Swap Reset Compounding Dispute

Scenario:

- An overnight-index swap coupon uses daily compounded rates.
- Notional is USD 10,000,000.
- Internal compounded rate is 5.126%.
- Dealer compounded rate is 5.119%.
- Accrual factor is 0.25.

Disputed coupon delta:

```text
internal_coupon = 10,000,000 x 5.126% x 0.25 = 128,150
dealer_coupon = 10,000,000 x 5.119% x 0.25 = 127,975
coupon_dispute = 128,150 - 127,975 = 175
```

Correct treatment:

- preserve daily fixing inputs, compounding convention, observation shift, lookback, holiday calendar and dealer statement;
- keep internal and dealer coupon values separately until the governing calculation is resolved;
- post adjustment cashflows only when settlement correction evidence exists;
- distinguish reset methodology disputes from market-value movement and collateral calls.

QA assertions:

| Assertion | Expected result |
|---|---|
| Internal and dealer compounded rates differ | Dispute amount is calculated and tracked. |
| Governing source supports one value | Resolved coupon and adjustment lineage are recorded. |
| Settlement already occurred | Correction posts as an adjustment cashflow, not an overwrite. |
| Coupon report is generated | Daily fixing source and compounding convention are explainable. |

## 68. Futures Exchange-For-Physical Processing

Scenario:

- A client uses an exchange-for-physical transaction to replace an index futures position with an underlying ETF basket.
- Futures position: 40 contracts.
- Futures multiplier: 250.
- Futures settlement price: 4,800.
- Physical basket invoice value: USD 48,050,000.

Basis adjustment:

```text
futures_settlement_value = 40 x 250 x 4,800 = 48,000,000
basis_adjustment = 48,050,000 - 48,000,000 = 50,000
```

Correct treatment:

- close the futures exposure and create the physical or ETF basket position only from exchange, broker and custodian evidence;
- preserve EFP reference, futures close quantity, physical basket details, invoice value and settlement date;
- separate basis adjustment, broker fees, margin release and physical acquisition cost;
- block duplicate exposure when futures close confirmation and physical delivery confirmation arrive at different times.

QA assertions:

| Assertion | Expected result |
|---|---|
| EFP is confirmed | Futures exposure closes and physical exposure opens with lineage. |
| Only one leg is confirmed | Workflow remains pending and avoids double-counted exposure. |
| Invoice differs from futures value | Basis adjustment is calculated and explained. |
| Margin is released | Margin release is separate from acquisition cost and basis. |

## 69. Client-Specific Collateral Segregation Election

Scenario:

- A client elects segregated collateral treatment for an uncleared OTC derivative relationship.
- Required margin is USD 1,200,000.
- Eligible segregated collateral market value is USD 1,150,000.
- Applicable haircut is 3%.
- Segregation custody fee is 0.20% annually for 30 days.

Collateral shortfall and fee:

```text
eligible_lending_value = 1,150,000 x (1 - 3%) = 1,115,500
segregated_collateral_shortfall = 1,200,000 - 1,115,500 = 84,500
segregation_fee = 1,150,000 x 0.20% x 30 / 360 = 191.67
```

Correct treatment:

- preserve client election, CSA terms, segregation account, reuse restriction, haircut, custodian and effective date;
- prevent commingling with reusable collateral pools and block rehypothecation when the election prohibits reuse;
- report margin eligibility, segregation status, shortfall and custody cost separately;
- require source-backed election and account setup before treating collateral as segregated.

QA assertions:

| Assertion | Expected result |
|---|---|
| Segregation election is active | Collateral is held in the segregated pool and not reused. |
| Haircut reduces lending value | Shortfall is calculated from lending value, not market value. |
| Segregated account is not set up | Collateral movement is blocked or exceptioned. |
| Client statement is generated | Margin, shortfall, segregation status and fee are separate. |

## 70. Derivative Valuation Reserve Release

Scenario:

- A hard-to-value OTC derivative carried a valuation reserve because independent quotes were limited.
- Prior clean value was USD 2,400,000.
- Current clean value is USD 2,460,000.
- Prior reserve was USD 180,000.
- Remaining approved reserve is USD 60,000 after new market evidence.

Reserve release:

```text
clean_value_change = 2,460,000 - 2,400,000 = 60,000
reserve_release = 180,000 - 60,000 = 120,000
net_valuation_change = clean_value_change + reserve_release = 180,000
```

Correct treatment:

- keep clean value movement, reserve release, approval evidence, model version and market-evidence source separate;
- do not silently fold reserve release into ordinary market P&L without explanation;
- preserve valuation committee approval, independent quote support and effective date;
- update client reporting with source-backed valuation confidence and reserve movement where policy allows disclosure.

QA assertions:

| Assertion | Expected result |
|---|---|
| Independent evidence improves | Reserve release is calculated from prior and remaining reserve. |
| Approval evidence is missing | Reserve release remains pending or blocked. |
| Clean value also changes | Clean movement and reserve release are separate components. |
| Valuation report is generated | Model version, evidence source and reserve movement are traceable. |

## 71. Option Barrier Dispute Settlement

Scenario:

- A client holds a barrier option with a USD 1,000,000 notional.
- Dealer source says the barrier was touched and payout is zero.
- Independent market data says the barrier was not touched.
- The agreed dispute settlement pays 45% of the maximum payout.

Dispute settlement:

```text
maximum_payout = 1,000,000
settlement_percentage = 45%
settled_payout = 1,000,000 x 45% = 450,000
unpaid_disputed_amount = 1,000,000 - 450,000 = 550,000
```

Correct treatment:

- preserve dealer observation, independent observation, governing source hierarchy, dispute notice and settlement agreement;
- keep disputed payoff provisional until settlement evidence is accepted;
- post the settled payout as dispute settlement, not as normal barrier payoff if the economics were compromised;
- retain the unpaid disputed amount for audit, complaint, tax and reporting explanation where policy requires;
- distinguish barrier-source dispute from market valuation movement.

QA assertions:

| Assertion | Expected result |
|---|---|
| Dealer and independent observations conflict | Payoff remains disputed or provisional. |
| Settlement agreement is accepted | Settled payout posts with dispute lineage. |
| Settlement percentage is missing | Final cashflow remains blocked or source-limited. |
| Client report is generated | Report separates contractual maximum, settled payout and dispute basis. |

## 72. Cleared Swap Compression Fee

Scenario:

- A cleared swap compression cycle reduces gross notional across accepted trades.
- Accepted notional reduction is USD 25,000,000.
- Clearing house fee is 0.8 bps of accepted notional.
- Broker processing fee is USD 3,500.

Compression cost:

```text
clearing_fee = 25,000,000 x 0.8 / 10,000 = 2,000
total_compression_fee = 2,000 + 3,500 = 5,500
```

Correct treatment:

- preserve compression run id, accepted trade list, rejected trade list, notional reduction and fee schedule;
- reduce notional only for accepted trades and keep compression fee separate from swap MTM;
- allocate fees by agreed rule, such as notional reduction, trade count or account owner;
- avoid booking fee before clearing-house confirmation;
- update exposure, margin and reporting after accepted compression is final.

QA assertions:

| Assertion | Expected result |
|---|---|
| Compression run has rejected trades | Fees and notional reduction apply only to accepted trades. |
| Clearing fee schedule is missing | Fee posting is blocked or source-limited. |
| Broker fee arrives separately | Total cost reconciles without duplicating clearing fee. |
| Exposure report is generated | Notional reduction and compression fee are separately visible. |

## 73. Futures Position-Limit Remediation

Scenario:

- A futures account breaches an exchange position limit after same-day fills.
- Exchange limit is 1,000 contracts.
- Current gross position is 1,080 contracts.
- Remediation plan reduces 120 contracts before the regulatory deadline.

Limit breach:

```text
excess_contracts = current_position - exchange_limit
excess_contracts = 1,080 - 1,000 = 80
post_remediation_position = 1,080 - 120 = 960
```

Correct treatment:

- preserve exchange limit, aggregation rule, positions, fills, breach timestamp, remediation order and deadline;
- block new orders that increase the breach until remediation completes;
- distinguish exchange hard limits from internal advisory concentration limits;
- update margin, exposure and client communication after remediation execution;
- keep breach evidence even after the position is back within limit.

QA assertions:

| Assertion | Expected result |
|---|---|
| Gross position exceeds exchange limit | Breach workflow opens with excess contracts. |
| New order increases breach | Order is blocked or escalated by policy. |
| Remediation trade executes | Position limit state returns within limit with lineage. |
| Report is generated after remediation | Historical breach and remediation remain auditable. |

## 74. FX Option Fixing Source Override

Scenario:

- An FX option cash settlement depends on a fixing source.
- Contract specifies Fixing Source A.
- Source A is unavailable at fixing time.
- Operations proposes Source B under fallback policy.

Fallback settlement:

| Input | Value |
|---|---:|
| Notional | 2,000,000 |
| Strike | 1.0800 |
| Source B fixing | 1.0950 |
| Payout currency | USD |

```text
intrinsic_rate = max(1.0950 - 1.0800, 0) = 0.0150
fallback_cash_settlement = 2,000,000 x 0.0150 = 30,000
```

Correct treatment:

- preserve contractual fixing source, outage evidence, fallback hierarchy, approval and source-B timestamp;
- label settlement as fallback-source based, not ordinary fixing-source settlement;
- block settlement if fallback policy or approval is missing;
- keep client/advisor explanation tied to contract terms and fallback protocol;
- preserve downstream tax, cash and valuation adjustment lineage.

QA assertions:

| Assertion | Expected result |
|---|---|
| Contract source is unavailable | Fallback workflow opens with outage evidence. |
| Fallback source is approved | Settlement uses fallback fixing with explicit label. |
| Approval is missing | Settlement remains blocked or source-limited. |
| Client report is generated | Fixing source override is visible without hiding the original source outage. |

## 75. Collateral Dispute Interest Claim

Scenario:

- A collateral dispute delays return of excess cash collateral.
- Excess collateral amount is USD 750,000.
- Agreed dispute interest rate is 4.80% annualized.
- Delay is 12 days.
- Day-count basis is 360.

Interest claim:

```text
dispute_interest_claim = 750,000 x 4.80% x 12 / 360 = 1,200
```

Correct treatment:

- preserve collateral call, agreed/disputed amount, resolution date, interest terms and settlement statement;
- calculate interest on the agreed delayed amount, not on the full collateral balance unless terms require it;
- keep collateral principal return, dispute interest and any fees separate;
- do not accrue client-ready interest when dispute terms or governing rate are missing;
- reconcile interest claim to dealer or custodian settlement.

QA assertions:

| Assertion | Expected result |
|---|---|
| Excess collateral return is delayed | Interest claim is calculated from source-backed terms. |
| Interest rate is missing | Claim remains pending or source-limited. |
| Dealer pays principal but not interest | Principal closes while interest claim remains open. |
| Client report is generated | Collateral principal, dispute interest and resolution state are distinct. |

## 76. Model Reserve Backtesting Exception

Scenario:

- A hard-to-value derivative has a valuation reserve calibrated from model backtesting.
- Backtesting shows realized exits are worse than model values by 1.20%.
- Current clean value is USD 5,000,000.
- Existing reserve is USD 40,000.
- Policy reserve required from backtest is 1.20%.

Reserve exception:

```text
required_reserve = 5,000,000 x 1.20% = 60,000
reserve_shortfall = 60,000 - 40,000 = 20,000
```

Correct treatment:

- preserve backtesting population, realized exits, model values, tolerance, exception owner and reserve policy;
- increase, retain or review reserve according to valuation governance;
- separate reserve shortfall from clean market-value movement;
- route repeated exceptions to model governance, not only trade operations;
- disclose valuation confidence or degraded state according to reporting policy.

QA assertions:

| Assertion | Expected result |
|---|---|
| Backtesting breach exceeds tolerance | Reserve exception workflow opens. |
| Existing reserve is below policy reserve | Reserve shortfall is calculated and routed for approval. |
| Model governance approves adjustment | Reserve changes with effective date and evidence. |
| Valuation report is generated | Clean value, reserve, backtest result and exception state are traceable. |

## 77. Cross-Product Delta-Limit Aggregation

Scenario:

- A client holds listed equity, equity options and a structured note linked to the same issuer.
- The internal concentration limit uses delta-adjusted exposure across direct and derivative positions.
- Direct equity market value is USD 400,000.
- Call option delta-adjusted exposure is USD 180,000.
- Structured note delta-adjusted exposure is USD 260,000.
- Client portfolio value is USD 5,000,000.
- Issuer delta-limit is 15.00% of portfolio value.

Limit usage:

```text
aggregate_delta_exposure = 400,000 + 180,000 + 260,000 = 840,000
delta_limit_amount = 5,000,000 x 15.00% = 750,000
excess_delta_exposure = 840,000 - 750,000 = 90,000
limit_usage = 840,000 / 750,000 = 112.00%
```

Correct treatment:

- preserve direct holdings, derivative contracts, structured-note sensitivity source, portfolio value and limit rule;
- aggregate only compatible as-of exposures with current delta or source-backed sensitivity;
- label missing or stale sensitivity as source-limited instead of assuming zero exposure;
- distinguish legal holding concentration from delta-adjusted economic exposure;
- route breach, waiver or remediation workflow according to mandate and advisory policy.

QA assertions:

| Assertion | Expected result |
|---|---|
| All sensitivities are current | Aggregate delta exposure and excess are calculated. |
| Structured-note delta is stale | Limit check is blocked, labelled partial or escalated by policy. |
| Direct holding is sold | Aggregate exposure updates without deleting derivative lineage. |
| Client report is generated | Legal holdings and delta-adjusted issuer exposure are not confused. |

## 78. Client Collateral Concentration Breach

Scenario:

- A client posts collateral for an OTC derivative portfolio.
- CSA allows equity collateral, but internal collateral policy limits any single issuer to 40.00% of posted collateral value.
- Posted collateral value is USD 1,200,000.
- Single-issuer stock collateral value is USD 620,000.
- Applicable haircut is 25.00%.

Concentration and lending value:

```text
issuer_concentration = 620,000 / 1,200,000 = 51.67%
issuer_concentration_excess = 51.67% - 40.00% = 11.67%
lending_value = 620,000 x (1 - 25.00%) = 465,000
```

Correct treatment:

- preserve CSA terms, internal concentration rule, collateral inventory, issuer mapping, source prices and haircuts;
- calculate concentration on market value while margin sufficiency uses haircut-adjusted lending value;
- do not release or substitute collateral until replacement is accepted and settled;
- separate CSA eligibility from private-bank collateral concentration policy;
- open remediation workflow with advisor, credit and operations visibility.

QA assertions:

| Assertion | Expected result |
|---|---|
| Issuer concentration exceeds limit | Breach workflow opens with concentration and excess. |
| Collateral is CSA-eligible | Breach may still remain under internal policy. |
| Replacement collateral is pending | Existing collateral remains active until settled. |
| Collateral report is generated | Market value, haircut value and concentration breach are distinct. |

## 79. OTC Confirmation Mismatch Ageing

Scenario:

- An OTC swap trade has an internal trade capture record and a dealer confirmation.
- Notional matches, but the fixed-rate leg differs.
- Mismatch has remained unresolved for 9 business days.
- Policy escalates unresolved economic mismatches after 5 business days.

Ageing:

```text
mismatch_age = business_days_between(mismatch_detected_date, today)
mismatch_age = 9
escalation_overdue_days = 9 - 5 = 4
```

Correct treatment:

- preserve internal trade terms, dealer confirmation, mismatch fields, timestamps, owner and escalation state;
- classify economic mismatches separately from non-economic text or reference mismatches;
- avoid downstream settlement or valuation overrides until governing terms are resolved;
- keep provisional valuation and exposure labelled when confirmation terms are disputed;
- escalate ageing mismatches to operations, risk and advisor-facing support where client reporting may be affected.

QA assertions:

| Assertion | Expected result |
|---|---|
| Economic field mismatches | Confirmation break opens with field-level difference. |
| Mismatch age exceeds threshold | Escalation state and overdue days are visible. |
| Dealer sends correction | Break closes only when governing terms reconcile. |
| Client report is generated before closure | Position is labelled provisional or source-limited by policy. |

## 80. Option Exercise Funding Shortfall

Scenario:

- A client holds in-the-money call options that are eligible for exercise.
- Exercise would require cash to buy the underlying shares.
- Available cash is USD 180,000.
- Exercise funding requirement is USD 260,000.
- Exercise deadline is today.

Funding shortfall:

```text
funding_shortfall = exercise_funding_required - available_cash
funding_shortfall = 260,000 - 180,000 = 80,000
exercise_allowed = funding_shortfall <= 0 or approved_credit_available
```

Correct treatment:

- preserve option contracts, exercise notice, strike, deliverable quantity, cash requirement, deadline and funding source;
- separate exercise value from settlement funding need;
- block, partial-exercise, finance or escalate according to mandate, credit and operational policy;
- avoid accidental exercise if funding, authority or settlement route is unresolved;
- reflect missed, partial or funded exercise in client reporting with clear opportunity-cost context where appropriate.

QA assertions:

| Assertion | Expected result |
|---|---|
| Funding is sufficient | Exercise can proceed subject to authority and deadline. |
| Funding is short | Exercise is blocked, resized, financed or escalated by policy. |
| Credit approval arrives before deadline | Exercise state updates with funding evidence. |
| Exercise deadline passes unresolved | Option expiry or non-exercise treatment is recorded with evidence. |

## 81. Clearing-House Default Fund Assessment

Scenario:

- A clearing house issues a default fund assessment after a member default.
- Client cleared derivatives are not closed out, but a pass-through assessment is allocated by clearing broker.
- Client share of default fund allocation base is 2.50%.
- Total assessment pool is USD 4,000,000.

Assessment:

```text
client_assessment = assessment_pool x client_allocation_share
client_assessment = 4,000,000 x 2.50% = 100,000
```

Correct treatment:

- preserve clearing-house notice, clearing-broker statement, allocation base, client share, assessment date and dispute state;
- separate default fund assessment from margin call, trading P&L and brokerage fees;
- check client agreement, pass-through terms, reporting classification and tax treatment before posting;
- keep affected cleared positions, margin and assessment evidence linked;
- route disputed assessments separately from ordinary margin operations.

QA assertions:

| Assertion | Expected result |
|---|---|
| Clearing-broker assessment is confirmed | Client assessment is calculated with allocation lineage. |
| Allocation basis is missing | Posting remains blocked or source-limited. |
| Client disputes assessment | Payable, dispute and settlement states remain separate. |
| Statement is generated | Assessment is not reported as derivative MTM or variation margin. |

## 82. Structured Payoff Model Recalibration

Scenario:

- A structured derivative payoff model is recalibrated because volatility surface inputs changed.
- Official issuer price is unchanged for the day, but scenario payoff and Greeks change.
- Old delta is 0.42.
- New delta is 0.51.
- Notional is USD 1,000,000.

Sensitivity change:

```text
delta_change = new_delta - old_delta
delta_change = 0.51 - 0.42 = 0.09
delta_equivalent_change = notional x delta_change
delta_equivalent_change = 1,000,000 x 0.09 = 90,000
```

Correct treatment:

- preserve model version, calibration inputs, volatility surface, effective timestamp, old sensitivities and new sensitivities;
- separate payoff model recalibration from official valuation movement when price is unchanged;
- update scenario analytics, concentration checks and hedge reporting from the recalibration effective date;
- require model governance approval where recalibration changes client-facing risk or suitability metrics;
- keep prior sensitivity available for audit, report restatement and explain-pack comparisons.

QA assertions:

| Assertion | Expected result |
|---|---|
| Model recalibration is approved | New sensitivities are effective from approved timestamp. |
| Official price is unchanged | Market value does not move solely because Greeks changed. |
| Mandate exposure check runs | It uses current source-backed sensitivities. |
| Client explain pack is generated | Old and new scenario results are versioned and traceable. |

## 83. Advisory And Mandate Checklist

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

## 84. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Contract position | contract id, notional, quantity, underlying, expiry, counterparty, market value, leg grouping where sourced | advanced scenario replay and automated strategy discovery |
| Exposure | notional, delta-equivalent, DV01/CS01, Greeks and volatility exposure where supplied | central cross-product exposure engine and full strategy-level attribution |
| Margin/collateral | broker/dealer margin postings, collateral balances, dispute status, agreed/disputed amounts, eligibility, haircuts, phase-in status and source-backed margin inputs | full margin simulation, optimization, substitution and dispute analytics |
| Valuation | vendor/dealer/exchange values with source timestamps, model labels, clean/adjusted value components and model-version evidence | independent model valuation with explain, calibration, valuation-adjustment ownership and variance/barrier replay |
| Lifecycle events | expiry, exercise, assignment, reset, fixing, settlement, novation, compression, clearing, porting, barrier events and notional exchanges when sourced | automated OTC confirmation matching, lifecycle replay engine and event generation |
| Reporting | market value, exposure, source date, stale/unsupported labels, hedge overlay split, strategy grouping and clearing state where sourced | advanced hedge-effectiveness reporting and multi-factor attribution |

## 85. Regression Test Pack

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
19. Variance swap settlement uses realized variance, not simple volatility or index return.
20. Barrier option closes only when source-backed observation terms confirm knock-out or knock-in state.
21. Cross-currency swap separates initial exchange, coupon exchanges, final exchange, FX exposure and valuation.
22. Exercise-window workflow blocks late or unsupported exercise instructions.
23. Collateral dispute tracks call amount, agreed amount, disputed amount, threshold and resolution state.
24. Clearing-broker default workflow identifies affected contracts, margin, portability and close-out state.
25. Strategy-level attribution preserves legal option legs while showing grouped payoff and contribution.
26. SIMM-style margin input completeness blocks unsupported estimates when sensitivity, netting-set or threshold data is missing.
27. Volatility-surface shock labels approximate vega-based results and blocks stale or incompatible sensitivities.
28. Callable swap exercise workflow validates call date, notice deadline, dealer valuation and termination lineage.
29. Equity swap dividend adjustment separates synthetic dividend return from physical share income.
30. Futures delivery-risk control prevents unintended physical delivery without source notice evidence.
31. Prime-broker give-up workflow preserves execution lineage and blocks duplicate or rejected clearing acceptance.
32. Early termination amount separates close-out MTM, accruals, break funding, fees, residual notional and collateral release.
33. Close-out netting exposure includes only enforceable netting-set trades and tracks independent amount disputes separately.
34. CVA/DVA/FVA adjusted valuation preserves clean value, component ownership, model version and source timestamp.
35. Compression tear-up allocates residual P&L by agreed rule and prevents duplicate notional reduction on replay.
36. Cross-currency collateral optionality applies CSA eligibility, FX source, asset haircut and mismatch haircut.
37. Lifecycle replay posts a correction cashflow when a corrected reset arrives after settlement.
38. Barrier observation dispute blocks final knock-in or knock-out state until governing source evidence resolves the dispute.
39. Uncleared margin phase-in applies only after effective date and blocks unsupported SIMM-style estimates when inputs are missing.
40. Exchange position-limit control blocks or escalates orders that breach hard exchange or aggregation limits.
41. Cleared OTC porting preserves old and new clearing-broker lineage and separates rejected contracts from ported contracts.
42. Valuation model-change approval separates market movement from model impact and preserves versioned audit history.
43. Wrong-way risk flags related counterparty/underlying exposure and separates market payoff from counterparty concentration.
44. Collateral interest settlement fail tracks receivable/payable, failed cash state and SSI repair lineage.
45. Fallback-rate transition uses source-backed fallback benchmark, spread adjustment and compounding method.
46. Option amendment versions contract terms and posts amendment fee as lifecycle cashflow when sourced.
47. Cross-clearinghouse portability preserves continuity and calculates target margin top-up or release.
48. Model backtesting opens exception workflow when model value breaches independent quote tolerance.
49. Hedge-accounting evidence keeps designation, hedge ratio, effectiveness testing and de-designation state.
50. Client-reporting explain pack separates MTM, exposure, payoff condition, observation calendar and scenario risk.
51. Autocall observation correction preserves original and corrected observations and controls reversal of lifecycle cashflows.
52. Commodity futures delivery alternative blocks unintended physical delivery and records roll, close-out or EFP evidence.
53. FX option premium settlement separates trade-date position, premium payable/reserve and settled cash debit.
54. Swap portfolio novation wave separates accepted and rejected contracts and prevents false close/open P&L.
55. Collateral substitution dispute applies CSA eligibility and haircut rules before releasing existing collateral.
56. XVA sensitivity explain pack reconciles clean value, CVA, DVA, FVA and stressed adjustment movement.
57. Structured-product Greek restatement versions sensitivities without changing valuation when the price file is unchanged.
58. Intraday futures margin spike separates initial margin, intraday add-on, variation margin and liquidity escalation.
59. Uncleared margin threshold reset recalculates collateral calls from source-backed CSA trigger evidence.
60. Swap rate-fixing dispute preserves internal fixing, dealer fixing, governing source and adjustment cashflow lineage.
61. Portfolio compression rejection reduces notional only for accepted trades with final consent evidence.
62. Derivative tax-lot close-out reporting reconciles closed lots, remaining lots, proceeds and realized tax result.
63. Collateral rehypothecation restriction separates margin eligibility from collateral reuse permission.
64. FX barrier digital payout dispute remains provisional until governing observation source resolves the barrier state.
65. Option auto-exercise threshold applies exchange rules, client instructions, funding checks and final clearing confirmation.
66. Cleared margin model migration separates methodology-driven margin change from market MTM movement.
67. Swaption exercise settlement creates the correct resulting swap or cashflow according to settlement terms.
68. Total return swap corporate-action basket adjustment changes synthetic economics without creating physical holdings.
69. Volatility-control overlay rebalancing applies target volatility, source-backed realized volatility and mandate floors/caps.
70. Knock-in recovery basket uses governing barrier observation and final worst-underlying level for recovery.
71. Swap reset compounding dispute preserves internal and dealer values until the governing calculation resolves.
72. Futures exchange-for-physical processing closes futures and opens physical exposure only with both-leg evidence.
73. Client-specific collateral segregation election separates reusable collateral from segregated collateral and calculates shortfall after haircut.
74. Derivative valuation reserve release separates clean-value movement from approved reserve movement.
75. Option barrier dispute settlement preserves conflicting observations, settlement agreement and unpaid disputed amount.
76. Cleared swap compression fees apply only to accepted compressed trades and remain separate from MTM.
77. Futures position-limit remediation blocks breach-increasing orders and preserves historical breach evidence.
78. FX option fixing source override requires source outage evidence, fallback hierarchy and approval.
79. Collateral dispute interest claim separates principal return, dispute interest and open claim state.
80. Model reserve backtesting exception calculates reserve shortfall and routes repeated breaches to model governance.
81. Cross-product delta-limit aggregation combines compatible current exposures and labels stale or missing sensitivities.
82. Client collateral concentration breach separates CSA eligibility, haircut lending value and internal concentration policy.
83. OTC confirmation mismatch ageing escalates unresolved economic breaks and labels disputed downstream states.
84. Option exercise funding shortfall blocks, resizes, finances or escalates exercise before deadline.
85. Clearing-house default fund assessment is separated from margin, trading P&L and ordinary fees.
86. Structured payoff model recalibration versions sensitivities without changing official valuation when price is unchanged.

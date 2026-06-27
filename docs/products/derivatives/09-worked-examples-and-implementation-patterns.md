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

## 9. Advisory And Mandate Checklist

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

## 10. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Contract position | contract id, notional, quantity, underlying, expiry, counterparty, market value | multi-leg strategy decomposition and lifecycle replay |
| Exposure | notional, delta-equivalent, DV01/CS01 where supplied | central cross-product exposure engine |
| Margin/collateral | broker/dealer margin postings and collateral balances | full margin simulation and collateral optimization |
| Valuation | vendor/dealer/exchange values with source timestamps | independent model valuation with explain and calibration |
| Lifecycle events | expiry, exercise, assignment, reset, fixing, settlement when sourced | automated OTC confirmation matching and event generation |
| Reporting | market value, exposure, source date, stale/unsupported labels | hedge-effectiveness reporting and strategy-level attribution |

## 11. Regression Test Pack

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

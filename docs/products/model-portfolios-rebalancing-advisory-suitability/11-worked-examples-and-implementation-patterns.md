# 11 - Worked Examples and Implementation Patterns

Use these examples when designing model portfolios, advisory proposals, DPM mandates, rebalancing workflows, suitability checks, reporting, QA, and implementation boundaries.

## 1. Model Version Rollout

Scenario:

- Balanced model v1 target: 50% equities, 40% fixed income, 10% alternatives.
- Balanced model v2 target: 45% equities, 45% fixed income, 10% alternatives.
- Effective date: 2026-07-01.
- Portfolios assigned to the model should not be automatically traded until governance and mandate rules allow it.

Workflow:

```text
model draft -> investment committee approval -> effective date -> impacted portfolio analysis -> proposal/rebalance generation -> checks -> approval -> orders
```

Required records:

| Record | Why it matters |
|---|---|
| model_version_id | Proves which target produced the recommendation |
| approval record | Shows governance state before use |
| effective date | Prevents premature rollout |
| impacted portfolio list | Supports advisor/DPM review |
| rebalance policy | Determines whether drift creates action |
| override reason | Explains why a portfolio did not follow the model |

QA assertion:

A model target change should create analysis and candidate actions, not silent trades.

## 2. Drift Rebalance With Tolerance Bands

Scenario:

- Portfolio value: USD 1,000,000.
- Equity target: 50%.
- Equity tolerance: +/-5%.
- Current equity value: USD 575,000.

Calculation:

```text
current equity weight = 575,000 / 1,000,000 = 57.5%
upper tolerance = 55.0%
drift above band = 57.5% - 55.0% = 2.5%
trade to target = 575,000 - 500,000 = 75,000 sell
trade to upper band = 575,000 - 550,000 = 25,000 sell
```

Implementation decision:

| Policy | Result |
|---|---|
| rebalance to target | sell USD 75,000 equities |
| rebalance to band edge | sell USD 25,000 equities |
| advisor review required | generate proposal, not order |
| DPM auto rebalance allowed | generate order basket after checks |

Do not hardcode one rebalance amount without storing the policy basis.

## 3. Funding-Constrained Rebalance

Scenario:

- Target trade list requires USD 80,000 cash for fund purchases.
- Settled cash: USD 20,000.
- Pending equity sale proceeds: USD 50,000 settling T+2.
- Minimum cash buffer: USD 15,000.

Available for immediate purchase:

```text
available cash = settled cash - minimum buffer = 20,000 - 15,000 = 5,000
```

Execution posture:

- either stage purchases until proceeds settle;
- or create a bridge-funding workflow if policy allows;
- or reduce order size;
- or generate sell-first instruction sequence.

QA assertion:

The rebalance engine should distinguish target cash, settled cash, projected cash, reserved cash, and policy-available cash.

## 4. Tax-Aware Rebalance

Scenario:

- Sell candidate A: unrealized gain USD 25,000.
- Sell candidate B: unrealized loss USD 8,000.
- Both reduce the same overweight sector.
- Client has tax-aware preference enabled.

Decision support:

| Candidate | Portfolio effect | Tax effect | Advisory note |
|---|---|---|---|
| Sell A | reduces overweight | realizes gain | may create taxable event |
| Sell B | reduces overweight | realizes loss | may support loss harvesting if allowed |

Implementation boundary:

The platform may rank alternatives using source-backed lot data and client tax preference. It should not present tax advice unless reviewed under approved policy and jurisdictional rules.

## 5. Suitability Failure In Advisory Proposal

Scenario:

- Advisor proposes a structured product.
- Client risk profile: moderate.
- Product risk rating: high.
- Product complexity: complex.
- Client knowledge and experience: not validated.

Expected workflow:

```text
proposal draft -> product eligibility check fails -> suitability evidence required -> proposal blocked or escalated -> audit trail preserved
```

Reportable evidence:

| Evidence | Required |
|---|---|
| client risk profile source/date | yes |
| product risk score source/version | yes |
| complexity classification | yes |
| knowledge and experience status | yes |
| override approver and rationale | only if policy allows |

Do not allow a blocked proposal to become a client-ready recommendation by changing only display text.

## 6. Proposal Expiry

Scenario:

- Proposal generated on 2026-06-01.
- Validity: 10 calendar days.
- Market prices and suitability evidence are refreshed daily.
- Client accepts on 2026-06-15.

Expected behavior:

- proposal is expired;
- acceptance should not create orders;
- advisor must regenerate proposal or refresh evidence;
- audit trail should show expired state and attempted acceptance.

QA assertion:

Proposal validity should be enforced against proposal terms, not against the UI session date alone.

## 7. DPM Restriction And Substitution

Scenario:

- DPM model recommends buying a global equity fund.
- Client restriction excludes fossil-fuel exposure above a threshold.
- Fund look-through is missing.

Options:

| Action | Treatment |
|---|---|
| block purchase | safest if restriction cannot be evaluated |
| substitute approved fund | allowed only if substitution policy and model tolerance support it |
| manual override | requires documented authority and evidence |
| partial rebalance | may reduce drift without breaching restriction |

Implementation principle:

Restriction checks must preserve unknown states. Missing look-through is not the same as passing the restriction.

## 8. Post-Trade Residual Drift

Scenario:

- Target bond allocation: 40%.
- Proposed allocation after trades: 40%.
- Actual filled allocation: 38.5% because one order partially filled.

Post-trade workflow:

```text
fill ingestion -> post-trade portfolio snapshot -> residual drift calculation -> tolerance check -> action/no-action decision -> outcome review
```

Reporting:

- show proposed versus executed allocation;
- explain partial fills or rejected orders;
- store residual drift reason;
- avoid claiming rebalance completed to target if fills did not achieve target.

## 9. Advisory And DPM Boundary

| Question | Advisory proposal | DPM mandate |
|---|---|---|
| Who approves action? | client or authorized representative | discretionary manager under mandate |
| Output before trade | proposal and consent evidence | investment decision and order approval evidence |
| Suitability | client-specific suitability and disclosure | mandate/risk/investment guideline compliance |
| Reporting | recommendation status, consent, orders | mandate activity, drift, rationale, outcome |
| Failure state | expired, declined, suitability blocked | guideline breach, pending approval, execution failure |

Do not reuse DPM auto-approval behavior in advisory workflows unless client consent and policy explicitly allow it.

## 10. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Model assignment | portfolio-to-model, target weights, effective date | model marketplace and personalization engine |
| Drift analysis | current weights, target weights, tolerance bands | optimizer with taxes, liquidity, fees and lots |
| Proposal workflow | draft, checks, client consent, expiry, audit | collaborative advisor/client proposal workspace |
| DPM rebalance | mandate, model version, restrictions, approval state | wave-based execution and outcome attribution |
| Suitability checks | product/client/risk/complexity evidence | explainable suitability scoring and alternative ranking |
| Reporting | proposal state, drift, action rationale, post-trade outcome | client-ready rebalance narrative generation |

## 11. Regression Test Pack

Minimum release-gate scenarios:

1. Model version change creates impacted-portfolio analysis, not silent trades.
2. Drift calculation distinguishes rebalance-to-target from rebalance-to-band.
3. Cash-constrained rebalance blocks unfunded buys.
4. Tax-aware rebalance uses source-backed lot data and labels advice boundary.
5. Suitability failure blocks or escalates proposal.
6. Expired proposal cannot create orders.
7. Missing look-through blocks restricted-product pass decision.
8. Partial fills produce residual drift and outcome review.
9. Advisory workflow requires client consent where applicable.
10. DPM workflow records mandate authority and post-trade evidence.

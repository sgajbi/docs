# Worked Examples and Implementation Patterns

This file turns loans, Lombard lending, margin, collateral and buying-power concepts into practical examples for advisory workflows, portfolio analytics, reporting, implementation, QA, controls and production support.

The examples use simplified numbers. Production systems should use approved credit policy, facility terms, contractual day-count conventions, source-backed prices, FX rates, pledge records, concentration rules and jurisdiction-specific controls.

## Example 1. Simple Lombard availability

**Scenario:** A client has a USD 1,000,000 Lombard facility secured by a diversified portfolio.

| Item | Value |
|---|---:|
| Approved facility limit | 1,000,000 |
| Current drawn exposure | 400,000 |
| Accrued interest and fees | 5,000 |
| Total exposure | 405,000 |
| Eligible collateral market value | 900,000 |
| Weighted lending value after LTV/haircuts | 630,000 |

Availability:

```text
facility_headroom = 1,000,000 - 405,000 = 595,000
collateral_headroom = 630,000 - 405,000 = 225,000
availability = min(595,000, 225,000) = 225,000
```

**Reporting interpretation:** The facility is collateral-bound, not limit-bound.

**QA assertion:** Availability must use total exposure including accrued interest and fees, not only drawn principal.

## Example 2. Collateral haircut by asset type

**Scenario:** A collateral pool contains cash, investment-grade bonds and listed equities.

| Collateral | Market value | LTV | Lending value |
|---|---:|---:|---:|
| USD cash deposit | 200,000 | 95% | 190,000 |
| Investment-grade bond | 500,000 | 75% | 375,000 |
| Listed equity | 300,000 | 60% | 180,000 |
| Total | 1,000,000 |  | 745,000 |

**Implementation pattern:** Store LTV rule version, asset classification, issuer/rating/liquidity inputs and override reason. Do not hard-code LTV only by broad asset class.

**QA assertion:** Market value and lending value must appear as separate report fields.

## Example 3. FX haircut on foreign-currency collateral

**Scenario:** A USD loan is secured by SGD collateral.

| Item | Value |
|---|---:|
| SGD collateral market value | SGD 1,000,000 |
| USD/SGD rate | 1.3500 |
| USD equivalent value | 740,740.74 |
| Base LTV | 70% |
| FX haircut factor | 92% |

```text
fx_adjusted_lending_value = 740,740.74 x 70% x 92% = 477,777.78
```

**Advisor explanation:** Foreign-currency collateral may support less borrowing because the bank applies both asset LTV and FX stress.

**QA assertion:** If the FX rate is stale or missing, availability should fail closed or be labelled unavailable according to policy.

## Example 4. Rating downgrade haircut change

**Scenario:** A bond pledged as collateral is downgraded. The LTV changes from 75% to 50%.

| Item | Before downgrade | After downgrade |
|---|---:|---:|
| Bond market value | 800,000 | 780,000 |
| LTV | 75% | 50% |
| Lending value | 600,000 | 390,000 |
| Exposure supported by pool | 500,000 | 500,000 |
| Headroom / shortfall | +100,000 | -110,000 |

**Workflow:** The downgrade should trigger collateral revaluation, lending-value recalculation, margin-call assessment and advisor/client notification where required.

**QA assertion:** A rating change should recalculate lending value even if the bond price has not changed materially.

## Example 5. Margin-call threshold and cure amount

**Scenario:** A margin facility has warning, margin-call and liquidation thresholds.

| Threshold | LTV |
|---|---:|
| Maintenance | 65% |
| Margin call | 70% |
| Liquidation | 80% |

The client has USD 700,000 exposure and collateral market value has fallen to USD 950,000.

```text
current_ltv = 700,000 / 950,000 = 73.68%
```

The account is above the margin-call threshold but below the liquidation threshold.

To cure back to 65% maintenance LTV by repayment:

```text
target_exposure = 950,000 x 65% = 617,500
repayment_required = 700,000 - 617,500 = 82,500
```

**Alternative cure:** Pledge additional eligible collateral. If new collateral has 60% LTV, required market value is:

```text
additional_collateral = 82,500 / 60% = 137,500
```

**QA assertion:** Cure calculations should state whether the cure basis is repayment amount, gross collateral market value or lending value.

## Example 6. Forced liquidation sequence

**Scenario:** A margin call is overdue and the portfolio reaches liquidation LTV. The bank sells pledged securities to reduce exposure.

| Item | Value |
|---|---:|
| Loan exposure before liquidation | 850,000 |
| Collateral market value before sale | 1,000,000 |
| Current LTV | 85% |
| Forced sale gross proceeds | 250,000 |
| Trading costs and taxes | 1,500 |
| Net proceeds applied to loan | 248,500 |
| Remaining loan exposure | 601,500 |

Expected event chain:

```text
MarginCallOverdue
  -> LiquidationAuthorized
  -> CollateralSaleOrderCreated
  -> ExecutionConfirmed
  -> NetProceedsAppliedToLoan
  -> CollateralPoolRevalued
  -> MarginCallClosedOrResidualShortfallCreated
```

**Reporting treatment:** Forced sale creates realized P&L on collateral assets and repayment activity on the loan. Do not hide it as a generic collateral adjustment.

## Example 7. Buying-power reservation for in-flight order

**Scenario:** A client has USD 120,000 availability and places a USD 90,000 buy order with estimated fees of USD 500.

```text
reserved_exposure = 90,000 + 500 = 90,500
remaining_buying_power = 120,000 - 90,500 = 29,500
```

If the order is cancelled, the reservation should be released. If the order partially fills for USD 60,000 plus USD 350 fees, only the final exposure should remain.

**Implementation pattern:** Separate indicative buying power, reserved buying power and settled exposure.

**QA assertion:** Concurrent orders must not overspend the same buying power.

## Example 8. Collateral substitution

**Scenario:** A client wants to release a pledged bond and replace it with cash.

| Collateral | Current lending value | Proposed lending value |
|---|---:|---:|
| Bond to release | 300,000 | 0 |
| Cash to pledge | 0 | 285,000 |
| Net change |  | -15,000 |

If current headroom is USD 40,000, the substitution is acceptable after the net reduction:

```text
post_substitution_headroom = 40,000 - 15,000 = 25,000
```

**Control requirement:** Release should occur only after the replacement pledge is effective, not merely requested.

**QA assertion:** Collateral release cannot create negative headroom unless approved through an exception workflow.

## Example 9. Cross-pledge direction and non-transitive support

**Scenario:** Entity A pledges collateral to support Entity B's facility. Entity B supports Entity C commercially, but there is no legal pledge from A to C.

```text
Entity A collateral -> supports Entity B facility
Entity B relationship -> does not automatically support Entity C facility
```

**Implementation pattern:** Model pledge provider, borrower, beneficiary, secured obligations, effective dates and permitted scope explicitly.

**QA assertion:** Cross-pledge support must not be transitive unless the legal pledge scope explicitly permits it.

## Example 10. Interest accrual and capitalization

**Scenario:** A USD 500,000 drawdown accrues interest at 6% using ACT/360 for 15 days.

```text
accrued_interest = 500,000 x 6% x 15 / 360 = 1,250
```

If interest is capitalized:

```text
new_principal = 500,000 + 1,250 = 501,250
```

**Reporting treatment:** Show principal, accrued interest, capitalized interest and total liability separately. Capitalization changes future interest base and may increase LTV.

**QA assertion:** Interest accrual must follow facility day-count convention and rate reset schedule.

## Example 11. Concentrated single-stock collateral cap

**Scenario:** A portfolio contains one large listed-equity position with standard 60% LTV, but policy caps single-stock lending value at USD 250,000.

| Item | Value |
|---|---:|
| Stock market value | 600,000 |
| Standard LTV | 60% |
| Raw lending value | 360,000 |
| Single-stock cap | 250,000 |
| Final lending value | 250,000 |

**Advisor implication:** A concentrated position may produce less buying power than a diversified portfolio with the same market value.

**QA assertion:** Concentration caps should apply after raw lending value calculation and should preserve the capped amount and reason code.

## Example 12. Collateral stale-price block

**Scenario:** A fund pledged as collateral has a NAV that is five business days old. Policy allows collateral values only up to two business days stale.

Expected treatment:

| Output | Treatment |
|---|---|
| Holding report | Show fund holding with stale NAV date. |
| Collateral lending value | Block or haircut to zero according to policy. |
| Availability | Recalculate using reduced eligible collateral. |
| Advisor dashboard | Show data-quality exception and remediation action. |
| Client report | Show valuation-date disclosure if included. |

**QA assertion:** Stale collateral valuation must affect availability and must not be silently treated as current.

## Example 13. Shared collateral optimization

**Scenario:** A family office has two credit lines supported by one collateral pool.

| Facility | Exposure | Priority | Required lending value |
|---|---:|---:|---:|
| Facility A | 400,000 | Senior | 400,000 |
| Facility B | 250,000 | Secondary | 250,000 |
| Total | 650,000 |  | 650,000 |

Collateral lending value is USD 700,000, leaving USD 50,000 shared headroom. If Facility B requests a further USD 100,000 drawdown, the platform should block or route for exception because only USD 50,000 headroom remains.

**Implementation pattern:** Shared collateral requires allocation rules, priority, reservation and stress handling. Do not calculate each facility independently against the full pool.

## Example 14. Lending client report controls

A client/advisor report should separate:

| Field | Meaning |
|---|---|
| Facility limit | Approved borrowing amount. |
| Drawn principal | Amount currently borrowed. |
| Accrued interest and fees | Liability not yet paid. |
| Total exposure | Principal plus accrued amounts and reserved exposure if policy includes it. |
| Collateral market value | Current value of pledged assets before haircuts. |
| Lending value | Collateral value after eligibility, LTV, FX and concentration rules. |
| Availability | Remaining capacity after exposure and controls. |
| Margin-call status | Normal, warning, call active, overdue, cured or liquidation. |

**QA assertion:** Reports must not show facility limit as available credit when collateral or exposure is the binding constraint.

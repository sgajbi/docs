# Worked Examples and Implementation Patterns

This file turns insurance, annuity and protection-linked product concepts into practical examples for advisory workflows, reporting, analytics, data modelling, QA and platform implementation.

Insurance products require more care than normal holdings because one contract can contain investment value, protection benefit, premium obligations, surrender terms, policy loans, beneficiaries, riders and claim states. The examples below keep those meanings separate.

## Example 1. ILP premium allocation and policy charges

**Scenario:** A client pays a USD 10,000 regular premium into an investment-linked policy. The policy applies a 3% allocation charge. The net premium is invested into two sub-funds.

| Item | Value |
|---|---:|
| Gross premium paid | 10,000 |
| Allocation charge | 300 |
| Net invested premium | 9,700 |
| Equity sub-fund allocation | 60% |
| Bond sub-fund allocation | 40% |
| Equity sub-fund NAV | 12.50 |
| Bond sub-fund NAV | 9.70 |

Unit allocation:

```text
equity_units = 9,700 x 60% / 12.50 = 465.6000
bond_units = 9,700 x 40% / 9.70 = 400.0000
```

**Reporting treatment:** Show the gross premium as client cashflow, the allocation charge as a policy charge, and the sub-fund units as investment-linked policy components. Do not report the USD 300 charge as investment loss from the sub-funds.

**QA assertion:** Gross premium, charges and net invested amount must reconcile.

## Example 2. Monthly insurance and administration charge deduction

**Scenario:** An ILP deducts monthly charges by redeeming units from the cash or money-market sub-fund.

| Charge type | Amount |
|---|---:|
| Cost of insurance | 120 |
| Administration charge | 15 |
| Rider charge | 35 |
| Total monthly charge | 170 |
| Cash sub-fund NAV | 1.00 |

```text
units_redeemed = 170 / 1.00 = 170.0000 units
```

**Implementation pattern:** Model charges as policy-level transactions with source reason codes, not as unexplained negative market movement. The policy value falls because units are redeemed to pay charges, but the performance explanation should separate market return from charge drag.

**QA assertion:** Charge deduction reduces units and policy account value, while preserving a separate charge ledger for reporting and suitability review.

## Example 3. Net surrender value with policy loan

**Scenario:** A whole-life policy has cash value, surrender charges and an outstanding policy loan.

| Component | Value |
|---|---:|
| Cash value | 250,000 |
| Surrender charge | 12,000 |
| Policy loan principal | 60,000 |
| Accrued loan interest | 3,500 |

```text
net_surrender_value = 250,000 - 12,000 - 60,000 - 3,500 = 174,500
```

**Reporting treatment:** Net surrender value is the accessible liquidity estimate. Death benefit and projected maturity value are separate benefit measures and should not be substituted for surrender value.

**QA assertion:** A surrender quote must show quote date, source, charge basis, loan balance and expiry. If the quote has expired, report stale or unavailable rather than using it as current liquidity.

## Example 4. Policy loan interest and benefit reduction

**Scenario:** A universal-life policy has a USD 100,000 loan at 6% annual interest. Interest is accrued monthly and not paid in cash.

```text
monthly_interest = 100,000 x 6% / 12 = 500
new_loan_balance = 100,000 + 500 = 100,500
```

If the policy death benefit is USD 1,000,000 and the loan balance is deducted from claim proceeds:

```text
net_death_benefit_before_other_adjustments = 1,000,000 - 100,500 = 899,500
```

**Advisor implication:** Policy loans can provide liquidity, but they reduce policy value or claim proceeds and may increase lapse risk if unmanaged.

**Implementation pattern:** Track loan principal, accrued interest, interest capitalization, repayment transactions, collateral assignment and benefit-offset rules separately.

## Example 5. Missed premium, grace period and lapse workflow

**Scenario:** A regular-premium policy requires USD 5,000 quarterly premium. The premium due on 2026-03-31 is unpaid. The contract has a 30-day grace period.

| Date | State |
|---|---|
| 2026-03-31 | Premium due |
| 2026-04-01 | Grace period starts |
| 2026-04-30 | Grace period ends |
| 2026-05-01 | Lapse workflow or reduced paid-up workflow starts, depending on product terms |

**Platform behavior:** Do not keep the policy silently marked as fully active if premium status is overdue. Show premium due, grace-period status, lapse risk and available remediation actions.

**QA assertion:** A missed premium should create a visible lifecycle state and an advisor/service task if the policy is reportable or materially affects protection planning.

## Example 6. Partial withdrawal from an ILP

**Scenario:** A client withdraws USD 20,000 from an ILP. The policy applies a 2% withdrawal charge.

| Item | Value |
|---|---:|
| Requested gross withdrawal | 20,000 |
| Withdrawal charge | 400 |
| Net cash paid to client | 19,600 |
| Units redeemed at NAV 10.00 | 2,000.0000 |

**Reporting treatment:** Separate the gross redemption, charge and net cash paid. The withdrawal should reduce account value and may affect death benefit, guarantees or surrender schedule depending on contract terms.

**Implementation pattern:** Validate minimum remaining policy value, charge schedule, tax/reporting flags, surrender restrictions and whether the withdrawal changes coverage.

## Example 7. Death claim lifecycle and beneficiary allocation

**Scenario:** A policy pays a USD 1,200,000 death benefit to two beneficiaries: 70% and 30%. A policy loan balance of USD 80,000 is deducted before payout.

```text
net_claim_proceeds = 1,200,000 - 80,000 = 1,120,000
beneficiary_a = 1,120,000 x 70% = 784,000
beneficiary_b = 1,120,000 x 30% = 336,000
```

Expected lifecycle:

1. claim notified,
2. claim evidence received,
3. coverage and exclusions checked,
4. loan and assignment offsets applied,
5. beneficiary authority validated,
6. claim approved,
7. payment instructed,
8. policy closed or coverage component terminated.

**Control requirement:** Beneficiary data is sensitive. Access should follow authority and privacy rules, not ordinary portfolio-report visibility.

## Example 8. Immediate annuity payout schedule

**Scenario:** A client pays USD 500,000 for an immediate annuity that pays USD 2,400 monthly for life, with a 10-year guaranteed period.

| Item | Value |
|---|---:|
| Purchase payment | 500,000 |
| Monthly payout | 2,400 |
| Annual income | 28,800 |
| Guaranteed-period payments | 120 |
| Guaranteed-period payout total | 288,000 |

**Reporting treatment:** The annuity payout is an income stream. It is not the same as a liquid market value unless the insurer provides a surrender or commutation value.

**Advisor implication:** The product may improve retirement cashflow certainty but reduces liquidity and depends on insurer credit and contract terms.

## Example 9. Variable annuity account value versus income base

**Scenario:** A variable annuity has an account value of USD 420,000 and a guaranteed income base of USD 500,000. The withdrawal benefit pays 5% of the income base annually.

```text
annual_guaranteed_withdrawal = 500,000 x 5% = 25,000
```

**Reporting treatment:** Account value and income base are different. The account value is the investment-linked value; the income base is a benefit calculation base and is not necessarily withdrawable as a lump sum.

**QA assertion:** Client statements must not add account value and income base together as total wealth.

## Example 10. Premium-financed policy collateral view

**Scenario:** A client uses a bank loan to finance premiums for a universal-life policy assigned as collateral.

| Component | Value |
|---|---:|
| Policy surrender value | 800,000 |
| Bank collateral haircut | 25% |
| Lending value | 600,000 |
| Outstanding premium-financing loan | 550,000 |
| Collateral surplus | 50,000 |

**Implementation pattern:** Link policy value, assignment status, loan balance, haircut policy, interest accrual and margin-call workflow. A policy update can trigger a credit event even if the investment portfolio did not trade.

**QA assertion:** Collateral reporting must not use death benefit as lending value unless the credit policy explicitly permits it and source evidence supports it.

## Example 11. Replacement and surrender comparison

**Scenario:** An advisor reviews replacing an existing endowment policy with a new ILP.

Minimum comparison:

| Dimension | Existing policy | Proposed policy |
|---|---|---|
| Current surrender value | Required | Not applicable or initial value |
| Premium obligation | Required | Required |
| Coverage and riders | Required | Required |
| Guarantees | Required | Required |
| Surrender charge period | Remaining term | New period |
| Charges | Existing policy charges | New policy and fund charges |
| Investment exposure | Current basis | Proposed basis |
| Tax/regulatory impact | Required if applicable | Required if applicable |
| Lost benefits | Required | Not applicable |
| Suitability rationale | Required | Required |

**Control requirement:** Replacement should not be treated as a normal buy/sell switch. It needs explicit comparison evidence, client acknowledgement or consent where required, and archive of the versions reviewed.

## Example 12. Client report value labelling

**Scenario:** A family-office report includes two policies.

| Policy | Death benefit | Account/cash value | Surrender value | Reporting label |
|---|---:|---:|---:|---|
| Term life | 2,000,000 | 0 | 0 | Protection cover only |
| ILP | 500,000 | 280,000 | 265,000 | Investment-linked policy value and surrender value |

**Good report design:** Show protection benefit and accessible value in separate sections. Avoid a single "insurance value" column that mixes death benefit and surrender value.

**QA assertion:** Total net worth should include only the selected wealth-value basis, not protection-only death benefit.

## Example 13. Policy data migration reconciliation

**Scenario:** Policies are migrated from insurer statements and legacy CRM records.

| Reconciliation item | Expected control |
|---|---|
| Policy count | Source policy count equals migrated active, lapsed, surrendered and matured policy count. |
| Policy roles | Owner, insured, payer, beneficiary and assignee roles are mapped separately. |
| Values | Account value, cash value, surrender value and death benefit retain separate fields. |
| Dates | Issue, premium due, valuation, maturity and expiry dates retain source lineage. |
| Charges and loans | Outstanding loans, accrued interest and surrender charges are reconciled. |
| Sensitive data | Beneficiary and health/claim details receive restricted visibility. |
| Closed policies | Surrendered, matured and claimed policies are not migrated as active holdings. |

**Sign-off evidence:** Use sample client statements, policy-level reconciliations, exception reports and advisor review packs before publishing migrated insurance data.

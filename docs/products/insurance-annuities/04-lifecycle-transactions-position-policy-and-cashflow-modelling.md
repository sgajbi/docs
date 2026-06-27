# 04 — Lifecycle, Transactions, Position, Policy and Cashflow Modelling

## 1. Core modelling principle

Insurance products should be modelled using three layers:

```text
1. Policy contract layer
   Who owns it, who is insured, what benefits exist, what rules apply.

2. Financial value layer
   Premiums, account value, cash value, surrender value, policy loans, claims.

3. Investment/exposure layer
   ILP sub-funds, variable annuity sub-accounts, asset allocation and risk exposure.
```

A portfolio system may show a policy as an asset, but the accounting and analytics engine must understand that policy values are not the same as normal market prices.

## 2. Lifecycle states

| State | Meaning |
|---|---|
| APPLICATION_PENDING | Application submitted, not yet issued |
| UNDERWRITING | Insurer assessing health/financial risk |
| ISSUED_ACTIVE | Policy issued and active |
| FREE_LOOK_PERIOD | Client can cancel under applicable cooling-off rules |
| PREMIUM_DUE | Premium expected |
| GRACE_PERIOD | Premium overdue but coverage not yet terminated |
| LAPSED | Policy inactive due to non-payment or insufficient value |
| REINSTATED | Lapsed policy restored |
| PAID_UP | Reduced benefit with no further premiums |
| IN_FORCE_WITH_LOAN | Active policy with outstanding policy loan |
| SURRENDER_PENDING | Termination requested |
| SURRENDERED | Policy terminated, value paid |
| CLAIM_PENDING | Claim submitted and under review |
| CLAIM_PAID | Claim paid |
| MATURED | Maturity benefit paid |
| ANNUITIZED | Value converted into annuity payout |
| EXPIRED | Term policy ended without claim |

## 3. Lifecycle event versus transaction

Not every lifecycle event is a financial transaction.

| Event | Transaction? | Reason |
|---|---|---|
| Application submitted | No | No economic posting yet |
| Policy issued | Usually no | Contract becomes active, but premium may be separate |
| Premium paid | Yes | Cash movement |
| Premium due | No | Obligation/schedule event |
| Grace period starts | No | Status event |
| Policy lapses | Maybe | May write down policy value if tracked |
| Bonus declared | Maybe | Depends if value is credited/vested |
| Fund switch | Yes/internal | Units/exposures change, no external cash |
| Charge deducted | Yes | Policy value reduction |
| Claim submitted | No | Status event |
| Claim approved/paid | Yes | Benefit cashflow |
| Surrender requested | No | Status event |
| Surrender paid | Yes | Policy closed, cash paid |

## 4. Recommended transaction types

| Transaction type | Use |
|---|---|
| INS_PREMIUM_PAYMENT | Regular/single premium paid |
| INS_TOP_UP_PREMIUM | Additional premium/top-up |
| INS_PREMIUM_REFUND | Refunded premium/cancellation refund |
| INS_PREMIUM_ALLOCATION | Premium allocated to policy/account/sub-funds |
| INS_POLICY_CHARGE | Admin/policy charge deducted |
| INS_INSURANCE_CHARGE | Cost of insurance / mortality charge |
| INS_RIDER_CHARGE | Rider premium/charge |
| INS_FUND_MANAGEMENT_CHARGE | Explicit fund charge if not NAV-level |
| INS_SURRENDER_CHARGE | Early termination/withdrawal charge |
| INS_BONUS_DECLARED | Bonus/dividend declared |
| INS_BONUS_CREDITED | Bonus/dividend credited to value |
| INS_POLICY_LOAN_DRAWDOWN | Loan drawn against policy |
| INS_POLICY_LOAN_REPAYMENT | Policy loan repayment |
| INS_POLICY_LOAN_INTEREST | Policy loan interest accrual/charge |
| INS_PARTIAL_WITHDRAWAL | Partial withdrawal/encashment |
| INS_FULL_SURRENDER | Full surrender and closure |
| INS_MATURITY_PROCEEDS | Maturity benefit received |
| INS_DEATH_CLAIM_PROCEEDS | Death claim proceeds |
| INS_CI_CLAIM_PROCEEDS | Critical illness claim proceeds |
| INS_DISABILITY_BENEFIT | Disability benefit payment |
| INS_ANNUITY_INCOME_PAYMENT | Periodic annuity income |
| INS_FUND_SWITCH_OUT | Units sold from one ILP/VA fund |
| INS_FUND_SWITCH_IN | Units bought into another ILP/VA fund |
| INS_ASSIGNMENT | Policy assigned to lender/trust/collateral holder |
| INS_RELEASE_ASSIGNMENT | Assignment released |
| INS_CORRECTION_REVERSAL | Reversal/correction |

## 5. Policy position model

A policy position is not just quantity × price.

| Field | Meaning |
|---|---|
| policy_id | Unique policy contract identifier |
| account_id | Client portfolio/account where policy is tracked |
| insurer_id | Issuer/insurer |
| policy_type | Term, whole life, endowment, ILP, annuity, UL, etc. |
| policy_status | Active, lapsed, surrendered, matured, claim pending, etc. |
| owner_party_id | Policy owner |
| insured_party_id | Life assured/insured |
| beneficiary_summary | Beneficiary information if permitted for reporting |
| issue_date | Policy issue date |
| maturity_date | Maturity/end date if applicable |
| premium_amount | Premium amount |
| premium_frequency | Monthly, quarterly, annual, single |
| next_premium_due_date | Next expected premium |
| premium_status | Paid/current/overdue/waived |
| sum_assured | Base coverage amount |
| death_benefit | Current death benefit estimate |
| cash_value | Accumulated internal value |
| account_value | Investment-linked account value |
| surrender_value | Cash value accessible on surrender |
| guaranteed_value | Guaranteed value where applicable |
| non_guaranteed_value | Bonuses/projected/discretionary component |
| outstanding_policy_loan | Loan balance |
| net_policy_value | Surrender/account value less loans where appropriate |
| valuation_currency | Policy currency |
| reporting_value_type | Account value / surrender value / benefit value |

## 6. ILP / variable sub-fund position model

| Field | Meaning |
|---|---|
| policy_id | Parent policy |
| sub_fund_id | ILP sub-fund or variable account fund |
| units | Number of units |
| nav_per_unit | Latest NAV/unit price |
| fund_currency | Sub-fund currency |
| market_value | Units × NAV |
| allocation_pct | Weight in policy account value |
| risk_rating | Fund risk classification |
| asset_class | Look-through asset class |
| switch_allowed | Whether fund switching permitted |

## 7. Cashflow classification

| Cashflow | Portfolio performance treatment |
|---|---|
| Premium paid from external bank account | External client outflow, policy asset may increase |
| Premium paid from portfolio cash | Internal allocation from cash to policy |
| Charge deducted inside policy | Product expense, reduces value |
| Fund switch | Internal transaction, no external cashflow |
| Partial withdrawal into portfolio cash | Internal disposal/withdrawal from policy asset |
| Surrender proceeds into portfolio | Disposal of policy asset, cash received |
| Death claim into portfolio | Insurance proceeds; classify separately from market performance |
| Annuity income into portfolio | Income/cashflow; may need split between return of capital and income |
| Policy loan drawdown | Liability/cash inflow, not investment gain |
| Loan repayment | Liability reduction, cash outflow |

## 8. Transaction group examples

### Example A — ILP premium allocation

Client pays SGD 10,000 annual premium. Allocation charge is 5%. SGD 9,500 is invested in two sub-funds.

| Transaction | Amount / units | Effect |
|---|---:|---|
| INS_PREMIUM_PAYMENT | -10,000 cash | Client cash out |
| INS_POLICY_CHARGE | 500 | Charge recognized |
| INS_PREMIUM_ALLOCATION | 9,500 | Policy investment account funded |
| INS_FUND_SWITCH_IN / allocation | Fund A 60%, Fund B 40% | Sub-fund units created |

### Example B — Full surrender

Account value is SGD 120,000, surrender charge is SGD 4,000, policy loan is SGD 10,000.

```text
Cash surrender proceeds = 120,000 - 4,000 - 10,000 = SGD 106,000
```

| Transaction | Amount | Effect |
|---|---:|---|
| INS_FULL_SURRENDER | 120,000 gross | Close policy value |
| INS_SURRENDER_CHARGE | -4,000 | Charge |
| INS_POLICY_LOAN_REPAYMENT | -10,000 | Loan settled from proceeds |
| Cash receipt | +106,000 | Net cash to client/portfolio |

### Example C — Annuity income payment

| Transaction | Amount | Effect |
|---|---:|---|
| INS_ANNUITY_INCOME_PAYMENT | +3,000 | Cash income received |
| Tax withholding, if any | -X | Net cash adjusted |
| Remaining benefit base update | Derived | Reporting only if applicable |

## 9. Product status controls

Important consistency rules:

- A lapsed policy should not show active coverage unless in grace period.
- A surrendered policy should have zero active coverage and no future premium schedule.
- A matured endowment should have maturity proceeds booked and policy closed.
- A death claim should close the policy after payment unless there are residual benefits.
- A policy with outstanding loan should show net value and gross value separately.
- An ILP policy value should reconcile to sub-fund holdings less policy-level adjustments.

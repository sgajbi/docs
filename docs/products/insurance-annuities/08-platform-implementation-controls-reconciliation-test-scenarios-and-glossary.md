# 08 — Platform Implementation, Controls, Reconciliation, Test Scenarios and Glossary

## 1. Implementation architecture

Recommended bounded contexts:

| Context | Responsibility |
|---|---|
| Product master | Insurance product definitions, charges, riders, value types |
| Policy administration integration | Policy status, roles, values, premiums and claims from insurer/source |
| Transaction ledger | Premiums, charges, withdrawals, claims, annuity payments |
| Valuation service | Account value, surrender value, guaranteed/non-guaranteed values |
| Investment look-through | ILP/variable sub-fund holdings and exposures |
| Advisory/suitability | Needs analysis, affordability, replacement, risk disclosure |
| Reporting | Protection, policy value, income, cashflow and estate reports |
| Reconciliation | Source-to-platform validation |
| Analytics | Liquidity, risk, cashflow projection, MWR/IRR where appropriate |

## 2. Source systems and feeds

| Source | Data |
|---|---|
| Insurer policy administration system | Policy details, status, premiums, values, claims |
| Insurer statements | Periodic cash/account/surrender values |
| Fund price/NAV feed | ILP/variable sub-fund prices |
| Custodian/private bank platform | Cash movements, policy holdings, loans |
| Advisor CRM | Suitability rationale, client needs, beneficiary notes |
| Credit/lending system | Policy assignment, premium financing, collateral value |
| Document store | Policy contract, illustration, PHS, product summary, claim docs |
| Manual operations | Exceptions, corrections, legacy policies |

## 3. Control framework

| Control | Purpose |
|---|---|
| Policy status validation | Active policy must have valid issue/effective date |
| Role validation | Policy must have owner and insured/annuitant where applicable |
| Premium schedule reconciliation | Premium due/paid status must match cash postings |
| Value reconciliation | Account value and surrender value match insurer statement |
| ILP holding reconciliation | Sum of sub-fund values reconciles to account value |
| Charge reconciliation | Charges deducted match insurer transaction feed/statement |
| Claim reconciliation | Claim proceeds match policy closure and cash receipt |
| Surrender reconciliation | Surrender value less charges/loans equals cash proceeds |
| Policy loan reconciliation | Loan balance and interest match statement |
| Beneficiary privacy control | Sensitive beneficiary data shown only in permitted reports |
| Assignment control | Pledged policy cannot be surrendered without release workflow |
| Duplicate policy control | Prevent duplicate policy records from multiple feeds |

## 4. Data quality rules

| Rule | Severity |
|---|---|
| Active policy without owner | Critical |
| Active policy without insured/annuitant | Critical |
| Negative surrender value | Critical unless source explicitly reports negative net value due to loan |
| Surrendered policy with active premium schedule | High |
| Lapsed policy with active coverage status | High |
| Death claim paid but policy remains active | High |
| Account value not equal to sum of ILP fund holdings beyond tolerance | High |
| Guaranteed value greater than death benefit unexpectedly | Medium, product-specific |
| Next premium due date in past but status current | Medium |
| Policy currency missing | High |
| Beneficiary shares not summing to 100% | Medium / product-specific |

## 5. Reconciliation examples

### ILP account value reconciliation

```text
Expected Account Value = Σ (fund units × latest NAV)
Variance = Insurer Account Value - Expected Account Value
```

Investigate if variance exceeds tolerance due to:

- stale NAV;
- pending charges;
- currency conversion;
- unsettled premium allocation;
- fund switch in progress;
- insurer rounding;
- missing sub-fund.

### Surrender value reconciliation

```text
Expected Surrender Value
= Account/Cash Value - Surrender Charge - Policy Loan - Accrued Loan Interest - Other Adjustments
```

### Premium reconciliation

```text
Premium Paid Transactions
vs Premium Schedule
vs Insurer Statement
vs Portfolio Cash Movements
```

## 6. Test scenarios

| Scenario | Expected result |
|---|---|
| Term policy premium paid | Cash outflow recorded; coverage remains active; no investment value created |
| Term policy expires | Policy status expired; no cash proceeds |
| Whole life premium paid | Premium recorded; policy status active; value updated from source |
| Whole life policy loan drawn | Cash inflow; policy loan liability increases; net policy value decreases |
| Policy loan interest accrues | Loan balance increases; net value decreases |
| ILP premium allocated | Premium payment, charge and fund units created correctly |
| ILP fund switch | Old fund units decrease; new fund units increase; no external cashflow |
| ILP partial withdrawal | Units reduced; surrender charge applied if relevant; cash received |
| ILP full surrender | Policy closed; units zero; net cash equals value less charges/loans |
| Participating bonus declared | Bonus recorded as guaranteed/non-guaranteed according to terms |
| Endowment matures | Maturity proceeds booked; policy closed |
| Death claim paid | Claim proceeds booked; coverage/policy closed |
| Annuity annuitized | Account value converted to payout schedule |
| Annuity payment received | Cash income booked; remaining payout metadata updated |
| Policy lapses | Coverage inactive; value adjusted; premium schedule suspended |
| Policy reinstated | Status active; premium arrears/conditions captured |
| Assignment to lender | Policy flagged as pledged; surrender restricted |

## 7. Production triage checklist

When policy value or reporting looks wrong, check:

1. Is the policy status correct?
2. Are we using the right value basis: account value, cash value, surrender value or death benefit?
3. Is the valuation date stale?
4. Are policy loans deducted?
5. Are surrender charges deducted?
6. Are all ILP sub-funds loaded with latest NAV?
7. Are charges deducted at policy level or NAV level?
8. Is premium allocation pending?
9. Has there been a fund switch, claim or surrender not processed?
10. Is the policy assigned/pledged?
11. Is currency conversion correct?
12. Are guaranteed and non-guaranteed values separated?

## 8. Glossary

| Term | Meaning |
|---|---|
| Account value | Value of investment account/sub-funds inside policy |
| Annuitant | Person whose lifetime determines annuity payments |
| Annuitization | Conversion of account value into income stream |
| Beneficiary | Person/entity receiving policy benefit |
| Cash value | Accumulated value in permanent insurance |
| Cash surrender value | Net amount payable if policy is surrendered |
| Cost of insurance | Charge for protection cover |
| Death benefit | Amount payable on death |
| Endowment | Policy paying maturity benefit plus protection |
| Free-look period | Cooling-off period after purchase where cancellation may be allowed |
| Grace period | Time after premium due date before lapse |
| ILP | Investment-linked policy |
| Lapse | Policy termination/inactivity due to non-payment or insufficient value |
| Life assured | Person whose life is insured |
| Maturity benefit | Amount payable at maturity |
| Participating policy | Policy sharing in participating fund surplus via bonuses/dividends |
| Policy loan | Loan against policy value |
| Premium | Payment to maintain/purchase policy |
| Rider | Add-on coverage |
| Sum assured | Base coverage amount |
| Surrender charge | Charge for early withdrawal/termination |
| Universal life | Flexible premium permanent life policy |
| Variable annuity | Annuity with investment-linked sub-accounts |
| Waiver of premium | Rider waiving premiums after qualifying event |

## 9. Engineering design principles

- Use decimal arithmetic for all money, units, rates and percentages.
- Store value type explicitly; never infer surrender value from account value without rule/source.
- Keep policy lifecycle events separate from financial transactions.
- Make reporting value basis configurable and visible.
- Preserve original insurer values and derived platform values separately.
- Do not show death benefit as investable market value.
- Keep beneficiary and health/claim data under stricter privacy controls.
- Support manual overrides with audit trail because insurance data often arrives from statements.
- Include source timestamp and confidence level for every policy value.
- Build test scenarios around lifecycle edge cases, not only happy-path premiums.

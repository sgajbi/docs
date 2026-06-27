# 09 - Practitioner Reference, Review Questions and Product Combinations

## 1. Practitioner-ready explanation

"Insurance and annuity products are contractual products issued by insurers. They can provide protection, savings, investment-linked growth, retirement income or estate-planning benefits. Unlike a normal security, a policy has roles such as owner, insured, annuitant and beneficiary, and multiple values such as sum assured, account value, cash value, surrender value, death benefit and guaranteed or non-guaranteed benefits. From a wealth-platform perspective, I would model the policy contract separately from investment-linked fund holdings and cash transactions. Premiums, charges, policy loans, claims, surrenders, maturity proceeds and annuity payments should be proper transaction types, while underwriting, lapse, reinstatement, beneficiary changes and claim submission should be lifecycle events. For reporting, I would clearly separate investable value from protection benefit and surrender value."

## 2. Simple mental models

| Product | Mental model |
|---|---|
| Term life | Rent protection for a period |
| Whole life | Lifetime protection plus cash value |
| Endowment | Scheduled savings plus protection |
| Participating policy | Policy with guaranteed values plus discretionary bonuses |
| ILP | Insurance wrapper around investment funds plus charges and cover |
| Universal life | Flexible permanent policy with account value and insurance charges |
| Annuity | Convert capital into future income |
| Variable annuity | Annuity wrapper with investment sub-accounts and optional guarantees |
| Rider | Add-on protection feature |
| Premium-financed policy | Insurance policy plus loan leverage |

## 3. Product combinations

| Product | Combined with |
|---|---|
| Term life | Mortality protection only |
| Whole life | Mortality protection + cash value + possible bonuses |
| Endowment | Savings reserve + mortality protection + maturity benefit |
| Participating whole life | Guaranteed policy values + participating fund surplus sharing |
| Universal life | Flexible premium + account value + cost of insurance + death benefit |
| ILP | Policy wrapper + insurance cover + fund units + policy charges |
| Variable annuity | Insurance contract + sub-accounts + annuity payout options + riders |
| Fixed annuity | Insurer general account guarantee + income schedule |
| Indexed annuity | Annuity + index crediting formula + caps/floors/participation |
| Premium-financed UL | Universal life + bank loan + collateral assignment + rate risk |

## 4. Common comparison table

| Product | Cash value? | Investment risk? | Main benefit | Main risk |
|---|---|---|---|---|
| Term life | No | No | Low-cost death protection | Coverage expires/no value if no claim |
| Whole life | Yes | Limited/direct depends type | Lifetime cover + value | Higher premium, surrender risk |
| Endowment | Yes | Usually insurer/participating risk | Maturity value | Low liquidity/early surrender loss |
| ILP | Yes/account value | Yes | Fund exposure + protection | Market risk, charges, lapse |
| Universal life | Yes | Depends structure | Flexible estate/protection | Lapse, rate, charges, premium sufficiency |
| Fixed annuity | Usually yes before payout | Low/direct no market risk | Predictable income | Inflation, liquidity, insurer credit |
| Variable annuity | Yes/account value | Yes | Growth + income options | Fees, market risk, surrender |

## 5. Common client misconceptions

| Misconception | Correction |
|---|---|
| "Death benefit is my portfolio value" | Death benefit is protection benefit, not current liquid wealth |
| "Projected value is guaranteed" | Only explicitly guaranteed values are guaranteed |
| "ILP is just a mutual fund" | ILP is an insurance contract with fund units, charges and cover |
| "Cash value equals surrender value" | Surrender value may be lower after charges/loans |
| "Policy loan is free money" | It accrues interest and can reduce benefits or cause lapse |
| "Annuity guarantee means no risk" | Insurer credit, inflation and liquidity risks remain |
| "Surrender is simple" | Surrender may lose benefits, trigger charges and close protection |
| "Premium financing improves return automatically" | Leverage introduces interest-rate and collateral risk |

## 6. Platform practitioner checklist

Before implementing insurance support, answer:

1. Will policies be included in total wealth, investable assets, or only protection reporting?
2. Which value basis is shown by default?
3. Are death benefits excluded from portfolio market value?
4. Are guaranteed and non-guaranteed values separated?
5. Are ILP sub-funds modelled for look-through exposure?
6. Are policy loans modelled as liabilities?
7. Are premium obligations included in cashflow projections?
8. Are annuity payouts included in retirement income projections?
9. Are beneficiary and health/claim data protected?
10. Are surrender restrictions and assignments visible?
11. Are policy values sourced, dated and confidence-labelled?
12. Are claims, maturity and surrender lifecycle events reconciled to cash movements?

## 7. Advisory conversation structure

A good advisor explains:

- what the policy is for;
- what benefit is guaranteed and what is not;
- what premiums are required;
- what happens if premiums stop;
- what the client can access before maturity/death;
- what surrender charges apply;
- what investment risk applies for ILP/variable products;
- what fees/charges apply;
- who receives benefits;
- how the product fits the client's overall portfolio and estate plan.

## 8. Mandate and analytics practitioner view

Insurance products influence portfolios even when outside the DPM mandate:

| Impact | Example |
|---|---|
| Liquidity | Future premiums require cash reserve |
| Risk exposure | ILP sub-funds add equity/bond exposure |
| Credit exposure | Client exposed to insurer guarantees |
| Leverage | Premium financing creates loan exposure |
| Estate planning | Death benefit changes wealth transfer plan |
| Reporting | Protection summary differs from portfolio performance |
| Suitability | Illiquid policy may conflict with short-term objectives |
| Buying power | Assigned cash-value policy may serve as collateral |

## 9. Review questions and answers

### Q1. How is an ILP different from a mutual fund?

An ILP is an insurance contract with investment-linked sub-funds. The client has policy rights, protection coverage, charges, surrender terms and beneficiary rules. A mutual fund is a pooled investment vehicle. ILP reporting should show both policy-level values and sub-fund holdings where available.

### Q2. What is the difference between cash value and surrender value?

Cash value is the accumulated value inside a permanent policy. Surrender value is the net amount the client receives if the policy is terminated, after surrender charges, loans, accrued interest and other adjustments.

### Q3. How should death benefit be reported?

Death benefit should be reported in protection or estate-planning sections, not as current investable market value. Net worth reporting usually uses surrender value or net policy value if the policy is treated as an asset.

### Q4. How are premiums treated in performance?

Premiums are client contributions to a policy, not investment gains. For ILPs, the investment performance comes from sub-fund NAV changes net of charges. For protection policies, premium buys risk cover and should not be judged only by investment return.

### Q5. What makes annuities difficult for platforms?

Annuities may have account value, surrender value, guaranteed income base, death benefit base and payout schedules that are different from each other. After annuitization, the product may become an income stream rather than a liquid asset.

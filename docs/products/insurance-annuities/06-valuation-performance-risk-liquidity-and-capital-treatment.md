# 06 - Valuation, Performance, Risk, Liquidity and Capital Treatment

## 1. Valuation concepts

Insurance valuation is product-specific. A single market value field is not enough.

| Value | Meaning | Typical source |
|---|---|---|
| Account value | Value of investment account/sub-funds | Insurer statement / NAV x units |
| Cash value | Internal accumulated value | Insurer statement |
| Cash surrender value | Net amount available if surrendered | Insurer statement / surrender schedule |
| Guaranteed value | Contractually guaranteed under stated conditions | Policy illustration/statement |
| Non-guaranteed value | Bonus/dividend/projection value | Insurer statement/illustration |
| Death benefit | Payable on death | Policy contract/statement |
| Maturity benefit | Payable at maturity | Policy contract/statement |
| Policy loan balance | Outstanding loan plus accrued interest | Insurer statement |
| Benefit base | Calculation base for riders/annuity guarantees | Insurer statement |

## 2. Cash surrender value formula

A practical platform formula:

```text
Cash Surrender Value
= Account Value or Cash Value
+ Vested Bonuses / Credited Benefits
- Surrender Charges
- Outstanding Policy Loans
- Accrued Loan Interest
- Unpaid Charges / Tax / Adjustments
```

Not all components are available for every policy.

## 3. ILP / variable policy valuation

```text
Policy Account Value = sum (Sub-Fund Units x Sub-Fund NAV)
Net Policy Value = Policy Account Value - Policy Charges - Surrender Charges - Policy Loans
```

Important distinctions:

- fund NAV may already include fund management fees;
- policy-level charges may be deducted by unit cancellation;
- surrender value may be lower than account value;
- guaranteed death benefit may be higher than account value;
- policy value may lag if insurer statements are periodic.

## 4. Participating policy valuation

Participating policies may include:

| Component | Treatment |
|---|---|
| Guaranteed cash value | Contractual value |
| Reversionary bonus | Once declared, may become vested depending terms |
| Terminal bonus | Usually non-guaranteed and payable on claim/maturity/surrender under conditions |
| Policy loan | Reduces net value and/or death benefit |
| Surrender adjustment | Reduces accessible value |

For reporting, split guaranteed and non-guaranteed values.

## 5. Whole life / endowment valuation

| Value basis | Use case |
|---|---|
| Cash surrender value | Liquidity and balance sheet reporting |
| Guaranteed value | Conservative planning |
| Death benefit | Protection and estate planning |
| Projected maturity value | Planning illustration, not actual valuation |
| Premium paid | Client contribution history |

A platform should not report projected maturity value as current wealth.

## 6. Annuity valuation

Before annuitization:

- account value;
- surrender value;
- guaranteed minimum accumulation value;
- death benefit base;
- income benefit base.

After annuitization:

- income payment amount;
- guaranteed remaining payments;
- survivor benefit;
- present value estimate if required for planning;
- surrender value if contract allows commutation.

Important: many annuitized contracts are not fully liquid. Reporting a theoretical present value without explaining liquidity can mislead clients.

## 7. Performance measurement

Insurance products are difficult because premiums buy both protection and investment value.

### Useful performance measures

| Measure | Use |
|---|---|
| Premium-to-surrender-value IRR | Economic return if surrendered today |
| Premium-to-account-value MWR | ILP/variable investment performance |
| Investment sub-fund return | Fund performance inside policy |
| Net policy return | After policy charges and insurance cost |
| Protection efficiency | Coverage per premium, not investment performance |
| Income yield | Annuity payment / purchase value or policy value |
| Benefit realization | Claims/maturity vs premiums paid |

### Warning

For term life and protection riders, a low or zero investment return does not mean the product failed. The product purchased risk protection.

## 8. Performance classification by event

| Event | Performance classification |
|---|---|
| Premium paid | Contribution/premium; not investment gain |
| Allocation charge | Product expense |
| Cost of insurance | Protection cost |
| Fund NAV change | Investment performance |
| Bonus credited | Policy return/insurer crediting result |
| Surrender charge | Product/exit cost |
| Death claim | Insurance benefit, separate from market performance |
| Annuity payment | Income/return of capital mix depending policy |
| Policy loan | Financing/liability, not income |

## 9. Risk taxonomy

| Risk | Description |
|---|---|
| Mortality risk | Death timing affects benefits and insurer pricing |
| Morbidity risk | Illness/disability claim risk |
| Longevity risk | Client may outlive assets; annuities mitigate this |
| Market risk | ILP/variable account value falls |
| Interest-rate risk | Affects insurer guarantees, credited rates, annuity pricing |
| Insurer credit risk | Benefits depend on insurer ability to pay |
| Liquidity risk | Surrender restrictions/charges reduce access |
| Lapse risk | Policy terminates due to non-payment/insufficient value |
| Fee drag | Charges reduce account growth |
| Inflation risk | Fixed benefits/payouts lose purchasing power |
| Currency risk | Policy currency differs from client base currency |
| Complexity risk | Client misunderstands guarantees, projections or benefit bases |
| Financing risk | Premium financing costs rise or collateral falls |
| Tax/legal risk | Treatment depends on jurisdiction and ownership structure |

## 10. Liquidity treatment

Classify liquidity conservatively.

| Product | Liquidity treatment |
|---|---|
| Term life | No investment liquidity |
| Whole life | Surrender/loan value may be available, often poor early |
| Endowment | Limited liquidity before maturity |
| ILP | Partial withdrawal may be available but charges may apply |
| Universal life | Policy loan/withdrawal may be available, but lapse risk |
| Immediate annuity | Often illiquid after purchase |
| Variable annuity | Withdrawals may be possible with charges/restrictions |
| Premium-financed policy | Liquidity tied to loan/collateral requirements |

## 11. Capital and balance sheet treatment in wealth reports

A client balance sheet can show insurance in multiple sections:

| Section | Appropriate value |
|---|---|
| Investable assets | Usually exclude pure protection; include ILP/fund account value if mandate allows |
| Net worth | Use surrender value or net policy value, not death benefit |
| Protection summary | Use sum assured/death benefit |
| Retirement income | Use annuity income stream and/or surrender value if available |
| Estate planning | Show death benefit, beneficiaries, ownership/trust/assignment |
| Collateral | Show eligible value after lender haircut if pledged |

## 12. Buying power and collateral considerations

Insurance policies may be used as collateral, especially cash-value policies or premium-financed universal life. Platform considerations:

- eligibility of policy as collateral;
- insurer rating and jurisdiction;
- assignment status;
- surrender value and haircut;
- outstanding policy loan;
- premium financing loan exposure;
- currency mismatch;
- valuation frequency;
- lapse risk if premiums/loan interest are not serviced.

Collateral value should usually be based on **net surrender value after loans and haircuts**, not death benefit.

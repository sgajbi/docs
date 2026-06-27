# 02 - Life Insurance, ILP and Protection Product Deep Dive

## 1. Term life insurance

Term life insurance provides death protection for a defined period. It usually has no investment account and no cash value.

| Feature | Typical treatment |
|---|---|
| Premium | Regular fixed or renewable premium |
| Cash value | Usually none |
| Benefit | Death benefit if insured dies during term |
| Maturity value | Usually none |
| Main use | Protection gap, mortgage/family protection |
| Analytics value | Not normally included as investable wealth |
| Reporting value | Coverage amount and premium obligation |

### Lifecycle

| Event | Transaction / event type | Platform treatment |
|---|---|---|
| Policy issue | POLICY_ISSUE | Create policy record and coverage |
| Premium payment | INS_PREMIUM_PAYMENT | Cash outflow, no investment position increase |
| Missed premium | POLICY_PREMIUM_MISSED | Grace period tracking |
| Lapse | POLICY_LAPSE | Coverage inactive |
| Reinstatement | POLICY_REINSTATEMENT | Coverage reactivated if accepted |
| Death claim | INS_DEATH_CLAIM_PROCEEDS | Benefit payment; close policy |
| Expiry | POLICY_EXPIRY | Coverage ends |

### Advisory considerations

- Does the client have dependents?
- Does coverage match liabilities and income replacement needs?
- Is the premium affordable across the term?
- Is the policy renewable or convertible?
- Are exclusions understood?

## 2. Whole life insurance

Whole life insurance provides lifetime protection and generally builds cash value. Participating whole life policies may receive bonuses or dividends based on the insurer's participating fund experience.

| Feature | Typical treatment |
|---|---|
| Premium | Regular, limited-pay, or single premium |
| Protection | Lifetime death benefit |
| Cash value | Builds over time |
| Bonus/dividend | Guaranteed or non-guaranteed depending on terms |
| Surrender | Possible, but may be poor early in policy life |
| Policy loan | Often available against cash value |

### Components

```text
Whole Life Policy = Death Benefit + Cash Value + Optional Bonuses + Riders - Charges/Loans
```

### Transaction types

| Event | Transaction type | Effect |
|---|---|---|
| Premium paid | INS_PREMIUM_PAYMENT | Cash out; policy value/coverage maintained |
| Bonus declared | INS_BONUS_DECLARED | Increase guaranteed or non-guaranteed benefit/value depending terms |
| Policy loan drawdown | INS_POLICY_LOAN_DRAWDOWN | Cash in; policy loan liability increases |
| Policy loan interest | INS_POLICY_LOAN_INTEREST_ACCRUAL | Loan balance increases |
| Loan repayment | INS_POLICY_LOAN_REPAYMENT | Cash out; loan balance decreases |
| Partial withdrawal | INS_PARTIAL_WITHDRAWAL | Cash in; policy value/benefit reduced |
| Surrender | INS_POLICY_SURRENDER | Cash in; close policy |
| Death claim | INS_DEATH_CLAIM_PROCEEDS | Claim proceeds paid; close policy |

## 3. Endowment policy

An endowment policy combines life protection with a savings objective and pays a maturity value if the insured survives to maturity.

| Feature | Meaning |
|---|---|
| Maturity date | Date when maturity benefit is payable |
| Guaranteed maturity benefit | Contractual minimum if conditions met |
| Bonuses/dividends | May increase maturity/death benefit |
| Premium term | Often fixed |
| Use case | Education, retirement supplement, disciplined savings |

### Platform treatment

Endowment policies should be reported as long-term savings/protection contracts, not as ordinary bond holdings. Expected maturity cashflow may be useful for planning, but the current value is usually surrender value or insurer-provided policy value.

## 4. Universal life and variable universal life

Universal life offers flexible premiums and death benefits, with an internal account value. HNW universal life may be used for estate liquidity, wealth transfer and premium financing.

| Feature | Universal life | Variable universal life / ILP-style |
|---|---|---|
| Premium flexibility | High | High |
| Account value | Interest-crediting or general account | Linked to selected funds/sub-accounts |
| Investment risk | Usually insurer/general account formula | Policyholder bears fund risk |
| Charges | Cost of insurance, admin, surrender | Insurance charges, fund charges, admin, allocation, surrender |
| Death benefit | Level or increasing | Often level/increasing with account value component |

### Risk point

If charges and insurance costs exceed premium payments and account growth, the policy can lapse. This is especially important for universal life, VUL and ILP products.

## 5. Investment-linked policy, ILP

An ILP combines insurance protection with investment-linked fund units. Premiums are allocated partly or fully into sub-funds after applicable charges. The policy value depends on the value of units held in selected funds.

### Common ILP components

| Component | Meaning |
|---|---|
| Policy wrapper | Insurance contract |
| Sum assured | Protection component |
| ILP sub-funds | Investment choices under policy |
| Units | Fund units held inside policy |
| Bid/offer spread | Difference between buying/selling unit price, if applicable |
| Insurance charge | Charge for protection cover |
| Policy/admin charge | Ongoing policy cost |
| Fund management fee | Underlying fund cost |
| Surrender charge | Early withdrawal/termination cost |
| Fund switch | Change investment allocation inside policy |

### ILP lifecycle

| Event | Transaction / event type | Effect |
|---|---|---|
| Policy issue | POLICY_ISSUE | Create policy, coverage and investment account |
| Regular premium | INS_PREMIUM_PAYMENT | Cash out from client; premium received by insurer |
| Premium allocation | ILP_PREMIUM_ALLOCATION | Units bought in sub-funds after charges |
| Top-up premium | ILP_TOP_UP | Additional units/coverage depending terms |
| Insurance charge | ILP_INSURANCE_CHARGE | Units/cash deducted |
| Admin charge | ILP_ADMIN_CHARGE | Units/cash deducted |
| Fund management fee | Usually NAV-level | Reflected in sub-fund NAV unless explicit |
| Fund switch | ILP_FUND_SWITCH_OUT / ILP_FUND_SWITCH_IN | Units moved between sub-funds |
| Partial withdrawal | ILP_PARTIAL_WITHDRAWAL | Units redeemed; cash paid |
| Surrender | ILP_FULL_SURRENDER | Units redeemed, charges applied, policy closed |
| Death claim | ILP_DEATH_CLAIM | Death benefit paid; policy closed |

### ILP position model

There are two useful layers:

| Layer | Purpose |
|---|---|
| Policy-level position | Shows overall policy value, coverage, owner, insured, premium obligation |
| Sub-fund holdings | Shows underlying units and NAV-like investment exposure |

Do not replace the policy with only fund positions. The policy wrapper controls charges, protection, beneficiary rights, surrender terms, exclusions and tax/legal treatment.

## 6. Riders and supplementary benefits

Riders are add-ons to the base policy.

| Rider | Purpose | Modelling treatment |
|---|---|---|
| Critical illness | Pays on diagnosis of specified illness | Coverage component with claim trigger |
| Early critical illness | Earlier-stage CI coverage | Coverage component |
| Disability income | Replaces income on disability | Periodic benefit schedule |
| Waiver of premium | Waives premium on disability/illness | Premium obligation modifier |
| Accidental death | Additional death benefit | Additional coverage |
| Long-term care | Pays if care dependency occurs | Benefit schedule / claim trigger |
| Hospital cash | Pays per hospital day | Claim event and benefit schedule |

Riders may have separate premium, sum assured, expiry, exclusions and claim status.

## 7. Participating versus investment-linked policies

| Dimension | Participating policy | Investment-linked policy |
|---|---|---|
| Investment control | Insurer manages participating fund | Policyholder selects sub-funds |
| Values | Guaranteed + non-guaranteed bonuses | Account value based on units/NAV |
| Risk | Insurer smooths, but bonuses not guaranteed | Policyholder bears fund volatility |
| Transparency | Less granular holdings visibility | More fund-level visibility |
| Charges | Embedded/contractual | More explicit policy/fund charges |
| Reporting | Policy value, bonus, surrender value | Account value, units, fund allocation, surrender value |

## 8. Protection product reporting

For client/advisor reporting, show:

- policy type;
- insurer;
- policy owner;
- life assured;
- beneficiaries if reportable;
- premium amount/frequency;
- next premium due;
- death benefit/sum assured;
- riders and coverage amounts;
- cash value/account value;
- surrender value;
- outstanding policy loan;
- maturity date or expiry date;
- guaranteed versus non-guaranteed values;
- advisory notes on liquidity and early termination.

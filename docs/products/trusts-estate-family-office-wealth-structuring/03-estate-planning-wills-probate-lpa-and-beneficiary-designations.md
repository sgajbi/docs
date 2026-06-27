    # Estate Planning, Wills, Probate, LPA and Beneficiary Designations

    **Purpose:** professional knowledge-base material for wealth management, private banking, advisory, DPM/mandates, portfolio analytics, reporting, implementation, QA, operations and office knowledge sharing.  
    **Scope:** educational and platform-design reference only. It is not legal, tax, fiduciary or estate-planning advice. Actual client structures must be validated by qualified legal, tax, trust, compliance and jurisdiction specialists.

    ## 1. What is estate planning?

Estate planning defines how assets should be managed, transferred and controlled during life, incapacity and after death. It may involve wills, trusts, beneficiary nominations, insurance policies, joint ownership, business succession plans, powers of attorney, LPAs and family governance documents.

MoneySense positions estate planning as deciding how to transfer your estate and also highlights related topics such as wills, intestacy and CPF nomination. In Singapore, estate duty has been removed for deaths on and after 15 February 2008, according to IRAS; however, income tax, foreign estate/tax issues and administrative obligations may still matter.

## 2. Core estate planning instruments

| Instrument | Purpose | Platform relevance |
|---|---|---|
| Will | Directs distribution after death | Executor, probate, estate account |
| Trust | Holds assets for beneficiaries | Trustee account and beneficiary logic |
| LPA | Allows donee to act on incapacity | Authority management before death |
| Beneficiary nomination | Directs insurance/CPF/policy proceeds | May override estate distribution mechanics |
| Joint account | Shared ownership | Survivorship and authority rules |
| Insurance policy | Liquidity/protection/estate equalisation | Beneficiaries, claims, surrender value |
| Business succession plan | Transfer/control of operating business | Private company and family office workflows |
| Letter of wishes | Non-binding guidance to trustee | Document reference, not transaction rule unless formalised |

## 3. Will and probate lifecycle

| Stage | Description | Platform impact |
|---|---|---|
| Will created | Client names executor and beneficiaries | Document reference, estate planning advisory note |
| Client death notification | Bank receives death notice | Freeze/restrict relevant accounts as per policy |
| Probate/administration | Court/legal authority confirms executor/administrator | Authority records updated |
| Estate account opened | Estate assets administered | Estate entity/account created |
| Asset valuation | Date-of-death valuation prepared | Historical valuation and tax/admin reporting |
| Debts/expenses paid | Liabilities settled | Estate payment transactions |
| Distribution | Assets/cash distributed to beneficiaries | Transfer/distribution transactions |
| Estate closed | Administration completed | Close account/entity |

## 4. Estate account transaction types

| Transaction type | Description |
|---|---|
| ESTATE_ASSET_INVENTORY | Opening inventory of estate assets |
| ESTATE_DATE_OF_DEATH_VALUATION | Valuation snapshot event |
| ESTATE_CASH_RECEIPT | Cash received by estate |
| ESTATE_INCOME_RECEIPT | Dividends/coupons/rent received during administration |
| ESTATE_EXPENSE_PAYMENT | Legal/admin/probate expenses |
| ESTATE_TAX_PAYMENT | Tax/filing-related payment |
| ESTATE_DEBT_SETTLEMENT | Settlement of deceased liabilities |
| ESTATE_BENEFICIARY_CASH_DISTRIBUTION | Cash distribution |
| ESTATE_BENEFICIARY_SECURITY_DISTRIBUTION | Security transfer to beneficiary |
| ESTATE_TRANSFER_TO_TRUST | Transfer into testamentary trust |
| ESTATE_CLOSE | Closure event |

## 5. Lasting Power of Attorney / incapacity planning

A Lasting Power of Attorney lets a person appoint one or more trusted persons to act if the person loses mental capacity. MoneySense Singapore explains that LPAs can cover personal welfare and property and affairs, and the donor must be at least 21 and have mental capacity when making the LPA.

For platform design, LPA is an **authority overlay**, not an asset product.

| Field | Example |
|---|---|
| donor_party_id | Client |
| donee_party_id | Appointed person |
| scope | Property and affairs |
| effective_condition | Loss of mental capacity certified |
| effective_date | Date activated |
| restriction | Cannot change beneficiaries / cannot make gifts unless permitted |
| document_reference | LPA document ID |
| status | Draft / registered / activated / revoked / expired |

## 6. Beneficiary designations

Beneficiary designations can apply to insurance policies, retirement accounts, trust arrangements and certain nominations. They may bypass or interact with will/estate processes depending on jurisdiction and product.

Platform implications:

- beneficiary designation should be product/account-specific;
- beneficiaries may have percentage shares, priority order or contingent status;
- designations need effective dates and revocation history;
- a beneficiary may be different from portfolio reporting users;
- death claim processing should use product-specific beneficiary rules.

## 7. Joint ownership and survivorship

Joint ownership rules vary by jurisdiction and account documentation. From a platform perspective, do not infer survivorship purely from the presence of two owners.

| Joint account model | Platform concern |
|---|---|
| Joint tenancy / survivorship | Ownership may pass automatically to surviving owner |
| Tenancy in common | Deceased share may pass through estate |
| Either-to-sign | Instruction authority differs from ownership |
| Both-to-sign | Joint authority required |
| Convenience signer | Signer may not be beneficial owner |

## 8. Death and incapacity controls

| Control | Why it matters |
|---|---|
| Death notification workflow | Prevent unauthorized transactions |
| Account freeze/restriction | Protect estate pending legal authority |
| Pending trade review | Confirm settlement and cancellation policies |
| Corporate action handling | Estate may still receive dividends/rights |
| Loan/collateral review | Death may trigger credit review |
| DPM mandate review | Authority to continue management may change |
| Suitability refresh | Beneficiary or estate may have different profile |
| Reporting lock-down | Privacy and legal entitlement changes |

## 9. Estate planning analytics

Estate planning is not only legal documentation. Wealth platforms can provide analytics:

| Analytics | Description |
|---|---|
| Estate liquidity analysis | Cash available for expenses, taxes, equalisation and debts |
| Beneficiary allocation | Expected distribution by beneficiary/family branch |
| Illiquid asset concentration | Private equity, property, business ownership exposure |
| Insurance sufficiency | Coverage versus liquidity need |
| Debt and collateral stress | What happens to loans on death/incapacity |
| Cross-border exposure | Assets held across jurisdictions/currencies |
| Succession readiness score | Documents, nominations, access, reporting completeness |

## 10. Reporting outputs

| Report | User |
|---|---|
| Estate inventory report | Executor, legal advisor |
| Date-of-death valuation report | Estate administration/tax/legal |
| Beneficiary distribution report | Executor/trustee/beneficiaries |
| Estate cashflow report | Executor/family office |
| Succession readiness report | Advisor/family office |
| Estate liquidity report | Advisor/client/family office |

## 11. Common errors

| Error | Impact |
|---|---|
| Ignoring pending trades after death | Incorrect estate holdings |
| Using current market value instead of date-of-death value | Wrong estate report |
| Not separating pre-death and post-death income | Tax/admin issues |
| Treating nominee/beneficiary as account owner before claim completed | Legal/control issue |
| Allowing former POA after death | Authority issue; POA usually ceases on death |
| Consolidating estate assets into beneficiary wealth too early | Double-counting |

    # Wealth Structuring Fundamentals and Taxonomy

    **Purpose:** professional knowledge-base material for wealth management, private banking, advisory, DPM/mandates, portfolio analytics, reporting, implementation, QA, operations and office knowledge sharing.  
    **Scope:** educational and platform-design reference only. It is not legal, tax, fiduciary or estate-planning advice. Actual client structures must be validated by qualified legal, tax, trust, compliance and jurisdiction specialists.

    ## 1. What is wealth structuring?

Wealth structuring is the arrangement of legal ownership, beneficial ownership, governance, control, succession, reporting and asset management around a client’s wealth. It does not necessarily change the economic asset itself, but it changes how that asset is owned, controlled, distributed, reported and governed.

A simple client relationship may look like this:

```text
Individual client -> Account -> Portfolio -> Product holdings
```

A HNW/UHNW relationship often looks more like this:

```text
Family group
  ├── Individual family members
  ├── Trusts
  ├── Holding companies
  ├── Family office
  ├── Insurance policies
  ├── Private investment companies
  ├── VCC / fund vehicles
  ├── Estate accounts
  └── Multiple bank/custody/loan/advisory/DPM accounts
```

The platform must distinguish **relationship view** from **legal ownership view** and **portfolio analytics view**.

## 2. Why clients use wealth structures

| Objective | Structuring relevance |
|---|---|
| Succession planning | Transfer wealth across generations in a controlled manner |
| Estate planning | Avoid uncertainty and operational disruption after death |
| Asset protection | Separate personal assets from business/family risks, subject to law |
| Governance | Define who can decide, approve, instruct and benefit |
| Privacy | Use professional structures and consolidated reporting |
| Tax planning | Coordinate across jurisdictions; requires professional advice |
| Family continuity | Support family constitutions, investment committees, education and philanthropy |
| Philanthropy | Charitable trusts, foundations, donor-advised structures |
| Liquidity planning | Match cash needs, capital calls, distributions, insurance premiums and loans |
| Investment management | Centralise investments through family office, holding company or fund vehicle |
| Reporting | Consolidate assets across people, entities and custodians |

## 3. Common structure types

| Structure | Core purpose | Typical owner/controller | Platform implication |
|---|---|---|---|
| Individual account | Direct ownership by person | Client | Standard suitability and reporting |
| Joint account | Multiple legal owners | Joint account holders | Survivorship, authority and allocation rules matter |
| Trust | Trustee owns assets for beneficiaries | Trustee, governed by trust deed | Legal owner differs from beneficiary |
| Foundation | Legal entity with purpose/beneficiaries | Foundation council/board | Similar use to trust in some jurisdictions |
| Estate account | Assets of deceased person pending administration | Executor/administrator | Restricted lifecycle and reporting |
| Holding company | Company owns assets | Directors/shareholders | Corporate KYC, UBO, board authority |
| Private investment company | Family investment vehicle | Directors/shareholders/family office | Consolidated family investment entity |
| Family office | Operating entity serving family wealth | Family/principals/professionals | Often not asset owner itself; may advise/manage/report |
| VCC / fund vehicle | Investment fund structure | Fund manager + shareholders | Fund accounting, sub-funds, NAV, investor registry |
| Insurance wrapper | Policyholder owns policy; insurer manages contract | Policyholder/insurer | Surrender value and beneficiaries matter |
| Charitable vehicle | Philanthropy and purpose | Trustees/directors | Mission restrictions and distributions matter |

## 4. Ownership concepts

| Concept | Meaning |
|---|---|
| Legal owner | Person/entity legally holding title to asset/account |
| Beneficial owner | Person who ultimately benefits from the asset or controls ownership economics |
| Settlor | Person who creates a trust and contributes assets |
| Trustee | Person/company that holds and manages trust assets for beneficiaries |
| Beneficiary | Person/entity who may receive income/capital from trust/estate/policy |
| Protector | Person with oversight/veto/appointment powers under some trusts |
| Executor | Person appointed by will to administer estate |
| Administrator | Person appointed where no executor or no valid will |
| Donor | Person who makes LPA/power of attorney |
| Donee / attorney | Person authorised to act under LPA/power of attorney |
| Director | Person controlling a company’s decisions |
| Shareholder | Owner of shares in a company |
| UBO | Ultimate beneficial owner for AML/KYC purposes |

## 5. Wealth-structure risk categories

| Risk | Example |
|---|---|
| Legal authority risk | Wrong party allowed to instruct on account |
| Beneficiary mismatch risk | Reporting or distribution sent to wrong beneficiary |
| Fiduciary risk | Trustee action not aligned with trust deed or beneficiary interests |
| Tax classification risk | Incorrect tax status, residency or reporting flag |
| AML/KYC risk | UBO/control person not identified or refreshed |
| Suitability risk | Product suitable for structure but not beneficiaries, or vice versa |
| Mandate breach | DPM mandate ignores trust deed or board restrictions |
| Liquidity risk | Trust cannot meet distributions, fees, premiums or capital calls |
| Succession risk | Account freezes or authority gaps after death/incapacity |
| Reporting risk | Incorrect consolidation or double-counting across family entities |
| Privacy/access risk | A family member sees data they are not entitled to see |

## 6. Wealth platform implications

A wealth platform must support at least four views:

| View | Question answered |
|---|---|
| Legal account view | Who owns the account and can instruct? |
| Beneficial view | Who benefits economically? |
| Household/family view | What is the family’s consolidated wealth? |
| Mandate/advisory view | What restrictions and suitability rules apply? |

## 7. Analytics implications

Structured ownership changes analytics:

- concentration may be measured per account, legal entity, beneficiary, family group or mandate;
- liquidity may be measured against expected distributions, capital calls, loan interest and tax payments;
- performance may be reported by legal owner, beneficiary, trust, family branch or consolidated household;
- risk may be looked through to underlying holdings, but authority and suitability may be structure-level;
- cost, income and tax reports may need to be split by beneficiary entitlement rules.

## 8. Reference architecture framing

A strong platform should avoid hardcoding hierarchy as account-parent-child only. Use a relationship model:

```text
Entity A --relationship_type--> Entity B
Relationship attributes:
  - role
  - effective date
  - expiry date
  - authority level
  - beneficial percentage
  - income entitlement
  - capital entitlement
  - reporting entitlement
  - instruction entitlement
  - legal basis / document reference
```

This allows the same platform to support trusts, family office groups, holding companies, joint accounts, insurance beneficiaries and estate workflows.

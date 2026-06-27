    # Trusts, Foundations and Fiduciary Structures

    **Purpose:** professional knowledge-base material for wealth management, private banking, advisory, DPM/mandates, portfolio analytics, reporting, implementation, QA, operations and office knowledge sharing.  
    **Scope:** educational and platform-design reference only. It is not legal, tax, fiduciary or estate-planning advice. Actual client structures must be validated by qualified legal, tax, trust, compliance and jurisdiction specialists.

    ## 1. What is a trust?

A trust is a legal arrangement where a settlor transfers assets to a trustee, and the trustee holds or manages those assets for the benefit of beneficiaries or for a defined purpose. In Singapore investor education, MoneySense explains that the trustee takes ownership of assets and manages them in the best interest of the beneficiaries according to trust terms.

## 2. Main trust parties

| Party | Role |
|---|---|
| Settlor | Creates trust and contributes assets |
| Trustee | Legal owner and fiduciary manager of trust assets |
| Beneficiary | Receives income, capital or discretionary benefit |
| Protector | May supervise trustee or approve key actions |
| Investment advisor | May advise trustee/family office on investments |
| Custodian bank | Holds trust assets in custody account |
| Family office | May coordinate reporting, governance and investment oversight |
| Tax/legal advisor | Advises on law, tax and jurisdiction issues |

## 3. Trust deed as the operating contract

For platform purposes, the trust deed is the governing document. It may define:

- trust name and jurisdiction;
- settlor and trustee powers;
- beneficiary classes;
- income and capital distribution rules;
- investment powers and restrictions;
- permitted borrowing, leverage and derivatives;
- whether trustee can appoint investment managers;
- protector consent requirements;
- succession of trustees/protectors;
- duration and termination events;
- reporting and confidentiality rules.

A platform does not need to store the full legal deed in structured fields, but it must capture actionable restrictions and document references.

## 4. Trust types

| Trust type | Explanation | Platform implications |
|---|---|---|
| Revocable trust | Settlor may revoke/amend subject to law | Settlor control may remain high |
| Irrevocable trust | Settlor gives up certain rights | Beneficial/control analysis more complex |
| Discretionary trust | Trustee decides distributions | Beneficiary entitlement may be non-fixed |
| Fixed trust | Entitlements predefined | Income/capital split can be modelled by percentage |
| Purpose trust | Created for specific purpose | No normal beneficiary allocation |
| Charitable trust | Philanthropic purpose | Mission and distribution restrictions |
| Testamentary trust | Created by will after death | Estate-to-trust transition required |
| Special needs trust | Support vulnerable beneficiary | Liquidity and distribution safeguards |
| Insurance trust | Holds life policies | Premium, policy and beneficiary integration |
| PTC structure | Private trust company acts as trustee | Corporate governance and family control overlay |

## 5. Foundation versus trust

A foundation is usually a legal entity with its own legal personality, while a trust is an arrangement where the trustee holds legal ownership. Exact treatment depends on jurisdiction.

| Dimension | Trust | Foundation |
|---|---|---|
| Legal personality | Usually no separate personality; trustee owns assets | Usually separate legal entity |
| Governing document | Trust deed | Charter/bylaws/regulations |
| Controller | Trustee/protector | Council/board/founder rights |
| Beneficiaries | Beneficiaries or purposes | Beneficiaries or purposes |
| Platform treatment | Fiduciary account / trust entity | Legal entity account |

## 6. Fiduciary governance

Trustees generally act under fiduciary duties. Platform controls should support:

- documented authority before accepting instructions;
- verification of trustee/protector powers;
- segregation between settlor wishes and trustee legal authority;
- investment restrictions from trust deed;
- conflict-of-interest controls;
- beneficiary confidentiality restrictions;
- audit trail for distributions and investment decisions;
- periodic review of trust status, UBO/control persons and beneficiaries.

## 7. Trust lifecycle

| Stage | Description | Platform objects |
|---|---|---|
| Establishment | Trust deed executed, trustee appointed | Trust entity, parties, documents |
| Funding | Assets transferred into trust | Transfer transactions, cost basis records |
| Investment setup | Account opened, mandate assigned | Account, portfolio, mandate |
| Ongoing administration | Investment management, fees, distributions | Holdings, transactions, events |
| Beneficiary changes | Birth, death, addition/removal | Relationship updates |
| Trustee/protector change | Appointment/resignation | Authority records, KYC refresh |
| Distribution | Income/capital paid to beneficiary | Distribution transaction |
| Loan/pledge | Trust assets used for credit | Collateral records, authority checks |
| Termination | Assets distributed, trust closed | Closing transactions, status |

## 8. Trust transactions

| Transaction type | Meaning |
|---|---|
| TRUST_FUNDING_CASH | Cash contributed into trust |
| TRUST_FUNDING_SECURITY | Securities transferred into trust |
| TRUST_DISTRIBUTION_INCOME | Income distributed to beneficiary |
| TRUST_DISTRIBUTION_CAPITAL | Capital distributed to beneficiary |
| TRUST_FEE | Trustee/admin/professional fee |
| TRUST_EXPENSE | Legal, audit, tax, family office expense |
| TRUST_TAX_PAYMENT | Tax paid by trustee/trust |
| TRUST_ASSET_TRANSFER_OUT | Asset transferred to beneficiary/another structure |
| TRUST_RESTRUCTURE_TRANSFER | Transfer due to restructure/change of trustee |
| TRUST_TERMINATION_DISTRIBUTION | Final distribution at trust termination |

## 9. Position modelling for trust accounts

The trust account owns product positions like any portfolio, but metadata differs:

| Attribute | Importance |
|---|---|
| Legal owner | Trustee/trust vehicle |
| Beneficial class | Beneficiary or class entitlement |
| Distribution restriction | Whether asset/income may be distributed |
| Investment restriction | Trust deed or mandate constraints |
| Cost basis source | Original settlor basis versus transfer market value |
| Reporting entitlement | Which beneficiary/family branch can view |
| Fiduciary approval flag | Whether transaction requires trustee/protector/committee approval |

## 10. Advisory implications

The advisor should not assume the settlor is the client for suitability purposes. Depending on structure, relevant parties may include trustee, beneficiaries, settlor, protector, investment committee, underlying family and legal entity.

Key questions:

- Who is the legal client of the bank?
- Who can instruct?
- Who has beneficial interest?
- Does the trust deed permit the investment?
- Is leverage or derivatives permitted?
- Are distributions expected soon?
- Are beneficiaries vulnerable, minors or in different tax jurisdictions?
- Does the investment fit the trust’s time horizon and purpose?

## 11. Reporting implications

Trust reporting may require:

- trustee statement;
- beneficiary reporting with restricted visibility;
- consolidated family report;
- income versus capital split;
- realized/unrealized P&L;
- distributions by beneficiary;
- trust fees and expenses;
- portfolio risk and mandate compliance;
- document/audit trail.

## 12. Common implementation mistakes

| Mistake | Better design |
|---|---|
| Treat trust as normal individual account | Model trust parties, roles and authority |
| Store only one beneficiary | Support multiple classes and changing entitlements |
| Ignore income/capital split | Classify distributions and income properly |
| Allow settlor to instruct automatically | Validate legal authority from documents |
| Consolidate all family views without entitlements | Enforce reporting access per role |
| Ignore trust deed restrictions | Capture actionable mandate restrictions |

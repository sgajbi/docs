# 10 - Worked Examples and Implementation Patterns

Use these examples when modelling trusts, estates, foundations, family offices, holding companies, beneficiaries, authority, access control, reporting groups, advisory workflows, QA, and implementation boundaries.

## 1. Relationship Graph Pattern

Wealth structures should be modelled as relationships, not as account labels.

| Node | Example |
|---|---|
| legal entity | trust, foundation, company, estate, VCC, family office |
| person | settlor, beneficiary, protector, director, executor, advisor |
| account/portfolio | custody account, DPM portfolio, advisory portfolio, loan account |
| authority | trustee instruction authority, director authority, executor authority |
| benefit | income beneficiary, capital beneficiary, discretionary class |
| reporting group | family consolidated report, entity report, beneficiary view |
| restriction | distribution rule, investment restriction, privacy limit, tax flag |

Core rule:

```text
legal owner, beneficial owner, controller, reporting recipient and instruction authority can be different parties
```

## 2. Trust Distribution

Scenario:

- Trust holds a portfolio.
- Trustee approves USD 100,000 distribution.
- Beneficiary receives USD 100,000 cash.

Required workflow:

```text
distribution request -> authority check -> trustee approval -> liquidity check -> cash movement -> beneficiary reporting -> audit record
```

Controls:

- beneficiary identity and entitlement must be source-backed;
- trustee authority must be valid on approval date;
- distribution category should be captured if source provides income/capital split;
- tax/legal treatment should not be inferred;
- portfolio performance should classify the distribution according to reporting policy.

## 3. Estate Account After Death Notification

Scenario:

- Individual client dies.
- Account becomes estate-managed pending probate.
- Executor authority is not yet validated.

Expected platform states:

| State | Treatment |
|---|---|
| death notification received | freeze or restrict actions according to policy |
| executor pending validation | no unrestricted instruction authority |
| probate document received | authority review workflow |
| estate account active | reporting and permitted actions follow estate rules |

Do not let previous individual authority continue automatically after death notification if policy requires restriction.

## 4. Holding Company Pledge

Scenario:

- Holding company owns securities portfolio.
- Beneficial owner is an individual family member.
- Holding company pledges assets for a credit facility.

Modelling:

| Dimension | Source-backed fact |
|---|---|
| legal owner | holding company |
| beneficial owner | individual or structure according to KYC/source |
| pledgor | holding company |
| borrower | may be company, individual, trust or related entity |
| collateral pool | pledged company account/portfolio |
| authority | company signatories/directors |

Implementation warning:

Do not calculate individual client buying power from company-owned pledged assets unless cross-pledge and authority rules explicitly support it.

## 5. Beneficiary Reporting Access

Scenario:

- Trust has three beneficiaries.
- Beneficiary A has full report access.
- Beneficiary B has income-only report access.
- Beneficiary C has no direct reporting access.

Access-control model:

| Role | Holdings | Income | Capital distributions | Other beneficiaries | Trustee notes |
|---|---|---|---|---|---|
| Beneficiary A | allowed | allowed | allowed | restricted | restricted |
| Beneficiary B | restricted | allowed | restricted | restricted | restricted |
| Beneficiary C | restricted | restricted | restricted | restricted | restricted |

QA assertion:

Reporting access must follow relationship role and consent/authority rules, not account ownership alone.

## 6. Family Office Consolidated Report

Scenario:

- Family office requests consolidated report across:
  - personal account;
  - trust account;
  - holding company account;
  - foundation account.

Controls:

- every included entity must have reporting authority and consent evidence;
- beneficial ownership and legal ownership should remain visible;
- restricted accounts or restricted beneficiaries must be excluded or masked;
- consolidation currency and valuation date must be explicit;
- inter-entity loans or transfers should not be double counted.

Reporting output:

| Section | Required treatment |
|---|---|
| consolidated allocation | show inclusion perimeter |
| entity breakdown | legal owner and reporting owner |
| performance | avoid combining incompatible mandates without disclosure |
| liabilities/pledges | show entity-specific obligations and cross-pledges |
| restricted data | mask or omit according to access policy |

## 7. Insurance Policy Ownership Role

Scenario:

- Policy owner: trust.
- Life insured: settlor.
- Beneficiaries: trust beneficiaries.
- Premium payer: holding company.

Platform implication:

| Role | Why it matters |
|---|---|
| policy owner | legal control and reporting ownership |
| life insured | claim/lifecycle trigger |
| beneficiary | claim recipient or economic benefit |
| premium payer | cashflow source and funding relationship |
| advisor | access and service relationship |

Do not assume the policy owner, insured life, premium payer and beneficiary are the same party.

## 8. Authority Change

Scenario:

- A family office director resigns.
- New director is appointed.
- Existing trading authority and report access must change.

Workflow:

```text
source document received -> authority effective date -> entitlement update -> access certification -> audit record -> impacted workflow review
```

Controls:

- effective date must be stored;
- pending orders or approvals by old authority should be reviewed;
- report delivery lists must be refreshed;
- access changes must be testable through entitlement evidence.

## 9. Protector Veto On Distribution

Scenario:

- Trustee approves a USD 250,000 capital distribution.
- Trust deed requires protector consent for capital distributions above USD 100,000.
- Protector vetoes the distribution before cash movement.

Workflow:

```text
distribution request -> trustee approval -> protector consent check -> veto recorded -> cash movement blocked -> beneficiary/advisor notice -> audit record
```

Controls:

- consent thresholds must be source-backed from trust deed or legal summary;
- veto authority must be effective on decision date;
- blocked distribution should not reduce portfolio cash or create beneficiary receivable;
- reporting should show request status, not a completed distribution;
- any override or later approval requires new source evidence and approval lineage.

## 10. Directed Trust Investment Authority

Scenario:

A directed trust separates administrative trustee duties from investment-direction authority. An investment advisor may direct portfolio trades, but the trustee retains distribution and administrative control.

| Role | Authority |
|---|---|
| Administrative trustee | account administration, distributions, reporting approval |
| Investment direction advisor | investment instructions within mandate |
| Protector | appointment/removal or veto rights where deed allows |
| Beneficiary | reporting or benefit rights only, unless deed says otherwise |

Implementation treatment:

- trade authority, distribution authority and report authority must be separate permissions;
- suitability and mandate checks must use the correct profile and governing document;
- advisor-directed trades should preserve instruction source and authority basis;
- trustee visibility does not automatically mean trade approval is required unless policy says so.

## 11. Family-Office Investment Committee Approval

Scenario:

A family-office mandate requires investment committee approval for alternatives above 10% of consolidated reportable assets.

| Attribute | Value |
|---|---:|
| Consolidated reportable assets | 50,000,000 |
| Existing alternatives exposure | 4,000,000 |
| Proposed private fund commitment | 2,000,000 |
| Alternatives threshold | 10% |

Calculation:

```text
post-trade alternatives exposure = 4,000,000 + 2,000,000 = 6,000,000
post-trade alternatives weight = 6,000,000 / 50,000,000 = 12%
```

Correct treatment:

- approval workflow should trigger before commitment or order submission;
- committee membership, quorum, approval date and conflicts should be source-backed;
- consolidated exposure should respect inclusion perimeter and restricted entities;
- DPM/advisory workflow should not bypass committee rule because account-level mandate appears eligible.

## 12. Foundation Council Change

Scenario:

A foundation council member resigns and a replacement is appointed. The council controls investment authority and report recipient approvals.

Workflow:

```text
source document -> council role update -> quorum recalculation -> entitlement update -> pending approval review -> access certification
```

QA assertions:

| Test | Expected result |
|---|---|
| Old council member had pending approval | Pending approval is reviewed or invalidated according to policy. |
| New council member lacks KYC completion | Authority remains pending until validation completes. |
| Quorum changes | Approval workflow recalculates required approvers. |
| Reports were scheduled to old member | Delivery list updates with audit trail. |

## 13. Multi-Generational Beneficiary Classes

Scenario:

A trust defines income beneficiaries, capital beneficiaries and remainder beneficiaries across generations.

| Beneficiary class | Example rights | Reporting treatment |
|---|---|---|
| Income beneficiary | May receive income distributions. | Income report access if authorized. |
| Capital beneficiary | May receive capital distributions. | Capital distribution visibility if authorized. |
| Remainder beneficiary | May benefit after a future event. | Usually limited or no current portfolio visibility unless deed allows. |
| Minor beneficiary | Benefit rights may exist, but access is through guardian/trustee. | Masked or guardian-mediated reporting. |

Implementation warning:

Do not treat every named beneficiary as an account owner, report recipient or instruction authority. Rights, access and reporting scope must come from source documents and effective-dated rules.

## 14. VCC Sub-Fund Reporting Perimeter

Scenario:

A variable capital company has two segregated sub-funds. A family owns shares in Sub-Fund A but not Sub-Fund B.

| Entity | Exposure | Reporting treatment |
|---|---|---|
| VCC umbrella | legal umbrella entity | show only if relevant to legal structure. |
| Sub-Fund A | family-owned interest | include in report and exposure. |
| Sub-Fund B | no family-owned interest | exclude unless authorized aggregate disclosure exists. |

Controls:

- sub-fund assets and liabilities should not be merged unless legal/reporting policy permits;
- exposure, performance and risk should be calculated at the held sub-fund level;
- privacy rules may restrict disclosure of other sub-funds;
- custody, fund admin and legal documents must agree on sub-fund identifier.

## 15. Cross-Border Tax Residency Change

Scenario:

A beneficiary changes tax residency during the year. Reporting and withholding assumptions may change from the effective date.

| Attribute | Value |
|---|---|
| Prior tax residence | Country A |
| New tax residence | Country B |
| Effective date | 2026-07-01 |
| Documentation status | New self-certification pending |

Correct treatment:

- tax residency is effective-dated and source-backed;
- distributions before and after effective date may require different reporting treatment;
- missing updated documentation creates exception state rather than assumed treaty treatment;
- report delivery, privacy and cross-border restrictions may change with residency;
- historical reports should preserve prior residency basis.

## 16. Advisory And Mandate Checklist

| Dimension | Required question |
|---|---|
| Legal owner | Which entity owns the account or asset? |
| Beneficial owner | Who economically benefits, and how is this sourced? |
| Authority | Who may instruct, approve, revoke, view or receive reports? |
| Suitability | Which person/entity profile applies to advisory or mandate decisions? |
| Mandate | Is the mandate at account, entity, family, sub-portfolio or beneficiary level? |
| Tax/reporting | Which jurisdiction, residency, entity and beneficiary flags matter? |
| Privacy | Which parties must be masked or excluded from reports? |
| Succession | What changes on death, incapacity, resignation, removal or distribution? |

## 17. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Entity/account hierarchy | party, account, portfolio, legal owner, relationship role | graph-based ownership and influence analytics |
| Authority | role, effective date, document evidence, access scope, veto/committee/council rules where sourced | authority workflow with expiry and recertification |
| Reporting group | inclusion perimeter, access rights, masking rules, sub-fund perimeter where sourced | dynamic family-office report builder |
| Distributions | approved cash movement, beneficiary recipient, consent/veto status | income/capital distribution classification engine |
| Estate state | death notification, executor validation, account restrictions | probate lifecycle workflow |
| Cross-entity collateral | pledgor, borrower, collateral, pledge direction | multi-entity credit optimization with legal controls |
| Cross-border status | tax residency, documentation status, effective date | cross-regime reportability and access policy engine |

## 18. Letter Of Wishes Handling

Scenario:

- A settlor provides a letter of wishes alongside a discretionary trust.
- The trustee may consider it, but it is not an automatic distribution rule.
- A beneficiary asks why a discretionary distribution was not made exactly as described in the letter.

Implementation treatment:

| Dimension | Correct treatment |
|---|---|
| Document type | Store as advisory intent or non-binding guidance unless legal review says otherwise. |
| Decision basis | Distribution decision remains with authorized trustee or fiduciary body. |
| Reporting | Do not expose private settlor intent to beneficiaries unless source and policy allow. |
| Workflow | Use the letter as contextual evidence, not as a system-enforced entitlement rule. |
| Audit | Preserve decision rationale, trustee minutes and document version used. |

QA assertions:

| Test | Expected result |
|---|---|
| Beneficiary is named in letter only | No automatic income, capital or report entitlement is created. |
| Trustee approves a different distribution | System allows decision when authority and policy are valid. |
| Letter is superseded | Workflow uses effective document version and preserves prior audit basis. |
| Beneficiary requests full letter | Access is blocked or redacted unless policy permits disclosure. |

## 19. Reserved Powers In A Settlor-Controlled Structure

Scenario:

- A trust deed reserves investment approval power to the settlor.
- Trustee retains administration and distribution responsibilities.
- An investment trade is placed without checking the reserved power.

Control model:

| Action type | Required authority |
|---|---|
| Investment trade above reserved threshold | Settlor approval plus trustee policy check |
| Distribution | Trustee approval and any protector consent required by deed |
| Report delivery | Reporting authority and privacy policy |
| Administrative update | Trustee or administrator authority |

QA assertions:

| Test | Expected result |
|---|---|
| Trade requires reserved-power approval | Order is blocked or routed to approval workflow. |
| Settlor authority expired | Approval cannot be used after effective revocation date. |
| Trustee approves distribution | Distribution workflow does not require investment reserved-power approval unless deed says so. |
| Report is generated | Approval evidence is visible to internal control users, not necessarily to all report recipients. |

## 20. Charitable Purpose Foundation

Scenario:

- A foundation exists for charitable and philanthropic purposes.
- It holds a diversified portfolio and makes grants to approved charities.
- Council approval and purpose alignment are required before disbursement.

| Attribute | Value |
|---|---:|
| Annual grant budget | 1,000,000 |
| Grants already approved | 625,000 |
| Proposed grant | 250,000 |
| Remaining budget after proposed grant | 125,000 |

Calculation:

```text
post_grant_utilization = (625,000 + 250,000) / 1,000,000 = 87.50%
remaining_budget = 1,000,000 - 625,000 - 250,000 = 125,000
```

Controls:

- grant recipient due diligence must be complete;
- grant purpose must align to foundation charter or purpose statement;
- council approval, quorum and conflict checks must be source-backed;
- investment portfolio liquidity should be checked before grant payment;
- reporting should separate grant activity from investment performance.

## 21. Nominee Arrangement With Beneficial Owner Reporting

Scenario:

- A nominee company is legal holder of record.
- A private client is beneficial owner.
- External custody statement shows nominee name, while internal reporting must show beneficial exposure subject to privacy rules.

Implementation treatment:

| Layer | Treatment |
|---|---|
| Legal holding | Record nominee as holder of record. |
| Beneficial ownership | Store beneficial owner relationship with effective date and source evidence. |
| Tax/reporting | Apply beneficial-owner reporting rules only when documentation is valid. |
| Entitlements | Do not give nominee operators personal-client report access unless role permits. |
| Reconciliation | Reconcile custody to nominee position and internal exposure to beneficial owner. |

QA assertions:

| Test | Expected result |
|---|---|
| Custody file names nominee | Position reconciles to legal holder. |
| Client report is generated | Beneficial exposure appears only for authorized report recipient. |
| Beneficial-owner evidence expires | Reporting or tax workflow enters exception state. |
| Nominee holds assets for multiple clients | Reports avoid cross-client disclosure. |

## 22. Philanthropic Grant Workflow

Scenario:

- A family office requests a grant from a donor-advised or philanthropic account.
- Grant amount is 150,000.
- Liquid cash is 90,000 and expected bond sale proceeds are 70,000.
- Grant cannot be released until both approval and funding are complete.

Funding check:

```text
available_funding = 90,000 + 70,000 = 160,000
funding_surplus = 160,000 - 150,000 = 10,000
```

Workflow:

```text
grant request -> recipient due diligence -> purpose check -> authority approval -> liquidity check -> cash reservation -> payment -> grant reporting -> audit record
```

Controls:

- recipient screening and purpose alignment are not optional;
- expected sale proceeds should not be treated as available cash before settlement policy allows;
- grant reporting should show charitable outflow separately from portfolio withdrawals;
- failed payment, rejected recipient or revoked approval should reverse reservation without deleting evidence.

## 23. Trust Termination And Final Distribution

Scenario:

- A trust is terminating after a defined event.
- Portfolio assets are liquidated and final distributions are made to three beneficiaries.
- Final account closure must reconcile assets, cash, fees and residual tax reserve.

| Item | Amount |
|---|---:|
| Final portfolio liquidation proceeds | 3,200,000 |
| Accrued fees | 35,000 |
| Tax reserve | 65,000 |
| Distributable cash | 3,100,000 |

Calculation:

```text
distributable_cash = 3,200,000 - 35,000 - 65,000 = 3,100,000
```

Correct workflow:

| Step | Treatment |
|---|---|
| Termination trigger | Verify source event and authority to terminate. |
| Asset liquidation | Preserve trade, cash, tax and fee lineage. |
| Final allocation | Apply deed or approved distribution schedule. |
| Residual reserve | Track tax, fee or claim reserve until released or paid. |
| Closure | Close account only after zero-position, cash and obligation checks pass. |

QA assertions:

| Test | Expected result |
|---|---|
| Residual tax reserve exists | Account cannot be fully closed without reserve handling state. |
| Beneficiary allocation totals exceed distributable cash | Workflow blocks final distribution. |
| Late fee arrives after closure request | Closure moves to exception or reserve adjustment workflow. |
| Final statement is generated | Report includes liquidation, fees, reserves and distributions with lineage. |

## 24. Beneficiary Dispute Restriction

Scenario:

- Two beneficiaries dispute a proposed capital distribution.
- Trustee freezes discretionary distributions while legal review is pending.
- Routine income payments may continue only if governing documents and legal advice permit.

Implementation treatment:

| Area | Treatment |
|---|---|
| Restriction state | Store dispute restriction with scope, effective date and review owner. |
| Cash movement | Block affected discretionary distributions. |
| Reporting | Show pending or restricted status without disclosing unrelated beneficiary data. |
| Investment authority | Do not freeze unrelated portfolio management unless restriction scope requires it. |
| Evidence | Link dispute notice, legal advice, trustee decision and resolution record. |

QA assertions:

| Test | Expected result |
|---|---|
| Restricted distribution is submitted | Payment is blocked with dispute reason. |
| Unrelated fee payment is submitted | Payment proceeds if outside restriction scope. |
| Beneficiary report is generated | Restricted status is shown without exposing other beneficiary details. |
| Dispute is resolved | Restriction closes with authority, date and outcome evidence. |

## 25. Family Governance Conflict-Of-Interest Control

Scenario:

- A family-office investment committee reviews a private fund commitment.
- One committee member is also a director of the fund manager.
- Policy requires conflicted members to recuse and quorum to be recalculated.

| Attribute | Value |
|---|---:|
| Committee members | 5 |
| Conflicted members | 1 |
| Voting members after recusal | 4 |
| Quorum requirement | 4 |

Calculation:

```text
eligible_voting_members = 5 - 1 = 4
quorum_met = eligible_voting_members >= 4
```

Controls:

- conflict declaration should be captured before approval;
- conflicted member should not count as voting approval if policy requires recusal;
- quorum should be recalculated after exclusions;
- approval evidence should include eligible voters, abstentions, conflicts and rationale;
- advisory suitability and DPM mandate checks remain separate from governance approval.

## 26. Regression Test Pack

Minimum release-gate scenarios:

1. Trust distribution requires valid trustee approval.
2. Beneficiary access shows only permitted report sections.
3. Death notification restricts authority pending estate validation.
4. Holding-company pledge does not create individual buying power without cross-pledge rule.
5. Family-office consolidated report respects inclusion perimeter and restricted data.
6. Insurance policy displays distinct owner, insured, payer and beneficiary roles.
7. Authority change revokes old access and preserves audit trail.
8. Suitability check uses correct entity/person profile.
9. Consolidated performance excludes restricted accounts or labels partial perimeter.
10. Missing source document blocks client-ready ownership or authority claim.
11. Protector veto blocks distribution without changing cash or beneficiary receivable.
12. Directed trust separates investment authority from trustee distribution authority.
13. Investment committee threshold triggers approval before commitment or order.
14. Foundation council change updates quorum, entitlements and pending approvals.
15. Beneficiary class controls reporting access by rights and source document.
16. VCC sub-fund reporting excludes non-held sub-funds and avoids umbrella-level overstatement.
17. Tax residency change applies from effective date and preserves historical basis.
18. Letter of wishes does not create automatic entitlement or authority.
19. Reserved powers route affected actions to the correct authority workflow.
20. Charitable foundation grants validate purpose, budget, recipient due diligence and council approval.
21. Nominee reporting separates legal holder, beneficial owner, report recipient and privacy scope.
22. Philanthropic grant workflow separates approval, funding, settlement and payment evidence.
23. Trust termination reconciles liquidation proceeds, fees, reserves, final distributions and closure state.
24. Beneficiary dispute restrictions block only the scoped activity and preserve confidentiality.
25. Family governance conflict controls recalculate eligible voting members and quorum.

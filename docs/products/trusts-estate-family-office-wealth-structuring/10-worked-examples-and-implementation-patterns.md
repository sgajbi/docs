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

## 26. Protector Succession

Scenario:

- A trust protector resigns and a successor protector is appointed.
- The protector has veto rights over capital distributions and trustee replacement.
- A distribution request is pending during the succession window.

| Authority state | Effective date | Status |
|---|---|---|
| Outgoing protector | Until 30 Jun | Resigned |
| Successor protector | From 1 Jul | Appointment pending acceptance |
| Pending distribution request | 28 Jun | Awaiting protector review |

Control treatment:

```text
active_protector = protector where effective_from <= action_date < effective_to and acceptance_status = accepted
succession_gap = appointment_effective_date reached and acceptance_status != accepted
```

Correct workflow:

- preserve outgoing appointment, resignation notice, successor appointment, acceptance state and effective dates;
- block or route actions requiring protector consent when no accepted protector is active;
- distinguish trustee authority from protector veto authority;
- keep beneficiary-facing reporting limited to action status and permitted governance context.

QA assertions:

| Test | Expected result |
|---|---|
| Distribution requires protector consent during succession gap | Workflow blocks or routes to legal/trustee exception handling. |
| Successor accepts appointment | Active protector resolves from effective date with audit lineage. |
| Outgoing protector tries to veto after resignation | Action is rejected unless governing document preserves transitional power. |
| Beneficiary report is generated | Report shows pending governance status without exposing restricted documents. |

## 27. Delegated Investment-Manager Replacement

Scenario:

- Trustees delegate portfolio management to an external investment manager.
- The manager is replaced after an investment-policy breach.
- Pending orders, model links and DPM permissions must move only after new authority is effective.

| Item | Existing manager | Replacement manager |
|---|---|---|
| Mandated portfolio value | 18,000,000 | 18,000,000 |
| Pending orders | 4 | 0 until effective |
| Authority effective date | Until 15 Aug | From 16 Aug |
| Due-diligence status | Breach review | Approved |

Transition control:

```text
orders_requiring_reapproval = pending_orders_before_manager_change
active_manager = manager where effective_from <= order_action_date < effective_to
```

Correct workflow:

- preserve manager appointment, termination reason, due-diligence approval and effective dates;
- freeze or reapprove pending orders when manager authority changes;
- update model links, discretionary permissions and contact ownership from the effective date;
- keep investment performance continuous while governance responsibility changes.

QA assertions:

| Test | Expected result |
|---|---|
| Old manager submits order after effective termination | Order is blocked. |
| Pending order exists before replacement | Order requires cancellation, reapproval or explicit transition evidence. |
| New manager lacks due-diligence approval | Authority activation is blocked. |
| Performance report spans transition date | Performance remains continuous while manager responsibility is effective-dated. |

## 28. Beneficiary Loan

Scenario:

- A beneficiary requests a short-term loan from a family trust.
- The governing document permits loans only with trustee approval, interest terms and exposure limit.
- The beneficiary already has an outstanding loan.

| Attribute | Value |
|---|---:|
| Existing beneficiary loan | 300,000 |
| Proposed new loan | 250,000 |
| Beneficiary loan limit | 500,000 |
| Annual interest rate | 4.50% |

Limit check:

```text
post_loan_exposure = 300,000 + 250,000 = 550,000
limit_breach = 550,000 - 500,000 = 50,000
annual_interest = 250,000 x 4.50% = 11,250
```

Correct workflow:

- separate loan receivable from beneficiary distribution and entitlement;
- validate governing-document permission, trustee approval, terms, maturity and interest basis;
- block or exception-route loans that breach beneficiary loan limits;
- report outstanding loans, accrued interest and repayments without exposing other beneficiary balances.

QA assertions:

| Test | Expected result |
|---|---|
| Proposed loan breaches limit | Workflow blocks or routes to approved exception. |
| Trustee approval is missing | No loan receivable or cash payment is created. |
| Loan is approved | Receivable, cash movement, interest accrual and maturity are tracked. |
| Beneficiary report is generated | Report shows only authorized beneficiary loan information. |

## 29. Family Charter Amendment

Scenario:

- A family charter changes the approval threshold for private-market commitments.
- The amendment is approved by the family council but becomes effective after a notice period.
- A commitment proposal is submitted during the transition.

| Rule version | Threshold | Effective period |
|---|---:|---|
| Current charter | 5,000,000 | Until 31 Oct |
| Amended charter | 3,000,000 | From 1 Nov |
| Proposed commitment | 4,000,000 | Submitted 20 Oct |

Threshold evaluation:

```text
approval_required_under_current_rule = 4,000,000 >= 5,000,000 = false
approval_required_under_amended_rule = 4,000,000 >= 3,000,000 = true
active_rule = rule where effective_from <= proposal_date < effective_to
```

Correct workflow:

- version family charter provisions and effective dates;
- evaluate proposals against the rule active on the relevant action date;
- preserve council approval, notice period, dissenting notes where allowed and amendment evidence;
- avoid retroactively invalidating actions approved under the prior active rule unless legal review requires it.

QA assertions:

| Test | Expected result |
|---|---|
| Proposal date is before amendment effective date | Current threshold applies. |
| Investment is approved after amendment effective date | New threshold applies when policy says approval date governs. |
| Charter evidence is missing | Governance rule change remains inactive. |
| Report explains governance threshold | It shows rule version and effective date, not informal family intent. |

## 30. Multi-Branch Reporting Restriction

Scenario:

- A family office has accounts across two booking centres and one external custodian.
- A regional branch team can view local accounts but is restricted from viewing another branch's trust details.
- The consolidated family report must preserve perimeter and masking rules.

| Reporting component | Value | Branch visibility |
|---|---:|---|
| Branch A portfolios | 12,000,000 | Visible to Branch A |
| Branch B trust assets | 8,500,000 | Restricted from Branch A |
| External custodian assets | 4,000,000 | Visible only to family-office reporting team |

Perimeter treatment:

```text
authorized_reportable_value_for_branch_a = 12,000,000
full_family_office_value = 12,000,000 + 8,500,000 + 4,000,000 = 24,500,000
masked_value_for_branch_a = 12,500,000
```

Correct workflow:

- separate full consolidated report generation from branch-specific delivery entitlement;
- apply booking-centre, trust-document, privacy and recipient-role restrictions;
- show partial perimeter labels when restricted assets are excluded or masked;
- avoid leaking existence, value or beneficiary details where the recipient is not authorized.

QA assertions:

| Test | Expected result |
|---|---|
| Branch A user requests family report | Report includes only authorized perimeter or masked summary. |
| Family-office reporting team is authorized | Full consolidated report is available with source lineage. |
| Restricted trust asset changes value | Branch A report does not reveal restricted change. |
| Recipient role changes | Report entitlement recalculates from effective-dated authority. |

## 31. Charitable Pledge Cancellation

Scenario:

- A family foundation approved a multi-year charitable pledge.
- The recipient fails a due-diligence refresh before the second installment.
- The remaining pledge must be cancelled or suspended with evidence, without rewriting the paid installment.

| Pledge component | Amount | Status |
|---|---:|---|
| Total approved pledge | 600,000 | Approved |
| Installment already paid | 200,000 | Completed |
| Remaining pledge | 400,000 | Pending cancellation |
| Due-diligence result | n/a | Failed refresh |

Cancellation treatment:

```text
remaining_pledge = total_approved_pledge - paid_installments
remaining_pledge = 600,000 - 200,000 = 400,000
cancelled_commitment = 400,000
```

Correct workflow:

- preserve original pledge approval, paid installment, due-diligence failure and cancellation decision;
- stop future payments while retaining completed payment evidence;
- release reserved liquidity only after cancellation or suspension is approved;
- update philanthropic reporting to show paid grants, cancelled commitments and reason codes separately.

QA assertions:

| Test | Expected result |
|---|---|
| Recipient due diligence fails | Remaining pledge payment is blocked. |
| First installment was already paid | Historical grant payment remains intact with evidence. |
| Cancellation is approved | Reserved liquidity releases and commitment state updates. |
| Report is generated | Paid grant and cancelled pledge are reported as separate lifecycle events. |

## 32. Excluded-Person Control

Scenario:

- A trust deed excludes a named person from receiving distributions or report access.
- The person is related to an active beneficiary and appears in an external CRM relationship group.
- A distribution request and report-sharing request both need exclusion screening.

| Attribute | Value |
|---|---|
| Requested distribution | 75,000 |
| Excluded person relationship | Family member |
| Trust deed status | Active exclusion |
| Report access request | Pending |

Control result:

```text
distribution_allowed = beneficiary_not_excluded and trustee_approval_present
report_access_allowed = recipient_not_excluded and recipient_role_entitled
```

Correct workflow:

- preserve exclusion clause, named person identity, relationship context, effective dates and legal/trustee review evidence;
- block distributions, loans, report delivery and instruction authority for excluded persons;
- avoid exposing restricted trust details when denying access or explaining the blocked state;
- distinguish an excluded person from a beneficiary with temporarily suspended rights.

QA assertions:

| Test | Expected result |
|---|---|
| Excluded person requests report access | Access is blocked without leaking trust details. |
| Advisor attempts distribution to excluded person | Workflow blocks before cash or beneficiary receivable is created. |
| Exclusion is amended | Effective-dated source evidence controls future access. |
| Related active beneficiary receives report | Active beneficiary access is evaluated independently. |

## 33. Foundation Redomiciliation

Scenario:

- A private foundation changes domicile from one jurisdiction to another.
- Council members remain the same, but reporting, tax status, governing law and bank documentation must be updated from the redomiciliation effective date.

| Attribute | Before | After |
|---|---|---|
| Domicile | Jurisdiction A | Jurisdiction B |
| Governing law | A foundation law | B foundation law |
| Effective date | Until 30 Sep | From 1 Oct |
| Open documentation tasks | 0 | 6 |

Transition control:

```text
active_domicile = domicile where effective_from <= action_date < effective_to
open_redomiciliation_tasks = 6
```

Correct workflow:

- preserve redomiciliation approval, registry evidence, legal opinion, effective date and impacted account list;
- update governing law, reporting labels, tax status and document requirements from the effective date;
- keep historical reporting under prior domicile and avoid retroactive rewriting unless legal review requires it;
- block affected actions when mandatory bank or tax documentation is incomplete.

QA assertions:

| Test | Expected result |
|---|---|
| Action date is before redomiciliation | Prior domicile rules apply. |
| Effective date passes | New domicile, governing law and documentation requirements become active. |
| Registry evidence is missing | Redomiciliation remains pending or source-limited. |
| Report spans the transition | Report shows effective-dated domicile history. |

## 34. Emergency Trustee Powers

Scenario:

- A trustee must exercise emergency powers to preserve assets during market disruption.
- Normal investment committee approval cannot be obtained before the market cut-off.
- The governing document allows emergency action with post-event ratification.

| Attribute | Value |
|---|---|
| Proposed protective sale | 1,200,000 |
| Normal approval status | Unavailable |
| Emergency authority | Permitted |
| Ratification deadline | 5 business days |

Emergency authority:

```text
emergency_action_allowed = emergency_clause_active and action_purpose == asset_preservation
ratification_required = true
```

Correct workflow:

- preserve emergency clause, triggering event, trustee decision, asset-preservation rationale and market cut-off evidence;
- allow only scoped protective actions, not unrelated discretionary activity;
- require post-event ratification, beneficiary/confidentiality review and audit evidence;
- label reporting as emergency-governed until ratification is complete.

QA assertions:

| Test | Expected result |
|---|---|
| Emergency clause applies | Scoped protective workflow can proceed with trustee evidence. |
| Action is unrelated to preservation | Normal approval workflow is required. |
| Ratification deadline passes | Exception or breach workflow opens. |
| Client report is generated | Emergency status and approval lineage are visible to authorized recipients. |

## 35. Private-Trust-Company Board Change

Scenario:

- A private trust company replaces two directors.
- Board quorum, signing authority and pending approvals must be recalculated from the effective appointment date.

| Attribute | Before | After |
|---|---:|---:|
| Board members | 5 | 5 |
| Directors replaced | 0 | 2 |
| Quorum requirement | 3 | 3 |
| Pending approvals | 4 | 4 requiring review |

Approval impact:

```text
approvals_requiring_revalidation = pending_approvals_at_board_change
quorum_met = eligible_active_directors >= quorum_requirement
```

Correct workflow:

- preserve resignation, appointment, acceptance, register update and effective dates for each director;
- recalculate quorum, signing authority, committee membership and pending approval validity;
- require revalidation where prior approvals depended on outgoing directors or expired authority;
- keep trust-level ownership and portfolio performance unchanged while governance authority changes.

QA assertions:

| Test | Expected result |
|---|---|
| Outgoing director approves after effective resignation | Approval is rejected. |
| Pending approval used outgoing-director authority | Revalidation workflow opens. |
| Register evidence is missing | New director authority remains pending. |
| Governance report is generated | Board history and active authority are effective-dated. |

## 36. Beneficiary Consent Campaign

Scenario:

- A restructuring requires consent from multiple beneficiary classes.
- Some beneficiaries have consent rights, some receive notice only, and one beneficiary is under representation restrictions.

| Beneficiary group | Required consents | Received consents |
|---|---:|---:|
| Income beneficiaries | 4 | 3 |
| Capital beneficiaries | 3 | 3 |
| Notice-only beneficiaries | 0 | 0 |

Consent coverage:

```text
required_consents = 4 + 3 = 7
received_consents = 3 + 3 = 6
consent_coverage = 6 / 7 = 85.71%
missing_consents = 1
```

Correct workflow:

- preserve consent class, notice package, delivery evidence, response status, representative authority and deadline;
- distinguish consent-required, notice-only and restricted/minor/represented beneficiaries;
- block final restructuring until required consent threshold and trustee approval are met;
- show campaign status without exposing other beneficiaries' private details.

QA assertions:

| Test | Expected result |
|---|---|
| Required consent is missing | Restructuring remains pending. |
| Notice-only beneficiary has not responded | Consent coverage is unaffected. |
| Representative authority is missing | Beneficiary response is invalid or pending review. |
| Campaign report is generated | Coverage, missing consents and deadlines are visible to authorized users. |

## 37. Family-Office Expense Allocation Dispute

Scenario:

- A family office allocates shared operating expenses across branches and entities.
- One branch disputes its allocation because a service was not used by its accounts.

| Branch/entity | Proposed allocation | Accepted allocation |
|---|---:|---:|
| Branch A | 90,000 | 90,000 |
| Branch B | 70,000 | 45,000 |
| Holding company | 40,000 | 40,000 |

Dispute amount:

```text
disputed_expense = proposed_branch_b_allocation - accepted_branch_b_allocation
disputed_expense = 70,000 - 45,000 = 25,000
accepted_total = 90,000 + 45,000 + 40,000 = 175,000
```

Correct workflow:

- preserve expense policy, service usage basis, allocation run, disputed amount, approval owner and resolution state;
- separate payable expense, disputed allocation, accepted allocation and any reallocation to other entities;
- avoid allocating private entity costs to unrelated beneficiaries or branches;
- update family-office reporting only after approved allocation or provisional-dispute labelling.

QA assertions:

| Test | Expected result |
|---|---|
| Branch disputes allocation | Disputed amount is tracked separately from accepted expense. |
| Allocation basis is missing | Expense allocation remains provisional or blocked. |
| Reallocation is approved | Impacted branches/entities receive versioned allocation updates. |
| Report is generated | Accepted and disputed costs are labelled by policy and recipient authority. |

## 38. Reserved Investment Powers After Incapacity

Scenario:

- A settlor reserved investment powers while capable.
- A medical incapacity event is recorded.
- The governing documents specify that reserved powers suspend on incapacity and trustee authority resumes for investment decisions.

| Attribute | Value |
|---|---|
| Reserved power scope | Investment direction |
| Incapacity evidence | Received |
| Trustee fallback authority | Active |
| Pending investment instructions | 3 |

Authority result:

```text
settlor_investment_authority_active = reserved_power_active and incapacity_status != confirmed
pending_instructions_requiring_review = 3
```

Correct workflow:

- preserve reserved-power clause, capacity evidence, effective date, legal review, trustee fallback authority and impacted pending instructions;
- suspend new settlor-directed investment instructions after confirmed incapacity when the document requires it;
- re-route pending instructions to trustee or committee approval rather than silently cancelling them;
- keep reporting labels clear: ownership does not change, but decision authority does.

QA assertions:

| Test | Expected result |
|---|---|
| Incapacity is confirmed | Reserved investment authority is suspended from the effective date. |
| Pending instruction predates incapacity | Review workflow determines whether it remains valid. |
| Advisor attempts new settlor-directed order | Order is blocked or routed to trustee authority. |
| Governance report is generated | Authority history and incapacity evidence are effective-dated. |

## 39. Letter-Of-Wishes Disclosure Dispute

Scenario:

- A beneficiary asks to see the letter of wishes.
- The trustee has discretion over disclosure and another beneficiary contests release.
- The platform must preserve the request, dispute and trustee decision without exposing restricted content.

| Attribute | Value |
|---|---|
| Disclosure request | Open |
| Objecting beneficiary | 1 |
| Trustee decision | Pending |
| Restricted document sections | 4 |

Disclosure control:

```text
disclosure_allowed = trustee_decision == approved and recipient_scope_allows_document
restricted_sections_withheld = 4
```

Correct workflow:

- preserve requestor, requested document, objection, trustee decision owner, confidentiality scope and legal review state;
- avoid granting report access or document visibility before approved disclosure scope is recorded;
- support partial disclosure or summary-only disclosure where policy permits;
- track the dispute separately from financial entitlement, distribution rights and investment authority.

QA assertions:

| Test | Expected result |
|---|---|
| Disclosure decision is pending | Document is not visible to the requesting beneficiary. |
| Trustee approves partial disclosure | Only approved sections or summary are visible. |
| Objection is recorded | Dispute evidence remains linked to the decision. |
| Beneficiary report is generated | Report does not reveal restricted letter-of-wishes content. |

## 40. Trust-To-Company Asset Transfer

Scenario:

- A trust transfers an investment asset to a family holding company.
- The economic owner changes from trust account to company account, but beneficial ownership and reporting group continuity need to be preserved.

| Attribute | Value |
|---|---:|
| Asset market value | 2,400,000 |
| Transfer consideration | 0 |
| Trust reporting value after transfer | 0 |
| Holding company reporting value after transfer | 2,400,000 |

Reporting movement:

```text
entity_level_transfer_value = 2,400,000
consolidated_family_group_change = 0
```

Correct workflow:

- preserve trustee approval, company board approval, transfer deed, valuation basis, effective date and beneficial-owner linkage;
- book the asset movement as a transfer, contribution or restructuring event according to source evidence, not as a market sale unless consideration exists;
- update entity-level holdings while preserving consolidated family-office continuity where the reporting perimeter includes both entities;
- trigger tax, pledge, suitability and restriction impact review before client-ready reporting.

QA assertions:

| Test | Expected result |
|---|---|
| Asset moves from trust to company | Trust and company positions update with transfer lineage. |
| Consolidated family report includes both entities | Group exposure is not double counted or shown as realized sale proceeds. |
| Approval evidence is missing | Transfer remains pending or support-limited. |
| Asset was pledged | Pledge release or substitution workflow is required before transfer completion. |

## 41. Protector Conflict Recusal

Scenario:

- A protector must consent to a distribution but has a declared conflict because the distribution benefits a related person.
- The governing document allows alternate protector or trustee committee consent after recusal.

| Attribute | Value |
|---|---|
| Protector consent required | Yes |
| Conflict declared | Yes |
| Alternate consent route | Trustee committee |
| Distribution amount | 250,000 |

Recusal control:

```text
protector_vote_eligible = consent_required and conflict_declared == false
alternate_route_required = consent_required and conflict_declared == true
```

Correct workflow:

- preserve conflict declaration, affected transaction, recusal rule, alternate authority and consent evidence;
- prevent the conflicted protector from approving or vetoing the scoped transaction when recusal applies;
- route approval to alternate authority only within the permitted scope;
- keep unrelated protector rights active when the conflict is transaction-specific.

QA assertions:

| Test | Expected result |
|---|---|
| Conflict is declared | Protector is removed from approval for the scoped transaction. |
| Alternate route is permitted | Workflow routes to trustee committee or alternate protector. |
| Conflicted protector attempts approval | Approval is rejected and audited. |
| Unrelated action is submitted | Protector authority is evaluated independently. |

## 42. Family-Office Service-Level Chargeback

Scenario:

- A family office charges branches for advisory, reporting and administration services.
- One branch has a premium reporting SLA and another receives only basic administration.

| Branch/entity | Service tier | Charge base | Charge |
|---|---|---:|---:|
| Branch A | Premium reporting | 1,800,000 | 18,000 |
| Branch B | Basic administration | 1,200,000 | 6,000 |
| Holding company | Shared governance | 2,000,000 | 10,000 |

Chargeback rate:

```text
branch_a_rate = 18,000 / 1,800,000 = 1.00%
branch_b_rate = 6,000 / 1,200,000 = 0.50%
```

Correct workflow:

- preserve service catalogue, SLA tier, charge basis, allocation run, approval and dispute state;
- separate advisory, reporting, administration and governance service categories;
- prevent one branch's premium service cost from being allocated to branches that did not receive the service;
- expose chargeback calculation, allocation basis and recipient authority in family-office reports.

QA assertions:

| Test | Expected result |
|---|---|
| Branch has premium SLA | Premium charge rate applies only to that branch. |
| Service tier evidence is missing | Chargeback remains provisional or blocked. |
| Branch disputes charge | Disputed charge is separated from accepted charge. |
| Consolidated report is generated | Chargebacks are visible only to authorized recipients. |

## 43. Estate Liquidity Reserve Release

Scenario:

- An estate retains a liquidity reserve for tax, legal fees and final expenses.
- After obligations are settled, part of the reserve can be released to beneficiaries.

| Reserve component | Reserved | Settled | Remaining |
|---|---:|---:|---:|
| Tax reserve | 400,000 | 320,000 | 80,000 |
| Legal fees | 120,000 | 95,000 | 25,000 |
| Final expenses | 60,000 | 40,000 | 20,000 |

Release amount:

```text
remaining_reserve = 80,000 + 25,000 + 20,000 = 125,000
approved_release = remaining_reserve - retained_contingency
```

Correct workflow:

- preserve reserve policy, estate administrator approval, settled obligations, retained contingency and beneficiary allocation;
- release reserve only after required obligations are settled or formally reduced;
- distinguish estate cash reserve release from investment income, trust distribution and inheritance-tax estimate;
- update beneficiary statements with approved release, retained reserve and remaining estate obligations.

QA assertions:

| Test | Expected result |
|---|---|
| Tax obligation remains unsettled | Reserve release is blocked or reduced. |
| Administrator approves partial release | Beneficiary allocation uses approved release amount only. |
| Retained contingency changes | Release amount recalculates with evidence. |
| Final estate report is generated | Settled obligations, retained reserve and released cash are separate. |

## 44. Purpose Trust Operating Budget

Scenario:

- A purpose trust funds maintenance, administration and oversight activities for a defined non-charitable purpose.
- The annual operating budget is close to being exceeded after a proposed facility-maintenance payment.

| Budget item | Amount |
|---|---:|
| Approved annual budget | 600,000 |
| Committed spend to date | 420,000 |
| Required contingency reserve | 50,000 |
| Proposed maintenance payment | 150,000 |

Budget headroom:

```text
available_budget_headroom = approved_budget - committed_spend - required_contingency
available_budget_headroom = 600,000 - 420,000 - 50,000 = 130,000
budget_shortfall = proposed_payment - available_budget_headroom = 20,000
```

Correct workflow:

- preserve purpose trust deed, approved budget, enforcer or protector review, trustee approval, invoice and purpose-alignment evidence;
- separate committed spend, proposed spend, contingency reserve and budget shortfall;
- block or escalate payments that exceed approved budget headroom unless supplemental approval exists;
- report purpose-trust spend against approved categories without treating operating disbursements as beneficiary distributions.

QA assertions:

| Test | Expected result |
|---|---|
| Proposed spend exceeds budget headroom | Payment is blocked or escalated for supplemental approval. |
| Purpose evidence is missing | Payment remains pending or support-limited. |
| Contingency reserve is required | Available budget excludes retained contingency. |
| Report is generated | Operating spend is shown separately from beneficiary distributions. |

## 45. Beneficiary Privacy Masking Appeal

Scenario:

- A discretionary beneficiary appeals masked reporting output and asks to see capital accounts, trustee notes and other beneficiary details.
- The trustee approves limited additional visibility but keeps confidential sections masked.

| Report section | Requested | Approved |
|---|---|---|
| Personal distribution history | Yes | Yes |
| Own tax statement | Yes | Yes |
| Capital account summary | Yes | No |
| Trustee notes | Yes | No |
| Other beneficiary details | Yes | No |

Appeal outcome:

```text
requested_sections = 5
approved_sections = 2
denied_sections = requested_sections - approved_sections = 3
approval_rate = 2 / 5 = 40%
```

Correct workflow:

- preserve appeal request, beneficiary class, governing document, trustee decision, approved sections, denied sections and masking rule version;
- expand visibility only for trustee-approved sections and effective dates;
- avoid exposing other beneficiaries, trustee deliberations or restricted capital information through consolidated report views;
- keep the appeal outcome auditable without disclosing restricted content in the denial reason.

QA assertions:

| Test | Expected result |
|---|---|
| Appeal is partially approved | Only approved sections become visible. |
| Restricted content is requested | Denied sections remain masked with audit evidence. |
| Beneficiary class changes | Visibility recalculates from effective-dated class and trustee decision. |
| Report is regenerated | Masking follows the active rule version and approved scope. |

## 46. Cross-Border Probate Delay

Scenario:

- An estate holds assets in two jurisdictions.
- Probate is granted domestically, but foreign probate remains delayed, so only part of the estate can be distributed.

| Estate component | Amount | Status |
|---|---:|---|
| Domestic liquid assets | 2,500,000 | Probate granted |
| Foreign investment account | 1,500,000 | Probate pending |
| Known liabilities | 600,000 | Payable |
| Retained contingency | 200,000 | Required |

Interim distributable amount:

```text
interim_distributable = domestic_liquid_assets - known_liabilities - retained_contingency
interim_distributable = 2,500,000 - 600,000 - 200,000 = 1,700,000
foreign_assets_locked = 1,500,000
```

Correct workflow:

- preserve death certificate, domestic grant, foreign probate status, executor approval, asset inventory and liability schedule;
- separate domestic distributable assets from foreign assets locked by probate delay;
- prevent final estate closure while foreign probate, tax or creditor steps remain unresolved;
- label interim distributions clearly and retain reserve for liabilities and cross-border costs.

QA assertions:

| Test | Expected result |
|---|---|
| Foreign probate is pending | Foreign asset value remains restricted and excluded from final distribution. |
| Domestic probate is granted | Interim distribution can proceed only within approved domestic liquidity. |
| Liabilities remain open | Distributable amount is reduced by liabilities and contingency. |
| Final estate report is requested | Report shows interim, restricted and pending-probate components separately. |

## 47. Family Governance Voting Deadlock

Scenario:

- A family investment committee votes on a strategic asset allocation change.
- Quorum is met, but votes are split evenly and the family charter requires a majority of eligible voting members.

| Attribute | Value |
|---|---:|
| Eligible voting members | 6 |
| Quorum requirement | 4 |
| Votes for | 3 |
| Votes against | 3 |
| Majority required | 4 |

Decision state:

```text
quorum_met = eligible_voting_members >= quorum_requirement
majority_met = votes_for >= majority_required
decision_deadlocked = quorum_met and majority_met == false and votes_for == votes_against
```

Correct workflow:

- preserve charter version, meeting notice, attendance, votes, conflicts, quorum rule and escalation mechanism;
- block the proposed mandate or allocation change when the majority threshold is not met;
- route deadlock to the tie-break, mediation, cooling-off or trustee decision process specified by the governance document;
- prevent implementation teams from treating quorum as approval when voting threshold fails.

QA assertions:

| Test | Expected result |
|---|---|
| Quorum is met but majority fails | Proposal remains not approved. |
| Votes are tied | Deadlock state opens with escalation route. |
| Charter has tie-break rule | Workflow routes to the specified decision mechanism. |
| Investment order is attempted | Order is blocked until governance approval is final. |

## 48. Trustee Fee Tier Dispute

Scenario:

- A trustee invoice applies a flat fee rate, but the governing fee schedule uses tiered pricing.
- The family office disputes the excess charge before allocating it to trust beneficiaries.

| Fee component | Amount |
|---|---:|
| Trust assets under administration | 25,000,000 |
| Flat fee charged at 0.50% | 125,000 |
| First tier: 0.50% on first 10,000,000 | 50,000 |
| Second tier: 0.35% on next 15,000,000 | 52,500 |

Disputed fee:

```text
contracted_tiered_fee = 50,000 + 52,500 = 102,500
disputed_fee = charged_fee - contracted_tiered_fee
disputed_fee = 125,000 - 102,500 = 22,500
```

Correct workflow:

- preserve trustee invoice, fee schedule, asset base, calculation version, dispute notice, credit-note status and allocation policy;
- separate charged fee, contracted fee, accepted fee and disputed fee;
- avoid allocating disputed fee to beneficiaries until dispute policy allows pass-through or absorption;
- report fee tiering and dispute status to authorized recipients only.

QA assertions:

| Test | Expected result |
|---|---|
| Invoice exceeds fee schedule | Disputed fee amount is calculated and exceptioned. |
| Credit note is received | Accepted and credited amounts reconcile to final fee. |
| Allocation is requested before resolution | Disputed amount is excluded or separately labelled by policy. |
| Report is generated | Fee schedule, charge and dispute status are auditable. |

## 49. Foundation Dissolution Distribution

Scenario:

- A foundation is dissolved after completing its purpose.
- Remaining assets must be distributed to approved successor recipients after liabilities, restricted reserves and wind-down costs.

| Component | Amount |
|---|---:|
| Foundation assets | 2,000,000 |
| Known liabilities | 300,000 |
| Restricted reserve | 100,000 |
| Wind-down costs | 50,000 |

Final distributable amount:

```text
final_distributable = foundation_assets - known_liabilities - restricted_reserve - wind_down_costs
final_distributable = 2,000,000 - 300,000 - 100,000 - 50,000 = 1,550,000
```

Correct workflow:

- preserve council approval, registry dissolution evidence, successor recipient due diligence, liability schedule, reserve basis and distribution plan;
- separate final distributable assets from liabilities, restricted reserves and wind-down costs;
- block distribution to recipients that do not satisfy purpose, eligibility, sanctions or due-diligence controls;
- close the foundation only after assets, liabilities, bank accounts, records and reports reconcile to zero or retained evidence state.

QA assertions:

| Test | Expected result |
|---|---|
| Liabilities remain open | Final distribution is reduced or blocked. |
| Successor recipient is not approved | Distribution to that recipient is blocked. |
| Restricted reserve remains required | Reserve is excluded from distributable amount. |
| Dissolution is completed | Final report shows distributions, retained evidence and closure state. |

## 50. Regression Test Pack

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
26. Protector succession blocks consent-dependent actions when no accepted protector is active.
27. Delegated investment-manager replacement freezes, cancels or reapproves pending orders across the effective date.
28. Beneficiary loan workflow separates loan exposure from distributions and enforces limits.
29. Family charter amendment applies the correct rule version and effective date.
30. Multi-branch reporting restrictions preserve consolidated and recipient-specific reporting perimeters.
31. Charitable pledge cancellation stops future installments while preserving paid-grant evidence.
32. Excluded-person controls block distributions and report access without leaking restricted trust details.
33. Foundation redomiciliation updates domicile, governing law, documentation and reporting from the effective date.
34. Emergency trustee powers allow only scoped protective actions and require post-event ratification.
35. Private-trust-company board changes recalculate quorum, signing authority and pending approval validity.
36. Beneficiary consent campaigns distinguish consent-required, notice-only and represented beneficiaries.
37. Family-office expense allocation disputes separate accepted, disputed and reallocated cost amounts.
38. Reserved investment powers after incapacity suspend settlor-directed investment authority and re-route pending instructions.
39. Letter-of-wishes disclosure disputes preserve trustee disclosure decisions without exposing restricted content.
40. Trust-to-company asset transfers update entity-level holdings while preserving consolidated reporting continuity.
41. Protector conflict recusal removes conflicted protector approval for scoped transactions and routes permitted alternate authority.
42. Family-office service-level chargebacks apply service-tier-specific allocation and preserve dispute evidence.
43. Estate liquidity reserve releases separate settled obligations, retained contingency and approved beneficiary release.
44. Purpose trust operating budgets separate committed spend, proposed spend, contingency and supplemental approval.
45. Beneficiary privacy masking appeals expand only trustee-approved report sections and preserve restricted masking.
46. Cross-border probate delays separate domestic interim distributable assets from foreign probate-restricted assets.
47. Family governance voting deadlocks distinguish quorum from approval threshold and block implementation until resolved.
48. Trustee fee tier disputes separate charged, contracted, accepted and disputed fee amounts before allocation.
49. Foundation dissolution distributions deduct liabilities, restricted reserves and wind-down costs before recipient distribution.

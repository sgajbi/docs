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

## 50. Protector Indemnity Claim

Scenario:

- A protector claims indemnity from the trust after defending a beneficiary challenge.
- The trust deed permits indemnity only for approved costs that are not caused by willful misconduct.
- Part of the claim is disputed by the trustee.

| Component | Amount |
|---|---:|
| Claimed legal costs | 95,000 |
| Trustee-approved costs | 72,000 |
| Disputed costs | 23,000 |
| Retained reserve | 80,000 |

Indemnity reserve coverage:

```text
approved_indemnity_payable = trustee_approved_costs
approved_indemnity_payable = 72,000

remaining_reserve_after_approved_payment = retained_reserve - approved_indemnity_payable
remaining_reserve_after_approved_payment = 80,000 - 72,000 = 8,000
```

Correct workflow:

- preserve trust deed indemnity clause, protector claim, legal invoices, trustee review, misconduct exclusions and dispute owner;
- pay or reserve only approved indemnity amounts according to trustee authority;
- keep disputed costs separate from approved payable amounts;
- reflect reserve impact before beneficiary distributions or trust closure;
- avoid treating protector indemnity as beneficiary distribution or trustee fee.

QA assertions:

| Test | Expected result |
|---|---|
| Claim includes disputed costs | Approved and disputed amounts are separated. |
| Reserve is insufficient | Distribution or closure workflow is blocked or reprioritized. |
| Misconduct exclusion applies | Claim is rejected or routed to legal review. |
| Report is generated | Claim, approval, reserve and dispute state are auditable. |

## 51. Family-Office Capital Commitment Approval

Scenario:

- A family office investment committee approves a private-market capital commitment.
- The family charter requires a minimum approval threshold and liquidity reserve check before commitment.
- The proposed commitment exceeds available investment headroom unless a co-investment sleeve is excluded.

| Attribute | Value |
|---|---:|
| Proposed commitment | 1,500,000 |
| Available investment headroom | 1,250,000 |
| Co-investment sleeve excluded by policy | 300,000 |
| Required approval threshold | 75% |
| Approval received | 80% |

Adjusted headroom:

```text
adjusted_available_headroom = available_investment_headroom + policy_excluded_sleeve
adjusted_available_headroom = 1,250,000 + 300,000 = 1,550,000

commitment_headroom_surplus = adjusted_available_headroom - proposed_commitment
commitment_headroom_surplus = 1,550,000 - 1,500,000 = 50,000
```

Correct workflow:

- preserve investment memo, family charter rule, committee vote, liquidity reserve check, excluded sleeve policy and final approval;
- verify both approval threshold and funding headroom before commitment activation;
- separate approved commitment from funded capital calls;
- show residual headroom after commitment for future planning;
- avoid booking a commitment only from verbal committee support without documented authority.

QA assertions:

| Test | Expected result |
|---|---|
| Approval threshold is met | Approval check passes with vote evidence. |
| Headroom is insufficient before policy adjustment | Commitment remains blocked or requires policy-backed exclusion. |
| Commitment is approved | Commitment posts separately from cash funding. |
| Report is generated | Proposed commitment, approval and remaining headroom are visible. |

## 52. Estate In-Kind Asset Distribution

Scenario:

- An estate distributes securities in kind to beneficiaries instead of liquidating them.
- Beneficiary allocations must preserve asset value, fractional handling and restricted-asset exclusions.

| Attribute | Value |
|---|---:|
| Distributable securities value | 900,000 |
| Beneficiary A allocation | 60% |
| Beneficiary B allocation | 40% |
| Restricted securities excluded | 120,000 |

Distributable value after restriction:

```text
available_in_kind_value = distributable_securities_value - restricted_securities_excluded
available_in_kind_value = 900,000 - 120,000 = 780,000

beneficiary_a_in_kind_value = available_in_kind_value x beneficiary_a_allocation
beneficiary_a_in_kind_value = 780,000 x 60% = 468,000

beneficiary_b_in_kind_value = available_in_kind_value x beneficiary_b_allocation
beneficiary_b_in_kind_value = 780,000 x 40% = 312,000
```

Correct workflow:

- preserve probate approval, executor instruction, asset inventory, restriction flags, beneficiary allocation rule and transfer evidence;
- exclude restricted assets until release or court/executor approval exists;
- transfer securities with custody, tax-lot and valuation lineage;
- handle fractional or indivisible securities through approved equalization cash where needed;
- avoid reporting in-kind distribution as cash liquidation.

QA assertions:

| Test | Expected result |
|---|---|
| Restricted assets exist | Restricted value is excluded from available in-kind distribution. |
| Beneficiary percentages apply | In-kind values allocate according to approved estate rule. |
| Custody transfer completes | Beneficiary holdings link to estate source asset and transfer evidence. |
| Report is generated | In-kind transfer, restricted assets and any equalization cash are distinct. |

## 53. Foundation Grant Clawback

Scenario:

- A foundation grant agreement permits clawback when the recipient fails purpose-use conditions.
- Part of the grant has already been spent for approved purposes.
- The clawback applies only to unapproved or unused amounts.

| Component | Amount |
|---|---:|
| Original grant | 400,000 |
| Approved purpose spend | 250,000 |
| Unused cash returned voluntarily | 60,000 |
| Disputed non-compliant spend | 90,000 |

Remaining clawback claim:

```text
remaining_clawback_claim = original_grant - approved_purpose_spend - unused_cash_returned
remaining_clawback_claim = 400,000 - 250,000 - 60,000 = 90,000
```

Correct workflow:

- preserve grant agreement, purpose conditions, recipient spend evidence, voluntary return, council clawback decision and dispute owner;
- separate approved spend, returned cash and clawback claim;
- recognize returned cash only when settlement is received;
- track disputed clawback until recipient resolution or legal decision;
- avoid treating clawback claim as immediately available foundation cash.

QA assertions:

| Test | Expected result |
|---|---|
| Purpose condition fails | Clawback workflow opens with evidence. |
| Some funds are returned | Returned cash reduces claim only after receipt evidence. |
| Spend is approved | Approved spend is excluded from clawback claim. |
| Report is generated | Grant, approved spend, returned cash and claim are distinct. |

## 54. Private-Trust-Company Director Indemnity Reserve

Scenario:

- A private trust company establishes an indemnity reserve for directors before approving a complex restructuring.
- The reserve reduces distributable cash and must remain until the indemnity exposure expires or is released.

| Attribute | Value |
|---|---:|
| Available company cash | 1,100,000 |
| Approved indemnity reserve | 275,000 |
| Pending trustee distributions | 600,000 |
| Reserve release status | Not released |

Post-reserve liquidity:

```text
cash_after_indemnity_reserve = available_company_cash - approved_indemnity_reserve
cash_after_indemnity_reserve = 1,100,000 - 275,000 = 825,000

surplus_after_pending_distributions = cash_after_indemnity_reserve - pending_trustee_distributions
surplus_after_pending_distributions = 825,000 - 600,000 = 225,000
```

Correct workflow:

- preserve board approval, indemnity terms, restructuring matter, reserve amount, expiry/release condition and distribution impact;
- exclude reserve from distributable cash until release evidence exists;
- separate director indemnity reserve from trustee fee, operating expense and beneficiary distribution;
- show liquidity impact before approving distributions;
- avoid releasing reserve only because the transaction has closed if indemnity tail remains active.

QA assertions:

| Test | Expected result |
|---|---|
| Reserve is approved | Distributable cash reduces by reserve amount. |
| Distribution is requested | Distribution uses post-reserve liquidity. |
| Release condition is unmet | Reserve remains restricted. |
| Report is generated | Cash, reserve, distributions and surplus are visible. |

## 55. Beneficiary Address Confidentiality Override

Scenario:

- A beneficiary requests that their address remain confidential in family-office consolidated reports.
- A trustee later approves a limited override for legal-service delivery only.
- The platform must not expose the address in general beneficiary reporting.

| Attribute | Value |
|---|---:|
| Reports in scope | 8 |
| Reports approved for override | 1 |
| Reports still masked | 7 |

Masking outcome:

```text
masked_report_count = reports_in_scope - reports_approved_for_override
masked_report_count = 8 - 1 = 7
```

Correct workflow:

- preserve confidentiality request, trustee approval, override purpose, report scope, effective period and recipient restrictions;
- apply override only to the approved report or service workflow;
- keep default masking active for all other outputs;
- audit who viewed or received the unmasked address;
- avoid using address override as broad consent for other family-office reports.

QA assertions:

| Test | Expected result |
|---|---|
| Override is purpose-limited | Address is unmasked only for approved purpose and report. |
| Other reports are generated | Address remains masked outside override scope. |
| Override expires | Address returns to default masking. |
| Audit is requested | Access and delivery of unmasked address are traceable. |

## 56. Protector Indemnity Reserve Release Dispute

Scenario:

- A protector indemnity reserve was retained after a disputed legal claim.
- The protector requests release of the unused reserve after approved costs are paid.
- The trustee disputes part of the release because an appeal period is still open.

| Component | Amount |
|---|---:|
| Original retained reserve | 80,000 |
| Approved indemnity paid | 72,000 |
| Protector requested release | 8,000 |
| Trustee-disputed release | 3,000 |

Release amount:

```text
potential_reserve_release = original_retained_reserve - approved_indemnity_paid
potential_reserve_release = 80,000 - 72,000 = 8,000

undisputed_release_amount = potential_reserve_release - trustee_disputed_release
undisputed_release_amount = 8,000 - 3,000 = 5,000
```

Correct workflow:

- preserve indemnity clause, approved payment, reserve ledger, protector release request, appeal-period evidence and trustee decision;
- release only undisputed reserve amounts until appeal exposure is resolved;
- keep reserve release separate from beneficiary distribution and trustee fee treatment;
- show disputed reserve in closure and liquidity reporting;
- avoid treating payment of approved costs as automatic release of all remaining reserve.

QA assertions:

| Test | Expected result |
|---|---|
| Appeal period remains open | Disputed reserve stays restricted. |
| Trustee approves partial release | Only undisputed release is made available. |
| Appeal period closes | Remaining reserve is released or retained from final evidence. |
| Report is generated | Original reserve, paid costs, released amount and disputed reserve reconcile. |

## 57. Family-Office Co-Investment Overcommitment Control

Scenario:

- A family office approves a co-investment that shares liquidity headroom with existing private-market commitments.
- The proposed allocation would exceed the family charter overcommitment limit unless an excluded sleeve is formally approved.

| Attribute | Value |
|---|---:|
| Existing commitments | 7,800,000 |
| Proposed co-investment | 1,400,000 |
| Maximum allowed commitments | 9,000,000 |
| Excluded sleeve approved | 300,000 |

Overcommitment amount:

```text
adjusted_commitments_after_exclusion = existing_commitments + proposed_co_investment - excluded_sleeve_approved
adjusted_commitments_after_exclusion = 7,800,000 + 1,400,000 - 300,000 = 8,900,000

remaining_commitment_headroom = maximum_allowed_commitments - adjusted_commitments_after_exclusion
remaining_commitment_headroom = 9,000,000 - 8,900,000 = 100,000
```

Correct workflow:

- preserve family charter limit, investment memo, co-investment terms, excluded sleeve approval, liquidity reserve and committee vote;
- test overcommitment before approving the co-investment;
- separate commitment approval from capital-call funding;
- block or escalate if adjusted commitments exceed the approved limit;
- retain residual headroom for future commitment planning.

QA assertions:

| Test | Expected result |
|---|---|
| Proposed co-investment exceeds limit before exclusion | Approval requires valid exclusion or escalation. |
| Excluded sleeve is approved | Commitment test uses adjusted exposure. |
| Capital call arrives later | Funding is tested separately from commitment approval. |
| Report is generated | Existing commitments, proposed commitment, exclusions and headroom are visible. |

## 58. Estate In-Kind Equalization Cash Shortfall

Scenario:

- An estate distributes concentrated securities in kind.
- Beneficiary allocations require equalization cash because securities are indivisible.
- The estate liquidity reserve is insufficient to fund the equalization amount.

| Attribute | Value |
|---|---:|
| Required equalization cash | 85,000 |
| Available estate cash after reserves | 52,000 |
| Equalization shortfall | 33,000 |
| Restricted assets excluded | 120,000 |

Equalization shortfall:

```text
equalization_cash_shortfall = required_equalization_cash - available_estate_cash_after_reserves
equalization_cash_shortfall = 85,000 - 52,000 = 33,000
```

Correct workflow:

- preserve executor instruction, in-kind allocation schedule, reserve policy, restricted-asset list, equalization calculation and beneficiary approval;
- transfer only supportable in-kind assets while equalization cash remains unresolved;
- route shortfall to sale, reserve release, beneficiary waiver or revised allocation decision;
- keep equalization cash separate from estate expenses and beneficiary distributions;
- avoid reporting unequal distributions as final while shortfall is open.

QA assertions:

| Test | Expected result |
|---|---|
| Equalization cash exceeds available estate cash | Shortfall is opened. |
| Beneficiary waiver is approved | Equalization obligation updates from waiver evidence. |
| Asset sale funds equalization | Cash settlement links to sale and allocation schedule. |
| Estate report is generated | In-kind values, equalization cash and shortfall are distinct. |

## 59. Foundation Grant Restriction Breach Remediation

Scenario:

- A foundation grant recipient breaches a purpose restriction.
- The foundation council approves remediation instead of immediate clawback.
- The remediated amount must be tracked separately from approved spend and unresolved breach exposure.

| Component | Amount |
|---|---:|
| Original grant | 400,000 |
| Restricted-purpose breach amount | 90,000 |
| Remediation accepted | 55,000 |
| Unresolved breach exposure | 35,000 |

Remediation residual:

```text
unresolved_breach_exposure = restricted_purpose_breach_amount - remediation_accepted
unresolved_breach_exposure = 90,000 - 55,000 = 35,000
```

Correct workflow:

- preserve grant agreement, purpose restriction, breach evidence, recipient remediation plan, council decision and monitoring owner;
- separate approved spend, accepted remediation, unresolved breach and clawback exposure;
- keep unresolved exposure out of available foundation cash until recovered or waived;
- track remediation deadlines and evidence submissions;
- avoid closing the breach solely because a remediation plan was proposed.

QA assertions:

| Test | Expected result |
|---|---|
| Remediation is approved | Accepted remediation reduces breach exposure. |
| Remediation is incomplete | Residual breach remains open. |
| Clawback is triggered later | Claim links to unresolved breach evidence. |
| Report is generated | Grant, breach, remediation and residual exposure reconcile. |

## 60. Private-Trust-Company Director Resignation Authority Gap

Scenario:

- A private trust company director resigns before a pending restructuring approval is completed.
- The board still meets headcount requirements but loses a required specialist signatory.
- Pending approvals must be revalidated against the post-resignation authority matrix.

| Attribute | Value |
|---|---:|
| Required signatories | 3 |
| Valid signatories after resignation | 2 |
| Pending approvals affected | 4 |
| Interim delegate appointments | 1 |

Authority gap:

```text
signatory_shortfall = required_signatories - valid_signatories_after_resignation
signatory_shortfall = 3 - 2 = 1
```

Correct workflow:

- preserve resignation notice, board register, authority matrix, pending approvals, delegate appointment and effective date;
- suspend affected approvals until the authority gap is cured;
- distinguish quorum sufficiency from specialist signatory sufficiency;
- revalidate approvals signed before and after the resignation effective date;
- avoid carrying forward stale director authority on pending actions.

QA assertions:

| Test | Expected result |
|---|---|
| Director resigns before approval completion | Pending approvals are revalidated. |
| Board quorum remains valid | Specialist signatory gap is still flagged. |
| Interim delegate is appointed | Authority gap closes only for delegated scope. |
| Approval report is generated | Resignation, authority gap and pending approval impact are visible. |

## 61. Beneficiary Contact-Data Redaction Appeal

Scenario:

- A beneficiary appeals redaction of contact data in a family-office consolidated report.
- The trustee approves email disclosure for coordination but denies address and phone disclosure.
- Reporting must apply field-level redaction, not all-or-nothing access.

| Attribute | Value |
|---|---:|
| Contact fields requested | 3 |
| Fields approved for disclosure | 1 |
| Fields remaining redacted | 2 |

Redaction outcome:

```text
remaining_redacted_fields = contact_fields_requested - fields_approved_for_disclosure
remaining_redacted_fields = 3 - 1 = 2
```

Correct workflow:

- preserve redaction request, trustee decision, approved fields, denied fields, purpose, recipients and expiry;
- disclose only approved contact fields for the approved report or workflow;
- keep denied fields masked in consolidated reports and exports;
- retain audit evidence for every unmasked field delivery;
- avoid treating approval for one contact field as consent to disclose all contact data.

QA assertions:

| Test | Expected result |
|---|---|
| Field-level approval is partial | Only approved fields are unmasked. |
| Report is outside approved purpose | All restricted fields remain masked. |
| Approval expires | Fields return to default redaction. |
| Audit is requested | Delivered fields, recipients and purpose are traceable. |

## 62. Protector Reserve Release Appeal Closure

Scenario:

- A protector indemnity reserve was retained while a beneficiary appeal was open.
- The appeal is now closed with trustee approval to release part of the reserve.
- A residual amount remains restricted for final cost true-up and appeal-period evidence retention.

| Attribute | Value |
|---|---:|
| Appeal-period reserve | 400,000 |
| Trustee-approved release | 250,000 |
| Residual true-up reserve | 150,000 |

Reserve closure:

```text
residual_true_up_reserve = appeal_period_reserve - trustee_approved_release
residual_true_up_reserve = 400,000 - 250,000 = 150,000
```

Correct workflow:

- preserve original reserve approval, appeal closure, trustee release approval, beneficiary notice, residual reserve basis and evidence retention date;
- move released cash from restricted reserve to distributable cash only after the appeal closure is effective;
- keep residual reserve restricted until cost true-up, expiry or separate trustee approval;
- produce a closure audit trail showing reserve opening, appeal state, release approval and remaining restriction;
- avoid releasing the full reserve merely because the appeal status changed to closed.

QA assertions:

| Test | Expected result |
|---|---|
| Appeal closes without trustee release approval | Reserve remains restricted. |
| Partial release is approved | Only the approved amount becomes distributable. |
| Residual reserve has no basis | Workflow requires trustee confirmation or release. |
| Closure report is generated | Opening reserve, release, residual restriction and evidence dates are visible. |

## 63. Family-Office Co-Investment Syndication Reallocation

Scenario:

- A family-office co-investment was approved with an initial allocation.
- One participating entity reduces its allocation after syndication.
- The investment committee reallocates part of the released amount to waitlisted entities and leaves the balance uncommitted.

| Attribute | Value |
|---|---:|
| Original approved allocation | 2,000,000 |
| Allocation surrendered | 500,000 |
| Reallocated to waitlisted entities | 350,000 |
| Uncommitted balance | 150,000 |

Reallocation:

```text
uncommitted_balance = allocation_surrendered - reallocated_to_waitlisted_entities
uncommitted_balance = 500,000 - 350,000 = 150,000
```

Correct workflow:

- preserve original allocation approval, surrender notice, waitlist priority, committee reallocation approval, entity-level funding evidence and revised commitment ledger;
- validate that each recipient entity has authority, eligibility, funding headroom and concentration capacity;
- reduce the surrendering entity's commitment before increasing recipient commitments;
- keep uncommitted balance out of portfolio exposure, capital-call forecasts and fee allocation;
- avoid reallocating released capacity without a fresh committee decision and recipient-level checks.

QA assertions:

| Test | Expected result |
|---|---|
| Recipient lacks funding headroom | Reallocation is blocked for that recipient. |
| Surrender is not effective | Original commitment remains active. |
| Partial reallocation is approved | Uncommitted balance is tracked separately. |
| Capital-call forecast is recalculated | Forecast uses revised commitment by entity. |

## 64. Estate Equalization Cash Waiver Reversal

Scenario:

- A beneficiary previously waived part of an equalization cash payment linked to an in-kind estate distribution.
- The waiver is reversed after a valid dispute outcome.
- Replacement funding is partial, so the estate cannot close the final distribution.

| Attribute | Value |
|---|---:|
| Reversed waiver amount | 300,000 |
| Replacement cash funded | 120,000 |
| Residual equalization shortfall | 180,000 |

Shortfall:

```text
residual_equalization_shortfall = reversed_waiver_amount - replacement_cash_funded
residual_equalization_shortfall = 300,000 - 120,000 = 180,000
```

Correct workflow:

- preserve waiver, reversal decision, dispute outcome, funding source, beneficiary entitlement basis and revised distribution schedule;
- re-open the equalization obligation from the reversal effective date;
- block final estate closure while the residual shortfall remains unresolved;
- distinguish funded replacement cash from unfunded equalization receivable;
- avoid treating a historical waiver as permanent when a valid reversal decision exists.

QA assertions:

| Test | Expected result |
|---|---|
| Waiver reversal is accepted | Equalization obligation is re-opened. |
| Replacement funding is partial | Residual shortfall blocks final distribution. |
| Beneficiary report is generated | Funded and unfunded equalization amounts are separated. |
| Estate closure is requested | Closure is blocked until shortfall is funded, waived again or reallocated. |

## 65. Foundation Remediation Deadline Breach

Scenario:

- A foundation grant restriction breach had an approved remediation plan.
- The recipient completed part of the remediation before the deadline.
- The remaining amount is overdue and must be escalated for council review.

| Attribute | Value |
|---|---:|
| Required remediation by deadline | 500,000 |
| Accepted remediation completed on time | 300,000 |
| Overdue remediation exposure | 200,000 |

Deadline breach:

```text
overdue_remediation_exposure = required_remediation_by_deadline - accepted_remediation_completed_on_time
overdue_remediation_exposure = 500,000 - 300,000 = 200,000
```

Correct workflow:

- preserve remediation plan, deadline, accepted evidence, overdue exposure, escalation owner, council decision and recipient communication;
- keep accepted remediation separate from overdue remediation exposure;
- escalate overdue exposure to council, legal or grant monitoring workflow as required by governing documents;
- suspend further grant releases when the plan requires completion before additional funding;
- avoid marking the original breach resolved until overdue remediation is accepted or formally waived.

QA assertions:

| Test | Expected result |
|---|---|
| Deadline passes with partial remediation | Overdue exposure is calculated. |
| Further grant release is requested | Release is blocked when completion is a condition precedent. |
| Council grants extension | Overdue state changes only within approved extension scope. |
| Monitoring report is produced | Completed, overdue and escalated amounts are visible. |

## 66. Director Resignation Delegated-Authority Expiry

Scenario:

- A private-trust-company director resigned and an interim delegate was appointed.
- The delegation expired before all pending approvals were completed.
- Remaining approvals must be re-routed to a current authority path.

| Attribute | Value |
|---|---:|
| Pending approvals under delegated authority | 7 |
| Approvals completed before delegation expiry | 4 |
| Approvals requiring reapproval | 3 |

Reapproval requirement:

```text
approvals_requiring_reapproval = pending_approvals_under_delegated_authority - approvals_completed_before_delegation_expiry
approvals_requiring_reapproval = 7 - 4 = 3
```

Correct workflow:

- preserve resignation notice, delegation instrument, delegation scope, expiry, pending approvals, completed approvals and replacement authority route;
- validate each pending approval against delegation scope and effective dates;
- freeze or re-route approvals that were not completed before delegation expiry;
- distinguish valid approvals completed during the delegated period from approvals requiring fresh authority;
- avoid extending delegated authority implicitly because the underlying transaction is still open.

QA assertions:

| Test | Expected result |
|---|---|
| Approval completed before delegation expiry | Approval remains valid if in scope. |
| Approval remains pending after expiry | Approval requires reapproval or alternate authority. |
| Delegation scope excludes transaction type | Approval is blocked even before expiry. |
| Authority report is generated | Completed, expired and re-routed approvals are separated. |

## 67. Beneficiary Redaction Appeal Expiry Dispute

Scenario:

- A beneficiary had temporary approval to unmask selected contact fields for a coordination workflow.
- The approval expired, but an extension dispute is pending.
- Reporting must return fields to masked state unless a current approval exists.

| Attribute | Value |
|---|---:|
| Temporarily unmasked fields | 2 |
| Fields with current extension approval | 0 |
| Fields to remask | 2 |

Expiry treatment:

```text
fields_to_remask = temporarily_unmasked_fields - fields_with_current_extension_approval
fields_to_remask = 2 - 0 = 2
```

Correct workflow:

- preserve original approval, approved fields, purpose, expiry, extension request, dispute state, trustee decision and report delivery evidence;
- remask fields immediately after expiry unless current extension approval exists;
- keep the extension dispute visible to operations without exposing restricted contact data in reports;
- apply the outcome prospectively unless trustee decision explicitly authorizes retrospective disclosure;
- avoid treating a pending extension request as active permission to disclose.

QA assertions:

| Test | Expected result |
|---|---|
| Approval expires with no extension | Previously unmasked fields are remasked. |
| Extension dispute is pending | Reports remain masked until approval exists. |
| Partial extension is approved | Only approved fields remain unmasked. |
| Audit is requested | Original approval, expiry and dispute state are traceable. |

## 68. Protector Residual Reserve True-Up

Scenario:

- A protector reserve was partly released after appeal closure.
- Final legal and advisory costs are lower than the retained true-up reserve.
- The excess reserve can be released only after the trustee accepts final cost evidence.

| Attribute | Value |
|---|---:|
| Residual true-up reserve | 150,000 |
| Accepted final costs | 90,000 |
| Excess reserve available for release | 60,000 |

True-up:

```text
excess_reserve_available_for_release = residual_true_up_reserve - accepted_final_costs
excess_reserve_available_for_release = 150,000 - 90,000 = 60,000
```

Correct workflow:

- preserve residual reserve basis, final invoices, trustee acceptance, cost allocation, beneficiary notice and release approval;
- pay or retain accepted costs before calculating excess reserve;
- release only the excess amount approved by trustee decision;
- keep cost evidence linked to the original reserve and appeal closure;
- avoid treating unused reserve as automatically distributable before cost evidence is accepted.

QA assertions:

| Test | Expected result |
|---|---|
| Final costs are not accepted | Residual reserve remains restricted. |
| Accepted costs are below reserve | Excess release amount is calculated. |
| Accepted costs exceed reserve | Shortfall is escalated instead of releasing cash. |
| Beneficiary statement is generated | Cost usage and excess release are shown separately. |

## 69. Co-Investment Waitlist Priority Dispute

Scenario:

- A surrendered co-investment allocation is available for syndication.
- Two eligible entities claim priority from the waitlist.
- The investment committee approves reallocation using documented waitlist order and keeps the disputed balance pending.

| Attribute | Value |
|---|---:|
| Surrendered allocation | 600,000 |
| Priority allocation approved | 400,000 |
| Disputed balance pending committee decision | 200,000 |

Priority dispute:

```text
disputed_balance_pending_committee_decision = surrendered_allocation - priority_allocation_approved
disputed_balance_pending_committee_decision = 600,000 - 400,000 = 200,000
```

Correct workflow:

- preserve waitlist timestamp, eligibility evidence, committee decision, dispute notice, recipient headroom and revised allocation ledger;
- allocate only the approved priority amount while the dispute is pending;
- exclude pending disputed balance from commitment exposure and capital-call forecasts;
- report the dispute state without implying final participation rights;
- avoid reallocating disputed balance based only on relationship seniority or informal instruction.

QA assertions:

| Test | Expected result |
|---|---|
| Waitlist priority is documented | Approved priority allocation is allowed. |
| Disputed balance remains unresolved | Balance stays pending and uncommitted. |
| Recipient lacks concentration headroom | Allocation is blocked even with waitlist priority. |
| Forecast is produced | Only approved allocations enter capital-call projections. |

## 70. Estate Equalization Replacement Funding Failure

Scenario:

- A reversed waiver created a renewed equalization cash obligation.
- Replacement funding was scheduled from estate liquidity.
- The funding payment failed, leaving the estate distribution package incomplete.

| Attribute | Value |
|---|---:|
| Replacement funding scheduled | 180,000 |
| Replacement funding settled | 0 |
| Funding failure exposure | 180,000 |

Funding failure:

```text
funding_failure_exposure = replacement_funding_scheduled - replacement_funding_settled
funding_failure_exposure = 180,000 - 0 = 180,000
```

Correct workflow:

- preserve funding instruction, settlement status, failed-payment reason, beneficiary entitlement, executor decision and revised estate timetable;
- keep the equalization obligation open until funding settles, is waived again or is reallocated;
- prevent final distribution and estate closure while funding failure exposure remains;
- separate operational payment failure from legal entitlement dispute;
- avoid marking equalization as funded based on scheduled payment alone.

QA assertions:

| Test | Expected result |
|---|---|
| Payment is scheduled but not settled | Equalization remains unfunded. |
| Payment fails after cut-off | Estate closure remains blocked. |
| Executor approves alternate funding | Obligation moves to alternate funding workflow. |
| Beneficiary report is generated | Scheduled, settled and failed funding are separated. |

## 71. Foundation Remediation Extension Waiver

Scenario:

- A remediation deadline was breached for a restricted-purpose foundation grant.
- The foundation council grants an extension waiver for part of the overdue exposure.
- The balance remains overdue and blocks further grant releases.

| Attribute | Value |
|---|---:|
| Overdue remediation exposure | 200,000 |
| Extension waiver approved | 120,000 |
| Residual overdue exposure | 80,000 |

Extension waiver:

```text
residual_overdue_exposure = overdue_remediation_exposure - extension_waiver_approved
residual_overdue_exposure = 200,000 - 120,000 = 80,000
```

Correct workflow:

- preserve breach record, extension request, council waiver, revised deadline, residual overdue exposure and grant-release condition;
- apply the waiver only to the approved amount and revised deadline;
- keep residual overdue exposure escalated until remediated or separately waived;
- block future releases when grant terms require full remediation;
- avoid converting an extension waiver into a full breach cure.

QA assertions:

| Test | Expected result |
|---|---|
| Partial extension waiver is approved | Only waived amount moves to extended status. |
| Residual exposure remains overdue | Escalation and release block remain active. |
| Revised deadline passes | Waived amount becomes overdue again unless remediated. |
| Council report is generated | Waived, residual and release-blocked amounts are visible. |

## 72. Delegated-Authority Renewal Conflict

Scenario:

- A delegated authority expired while approvals were pending.
- Two renewal instruments conflict on scope and effective date.
- Governance must route pending approvals to the most restrictive valid authority until the conflict is resolved.

| Attribute | Value |
|---|---:|
| Pending approvals at renewal date | 5 |
| Approvals covered by both renewal instruments | 2 |
| Approvals requiring conflict resolution | 3 |

Renewal conflict:

```text
approvals_requiring_conflict_resolution = pending_approvals_at_renewal_date - approvals_covered_by_both_renewal_instruments
approvals_requiring_conflict_resolution = 5 - 2 = 3
```

Correct workflow:

- preserve expired delegation, renewal instruments, scope comparison, effective dates, legal review, pending approvals and interim authority route;
- allow only approvals covered by unconflicted current authority;
- freeze or re-route approvals where renewal scope conflicts;
- show conflict state in approval dashboards and audit reports;
- avoid choosing the broader renewal instrument without formal conflict resolution.

QA assertions:

| Test | Expected result |
|---|---|
| Renewal scopes conflict | Conflicted approvals are blocked or re-routed. |
| Approval is covered by both instruments | Approval may proceed if other controls pass. |
| Effective dates overlap ambiguously | Legal or governance review is required. |
| Authority dashboard is viewed | Conflict status and affected approvals are visible. |

## 73. Beneficiary Redaction Retrospective Disclosure Control

Scenario:

- A trustee later approves retrospective disclosure of one previously masked contact field.
- Historical reports already delivered must not be silently overwritten.
- A correction package must show the approved field and preserve original delivery evidence.

| Attribute | Value |
|---|---:|
| Previously masked fields under appeal | 3 |
| Fields retrospectively approved | 1 |
| Fields remaining masked | 2 |

Retrospective disclosure:

```text
fields_remaining_masked = previously_masked_fields_under_appeal - fields_retrospectively_approved
fields_remaining_masked = 3 - 1 = 2
```

Correct workflow:

- preserve original report delivery, masking decision, retrospective trustee approval, approved field, recipients, correction package and remaining restrictions;
- issue a controlled correction or supplemental disclosure instead of modifying historical report evidence;
- unmask only the retrospectively approved field and only for approved recipients;
- maintain original masked report as delivered evidence;
- avoid treating retrospective approval as permission to expose all appealed contact fields.

QA assertions:

| Test | Expected result |
|---|---|
| Retrospective approval covers one field | Only that field appears in the correction package. |
| Historical report is queried | Original delivery evidence remains unchanged. |
| Recipient was not approved | Field remains masked for that recipient. |
| Audit report is generated | Original masking, approval and correction delivery are traceable. |

## 74. Protector Cost Shortfall Escalation

Scenario:

- Final protector indemnity costs exceed the residual true-up reserve.
- The trustee must decide whether to fund the shortfall from distributable cash, reject the excess cost, or seek further beneficiary notice.
- Distribution must remain blocked for the disputed shortfall amount until governance is complete.

| Attribute | Value |
|---|---:|
| Residual true-up reserve | 150,000 |
| Accepted final costs submitted | 190,000 |
| Cost shortfall requiring escalation | 40,000 |

Shortfall:

```text
cost_shortfall_requiring_escalation = accepted_final_costs_submitted - residual_true_up_reserve
cost_shortfall_requiring_escalation = 190,000 - 150,000 = 40,000
```

Correct workflow:

- preserve final cost submission, reserve balance, trustee review, disputed cost classification, beneficiary notice and funding decision;
- keep the shortfall amount blocked from final distribution until trustee approval or cost rejection;
- distinguish reserve exhaustion from approval to fund additional costs;
- update beneficiary reporting with reserve used, shortfall pending and final decision;
- avoid funding excess costs from distributable cash without an explicit authority path.

QA assertions:

| Test | Expected result |
|---|---|
| Costs exceed residual reserve | Shortfall is escalated and distribution is blocked for that amount. |
| Trustee rejects excess costs | Shortfall is removed and no additional reserve is created. |
| Trustee approves additional funding | Funding source and beneficiary notice are recorded. |
| Statement is generated | Reserve use and shortfall state are reported separately. |

## 75. Co-Investment Concentration Breach Cure

Scenario:

- A reallocated co-investment amount causes one family entity to exceed its concentration limit.
- The committee approves a cure by reducing the allocation and transferring the excess to a second eligible entity.
- The exposure model must use post-cure allocations, not the originally breached allocation.

| Attribute | Value |
|---|---:|
| Allocation before cure | 1,200,000 |
| Permitted concentration capacity | 950,000 |
| Allocation requiring cure | 250,000 |

Cure amount:

```text
allocation_requiring_cure = allocation_before_cure - permitted_concentration_capacity
allocation_requiring_cure = 1,200,000 - 950,000 = 250,000
```

Correct workflow:

- preserve breached allocation, concentration policy, cure approval, recipient entity, revised allocation ledger and exposure recalculation;
- reduce the breached entity before increasing another entity's allocation;
- validate recipient eligibility, headroom and authority before accepting the cure;
- recalculate concentration, commitment, fee and capital-call views after the cure;
- avoid reporting a cured position using the pre-cure breached allocation.

QA assertions:

| Test | Expected result |
|---|---|
| Allocation exceeds concentration capacity | Breach is detected before commitment finalization. |
| Cure recipient lacks eligibility | Cure transfer is blocked. |
| Cure is approved | Exposure and commitment views use revised allocation. |
| Advisor report is produced | Original breach, cure action and final exposure are visible. |

## 76. Estate Equalization Alternate Asset Substitution

Scenario:

- Equalization cash cannot be fully funded after a waiver reversal.
- The executor proposes substituting a liquid security position for part of the cash shortfall.
- The substitution is accepted only up to the approved valuation and transferable amount.

| Attribute | Value |
|---|---:|
| Equalization cash shortfall | 180,000 |
| Approved substitute asset value | 125,000 |
| Residual cash shortfall | 55,000 |

Substitution:

```text
residual_cash_shortfall = equalization_cash_shortfall - approved_substitute_asset_value
residual_cash_shortfall = 180,000 - 125,000 = 55,000
```

Correct workflow:

- preserve executor proposal, beneficiary consent if required, asset valuation, transferability check, custody instruction and residual cash obligation;
- apply haircut or transfer restriction rules before accepting substitute value;
- reduce equalization shortfall only by approved transferable value;
- keep residual cash shortfall open until funded, waived or further substituted;
- avoid treating proposed asset market value as accepted equalization value without approval and custody feasibility.

QA assertions:

| Test | Expected result |
|---|---|
| Substitute asset is restricted | Transfer is blocked or haircut is applied. |
| Approved asset value is below shortfall | Residual cash shortfall remains open. |
| Beneficiary consent is required but missing | Substitution is blocked. |
| Distribution report is generated | Cash shortfall, substituted value and residual amount are separated. |

## 77. Foundation Recurring Breach Monitoring

Scenario:

- A recipient has repeated restricted-purpose grant breaches across monitoring periods.
- The latest breach is partly remediated, but recurrence requires enhanced monitoring.
- The foundation council must decide whether to suspend future grants or impose additional evidence requirements.

| Attribute | Value |
|---|---:|
| Breach events in monitoring window | 3 |
| Breach recurrence threshold | 2 |
| Recurrence excess | 1 |

Recurrence:

```text
recurrence_excess = breach_events_in_monitoring_window - breach_recurrence_threshold
recurrence_excess = 3 - 2 = 1
```

Correct workflow:

- preserve breach history, monitoring window, remediation status, recurrence threshold, council decision and future grant restrictions;
- distinguish current financial exposure from recurrence risk status;
- escalate recurrence even when the latest breach is partly remediated;
- apply enhanced monitoring, suspension or evidence requirements according to council decision;
- avoid resetting breach history after each partial remediation.

QA assertions:

| Test | Expected result |
|---|---|
| Breaches exceed recurrence threshold | Enhanced monitoring or suspension workflow is triggered. |
| Latest breach is partly remediated | Recurrence state remains active. |
| Council approves conditional continuation | Future grants require additional evidence. |
| Monitoring report is generated | Current exposure and recurrence history are both visible. |

## 78. Authority Renewal Post-Approval Ratification

Scenario:

- An approval was signed during an ambiguous delegated-authority renewal period.
- Later legal review confirms the signatory lacked valid authority at signing time.
- Ratification is required before the approval can be treated as valid.

| Attribute | Value |
|---|---:|
| Approvals signed during ambiguous period | 4 |
| Approvals ratified by current authority | 3 |
| Approvals still invalid | 1 |

Ratification:

```text
approvals_still_invalid = approvals_signed_during_ambiguous_period - approvals_ratified_by_current_authority
approvals_still_invalid = 4 - 3 = 1
```

Correct workflow:

- preserve original approval, authority ambiguity, legal review, ratification decision, ratifying authority and downstream transaction impact;
- mark affected approvals as provisional until ratification is complete;
- re-run downstream transaction checks for approvals that are ratified after the fact;
- block unratified approvals from supporting distributions, investments or reporting access;
- avoid backdating authority validity without explicit ratification evidence.

QA assertions:

| Test | Expected result |
|---|---|
| Legal review invalidates signing authority | Approval becomes provisional or invalid. |
| Current authority ratifies approval | Approval becomes valid from approved ratification treatment. |
| Approval is not ratified | Downstream action remains blocked. |
| Audit report is requested | Original signature, authority gap and ratification evidence are visible. |

## 79. Beneficiary Correction-Package Delivery Failure

Scenario:

- A retrospective disclosure decision requires a correction package for approved beneficiaries.
- Delivery fails for one beneficiary due to an invalid delivery channel.
- The disclosure state must remain pending for that recipient until successful delivery or approved alternate delivery.

| Attribute | Value |
|---|---:|
| Correction packages approved | 5 |
| Correction packages delivered | 4 |
| Delivery failures pending remediation | 1 |

Delivery failure:

```text
delivery_failures_pending_remediation = correction_packages_approved - correction_packages_delivered
delivery_failures_pending_remediation = 5 - 4 = 1
```

Correct workflow:

- preserve correction approval, recipient list, delivery channel, failed-delivery evidence, alternate channel approval and final delivery confirmation;
- keep failed recipients in pending delivery state without broadening disclosure scope;
- avoid marking the correction complete until every required recipient is delivered, waived or escalated;
- retain original report delivery and correction-package delivery evidence separately;
- avoid exposing correction content through an unapproved alternate channel.

QA assertions:

| Test | Expected result |
|---|---|
| One delivery fails | Correction remains pending for that recipient. |
| Alternate channel lacks approval | Redelivery is blocked. |
| Recipient delivery is waived | Waiver evidence is required before closure. |
| Delivery audit is generated | Approved, delivered, failed and waived recipients are traceable. |

## 80. Protector Reserve Beneficiary Objection Window

Scenario:

- A trustee approves release of part of a protector reserve after final cost review.
- The governing process requires a beneficiary objection window before the release becomes distributable.
- One beneficiary objects within the window, so the disputed release amount remains restricted.

| Attribute | Value |
|---|---:|
| Trustee-approved release | 60,000 |
| Amount objected within window | 25,000 |
| Release available after objection window | 35,000 |

Objection window:

```text
release_available_after_objection_window = trustee_approved_release - amount_objected_within_window
release_available_after_objection_window = 60,000 - 25,000 = 35,000
```

Correct workflow:

- preserve trustee release approval, beneficiary notice date, objection deadline, objection evidence, disputed amount and final release instruction;
- keep objected amounts restricted until trustee, protector or dispute-resolution decision is complete;
- release only the non-objected approved amount after the objection window closes;
- record late objections separately from valid in-window objections;
- avoid treating trustee approval as immediately distributable when an objection window applies.

QA assertions:

| Test | Expected result |
|---|---|
| Objection is received within window | Objected amount remains restricted. |
| No objection is received by deadline | Approved release becomes distributable. |
| Objection is late | Late objection is recorded but does not automatically restrict release. |
| Beneficiary report is generated | Approved, objected and releasable amounts are separated. |

## 81. Co-Investment Allocation Cancellation Fee

Scenario:

- A family entity cancels part of a committed co-investment allocation after the cancellation deadline.
- The sponsor charges a cancellation fee.
- The fee is charged to the cancelling entity and must not be allocated to remaining participants.

| Attribute | Value |
|---|---:|
| Cancelled allocation | 500,000 |
| Cancellation fee rate | 2% |
| Cancellation fee | 10,000 |

Cancellation fee:

```text
cancellation_fee = cancelled_allocation * cancellation_fee_rate
cancellation_fee = 500,000 * 2% = 10,000
```

Correct workflow:

- preserve cancellation request, deadline, sponsor fee terms, entity-level allocation ledger, fee approval and chargeback instruction;
- reduce the cancelling entity's commitment by the cancelled amount;
- charge cancellation fees only to the cancelling entity unless governing documents say otherwise;
- exclude the cancelled allocation from future capital-call forecasts;
- avoid spreading cancellation fees across non-cancelling participants as a general family-office expense.

QA assertions:

| Test | Expected result |
|---|---|
| Cancellation occurs after deadline | Cancellation fee is calculated. |
| Cancellation occurs before deadline | Fee is not charged unless terms require it. |
| Fee report is produced | Cancelling entity bears the fee. |
| Forecast is refreshed | Cancelled allocation is removed from future calls. |

## 82. Estate Substitution Valuation Appeal

Scenario:

- A beneficiary appeals the valuation used for a substitute asset in an estate equalization settlement.
- An independent valuation is accepted at a lower value.
- The residual cash shortfall must be recalculated from the accepted appeal value.

| Attribute | Value |
|---|---:|
| Original substitute asset value | 125,000 |
| Accepted appeal valuation | 110,000 |
| Valuation reduction | 15,000 |

Valuation appeal:

```text
valuation_reduction = original_substitute_asset_value - accepted_appeal_valuation
valuation_reduction = 125,000 - 110,000 = 15,000
```

Correct workflow:

- preserve original valuation, appeal notice, independent valuation, accepted value, beneficiary decision and recalculated equalization schedule;
- replace provisional substitute value with accepted appeal value from the decision effective date;
- increase residual cash shortfall when accepted valuation is lower;
- keep previous report values as historical evidence and issue a correction if already delivered;
- avoid using market movement after the appeal date unless the decision explicitly requires a fresh valuation date.

QA assertions:

| Test | Expected result |
|---|---|
| Appeal valuation is accepted | Substitute value is updated to accepted value. |
| Accepted value is lower | Residual cash shortfall increases. |
| Prior report was delivered | Correction evidence is generated. |
| Appeal is rejected | Original substitute value remains effective. |

## 83. Foundation Suspension Lift Condition

Scenario:

- A foundation suspended future grants after recurring restricted-purpose breaches.
- The council agrees to lift the suspension only after evidence of remediation and enhanced monitoring acceptance.
- Future grant release remains blocked until all lift conditions are satisfied.

| Attribute | Value |
|---|---:|
| Suspension lift conditions required | 3 |
| Lift conditions satisfied | 2 |
| Conditions still open | 1 |

Lift conditions:

```text
conditions_still_open = suspension_lift_conditions_required - lift_conditions_satisfied
conditions_still_open = 3 - 2 = 1
```

Correct workflow:

- preserve suspension decision, lift conditions, remediation evidence, monitoring acceptance, council approval and future grant release status;
- track each lift condition independently;
- keep grant release blocked until every mandatory condition is satisfied or formally waived;
- retain recurrence history after suspension is lifted;
- avoid reopening grant release solely because one remediation item is complete.

QA assertions:

| Test | Expected result |
|---|---|
| One lift condition remains open | Suspension remains active. |
| All conditions are satisfied | Council approval can lift suspension. |
| Condition is waived | Waiver evidence and scope are required. |
| Grant release is requested | Release is blocked until suspension is lifted. |

## 84. Authority Ratification Downstream Reversal

Scenario:

- An approval was ratified after an ambiguous authority period.
- A later review reverses the ratification for one downstream action.
- The affected transaction must be unwound or reapproved through a valid authority path.

| Attribute | Value |
|---|---:|
| Downstream actions relying on ratification | 4 |
| Actions still valid after reversal review | 3 |
| Actions requiring reversal or reapproval | 1 |

Downstream reversal:

```text
actions_requiring_reversal_or_reapproval = downstream_actions_relying_on_ratification - actions_still_valid_after_reversal_review
actions_requiring_reversal_or_reapproval = 4 - 3 = 1
```

Correct workflow:

- preserve original ratification, reversal review, affected approval, downstream transaction list, unwind decision and reapproval route;
- isolate only downstream actions affected by the reversed ratification;
- block future reliance on the invalidated ratification;
- decide whether affected actions require cancellation, reversal entry, fresh approval or reporting correction;
- avoid assuming all ratified actions are invalid when reversal applies only to a scoped action.

QA assertions:

| Test | Expected result |
|---|---|
| Ratification is reversed for one action | Only scoped downstream actions are affected. |
| Transaction already settled | Unwind or remediation workflow is triggered. |
| Fresh authority approves action | Action can proceed under new approval evidence. |
| Audit report is generated | Ratification, reversal and affected actions are traceable. |

## 85. Correction-Package Recipient Waiver Governance

Scenario:

- A beneficiary correction package cannot be delivered to one approved recipient.
- The trustee accepts a recipient-specific delivery waiver after documented contact attempts.
- The correction workflow can close only for the waived recipient and must preserve waiver evidence.

| Attribute | Value |
|---|---:|
| Delivery failures pending remediation | 1 |
| Recipient waivers approved | 1 |
| Delivery failures remaining open | 0 |

Recipient waiver:

```text
delivery_failures_remaining_open = delivery_failures_pending_remediation - recipient_waivers_approved
delivery_failures_remaining_open = 1 - 1 = 0
```

Correct workflow:

- preserve failed-delivery evidence, contact attempts, trustee waiver approval, recipient scope, correction package identifier and closure decision;
- apply waiver only to the named recipient and correction package;
- keep other recipients' delivery requirements unchanged;
- report waiver closure separately from successful delivery;
- avoid using a delivery waiver as permission to suppress future required notices.

QA assertions:

| Test | Expected result |
|---|---|
| Waiver is approved for one recipient | Only that recipient's delivery failure closes. |
| Waiver lacks trustee approval | Correction remains pending. |
| Another package is generated later | Prior waiver does not automatically apply. |
| Closure report is generated | Delivered, failed and waived recipients are distinguishable. |

## 86. Protector Objection Late Acceptance Dispute

Scenario:

- A beneficiary objection to a protector reserve release arrives after the stated objection deadline.
- The trustee has discretion to accept late objections only with documented cause.
- The late-accepted portion remains restricted while the non-accepted portion proceeds to release.

| Attribute | Value |
|---|---:|
| Late objection amount | 40,000 |
| Late objection accepted for review | 15,000 |
| Late objection not accepted | 25,000 |

Late acceptance:

```text
late_objection_not_accepted = late_objection_amount - late_objection_accepted_for_review
late_objection_not_accepted = 40,000 - 15,000 = 25,000
```

Correct workflow:

- preserve original objection deadline, late objection timestamp, stated cause, trustee late-acceptance decision, accepted amount and release instruction;
- restrict only the late objection amount accepted for review;
- release non-accepted late objection amounts if all other release controls pass;
- report late acceptance separately from valid in-window objections;
- avoid reopening the entire reserve release because one late objection is partially accepted.

QA assertions:

| Test | Expected result |
|---|---|
| Late objection lacks accepted cause | Objection does not automatically restrict release. |
| Trustee accepts part of late objection | Accepted amount remains restricted. |
| Trustee rejects late objection | Release can proceed for rejected amount. |
| Audit report is generated | Deadline, late cause, decision and amounts are visible. |

## 87. Co-Investment Cancellation Fee Waiver

Scenario:

- A sponsor charges a cancellation fee after a family entity cancels a co-investment allocation.
- The investment committee approves a partial fee waiver due to sponsor-side documentation delay.
- Only the net fee is charged to the cancelling entity.

| Attribute | Value |
|---|---:|
| Gross cancellation fee | 10,000 |
| Approved fee waiver | 4,000 |
| Net cancellation fee charged | 6,000 |

Fee waiver:

```text
net_cancellation_fee_charged = gross_cancellation_fee - approved_fee_waiver
net_cancellation_fee_charged = 10,000 - 4,000 = 6,000
```

Correct workflow:

- preserve gross fee terms, waiver request, waiver rationale, committee approval, sponsor evidence and final chargeback instruction;
- charge only the net fee to the cancelling entity;
- keep waived fee separate from reimbursed fee and disputed fee;
- update family-office expense reporting without allocating waived cost to other entities;
- avoid suppressing the fee event entirely when a partial waiver is granted.

QA assertions:

| Test | Expected result |
|---|---|
| Partial waiver is approved | Net fee is calculated and charged. |
| Waiver lacks approval | Gross fee remains chargeable. |
| Fee is disputed separately | Dispute and waiver states remain distinct. |
| Expense report is generated | Gross fee, waiver and net charge are visible. |

## 88. Estate Valuation Appeal Counterparty Dispute

Scenario:

- An estate substitution valuation appeal is accepted by the executor.
- The receiving beneficiary disputes the independent valuation provider's methodology.
- The substituted value remains provisional until counterparty dispute resolution.

| Attribute | Value |
|---|---:|
| Accepted appeal valuation | 110,000 |
| Counterparty disputed valuation adjustment | 12,000 |
| Provisional valuation floor | 98,000 |

Counterparty dispute:

```text
provisional_valuation_floor = accepted_appeal_valuation - counterparty_disputed_valuation_adjustment
provisional_valuation_floor = 110,000 - 12,000 = 98,000
```

Correct workflow:

- preserve accepted appeal valuation, counterparty dispute notice, disputed methodology, provisional value, independent review route and equalization impact;
- mark substituted value as provisional while the counterparty dispute is open;
- prevent final estate closure until accepted value is confirmed or settlement is approved;
- disclose provisional valuation status in beneficiary reporting;
- avoid treating an executor-accepted valuation as final when the receiving counterparty has a valid dispute right.

QA assertions:

| Test | Expected result |
|---|---|
| Counterparty dispute is valid | Substitute valuation becomes provisional. |
| Dispute is resolved at lower value | Equalization shortfall is recalculated. |
| Dispute is rejected | Accepted appeal valuation remains effective. |
| Estate closure is requested | Closure is blocked while valuation remains provisional. |

## 89. Foundation Conditional Restart Monitoring

Scenario:

- A foundation lifts a grant suspension after remediation evidence is accepted.
- Restart is conditional on enhanced monitoring for the next grant cycle.
- Failure to provide monitoring evidence reactivates the release block for future grants.

| Attribute | Value |
|---|---:|
| Monitoring checkpoints required | 4 |
| Monitoring checkpoints completed | 3 |
| Checkpoints still required | 1 |

Restart monitoring:

```text
checkpoints_still_required = monitoring_checkpoints_required - monitoring_checkpoints_completed
checkpoints_still_required = 4 - 3 = 1
```

Correct workflow:

- preserve suspension lift approval, restart conditions, monitoring checkpoints, evidence submissions, missed checkpoint escalation and future grant release status;
- allow restart only within approved conditional scope;
- keep future grant release contingent on completed monitoring evidence;
- reactivate release block when monitoring conditions fail;
- avoid treating suspension lift as permanent removal of monitoring obligations.

QA assertions:

| Test | Expected result |
|---|---|
| Restart condition is incomplete | Future grant release remains conditional or blocked. |
| Monitoring checkpoint is missed | Release block is reactivated. |
| All checkpoints pass | Conditional status can be closed by council decision. |
| Monitoring report is generated | Completed, missed and outstanding checkpoints are visible. |

## 90. Downstream Ratification Remediation Reporting

Scenario:

- A downstream action was affected by ratification reversal.
- Operations remediates the action with a fresh approval and correction entry.
- Reporting must show original action, invalid authority, remediation action and final state.

| Attribute | Value |
|---|---:|
| Affected downstream actions | 1 |
| Actions remediated with fresh approval | 1 |
| Actions still requiring remediation | 0 |

Remediation reporting:

```text
actions_still_requiring_remediation = affected_downstream_actions - actions_remediated_with_fresh_approval
actions_still_requiring_remediation = 1 - 1 = 0
```

Correct workflow:

- preserve affected action, invalidated ratification, remediation approval, correction entry, client/reporting impact and final audit state;
- show remediation separately from original approval and reversal;
- close only the remediated action, not unrelated ratification review items;
- ensure client-facing reporting uses corrected state while audit retains the original path;
- avoid deleting or overwriting the original invalid authority trail.

QA assertions:

| Test | Expected result |
|---|---|
| Fresh approval remediates action | Action moves to remediated state. |
| Correction entry is missing | Remediation remains incomplete. |
| Report is generated | Original action, reversal and remediation are traceable. |
| Unrelated actions exist | They are not closed by scoped remediation. |

## 91. Correction Waiver Expiry Control

Scenario:

- A recipient-specific correction delivery waiver was granted for a defined period.
- The waiver expires before a later correction package is issued.
- The recipient must return to normal delivery requirements unless a new waiver is approved.

| Attribute | Value |
|---|---:|
| Recipients covered by expired waiver | 1 |
| Recipients with renewed waiver | 0 |
| Recipients requiring delivery attempt | 1 |

Waiver expiry:

```text
recipients_requiring_delivery_attempt = recipients_covered_by_expired_waiver - recipients_with_renewed_waiver
recipients_requiring_delivery_attempt = 1 - 0 = 1
```

Correct workflow:

- preserve original waiver, waiver scope, expiry date, new correction package, renewed waiver status and delivery attempt evidence;
- apply waiver only within approved period and package scope;
- require fresh delivery attempt or new trustee waiver after expiry;
- prevent historical waivers from suppressing future correction packages;
- avoid carrying forward recipient waiver state without effective-date validation.

QA assertions:

| Test | Expected result |
|---|---|
| Prior waiver has expired | Recipient requires delivery attempt or renewed waiver. |
| New package is outside waiver scope | Prior waiver does not apply. |
| Trustee renews waiver | Renewal evidence and expiry are recorded. |
| Closure report is generated | Expired, renewed and delivered recipient states are visible. |

## 92. Protector Reserve Mediation Outcome

Scenario:

- A reserve release dispute enters mediation after a beneficiary objection.
- Mediation allocates part of the objected amount to release and keeps the balance restricted for final trustee review.
- Reporting must distinguish mediated release from unresolved restricted reserve.

| Attribute | Value |
|---|---:|
| Objected reserve amount | 25,000 |
| Mediated amount approved for release | 18,000 |
| Unresolved restricted reserve | 7,000 |

Mediation outcome:

```text
unresolved_restricted_reserve = objected_reserve_amount - mediated_amount_approved_for_release
unresolved_restricted_reserve = 25,000 - 18,000 = 7,000
```

Correct workflow:

- preserve objection, mediation record, mediated release amount, trustee acceptance, unresolved amount and beneficiary communication;
- release only the mediated amount after trustee acceptance;
- keep unresolved reserve restricted until final review or further settlement;
- report mediated release separately from trustee-only release decisions;
- avoid treating mediation outcome as full dispute closure when a residual amount remains unresolved.

QA assertions:

| Test | Expected result |
|---|---|
| Mediation is accepted by trustee | Mediated amount becomes releasable. |
| Residual reserve remains unresolved | Residual stays restricted. |
| Mediation lacks trustee acceptance | No release occurs. |
| Report is generated | Objected, mediated and unresolved amounts are separated. |

## 93. Co-Investment Cancelled-Allocation Replacement Dispute

Scenario:

- A cancelled co-investment allocation is offered to a replacement participant.
- The original cancelling entity disputes whether replacement participation should reduce its cancellation fee.
- Fee treatment remains disputed until sponsor and committee evidence are reconciled.

| Attribute | Value |
|---|---:|
| Net cancellation fee charged | 6,000 |
| Replacement credit requested | 2,500 |
| Fee amount still disputed | 3,500 |

Replacement dispute:

```text
fee_amount_still_disputed = net_cancellation_fee_charged - replacement_credit_requested
fee_amount_still_disputed = 6,000 - 2,500 = 3,500
```

Correct workflow:

- preserve cancellation fee, replacement participant approval, sponsor credit terms, dispute notice, committee decision and final chargeback;
- keep replacement allocation separate from fee-credit entitlement;
- apply credit only when sponsor terms or committee approval support it;
- hold disputed fee amount in exception state until resolved;
- avoid automatically reducing cancellation fees because another participant takes the cancelled allocation.

QA assertions:

| Test | Expected result |
|---|---|
| Replacement participant is approved | Allocation ledger updates for replacement participant. |
| Fee credit lacks evidence | Cancellation fee remains charged or disputed. |
| Committee approves partial credit | Net disputed fee is recalculated. |
| Fee report is produced | Charged, credited and disputed fee amounts are visible. |

## 94. Estate Substitute Asset Custody-Transfer Failure

Scenario:

- A substitute asset is approved to reduce an estate equalization cash shortfall.
- Custody transfer fails because the receiving account cannot hold the asset type.
- The equalization credit remains provisional until transfer succeeds or a different substitute is approved.

| Attribute | Value |
|---|---:|
| Approved substitute asset value | 110,000 |
| Value successfully transferred | 0 |
| Provisional credit to reverse | 110,000 |

Transfer failure:

```text
provisional_credit_to_reverse = approved_substitute_asset_value - value_successfully_transferred
provisional_credit_to_reverse = 110,000 - 0 = 110,000
```

Correct workflow:

- preserve substitute approval, custody instruction, receiving account eligibility, failed-transfer reason, revised equalization schedule and alternate asset decision;
- treat substitute credit as provisional until custody transfer is complete;
- reverse provisional credit if transfer cannot be completed;
- block estate closure while transfer failure keeps equalization unresolved;
- avoid reducing equalization cash shortfall based only on approved substitution when custody delivery fails.

QA assertions:

| Test | Expected result |
|---|---|
| Receiving account cannot hold asset | Custody transfer fails and credit remains provisional. |
| Transfer completes later | Equalization credit becomes effective. |
| Alternate substitute is approved | Equalization schedule is recalculated. |
| Estate closure is requested | Closure is blocked while transfer failure is unresolved. |

## 95. Foundation Restart Evidence Sampling

Scenario:

- A foundation restarts grant releases under enhanced monitoring.
- The council requires sample evidence checks across funded activities.
- One sample fails, requiring escalation before the next release.

| Attribute | Value |
|---|---:|
| Evidence samples required | 5 |
| Evidence samples accepted | 4 |
| Failed or missing samples | 1 |

Evidence sampling:

```text
failed_or_missing_samples = evidence_samples_required - evidence_samples_accepted
failed_or_missing_samples = 5 - 4 = 1
```

Correct workflow:

- preserve restart conditions, sample plan, accepted samples, failed samples, escalation owner, council decision and release status;
- keep sampling evidence linked to the conditional restart period;
- block next release when mandatory evidence sampling fails;
- distinguish sample failure from full breach unless council classifies it as such;
- avoid closing conditional restart monitoring based on aggregate progress when a required sample failed.

QA assertions:

| Test | Expected result |
|---|---|
| Required sample fails | Next release is blocked or escalated. |
| Sample failure is cured | Sampling status can move to accepted after evidence review. |
| Council waives sample | Waiver scope and rationale are recorded. |
| Monitoring report is generated | Required, accepted and failed samples are visible. |

## 96. Ratification Correction Client-Notice Exception

Scenario:

- A ratification remediation correction usually requires client notice.
- Legal approves an exception because notice would disclose restricted family-governance information.
- The exception must be recorded and reporting must preserve corrected state without exposing restricted rationale.

| Attribute | Value |
|---|---:|
| Correction notices normally required | 3 |
| Client-notice exceptions approved | 1 |
| Notices still required | 2 |

Notice exception:

```text
notices_still_required = correction_notices_normally_required - client_notice_exceptions_approved
notices_still_required = 3 - 1 = 2
```

Correct workflow:

- preserve correction requirement, legal exception approval, restricted rationale, notice population, notices still required and audit evidence;
- apply exception only to the approved recipient or notice type;
- keep restricted rationale out of client-facing reporting;
- continue issuing required notices for non-exempt recipients;
- avoid suppressing all notices because one restricted exception was approved.

QA assertions:

| Test | Expected result |
|---|---|
| Legal exception is approved | Notice is suppressed only for approved scope. |
| Other recipients require notice | Notices remain required. |
| Client report is produced | Corrected state is shown without restricted rationale. |
| Audit is requested | Exception approval and rationale are available to authorized users. |

## 97. Correction-Waiver Renewal Denial

Scenario:

- A recipient-specific correction delivery waiver expires.
- Trustee denies renewal because contact details were remediated.
- Delivery attempts must resume for the recipient.

| Attribute | Value |
|---|---:|
| Renewal requests submitted | 1 |
| Renewal requests denied | 1 |
| Delivery attempts required after denial | 1 |

Renewal denial:

```text
delivery_attempts_required_after_denial = renewal_requests_submitted - (renewal_requests_submitted - renewal_requests_denied)
delivery_attempts_required_after_denial = 1 - (1 - 1) = 1
```

Correct workflow:

- preserve expired waiver, renewal request, trustee denial, remediated contact evidence, delivery channel and new delivery attempt status;
- stop applying waiver suppression after denial;
- resume normal delivery attempts for the recipient;
- preserve denial evidence for audit and future waiver decisions;
- avoid treating a prior waiver as still active after renewal is denied.

QA assertions:

| Test | Expected result |
|---|---|
| Renewal is denied | Recipient returns to delivery-required state. |
| Contact details are remediated | Delivery uses updated channel. |
| Delivery fails again | Standard failed-delivery workflow restarts. |
| Closure report is generated | Expired waiver, denial and new delivery attempt are traceable. |

## 98. Protector Mediated-Release Payment Failure

Scenario:

- A mediated reserve release is accepted by the trustee.
- Payment instruction fails due to beneficiary account validation failure.
- The mediated amount remains payable but not distributed until payment repair succeeds.

| Attribute | Value |
|---|---:|
| Mediated release approved | 18,000 |
| Release payment settled | 0 |
| Release payment pending repair | 18,000 |

Payment failure:

```text
release_payment_pending_repair = mediated_release_approved - release_payment_settled
release_payment_pending_repair = 18,000 - 0 = 18,000
```

Correct workflow:

- preserve mediation approval, trustee acceptance, payment instruction, payment failure reason, beneficiary account remediation and repaired settlement evidence;
- keep mediated amount in payable or pending-settlement state rather than returning it to unrestricted cash;
- block closure until payment settles, is redirected with approval or is re-reserved;
- report payment failure separately from reserve dispute status;
- avoid treating payment instruction creation as completed distribution.

QA assertions:

| Test | Expected result |
|---|---|
| Payment instruction fails | Mediated release remains pending repair. |
| Beneficiary account is corrected | Payment can be reissued with evidence. |
| Payment is redirected | Trustee or authorized approval is required. |
| Closure report is generated | Approved, instructed, failed and settled states are visible. |

## 99. Replacement Participant Funding Shortfall

Scenario:

- A replacement participant accepts a cancelled co-investment allocation.
- The participant funds only part of the required commitment by the funding deadline.
- Unfunded replacement amount must be reallocated, cancelled or escalated.

| Attribute | Value |
|---|---:|
| Replacement allocation approved | 300,000 |
| Replacement funding received | 210,000 |
| Replacement funding shortfall | 90,000 |

Funding shortfall:

```text
replacement_funding_shortfall = replacement_allocation_approved - replacement_funding_received
replacement_funding_shortfall = 300,000 - 210,000 = 90,000
```

Correct workflow:

- preserve replacement approval, funding deadline, cash receipt, shortfall notice, committee decision and revised allocation ledger;
- keep funded and unfunded replacement amounts separate;
- prevent the unfunded amount from entering committed exposure until cure or reallocation is approved;
- decide whether the shortfall creates cancellation fees, replacement search or sponsor escalation;
- avoid showing full replacement allocation as funded commitment before cash is received.

QA assertions:

| Test | Expected result |
|---|---|
| Replacement funding is partial | Shortfall is calculated and escalated. |
| Funding deadline is extended | Extension approval and revised deadline are recorded. |
| Shortfall is reallocated | New recipient authority and headroom are checked. |
| Capital-call forecast is produced | Forecast uses funded and approved commitment state. |

## 100. Substitute Asset Title-Defect Cure

Scenario:

- An estate substitute asset has a title defect that prevents custody transfer.
- The executor provides cure documents and title review accepts part of the asset value.
- Equalization credit is applied only to the cured transferable value.

| Attribute | Value |
|---|---:|
| Original substitute asset value | 110,000 |
| Value accepted after title cure | 85,000 |
| Value still blocked by title defect | 25,000 |

Title cure:

```text
value_still_blocked_by_title_defect = original_substitute_asset_value - value_accepted_after_title_cure
value_still_blocked_by_title_defect = 110,000 - 85,000 = 25,000
```

Correct workflow:

- preserve title defect notice, cure documents, legal/title review, accepted transferable value, rejected value and revised equalization schedule;
- apply equalization credit only to value accepted after cure;
- keep blocked value in unresolved status until further cure, cash replacement or waiver;
- maintain custody-transfer evidence separately from valuation evidence;
- avoid treating title cure submission as accepted transferability before review is complete.

QA assertions:

| Test | Expected result |
|---|---|
| Title defect remains partly uncured | Only accepted value reduces shortfall. |
| Cure documents are incomplete | Substitute credit remains provisional. |
| Full title cure is accepted | Full accepted value can be credited. |
| Estate report is generated | Original value, cured value and blocked value are separated. |

## 101. Foundation Sampling Waiver Expiry

Scenario:

- A council waiver temporarily reduced evidence sampling requirements during conditional restart.
- The waiver expires before the next monitoring cycle.
- Full sampling requirements return unless a new waiver is approved.

| Attribute | Value |
|---|---:|
| Samples required after waiver expiry | 5 |
| Samples covered by renewed waiver | 0 |
| Samples requiring evidence | 5 |

Waiver expiry:

```text
samples_requiring_evidence = samples_required_after_waiver_expiry - samples_covered_by_renewed_waiver
samples_requiring_evidence = 5 - 0 = 5
```

Correct workflow:

- preserve sampling waiver, waiver scope, expiry date, monitoring cycle, renewed waiver status and evidence plan;
- restore full sampling requirements after expiry;
- block grant release if required post-expiry samples are missing;
- retain expired waiver evidence for audit without applying it to new cycles;
- avoid carrying forward reduced sampling requirements without current council approval.

QA assertions:

| Test | Expected result |
|---|---|
| Sampling waiver expires | Full evidence requirement returns. |
| Renewal is approved | Renewed scope and expiry control the next cycle. |
| Samples are missing after expiry | Release is blocked or escalated. |
| Monitoring report is generated | Expired waiver and current sample requirement are visible. |

## 102. Client-Notice Exception Audit Challenge

Scenario:

- A client-notice exception was approved for a ratification correction.
- Audit challenges whether the exception scope was too broad.
- The team must separate approved exception scope from notices that still require delivery.

| Attribute | Value |
|---|---:|
| Notices suppressed under exception | 3 |
| Notices confirmed in approved scope | 2 |
| Notices requiring remediation | 1 |

Audit challenge:

```text
notices_requiring_remediation = notices_suppressed_under_exception - notices_confirmed_in_approved_scope
notices_requiring_remediation = 3 - 2 = 1
```

Correct workflow:

- preserve exception approval, suppressed notice population, audit challenge, scope review, remediation decision and notice delivery evidence;
- confirm whether each suppressed notice is inside approved exception scope;
- deliver or remediate notices outside approved scope;
- keep restricted rationale visible only to authorized audit/governance users;
- avoid defending all suppressed notices when only part of the exception scope is supported.

QA assertions:

| Test | Expected result |
|---|---|
| Audit challenge finds out-of-scope notice | Remediation notice is required. |
| Notice is within exception scope | Suppression remains valid. |
| Restricted rationale is requested by unauthorized user | Rationale remains hidden. |
| Audit report is generated | Suppressed, supported and remediated notices are traceable. |

## 103. Correction-Waiver Denial Appeal Reversal

Scenario:

- A correction-waiver renewal was denied and delivery attempts resumed.
- The beneficiary appeals the denial and trustee reverses it for a limited period.
- Delivery requirements are paused only for the approved appeal-reversal period.

| Attribute | Value |
|---|---:|
| Delivery attempts required after denial | 1 |
| Delivery attempts paused by appeal reversal | 1 |
| Delivery attempts still required now | 0 |

Appeal reversal:

```text
delivery_attempts_still_required_now = delivery_attempts_required_after_denial - delivery_attempts_paused_by_appeal_reversal
delivery_attempts_still_required_now = 1 - 1 = 0
```

Correct workflow:

- preserve waiver denial, appeal request, trustee reversal, reversal scope, reversal expiry and delivery status;
- pause delivery only for the approved period and recipient scope;
- resume delivery when reversal expires unless renewed;
- keep denial, appeal and reversal evidence distinct;
- avoid treating appeal reversal as permanent waiver renewal.

QA assertions:

| Test | Expected result |
|---|---|
| Appeal reversal is approved | Delivery pauses only for approved scope. |
| Reversal expires | Delivery-required state resumes. |
| Appeal reversal is partial | Non-covered packages still require delivery. |
| Closure report is generated | Denial, appeal, reversal and expiry are traceable. |

## 104. Protector Repaired-Payment Reversal

Scenario:

- A mediated reserve release payment failed, was repaired and then settled.
- The receiving bank later reverses the repaired payment due to beneficiary account closure.
- The release must return to pending-payment state while preserving the mediation and repair evidence.

| Attribute | Value |
|---|---:|
| Repaired payment settled | 18,000 |
| Repaired payment reversed | 18,000 |
| Release requiring new payment action | 18,000 |

Payment reversal:

```text
release_requiring_new_payment_action = repaired_payment_settled - (repaired_payment_settled - repaired_payment_reversed)
release_requiring_new_payment_action = 18,000 - (18,000 - 18,000) = 18,000
```

Correct workflow:

- preserve original mediation approval, failed payment, repair instruction, settlement evidence, reversal notice and new payment decision;
- return the reversed amount to pending-payment state, not unrestricted reserve;
- require updated beneficiary payment details or trustee-approved redirection before reissue;
- keep settlement and reversal events visible in audit and beneficiary reporting;
- avoid marking mediation closure complete after a repaired payment is reversed.

QA assertions:

| Test | Expected result |
|---|---|
| Repaired payment reverses | Release returns to pending-payment state. |
| New account details are provided | Payment can be reissued with approval evidence. |
| Redirection is requested | Trustee or authorized approval is required. |
| Report is generated | Failure, repair, settlement and reversal are traceable. |

## 105. Replacement Funding Deadline Extension

Scenario:

- A replacement participant has a funding shortfall after accepting a cancelled co-investment allocation.
- The committee grants a deadline extension for part of the shortfall.
- Any amount outside the extension remains subject to cancellation or reallocation.

| Attribute | Value |
|---|---:|
| Replacement funding shortfall | 90,000 |
| Shortfall covered by deadline extension | 60,000 |
| Shortfall not extended | 30,000 |

Deadline extension:

```text
shortfall_not_extended = replacement_funding_shortfall - shortfall_covered_by_deadline_extension
shortfall_not_extended = 90,000 - 60,000 = 30,000
```

Correct workflow:

- preserve original funding deadline, shortfall, extension request, committee decision, extended amount, residual shortfall and revised funding date;
- keep extended and non-extended shortfall amounts separate;
- include only funded or formally extended amounts in approved commitment exposure;
- escalate non-extended shortfall for cancellation, replacement or sponsor action;
- avoid extending the full shortfall when approval covers only part of it.

QA assertions:

| Test | Expected result |
|---|---|
| Partial extension is approved | Only approved amount receives revised deadline. |
| Residual shortfall is not extended | Residual remains escalated. |
| Extended deadline passes unfunded | Shortfall workflow reopens. |
| Forecast is generated | Funded, extended and escalated amounts are separated. |

## 106. Title-Cure Residual Cash Call

Scenario:

- A substitute asset title defect is partly cured.
- The uncured value must be replaced with cash to complete estate equalization.
- A residual cash call is issued to the estate liquidity reserve.

| Attribute | Value |
|---|---:|
| Value blocked by title defect | 25,000 |
| Cash replacement funded | 15,000 |
| Residual cash call outstanding | 10,000 |

Residual cash call:

```text
residual_cash_call_outstanding = value_blocked_by_title_defect - cash_replacement_funded
residual_cash_call_outstanding = 25,000 - 15,000 = 10,000
```

Correct workflow:

- preserve title-cure outcome, blocked value, cash-call instruction, funding source, settlement status and revised equalization schedule;
- reduce unresolved equalization only by settled cash replacement;
- keep residual cash call open until funded, waived or reallocated;
- distinguish title-cured asset credit from cash replacement funding;
- avoid closing equalization when cash call is instructed but not fully settled.

QA assertions:

| Test | Expected result |
|---|---|
| Cash call is partially funded | Residual cash call remains open. |
| Funding source lacks liquidity | Estate closure remains blocked. |
| Cash replacement settles | Equalization shortfall reduces by settled amount. |
| Report is produced | Asset credit, cash replacement and residual call are separated. |

## 107. Sampling Waiver Renewal Dispute

Scenario:

- A foundation recipient requests renewal of an evidence-sampling waiver.
- Monitoring team disputes renewal because prior samples had exceptions.
- Waiver status remains pending and full sampling remains required until council decision.

| Attribute | Value |
|---|---:|
| Samples required without renewal | 5 |
| Samples suspended by approved renewal | 0 |
| Samples still required during dispute | 5 |

Renewal dispute:

```text
samples_still_required_during_dispute = samples_required_without_renewal - samples_suspended_by_approved_renewal
samples_still_required_during_dispute = 5 - 0 = 5
```

Correct workflow:

- preserve waiver request, prior sampling exceptions, monitoring objection, council review state, current sampling requirement and release condition;
- keep full sampling requirements active while renewal is disputed;
- block release if required samples are missing during dispute;
- apply renewal only after council approval and within approved scope;
- avoid treating a submitted renewal request as an active waiver.

QA assertions:

| Test | Expected result |
|---|---|
| Renewal is disputed | Full sampling remains required. |
| Council approves renewal | Waiver scope and expiry are applied prospectively. |
| Samples are missing during dispute | Grant release is blocked. |
| Monitoring report is generated | Request, objection and current requirement are visible. |

## 108. Notice-Exception Remediation Notice

Scenario:

- Audit finds that one client notice was incorrectly suppressed under a ratification correction exception.
- A remediation notice must be issued without exposing restricted rationale.
- The correction remains open until the remediation notice is delivered or formally waived.

| Attribute | Value |
|---|---:|
| Notices requiring remediation | 1 |
| Remediation notices delivered | 0 |
| Remediation notices still open | 1 |

Remediation notice:

```text
remediation_notices_still_open = notices_requiring_remediation - remediation_notices_delivered
remediation_notices_still_open = 1 - 0 = 1
```

Correct workflow:

- preserve audit challenge, out-of-scope notice, remediation notice content, restricted rationale filter, delivery evidence and closure decision;
- issue remediation notice with corrected client-safe wording;
- keep restricted rationale out of client-facing notice;
- close only after delivery, approved waiver or escalation;
- avoid reopening unrelated notices that were within the approved exception scope.

QA assertions:

| Test | Expected result |
|---|---|
| Suppression was out of scope | Remediation notice is required. |
| Notice is delivered | Remediation item can close. |
| Delivery fails | Failed-delivery workflow starts. |
| Client copy is reviewed | Restricted rationale is not exposed. |

## 109. Correction Appeal Reversal Expiry Monitoring

Scenario:

- A correction-waiver denial appeal reversal temporarily paused delivery attempts.
- The reversal expires before delivery is completed.
- Delivery requirement must automatically reactivate unless renewed.

| Attribute | Value |
|---|---:|
| Delivery attempts paused by appeal reversal | 1 |
| Pauses renewed before expiry | 0 |
| Delivery attempts reactivated | 1 |

Expiry monitoring:

```text
delivery_attempts_reactivated = delivery_attempts_paused_by_appeal_reversal - pauses_renewed_before_expiry
delivery_attempts_reactivated = 1 - 0 = 1
```

Correct workflow:

- preserve appeal reversal, pause scope, expiry date, renewal status, delivery reactivation and recipient communication;
- monitor reversal expiry as an effective-dated control;
- reactivate delivery when pause expires without renewal;
- keep renewal and expiry evidence separate from original denial;
- avoid allowing expired appeal reversal to suppress future delivery attempts.

QA assertions:

| Test | Expected result |
|---|---|
| Appeal reversal expires | Delivery-required state reactivates. |
| Renewal is approved before expiry | Delivery remains paused within renewed scope. |
| New correction package is outside scope | Delivery is required. |
| Status report is generated | Pause, expiry, renewal and reactivation are visible. |

## 110. Protector Payment Redirection Approval

Scenario:

- A repaired mediated-release payment reverses because the beneficiary account is closed.
- The beneficiary requests payment redirection to a new verified account.
- Trustee approval is required before the release can be reissued.

| Attribute | Value |
|---|---:|
| Release requiring new payment action | 18,000 |
| Redirection amount approved | 18,000 |
| Amount still pending approval | 0 |

Redirection approval:

```text
amount_still_pending_approval = release_requiring_new_payment_action - redirection_amount_approved
amount_still_pending_approval = 18,000 - 18,000 = 0
```

Correct workflow:

- preserve reversal notice, beneficiary redirection request, new account verification, trustee approval, payment instruction and settlement evidence;
- reissue only after account verification and approval are complete;
- keep the original failed and reversed payment history intact;
- distinguish payment redirection from beneficiary entitlement change;
- avoid sending funds to a new account based only on beneficiary instruction.

QA assertions:

| Test | Expected result |
|---|---|
| New account is not verified | Redirection is blocked. |
| Trustee approval is missing | Payment remains pending. |
| Redirection is approved | Payment can be reissued to verified account. |
| Audit report is generated | Original payment, reversal, redirection and settlement are traceable. |

## 111. Replacement Allocation Sponsor Default Event

Scenario:

- A replacement participant funds a co-investment allocation.
- Before final sponsor acceptance, the sponsor defaults on the replacement allocation process.
- Funded cash must be refunded or held under approved escrow treatment.

| Attribute | Value |
|---|---:|
| Replacement funding received | 210,000 |
| Funding accepted by sponsor | 0 |
| Funding requiring refund or escrow | 210,000 |

Sponsor default:

```text
funding_requiring_refund_or_escrow = replacement_funding_received - funding_accepted_by_sponsor
funding_requiring_refund_or_escrow = 210,000 - 0 = 210,000
```

Correct workflow:

- preserve replacement funding receipt, sponsor default notice, acceptance status, committee decision, refund instruction and escrow terms;
- remove unaccepted replacement funding from commitment exposure;
- decide whether cash is refunded, held in escrow or applied to another approved allocation;
- report sponsor default separately from participant funding failure;
- avoid treating funded cash as invested commitment when sponsor acceptance failed.

QA assertions:

| Test | Expected result |
|---|---|
| Sponsor default occurs before acceptance | Funding is removed from commitment exposure. |
| Refund is approved | Cash refund instruction is generated. |
| Escrow is approved | Escrow terms and ownership are recorded. |
| Exposure report is generated | Funded, accepted and defaulted amounts are separated. |

## 112. Residual Cash-Call Beneficiary Objection

Scenario:

- A residual cash call is issued after a title-cure shortfall.
- A beneficiary objects to funding the cash replacement from the estate liquidity reserve.
- The objected amount remains blocked pending executor or court direction.

| Attribute | Value |
|---|---:|
| Residual cash call outstanding | 10,000 |
| Cash call objected by beneficiary | 6,000 |
| Cash call not objected | 4,000 |

Beneficiary objection:

```text
cash_call_not_objected = residual_cash_call_outstanding - cash_call_objected_by_beneficiary
cash_call_not_objected = 10,000 - 6,000 = 4,000
```

Correct workflow:

- preserve cash-call instruction, beneficiary objection, objection amount, executor response, court direction if required and funding decision;
- block objected amount until authority resolves the objection;
- allow non-objected amount to proceed only if funding rules permit partial funding;
- keep title-cure shortfall and cash-call objection linked;
- avoid treating a beneficiary objection as a waiver of the underlying equalization obligation.

QA assertions:

| Test | Expected result |
|---|---|
| Beneficiary objects to part of cash call | Objected amount is blocked. |
| Partial funding is allowed | Non-objected amount can proceed. |
| Court direction is required | Closure is blocked until decision. |
| Estate report is generated | Outstanding, objected and non-objected amounts are separated. |

## 113. Sampling Renewal Conditional Approval

Scenario:

- A foundation council conditionally approves renewal of a sampling waiver.
- The waiver applies only if the recipient submits a remediation evidence pack by a specified date.
- Until the condition is met, full sampling remains required.

| Attribute | Value |
|---|---:|
| Samples required without active waiver | 5 |
| Samples suspended after condition met | 3 |
| Samples still required before condition met | 5 |

Conditional approval:

```text
samples_still_required_before_condition_met = samples_required_without_active_waiver
samples_still_required_before_condition_met = 5
```

Correct workflow:

- preserve conditional renewal approval, condition precedent, due date, evidence pack, activation decision and renewed waiver scope;
- keep full sampling active until the condition is satisfied;
- activate only the approved waiver scope after evidence acceptance;
- block release if mandatory samples are missing before activation;
- avoid applying conditional approval as an active waiver before conditions are met.

QA assertions:

| Test | Expected result |
|---|---|
| Condition precedent is not met | Full sampling remains required. |
| Evidence pack is accepted | Approved waiver scope activates. |
| Evidence is late | Waiver remains inactive unless council extends. |
| Monitoring report is generated | Conditional, active and expired waiver states are visible. |

## 114. Remediation Notice Localization Failure

Scenario:

- A remediation notice is required after audit finds an out-of-scope notice exception.
- The notice must be delivered in the beneficiary's approved language.
- Delivery is blocked because the localized template is missing.

| Attribute | Value |
|---|---:|
| Remediation notices still open | 1 |
| Localized notices available | 0 |
| Notices blocked by localization | 1 |

Localization failure:

```text
notices_blocked_by_localization = remediation_notices_still_open - localized_notices_available
notices_blocked_by_localization = 1 - 0 = 1
```

Correct workflow:

- preserve remediation notice requirement, recipient language preference, approved template version, localization gap, translation approval and delivery evidence;
- block client delivery until an approved localized notice exists;
- keep the remediation item open while localization is pending;
- retain audit evidence that content was not sent in an unsupported language;
- avoid delivering a non-approved language version to force closure.

QA assertions:

| Test | Expected result |
|---|---|
| Localized template is missing | Delivery is blocked. |
| Translation is approved | Notice can be delivered in approved language. |
| Recipient language preference changes | Template requirement is recalculated. |
| Closure report is generated | Localization gap and final delivery are traceable. |

## 115. Appeal Reversal Post-Expiry Reactivation Dispute

Scenario:

- A correction appeal reversal expires and delivery requirement reactivates.
- The beneficiary disputes reactivation, claiming the reversal should continue.
- Delivery remains required unless trustee grants a new pause.

| Attribute | Value |
|---|---:|
| Delivery attempts reactivated | 1 |
| New pause approved | 0 |
| Delivery attempts still required | 1 |

Reactivation dispute:

```text
delivery_attempts_still_required = delivery_attempts_reactivated - new_pause_approved
delivery_attempts_still_required = 1 - 0 = 1
```

Correct workflow:

- preserve reversal expiry, delivery reactivation, beneficiary dispute, trustee review, new pause decision and delivery attempt evidence;
- keep delivery-required state active while dispute is pending;
- pause delivery only if a new approval is granted;
- retain expiry evidence separate from dispute evidence;
- avoid extending an expired appeal reversal based only on beneficiary challenge.

QA assertions:

| Test | Expected result |
|---|---|
| Beneficiary disputes reactivation | Delivery remains required pending decision. |
| Trustee grants new pause | Delivery pauses within approved scope. |
| Trustee rejects dispute | Delivery workflow continues. |
| Status report is generated | Expiry, dispute and delivery state are traceable. |

## 116. Regression Test Pack

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
50. Protector indemnity claims separate approved costs, disputed costs and reserve impact before payment.
51. Family-office capital commitment approvals require both authority threshold and funding headroom evidence.
52. Estate in-kind distributions preserve restriction flags, allocation rule, custody transfer and valuation lineage.
53. Foundation grant clawbacks separate approved spend, returned cash, remaining claim and dispute state.
54. Private-trust-company director indemnity reserves reduce distributable cash until release evidence exists.
55. Beneficiary address confidentiality overrides unmask only trustee-approved report scope and preserve audit evidence.
56. Protector indemnity reserve release disputes keep appeal-period reserves restricted until trustee approval exists.
57. Family-office co-investment overcommitment controls test adjusted headroom before commitment approval.
58. Estate in-kind equalization cash shortfalls block final distribution until funded, waived or reallocated.
59. Foundation grant restriction breach remediation tracks accepted remediation separately from residual breach exposure.
60. Private-trust-company director resignation authority gaps revalidate pending approvals against the current authority matrix.
61. Beneficiary contact-data redaction appeals apply field-level disclosure and preserve masking outside approved scope.
62. Protector reserve release appeal closures release only trustee-approved cash and retain residual true-up reserves.
63. Family-office co-investment syndication reallocations validate recipient headroom and track uncommitted balances.
64. Estate equalization cash waiver reversals re-open equalization obligations and block closure while shortfalls remain.
65. Foundation remediation deadline breaches separate accepted remediation from overdue exposure and escalation state.
66. Director resignation delegated-authority expiries re-route pending approvals after delegation expiry.
67. Beneficiary redaction appeal expiry disputes remask expired fields unless current extension approval exists.
68. Protector residual reserve true-ups release only excess reserve after accepted final cost evidence.
69. Co-investment waitlist priority disputes allocate only documented approved priority amounts.
70. Estate equalization replacement funding failures keep obligations open until settled, waived or reallocated.
71. Foundation remediation extension waivers apply only to approved amounts and revised deadlines.
72. Delegated-authority renewal conflicts block or re-route approvals with conflicting authority scope.
73. Beneficiary redaction retrospective disclosure controls use correction packages without overwriting historical reports.
74. Protector cost shortfall escalations block distribution until excess costs are approved or rejected.
75. Co-investment concentration breach cures recalculate exposure after approved allocation changes.
76. Estate equalization alternate asset substitutions reduce cash shortfalls only by approved transferable value.
77. Foundation recurring breach monitoring preserves breach history across remediation periods.
78. Authority renewal post-approval ratification blocks unratified approvals from downstream use.
79. Beneficiary correction-package delivery failures keep recipient disclosure state pending until remediated.
80. Protector reserve beneficiary objection windows restrict objected amounts until dispute resolution.
81. Co-investment allocation cancellation fees are charged to the cancelling entity and excluded from future calls.
82. Estate substitution valuation appeals recalculate equalization from the accepted appeal valuation.
83. Foundation suspension lift conditions keep grant release blocked until every mandatory condition is met or waived.
84. Authority ratification downstream reversals affect only scoped downstream actions and require remediation evidence.
85. Correction-package recipient waiver governance closes only recipient-specific delivery failures with trustee evidence.
86. Protector objection late acceptance disputes restrict only late objection amounts accepted for review.
87. Co-investment cancellation fee waivers charge only approved net fees to the cancelling entity.
88. Estate valuation appeal counterparty disputes keep substitute values provisional until dispute resolution.
89. Foundation conditional restart monitoring reactivates grant release blocks when monitoring evidence fails.
90. Downstream ratification remediation reporting preserves original, reversal and remediation audit states.
91. Correction waiver expiry controls require renewed waiver or delivery attempt for later correction packages.
92. Protector reserve mediation outcomes release only trustee-accepted mediated amounts and keep residual reserves restricted.
93. Co-investment cancelled-allocation replacement disputes keep replacement allocation separate from cancellation fee credit.
94. Estate substitute asset custody-transfer failures keep equalization credit provisional until custody delivery succeeds.
95. Foundation restart evidence sampling blocks next release when mandatory samples fail.
96. Ratification correction client-notice exceptions suppress only approved notice scope while preserving audit rationale.
97. Correction-waiver renewal denials return recipients to delivery-required state.
98. Protector mediated-release payment failures keep approved release amounts pending until settlement repair succeeds.
99. Replacement participant funding shortfalls separate funded commitments from unfunded replacement exposure.
100. Substitute asset title-defect cures credit only accepted transferable value.
101. Foundation sampling waiver expiries restore full evidence requirements unless renewed.
102. Client-notice exception audit challenges remediate suppressed notices outside approved scope.
103. Correction-waiver denial appeal reversals pause delivery only for approved period and scope.
104. Protector repaired-payment reversals return mediated releases to pending-payment state.
105. Replacement funding deadline extensions apply only to approved shortfall amounts and revised dates.
106. Title-cure residual cash calls close equalization only when cash replacement settles.
107. Sampling waiver renewal disputes keep full sampling active until council approval.
108. Notice-exception remediation notices close only after delivery, waiver or escalation evidence.
109. Correction appeal reversal expiry monitoring reactivates delivery when pauses expire without renewal.
110. Protector payment redirection approvals require verified account details and trustee approval.
111. Replacement allocation sponsor default events remove unaccepted funding from commitment exposure.
112. Residual cash-call beneficiary objections block objected amounts without waiving equalization.
113. Sampling renewal conditional approvals keep full sampling active until conditions are met.
114. Remediation notice localization failures block delivery until approved localized templates exist.
115. Appeal reversal post-expiry reactivation disputes keep delivery required unless a new pause is approved.

# Worked Examples and Implementation Patterns

This file turns entitlement, digital-channel, advisor-workbench, workflow and interaction-history concepts into practical examples for platform design, QA, audit evidence, support and stakeholder communication.

## Example 1. Relationship-manager access to a client portfolio

**Scenario:** An RM opens a client 360 view for a portfolio booked in Singapore.

The platform should not answer this with a single `role = RM` check.

| Decision input | Example value | Why it matters |
|---|---|---|
| User role | Relationship manager | Allows advisory and relationship actions only within assignment scope. |
| Relationship assignment | Primary RM for client group | Confirms the RM is attached to the relevant client or household. |
| Booking centre | Singapore | May restrict cross-border visibility or advice. |
| Portfolio status | Active advisory portfolio | Determines available workflow actions. |
| Data sensitivity | Standard client data | Controls masking and export permissions. |
| Channel | Internal advisor workbench | Different controls from client portal or mobile. |

**Good outcome:** The RM can view client-level and portfolio-level data, launch an advisory review, and see open tasks. The RM cannot approve their own exception, export restricted documents, or view unrelated family entities without explicit coverage or delegation.

**Implementation pattern:** Evaluate functional permission, data scope, relationship scope, booking-centre policy and action context in one policy decision. Return a decision with reason codes and evidence references.

## Example 2. Advisor can view but cannot propose

**Scenario:** An advisor is temporarily covering a client while the primary RM is away.

| Capability | Expected result | Reason |
|---|---|---|
| View client summary | Allow | Temporary coverage assignment is active. |
| View portfolio holdings | Allow with masking if policy requires | Portfolio belongs to the covered relationship. |
| Create product proposal | Block | Temporary coverage excludes advice authority. |
| Add service note | Allow | Service continuity is permitted. |
| Submit trade order | Block | No order authority or mandate delegation. |

**Control requirement:** The UI should disable blocked actions, but the API must enforce the same decision. A direct API call to create a proposal must fail with a safe, non-sensitive reason code.

## Example 3. Digital consent for an advisory proposal

**Scenario:** A client approves a structured-note proposal through a portal.

The consent record should link to:

1. client identity and authority basis,
2. proposal ID and immutable proposal version,
3. product termsheet version,
4. suitability result and policy version,
5. risk disclosure version,
6. channel and session ID,
7. authentication strength and step-up result,
8. timestamp and effective business date,
9. consent status and revocation status,
10. downstream order or workflow reference.

**Bad pattern:** Store only `client_approved = true`.

**Good pattern:** Store consent as an immutable evidence object that can be replayed during complaint handling, audit review, order reconstruction and suitability investigation.

## Example 4. Document delivery entitlement

**Scenario:** A quarterly consolidated report is generated for a household. A beneficiary requests portal access.

| Question | Required answer |
|---|---|
| Who is the legal recipient? | Account holder, trustee, authorized representative or other approved recipient. |
| What is the report scope? | Individual account, portfolio, household, trust, company or consolidated group. |
| Is beneficiary access permitted? | Depends on legal arrangement, consent, policy and report scope. |
| Is masking required? | Yes if the report includes entities or accounts outside the recipient's authority. |
| What evidence is stored? | Report version, data snapshot, recipient, delivery status, view/download time and archive reference. |

**Implementation pattern:** Report delivery should be a workflow event with entitlement evidence, not just a file download.

## Example 5. Workflow state blocks order release

**Scenario:** An advisory order is ready for release, but the suitability override is pending supervisor approval.

```text
ProposalCreated
  -> SuitabilityCheckFailed
  -> OverrideRequested
  -> SupervisorApprovalPending
  -> OrderReleaseBlocked
```

The order system should not rely on the advisor-workbench screen state. It should query the workflow and policy state before accepting release.

**QA assertion:** A test should attempt order release through the UI and API while approval is pending. Both paths must block release and preserve the same workflow reason.

## Example 6. Break-glass access during production support

**Scenario:** A production incident requires temporary access to a restricted client record.

Minimum controls:

1. explicit incident or support ticket reference,
2. named approver or emergency policy rule,
3. limited data scope,
4. limited action scope,
5. short expiry time,
6. mandatory justification,
7. high-signal audit event,
8. post-use review,
9. automatic revocation,
10. alert to control owners if policy requires it.

**Anti-pattern:** Add the user to a permanent admin group and remove it manually later.

## Example 7. Client interaction history for complaint reconstruction

**Scenario:** A client disputes whether they approved a trade.

The reconstruction should show:

| Evidence | Purpose |
|---|---|
| Meeting note | Advisor discussion summary and attendees. |
| Proposal version | What was recommended. |
| Disclosure version | What risks were shown. |
| Suitability result | Whether the recommendation passed policy. |
| Consent evidence | Whether the client approved and through which channel. |
| Order event | Whether approval preceded order submission. |
| Document delivery | Whether confirmations and statements were delivered. |
| Audit events | Who viewed, changed, approved or overrode each step. |

**Good outcome:** The bank can reconstruct the timeline without relying on free-text notes alone.

## Example 8. Entitlement regression matrix

Use a compact matrix to prevent role changes from creating hidden defects.

| Persona | Client summary | Holdings | Proposal | Trade release | Report export | Admin override |
|---|---:|---:|---:|---:|---:|---:|
| Primary RM | Allow | Allow | Allow | Conditional | Conditional | Block |
| Covering RM | Allow | Allow | Conditional | Block | Conditional | Block |
| Portfolio manager | Conditional | Allow | Conditional | Conditional | Block | Block |
| Operations user | Conditional | Conditional | Block | Conditional | Block | Block |
| Supervisor | Conditional | Conditional | Approve | Conditional | Conditional | Conditional |
| Client portal user | Conditional | Conditional | Approve own | Block | Own reports only | Block |
| External delegate | Conditional | Conditional | Conditional | Conditional | Conditional | Block |

**Testing rule:** Every `Conditional` cell should have a named policy reason, not a vague manual interpretation.

## Example 9. Legacy CRM notes migration

**Scenario:** Legacy CRM notes are migrated into a new interaction-history service.

| Source issue | Migration treatment |
|---|---|
| Free-text notes with no category | Classify as service, advice, complaint, instruction, meeting or general note where evidence supports it. |
| Missing client link | Quarantine until relationship mapping is confirmed. |
| Sensitive note content | Apply masking or restricted visibility rules. |
| Unknown author | Preserve source author text and mark lineage quality. |
| Duplicated notes | Deduplicate only when source ID, timestamp and content hash support it. |
| Historical consent claim | Do not convert into structured consent unless required evidence exists. |

**Migration sign-off:** Reconcile note counts, interaction categories, restricted-note counts, failed mappings and sample timeline reconstruction before go-live.

## Example 10. Advisor-workbench operating checklist

Before calling an advisor workbench production-ready, check:

1. every visible action is backed by API authorization,
2. every blocked action has a safe reason code,
3. every proposal links to suitability, disclosure and consent evidence,
4. every task has owner, SLA, status and escalation behavior,
5. every client-facing document has version and delivery evidence,
6. every sensitive interaction has visibility and retention rules,
7. every break-glass event is time-limited and reviewable,
8. every workflow can be reconstructed from events,
9. every dashboard metric has a source owner,
10. every entitlement rule has regression coverage.

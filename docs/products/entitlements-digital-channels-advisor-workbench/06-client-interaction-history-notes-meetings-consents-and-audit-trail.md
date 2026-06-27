# Client Interaction History, Notes, Meetings, Consents and Audit Trail

## 1. Purpose

Client interaction history is the durable record of communication, advice, service, consent and decisions. In wealth management, it is central to client continuity, suitability evidence, complaint handling, conduct supervision, regulatory review and advisor productivity.

Interaction history should capture both human and digital interactions:

- meetings;
- calls;
- advisor notes;
- secure messages;
- email metadata where integrated;
- portfolio review discussions;
- proposal presentation;
- product disclosures;
- client consent;
- document delivery and acknowledgement;
- service requests;
- complaints;
- instructions;
- approvals and refusals.

## 2. Interaction taxonomy

| Interaction type | Description |
|---|---|
| MEETING | In-person or virtual meeting. |
| CALL | Phone/video call. |
| SECURE_MESSAGE | Portal/mobile secure message. |
| EMAIL_REFERENCE | Linked email metadata or archived communication. |
| PORTFOLIO_REVIEW | Formal review discussion. |
| ADVISORY_DISCUSSION | Investment recommendation discussion. |
| PROPOSAL_PRESENTED | Proposal shown to client. |
| DISCLOSURE_SHOWN | Risk disclosure or product document shown. |
| CONSENT_CAPTURED | Client approval or acknowledgement. |
| INSTRUCTION_RECEIVED | Client instruction. |
| COMPLAINT | Complaint or dissatisfaction. |
| SERVICE_REQUEST | Non-investment service action. |
| DOCUMENT_DELIVERED | Statement/report/confirmation delivered. |
| DIGITAL_ACTION | Login, document view, online approval. |

## 3. Interaction record model

| Field | Meaning |
|---|---|
| interaction_id | Unique ID. |
| interaction_type | Meeting, call, message, proposal, consent, etc. |
| interaction_datetime | When interaction happened. |
| captured_datetime | When recorded. |
| channel | In-person, phone, portal, mobile, email, video. |
| client_id / household_id | Relationship context. |
| account_id / portfolio_id | Optional investment context. |
| participants | Client, RM, IC, PM, delegate, interpreter, supervisor. |
| owner_user_id | Primary internal owner. |
| subject | Short description. |
| summary | Narrative or structured summary. |
| tags | Advice, suitability, complaint, instruction, risk, service. |
| linked_objects | Proposal, order, report, document, task, case. |
| sensitivity | Normal, confidential, restricted. |
| retention_category | Retention policy. |
| audit_hash | Tamper-evidence/hash where required. |

## 4. Meeting notes

Meeting notes should capture a structured minimum.

| Section | What to record |
|---|---|
| Participants | Who attended and in what capacity. |
| Purpose | Review, advice, service, complaint, onboarding. |
| Client objective | Income, growth, liquidity, protection, succession, leverage. |
| Portfolio context | Portfolio/account/report snapshot discussed. |
| Product discussion | Product families and specific products mentioned. |
| Risk discussion | Risks explained and client concerns. |
| Recommendation | If advice given, link to proposal/suitability. |
| Decision | Proceed, defer, reject, ask for more information. |
| Follow-up | Tasks, owners, due dates. |
| Evidence links | Documents, presentation, disclosure, report. |

## 5. Consent versus acknowledgement versus instruction

These are different concepts.

| Concept | Meaning | Example |
|---|---|---|
| Acknowledgement | Client confirms they received/read information. | Product risk disclosure acknowledged. |
| Consent | Client agrees to a specific action or processing. | Approves advisory proposal. |
| Instruction | Client directs the bank to do something. | Transfer cash, subscribe fund, elect corporate action. |
| Approval | Internal user approves workflow progression. | Supervisor approves suitability override. |
| Attestation | User declares review or control completed. | RM attests client review done. |

A good platform must model these separately.

## 6. Consent evidence requirements

| Evidence | Why important |
|---|---|
| Consenting party identity | Confirms who approved. |
| Authority basis | Confirms whether they had right to approve. |
| Object version | Shows exactly what was approved. |
| Disclosure version | Shows what was shown. |
| Authentication context | Shows confidence level. |
| Timestamp | Establishes sequence before execution. |
| Channel | Portal/mobile/in-person/advisor-assisted. |
| Device/session metadata | Supports investigation. |
| Withdrawal/revocation | Tracks later change. |

## 7. Audit trail design

Audit trail should record business actions, not just technical logs.

| Audit event | Example payload |
|---|---|
| USER_LOGIN | User, channel, device, result. |
| CLIENT_VIEWED | User, client, reason/context. |
| PORTFOLIO_REVIEW_GENERATED | Snapshot, valuation date, user. |
| PROPOSAL_CREATED | Proposal version, products, portfolio. |
| SUITABILITY_CHECK_RUN | Inputs, result, reason codes. |
| DISCLOSURE_SHOWN | Document version, language, channel. |
| CONSENT_CAPTURED | Consenting party, proposal version, timestamp. |
| ORDER_RELEASED | Order ID, approval evidence, user. |
| REPORT_DELIVERED | Document, recipient, channel, delivery status. |
| NOTE_CREATED | Note ID, author, visibility. |
| ENTITLEMENT_CHANGED | Changed permission, approver, effective dates. |
| DATA_CORRECTED | Before/after values, maker/checker, reason. |

## 8. Interaction visibility and privacy

Not every note should be visible to every user.

| Visibility type | Use |
|---|---|
| RM team | Normal service/advisory notes. |
| Supervisor | Conduct review and oversight. |
| Compliance restricted | Complaints, sensitive suitability issues. |
| Trust/fiduciary restricted | Trust beneficiaries, settlor instructions. |
| Legal hold | Litigation, investigation, complaint. |
| Client-visible | Client-entered messages or selected meeting outputs. |

Access should be entitlement-controlled and audited.

## 9. Complaints and sensitive interactions

A complaint should be elevated from ordinary notes into a controlled case.

Complaint indicators:

- client alleges loss due to advice;
- client disputes transaction/report/fee;
- client claims lack of disclosure;
- client alleges unauthorized transaction;
- client expresses formal dissatisfaction;
- client requests escalation or compensation.

Complaint record should preserve:

- original communication;
- involved products and accounts;
- advisor notes;
- proposal and disclosure versions;
- order timeline;
- report/documents shown;
- suitability and approval evidence;
- response timeline.

## 10. Timeline reconstruction

A platform should reconstruct a timeline like this:

```text
2026-06-01 09:15 RM viewed portfolio review snapshot v1
2026-06-01 10:00 RM met client and discussed income objective
2026-06-01 11:30 Proposal v1 created for bond ladder
2026-06-01 11:35 Suitability passed
2026-06-01 11:40 Risk disclosure v3 shown
2026-06-01 11:45 Client consent captured via mobile step-up
2026-06-01 11:50 Order released to OMS
2026-06-01 14:00 Trade executed
2026-06-02 09:00 Confirmation delivered
```

This is invaluable for audit, complaint handling and production support.

## 11. Interaction quality controls

| Control | Purpose |
|---|---|
| Mandatory note for advice | Evidence advisory discussion. |
| Note locking | Prevent late undocumented edits. |
| Structured tags | Enable surveillance and search. |
| Linked object requirement | Tie note to proposal/report/order. |
| Restricted words review | Identify possible complaint/promissory language. |
| Late note alert | Notes created too long after meeting. |
| Client instruction classification | Move instructions into workflow. |
| Retention policy | Ensure records retained and disposed appropriately. |

## 12. QA scenarios

| Scenario | Expected result |
|---|---|
| Advisor creates proposal without meeting note where required. | Warning/block per policy. |
| Note edited after lock period. | Requires correction workflow or addendum. |
| Client instruction sent via secure message. | Instruction workflow created. |
| Proposal consent cannot be linked to disclosure version. | Order release blocked. |
| User tries to view restricted complaint note. | Denied unless permitted. |
| Report delivered but not archived. | Delivery control exception. |
| Complaint case opened. | Relevant interactions preserved and linked. |
| Client asks what they approved. | Platform can show consent object and document versions. |

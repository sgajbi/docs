# Digital Channels, Client Portal, Mobile, Secure Messaging and Document Delivery

## 1. Digital-channel purpose

Digital channels extend wealth servicing beyond the advisor desktop. They allow clients and authorized parties to view information, receive documents, approve proposals, send secure messages, complete service requests and sometimes place orders.

Digital wealth channels must balance convenience with suitability, privacy, cross-border, security and operational controls.

## 2. Main digital-channel capabilities

| Capability | Client value | Control focus |
|---|---|---|
| Portfolio view | See holdings, performance, cash and transactions. | Data accuracy, valuation date, entitlements. |
| Statement access | Download statements and reports. | Recipient authority, archive, delivery evidence. |
| Secure messaging | Communicate with RM/service team. | Message retention, instruction classification. |
| Digital consent | Approve proposal, disclosure, document, instruction. | Authentication, timestamp, version evidence. |
| Service request | Update details, request statement, submit form. | Workflow, identity, maker-checker. |
| Order placement | Self-directed or advised order flow. | Suitability, appropriateness, product eligibility. |
| Alerts | Notify about maturity, margin call, document, action. | Timing, channel preference, delivery evidence. |
| Document upload | Submit KYC/tax/supporting documents. | Malware scanning, privacy, retention. |
| Appointment booking | Schedule review with advisor. | Calendar integration, privacy. |
| Push approval | Mobile approval of proposal/instruction. | Device binding, step-up authentication, non-repudiation. |

## 3. Client portal versus advisor workbench

| Area | Client portal | Advisor workbench |
|---|---|---|
| Audience | Client, authorized representative, family-office user. | RM, IC, PM, supervisor, operations. |
| Data view | Client-facing, curated, disclosure-aware. | Internal, richer, operational and exception-aware. |
| Language | Plain, explanatory, client-safe. | Professional, analytical, workflow-oriented. |
| Controls | Identity, consent, privacy, document delivery. | Entitlement, suitability, product, workflow, audit. |
| Actions | View, approve, message, request, sometimes trade. | Advise, propose, check, approve, submit, service. |
| Reporting | Final statements and reviews. | Draft/proposed/review views and exception panels. |

## 4. Authentication and session controls

Digital channels should use risk-based authentication.

| Control | Purpose |
|---|---|
| MFA | Reduce account takeover risk. |
| Device binding | Link trusted mobile/browser devices. |
| Step-up authentication | Require stronger proof for sensitive actions. |
| Session timeout | Limit exposure from unattended sessions. |
| Transaction signing | Bind approval to specific details shown to client. |
| Push confirmation | Confirm high-risk action from registered device. |
| Risk analytics | Detect unusual device/location/IP/action pattern. |
| Account lockout / throttling | Reduce brute-force and credential-stuffing risk. |
| Reauthentication | Confirm identity before viewing sensitive data or approving transaction. |

Sensitive actions include:

- approving advisory proposal;
- approving order or rebalance;
- changing contact details;
- adding delegate/authorized user;
- downloading large report package;
- changing delivery preference;
- sending funds transfer instruction;
- accepting product disclosure for complex products.

## 5. Digital consent model

Digital consent is not just a checkbox. It should capture what the client saw, when they saw it, and what they approved.

| Field | Meaning |
|---|---|
| consent_id | Unique consent record. |
| consenting_party_id | Client or authorized party. |
| authority_basis | Self, POA, trustee, signatory, delegate. |
| consent_type | Proposal approval, disclosure acknowledgement, document acceptance. |
| linked_object_type | Proposal, order, report, product, service request. |
| linked_object_version | Exact version approved. |
| channel | Portal, mobile, assisted digital. |
| authentication_context | AAL/MFA/step-up/device/session info. |
| displayed_summary_hash | Hash of key terms shown. |
| document_hashes | Proposal/disclosure/document versions. |
| consent_timestamp | Date/time of consent. |
| ip/device/location metadata | Risk and audit evidence. |
| withdrawal_or_revocation | If consent later revoked. |

### Digital consent should answer

- Who approved?
- Did that party have authority?
- What exactly was approved?
- Which version was approved?
- Which disclosure was shown?
- Was approval before execution?
- Was strong authentication used?
- Was there any later amendment?

## 6. Secure messaging

Secure messaging should be treated as a regulated interaction channel, not a chat feature.

| Message type | Treatment |
|---|---|
| General query | Route to RM/service queue. |
| Investment discussion | Link to advisory record if advice is given. |
| Client instruction | Convert to workflow case with verification. |
| Complaint | Route to complaint workflow. |
| Document request | Fulfil through document service. |
| Sensitive instruction | Require step-up/call-back/maker-checker. |

### Message metadata

| Field | Purpose |
|---|---|
| message_id | Unique message. |
| thread_id | Conversation grouping. |
| sender/recipient | Participants. |
| role_at_time | Client/RM/delegate/operations. |
| linked_client/account/portfolio | Context. |
| classification | Query, instruction, advice, complaint, service. |
| sensitivity | Normal, confidential, restricted. |
| retention_category | Recordkeeping requirement. |
| attachments | Document IDs and scan status. |
| workflow_case_id | If converted to case. |

## 7. Document delivery

Documents must be delivered only to authorized recipients and with evidence.

| Document type | Delivery concerns |
|---|---|
| Account statement | Recipient authority, consolidation scope, archive. |
| Portfolio review | Version, benchmark, performance disclosures, stale data flags. |
| Trade confirmation | Timeliness, account accuracy, settlement details. |
| Product disclosure | Correct product, language, jurisdiction, version. |
| Advisory proposal | Exact version before consent. |
| Tax document | Tax residency and recipient authority. |
| Corporate action notice | Deadline, eligible holder, election options. |
| Margin call notice | Time-critical delivery and escalation evidence. |

### Delivery state model

```text
GENERATED
-> APPROVED_FOR_DELIVERY
-> DELIVERED_TO_CHANNEL
-> VIEWED_OR_DOWNLOADED
-> ACKNOWLEDGED / EXPIRED / FAILED
-> ARCHIVED
```

Not every document requires acknowledgement, but every material document should have delivery evidence.

## 8. Client-visible reporting rules

Client channel must avoid showing internal-only diagnostics as client facts.

| Internal concept | Client-safe wording |
|---|---|
| Stale price flag | "Latest available price is from [date]." |
| Missing benchmark component | "Benchmark comparison is temporarily unavailable." |
| Suitability block | "This product is not currently available for online subscription." |
| Degraded calculation | "Some analytics may be incomplete due to pending data update." |
| Corporate action pending | "Action required by [deadline], subject to eligibility." |

## 9. Cross-border and channel restrictions

Digital channels must respect distribution and servicing restrictions.

| Restriction | Example |
|---|---|
| Product not distributable online. | Structured note requires advisor-assisted process. |
| Country-specific document. | Singapore PHS/KID, local disclosure variant. |
| Client outside servicing jurisdiction. | Disable proposal/trade action. |
| Language requirement. | Show disclosure in approved language. |
| Complex product access. | Require knowledge/experience assessment. |
| Report delivery restriction. | Do not deliver consolidated family-office report to unauthorized party. |

## 10. Channel event model

Digital channel actions should emit events.

```json
{
  "eventType": "DOCUMENT_VIEWED",
  "eventTime": "2026-06-27T11:00:00+08:00",
  "channel": "CLIENT_PORTAL",
  "actorId": "CLIENT123",
  "documentId": "DOC456",
  "documentVersion": "v3",
  "clientId": "CIF789",
  "authenticationContext": {
    "mfa": true,
    "deviceTrusted": true,
    "stepUp": false
  }
}
```

Events that often need durable storage:

- login;
- failed login;
- device registration;
- document delivered/viewed/acknowledged;
- consent captured;
- message sent/read;
- service request created;
- instruction submitted;
- disclosure shown;
- suitability result viewed;
- order submitted;
- report downloaded.

## 11. Mobile-specific considerations

| Topic | Design implication |
|---|---|
| Device binding | Device enrollment should require strong authentication. |
| Biometrics | Treat as local activation factor, not sole legal identity proof. |
| Push approval | Bind approval to transaction details. |
| Offline mode | Avoid showing stale sensitive data without clear timestamp. |
| Screenshot risk | Consider masking for highly sensitive screens. |
| Jailbreak/root detection | Risk signal, not sole control. |
| Notification privacy | Avoid exposing portfolio details in push previews. |
| App version | Block critical outdated versions when security fixes are required. |

## 12. QA scenarios

| Scenario | Expected result |
|---|---|
| Client downloads statement for own account. | Allowed, delivery event stored. |
| Beneficiary tries to view trust settlor account. | Denied unless authorized. |
| Client approves proposal after it was amended. | Must approve latest version; old consent invalid. |
| Secure message contains trade instruction. | Routed to instruction workflow, not treated as normal note. |
| Product disclosure updated after proposal sent. | Re-consent required if material. |
| Mobile approval from new device. | Step-up required or blocked. |
| Document delivery fails. | Retry/escalation and failed-delivery evidence. |
| Client outside permitted geography tries to subscribe. | Block or advisor-assisted path per policy. |

# Advisor Workbench, Client 360 and Portfolio Workflows

## 1. Purpose of the advisor workbench

An advisor workbench is the front-office control tower. It should help an advisor understand the client, review the portfolio, identify opportunities and risks, prepare recommendations, evidence suitability, capture consent and hand off orders or service requests.

A strong advisor workbench is not just a screen. It is an orchestration layer across:

- client master;
- account and portfolio master;
- product master and APU;
- market data and research;
- holdings and transactions;
- performance, risk and exposure analytics;
- mandate and suitability rules;
- proposal and order workflows;
- document and disclosure services;
- client interaction history;
- task and alert management.

## 2. Advisor workbench capability map

| Capability | Description | Platform dependency |
|---|---|---|
| Client search | Find clients, households, entities and portfolios. | Client/account master and entitlements. |
| Client 360 | Unified relationship view. | Master data, holdings, transactions, analytics, interactions. |
| Portfolio review | Holdings, allocation, performance, risk, income, liquidity. | Portfolio analytics and reporting engine. |
| Advisory ideas | Product ideas, model gaps, concentration issues, cash deployment. | Product governance, idea engine, suitability. |
| Proposal generation | Construct recommendation and impact analysis. | Rebalancing, simulation, mandate checks. |
| Suitability check | Validate product and recommendation against client profile. | Suitability rules, target market, disclosures. |
| Pre-trade checks | Cash, buying power, restrictions, product eligibility. | OMS, MIDAS-like buying power, rules engine. |
| Client consent | Capture approval and disclosures. | Digital signature, secure channel, audit. |
| Task management | Next best actions, follow-ups, approvals. | Workflow engine. |
| Interaction history | Calls, meetings, notes, emails/messages, documents. | CRM, secure messaging, document service. |

## 3. Client 360 structure

A practical client 360 should be layered.

| Section | Examples |
|---|---|
| Relationship header | Client name, segment, booking centre, RM, household, service model. |
| Alerts | KYC expiry, suitability expiry, cash concentration, mandate breach, margin call. |
| Total wealth | Consolidated market value, assets by booking centre/account/portfolio. |
| Portfolio summary | Allocation, performance, risk, income, liquidity, currency exposure. |
| Accounts | Banking, custody, DPM, advisory, execution-only, lending. |
| Products held | Equities, bonds, funds, notes, alternatives, cash, loans, insurance. |
| Advice status | Active proposals, pending consents, restricted products, suitability exceptions. |
| Interaction history | Last meeting, last review, notes, tasks, messages. |
| Documents | Statements, portfolio reviews, confirmations, disclosures, agreements. |
| Service cases | Open instructions, onboarding/KYC tasks, complaints, operational breaks. |

## 4. Portfolio workflow examples

### Portfolio review workflow

```text
Select client/portfolio
-> Check entitlement and service model
-> Load holdings, cash, transactions and valuations
-> Calculate allocation, performance, risk, concentration and income
-> Compare against mandate/model/benchmark
-> Highlight issues and opportunities
-> Generate review pack
-> Capture advisor notes and follow-up tasks
-> Archive snapshot and evidence
```

Key controls:

- report and dashboard numbers must come from a consistent valuation date;
- stale price and missing data exceptions must be visible;
- advisor notes should link to the reviewed snapshot;
- report version should be archived if shared with client.

### Proposal workflow

```text
Start proposal
-> Select objective and product/model idea
-> Simulate target portfolio
-> Show impact on allocation, risk, income, liquidity, concentration and cost
-> Run suitability, product eligibility, mandate and pre-trade checks
-> Generate proposal document and disclosures
-> Capture client consent
-> Submit order or rebalance workflow
-> Archive proposal, consent and decision evidence
```

Key controls:

- proposed holdings must be valued using defined price sources;
- suitability results must be point-in-time;
- rejected or overridden checks need reason and approval;
- consent must precede execution where required;
- execution should reference approved proposal version.

### DPM rebalance workflow

```text
Model or mandate drift detected
-> PM reviews drift and cash constraints
-> Generate trade list
-> Run mandate and pre-trade checks
-> Review liquidity and transaction cost
-> Approve rebalance
-> Send orders to OMS
-> Monitor execution and allocation
-> Post-trade compliance review
-> Update portfolio review and client reporting
```

Key controls:

- model version and target allocation must be effective-dated;
- account-specific restrictions must override model intent;
- small accounts, illiquid assets and tax/cost constraints may require exclusion;
- execution must maintain fairness and allocation policy.

## 5. Advisor alerts and next-best-action design

Alerts should be grouped by severity and actionability.

| Alert type | Example | Action |
|---|---|---|
| Regulatory | KYC expired, suitability expired. | Block advice/trade until refreshed. |
| Risk | Concentrated equity, high leverage, margin shortfall. | Review with client or risk team. |
| Mandate | DPM drift, breach, restricted holding. | Rebalance or document exception. |
| Portfolio | Cash drag, under-diversification, benchmark deviation. | Advisory idea/proposal. |
| Product lifecycle | Bond maturity, note observation, fund gate, corporate action. | Client/advisor action. |
| Service | Pending document, failed settlement, missing instruction. | Operations follow-up. |
| Relationship | Review overdue, birthday/event, meeting follow-up. | Contact client. |

Alert rules should include:

- source data;
- calculation timestamp;
- severity;
- owner;
- due date;
- suppress/resolve logic;
- evidence link;
- client-visible flag.

## 6. Advisor notes design

Advisor notes can become sensitive evidence. They should be structured enough to support audit, but flexible enough for real conversations.

| Field | Purpose |
|---|---|
| note_id | Unique note. |
| client_id / household_id | Relationship context. |
| portfolio_id | Optional portfolio context. |
| interaction_type | Meeting, call, review, proposal, service. |
| subject | Short topic. |
| note_body | Narrative. |
| tags | Suitability, risk, product, complaint, instruction. |
| linked_documents | Proposal, statement, disclosure. |
| linked_workflow | Case/proposal/order/task. |
| visibility | RM team, supervisor, compliance, restricted. |
| created_by / created_at | Audit. |
| immutable_after | Locking policy. |

### Note quality guidance

Good notes should capture:

- what was discussed;
- client objective or concern;
- risk/disclosure points explained;
- decision or no-decision outcome;
- follow-up action;
- who attended;
- whether any client instruction was received.

Bad notes:

- contain unsupported promises;
- include confidential information unrelated to the client;
- record investment advice without suitability evidence;
- omit meeting participants;
- cannot be linked to proposal/report shown.

## 7. Product-specific advisor-workbench needs

| Product area | Workbench needs |
|---|---|
| Bonds | Yield, duration, maturity ladder, issuer concentration, call risk. |
| Structured notes | Payoff scenario, barrier distance, issuer risk, lifecycle observations. |
| Funds | NAV date, fees, liquidity, share class, distribution policy, look-through. |
| Equities | corporate actions, dividends, concentration, ESG/restricted list. |
| Derivatives | notional, Greeks, margin, collateral, expiry, exercise. |
| Private markets | commitment, funded/unfunded, capital calls, NAV lag, distributions. |
| Loans/collateral | LTV, haircut, availability, margin call, pledge relationships. |
| Insurance | policy value, premium schedule, surrender charge, beneficiary. |

## 8. Advisor-dashboard metrics

| Metric | Meaning |
|---|---|
| Total AUM / AUA | Assets under management/advisory for advisor/team. |
| Review overdue count | Clients not reviewed within required frequency. |
| KYC/suitability expiry count | Clients requiring refresh. |
| Proposal pipeline | Draft, pending approval, client sent, accepted/rejected. |
| Mandate breaches | Active DPM/advisory mandate exceptions. |
| Concentration alerts | Clients with high issuer/currency/product concentration. |
| Cash opportunity | High idle cash or deposit maturity. |
| Maturity pipeline | Bonds/notes/deposits maturing soon. |
| Open service cases | Operational work requiring follow-up. |

## 9. Implementation anti-patterns

| Anti-pattern | Why it fails |
|---|---|
| Client 360 directly queries many source systems at runtime. | Slow, inconsistent and hard to audit. |
| Workbench has different numbers from statements. | Client trust and control issue. |
| Proposal can be edited after consent without versioning. | Audit and dispute risk. |
| Suitability checks only run at order time. | Advisor may discuss ineligible product earlier. |
| Notes are unstructured and unsearchable. | Weak evidence and poor continuity. |
| Alerts have no owner or resolution state. | Alert fatigue and missed controls. |
| Workflows are implemented only as UI screens. | No reliable state, SLA or audit trail. |

## 10. Practitioner Explanation

"An advisor workbench should not just aggregate screens. It should orchestrate the end-to-end advisory journey: client 360, portfolio review, idea generation, proposal simulation, suitability, mandate checks, pre-trade checks, consent capture, order hand-off, document archive and interaction history. The most important design principles are entitlement-aware data access, point-in-time evidence, consistent reporting snapshots, workflow state management and strong audit lineage."

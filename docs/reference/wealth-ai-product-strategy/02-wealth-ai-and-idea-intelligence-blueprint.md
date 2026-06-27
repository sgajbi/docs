# 02 - Wealth AI and Idea Intelligence Blueprint

## 1. Product intent

Wealth AI and Idea Intelligence should not be generic AI chat apps. They should be wealth-native intelligence products that sit above the wealth product, analytics, advisory, risk, reporting and platform capabilities.

The product vision:

```text
Wealth AI: the governed intelligence layer for wealth platforms.
Idea Intelligence: the idea generation, personalization and recommendation engine for advisors, portfolio managers and clients.
```

## 2. Suggested product boundary

| Capability | Wealth AI | Idea Intelligence |
|---|---|---|
| Product knowledge answer generation | Yes | Uses it indirectly |
| Portfolio explanation | Yes | Uses analytics as input |
| Report narrative generation | Yes | Uses generated insights |
| Advisor meeting preparation | Yes | Provides client/portfolio context |
| Idea generation | Supports | Core capability |
| Next best action | Supports | Core capability |
| Suitability-aware filtering | Shared | Core guardrail |
| Mandate-aware recommendation | Shared | Core guardrail |
| Agentic workflows | Core platform | Uses approved workflows |
| AI governance / evaluation | Core platform | Must inherit controls |

## 3. Wealth AI modules

```text
ai-orchestration-service
  |-- ai-gateway
  |-- context-builder
  |-- retrieval-service
  |-- prompt-registry
  |-- policy-and-guardrail-engine
  |-- tool-orchestrator
  |-- response-grounding-service
  |-- evaluation-service
  |-- feedback-service
  |-- audit-and-lineage-service
  `-- model-router
```

### 3.1 AI Gateway

The AI Gateway is the controlled entry point for AI interactions.

Responsibilities:

- authenticate and authorize the user
- apply channel and role policies
- enforce client/account/portfolio scope
- route to the correct model or tool workflow
- redact or mask sensitive data when required
- attach correlation ID, trace ID and audit metadata
- capture prompt, context references, response and feedback

### 3.2 Context Builder

The Context Builder assembles approved context for the AI request.

Inputs may include:

- user role and entitlements
- client profile
- portfolio holdings
- transactions and cashflows
- performance, risk and attribution results
- mandate rules and breaches
- product termsheets and product governance records
- market data snapshots
- previous interaction history
- report snapshots

Context should be minimal, relevant, entitlement-filtered and source-linked.

### 3.3 Retrieval Service

The Retrieval Service supports RAG over approved knowledge sources:

- product knowledge base
- product termsheets
- approved product universe
- suitability policy
- mandate policy
- report methodology
- calculation methodology
- source ownership guides
- operations runbooks
- client-facing disclosure libraries

### 3.4 Prompt Registry

The Prompt Registry stores versioned prompt templates.

Each prompt should have:

| Field | Meaning |
|---|---|
| prompt_id | Stable identifier |
| version | Version number |
| owner | Business/technology owner |
| use_case | Meeting prep, report narrative, product explanation, etc. |
| allowed_roles | Advisor, PM, Ops, QA, Admin |
| required_context | Inputs required |
| blocked_context | Inputs not allowed |
| output_schema | Expected structured response |
| evaluation_set | Regression tests |
| approval_status | Draft / approved / retired |

### 3.5 Policy and Guardrail Engine

The policy engine enforces:

- no advice without suitability context
- no trade action without approval
- no unsupported product recommendation
- no client data leakage across accounts
- no cross-border distribution violation
- no investment claim without source evidence
- no report-ready narrative without data-quality status
- no tool call beyond user entitlement

### 3.6 Tool Orchestrator

Tools should be narrow, typed and permissioned.

Examples:

- get portfolio summary
- get holdings by asset class
- get performance attribution
- check mandate drift
- check suitability flags
- search product knowledge
- create draft report commentary
- create advisor task
- generate test scenario
- explain transaction break

The AI should not directly update orders, trades, positions, suitability profiles or client reports without workflow approval.

## 4. Idea Intelligence modules

```text
idea-intelligence-service
  |-- idea-universe-service
  |-- signal-ingestion-service
  |-- portfolio-fit-engine
  |-- personalization-engine
  |-- suitability-filter
  |-- mandate-filter
  |-- risk-budget-filter
  |-- liquidity-and-cash-filter
  |-- idea-ranking-engine
  |-- idea-explanation-service
  |-- proposal-draft-service
  |-- idea-feedback-loop
  `-- idea-governance-service
```

## 5. Idea Intelligence capability map

| Capability | Description |
|---|---|
| Idea sourcing | Ingest house views, approved product lists, research themes, model changes and risk alerts. |
| Client relevance | Match ideas to client objectives, risk profile, holdings, currency, liquidity and preferences. |
| Portfolio fit | Check diversification, concentration, factor exposure, income objective and risk budget. |
| Suitability filtering | Remove ideas that fail client, jurisdiction, booking centre or product-governance constraints. |
| Mandate filtering | Ensure DPM / advisory mandate constraints are respected. |
| Ranking | Rank ideas by relevance, urgency, risk-adjusted fit, actionability and evidence strength. |
| Explanation | Explain why the idea is relevant and what risks should be discussed. |
| Proposal support | Draft a human-reviewed proposal with alternatives, risks and disclosures. |
| Feedback learning | Learn from advisor acceptance, rejection, client response and subsequent outcomes. |

## 6. Differentiation

Wealth AI / Idea Intelligence can differentiate by being:

| Differentiator | Explanation |
|---|---|
| Wealth-native | Built around portfolio, product, mandate, suitability and reporting concepts. |
| Multi-product | Handles notes, bonds, funds, equities, derivatives, alternatives, lending, insurance and real assets. |
| Analytics-aware | Uses performance, attribution, risk, exposure, concentration and cashflow analytics. |
| Mandate-aware | Understands DPM/advisory constraints, drift, restricted assets and breach severity. |
| Explainable | Produces narrative with source evidence and calculation lineage. |
| Governance-first | Designed with policy checks, audit, approval and model-risk controls. |
| Agent-ready | Uses controlled tools for safe workflow automation. |
| Repo-backed | Can use high-quality markdown knowledge base as product/domain grounding source. |

## 7. Minimum viable product

### MVP 1 - Advisor preparation copilot

Inputs:

- client profile
- holdings
- performance summary
- risk summary
- recent transactions
- upcoming maturities/capital calls
- mandate breaches

Outputs:

- meeting brief
- key portfolio changes
- risks to discuss
- questions to ask client
- possible follow-up tasks

### MVP 2 - Portfolio review narrative generator

Inputs:

- report snapshot
- TWR/MWR
- contribution/attribution
- asset allocation
- risk metrics
- benchmark comparison
- data quality flags

Outputs:

- executive summary
- performance explanation
- risk explanation
- benchmark-relative explanation
- exception wording

### MVP 3 - Product knowledge assistant

Inputs:

- product knowledge base
- product taxonomy
- notes/bonds/funds/equities/derivatives docs
- source notes

Outputs:

- product explanations
- lifecycle modelling help
- advisory talking points
- transaction modelling patterns
- QA scenarios

### MVP 4 - Idea generation assistant

Inputs:

- approved ideas
- model views
- client profile
- portfolio exposure
- cash/liquidity
- suitability/mandate rules

Outputs:

- ranked idea shortlist
- rationale
- risks
- required approvals
- proposal draft

## 8. What Wealth AI should not do initially

| Do not do | Reason |
|---|---|
| Fully autonomous advice | Regulated and high-risk. |
| Direct trade execution | Requires strict workflow, consent and controls. |
| Unsupported product recommendation | Must come from approved product universe. |
| Uncited claims | Not acceptable for wealth/reporting use. |
| Cross-client comparison using private data | Privacy and confidentiality risk. |
| Hidden learning from client data | Consent, privacy and model-risk concern. |

## 9. Success metrics

| Metric | Example |
|---|---|
| Productivity | Advisor meeting prep time reduced by 40%. |
| Quality | Portfolio explanations pass review checklist. |
| Grounding | 95% of factual claims have source references. |
| Safety | Zero unauthorized client-data leakage events. |
| Adoption | Weekly active advisor usage. |
| Business impact | More timely reviews, better coverage of clients. |
| Control impact | Faster exception triage and fewer unresolved breaks. |
| AI quality | Regression pass rate, hallucination rate, answer completeness. |

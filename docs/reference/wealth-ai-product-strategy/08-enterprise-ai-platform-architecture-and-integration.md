# 08 - Enterprise AI Platform Architecture and Integration

## 1. Target architecture

```text
Channels
  |-- Advisor Workbench
  |-- Client Portal / Mobile
  |-- Operations Console
  |-- Reporting UI
  |-- Engineering / Engineering Workbench
  `-- Admin / Governance Console

AI Experience APIs
  |-- Chat / answer generation API
  |-- Meeting Brief API
  |-- Portfolio Narrative API
  |-- Idea Generation API
  |-- Operations Triage API
  |-- Product Knowledge API
  `-- Evaluation API

AI Orchestration Services
  |-- AI Gateway
  |-- Model Router
  |-- Prompt Registry
  |-- Context Builder
  |-- Retrieval Service
  |-- Tool Orchestrator
  |-- Policy Engine
  |-- Response Validator
  |-- Evaluation Service
  `-- Audit / Lineage Service

Domain Services
  |-- Client Master
  |-- Portfolio Master
  |-- Product Master
  |-- Market Data
  |-- Holdings / Transactions / Cash
  |-- Performance / Risk / Attribution
  |-- Mandates / Suitability / APU
  |-- Reporting / Documents
  `-- Workflow / Interaction History

Data Stores
  |-- Knowledge Store
  |-- Vector Index
  |-- Prompt Store
  |-- Evaluation Store
  |-- Audit Store
  |-- Feature / Signal Store
  `-- Feedback Store
```

## 2. AI service boundaries

| Service | Responsibility |
|---|---|
| AI Gateway | Auth, policy entry point, rate limits, audit, routing. |
| Model Router | Chooses model based on use case, risk, cost, latency and data class. |
| Context Builder | Builds minimal, entitled and task-specific context. |
| Retrieval Service | RAG over approved documents and knowledge stores. |
| Tool Orchestrator | Executes typed, permissioned tools. |
| Policy Engine | Blocks unsafe or unauthorized requests/actions. |
| Response Validator | Checks schema, citations, prohibited content and consistency. |
| Evaluation Service | Runs regression and quality tests. |
| Feedback Service | Captures human corrections and user ratings. |
| Audit Service | Stores trace, prompt, context IDs, model, tool calls and approvals. |

## 3. API patterns

### 3.1 Meeting brief API

```json
{
  "clientId": "C123",
  "portfolioIds": ["P100", "P200"],
  "briefType": "PORTFOLIO_REVIEW_PREP",
  "asOfDate": "2026-06-27",
  "includeSections": ["performance", "risk", "mandates", "cashflows", "actions"]
}
```

Response should include:

- structured sections
- source references
- data-quality flags
- generated narrative
- confidence/status
- audit ID

### 3.2 Portfolio narrative API

Inputs:

- report_snapshot_id
- section requested
- narrative style
- audience
- language
- disclosure profile

Outputs:

- draft narrative
- evidence references
- unsupported claim warnings
- data-quality caveats

### 3.3 Idea generation API

Inputs:

- client ID
- portfolio ID
- objective
- constraints
- idea universe
- suitability/mandate check required

Outputs:

- eligible ideas
- rejected ideas and reasons
- ranking rationale
- risks and alternatives
- required approvals

## 4. Event patterns

AI can react to domain events:

| Event | AI workflow |
|---|---|
| PortfolioReviewRequested | Generate draft summary and discussion points. |
| MandateBreachDetected | Draft breach explanation and remediation options. |
| BondMaturityUpcoming | Generate reinvestment review task. |
| ProductSuspended | Identify impacted portfolios and advisor tasks. |
| PriceStaleDetected | Draft reporting exception wording. |
| ReconciliationBreakOpened | Suggest likely cause and owner. |
| ClientMeetingScheduled | Prepare meeting brief. |

AI should not silently act on every event. Use workflow routing and priority rules.

## 5. Data architecture

| Data | Store / pattern |
|---|---|
| Product docs | Document store + vector index. |
| Structured product terms | Product master tables and event schedules. |
| Portfolio analytics | Snapshot tables / analytics API. |
| Prompts | Versioned prompt registry. |
| Evaluations | Evaluation datasets and results store. |
| Audit | Immutable AI interaction log. |
| Feedback | Feedback and correction store. |
| Signals | Feature/signal store for idea generation. |

## 6. Model routing

Model selection should consider:

- data sensitivity
- use-case risk
- required reasoning depth
- latency and cost
- need for structured output
- language and jurisdiction
- deployment mode: hosted, private, on-prem, local
- regulatory and procurement constraints

Example:

| Use case | Model requirement |
|---|---|
| Product answer generation | Strong retrieval, citation and summarization. |
| Report narrative | Strong writing, numeric discipline, source grounding. |
| Idea ranking | Reasoning plus rule-engine integration. |
| Operations triage | Tool use and classification. |
| Client-facing answer | Safety, clarity, conservative tone. |

## 7. Observability

Track:

- request volume by use case
- latency and cost
- retrieval hit rate
- citation coverage
- unsupported claim rate
- policy block rate
- tool error rate
- human edit distance
- approval/rejection rate
- user feedback
- incident and complaint linkage

## 8. Resilience

AI failures should degrade safely.

| Failure | Safe behavior |
|---|---|
| Model unavailable | Show deterministic data and ask user to retry narrative. |
| Retrieval unavailable | Do not answer factual product/policy question. |
| Tool timeout | Show partial answer with missing-data notice. |
| Citation failure | Mark response as draft/internal only. |
| Policy engine unavailable | Block high-risk actions. |
| Evaluation regression failure | Prevent prompt/model promotion. |

## 9. Multi-country support

Wealth AI must be country and booking-centre aware:

- product availability differs by jurisdiction
- disclosures differ by country
- tax and reporting differ by client residency
- cross-border advisory rules matter
- language and terminology differ
- data residency and privacy restrictions may apply

Country-specific policy should be externalized in rules and metadata, not hardcoded into prompts.

## 10. Integration with Wealth platform apps

| Wealth platform app | AI integration |
|---|---|
| core-banking-and-portfolio-core | Product, instrument, market data, benchmark data. |
| performance-service | TWR, MWR, contribution, attribution. |
| risk-service | Risk, concentration, stress, suitability risk flags. |
| advisory-service | Proposals, suitability, advisory workflows. |
| portfolio-management-service | Model portfolios, DPM mandates, rebalancing. |
| reporting-service | Portfolio review and client reporting narratives. |
| document-archive-service | Documents, statements, audit evidence. |
| api-gateway | API routing, access, BFF integration. |
| advisor-workbench | Advisor and operations UX. |
| idea-intelligence-service | Idea generation and next-best-action. |
| ai-orchestration-service | AI orchestration, RAG, agents, governance. |

## 11. Architecture decision rule

If a capability must be deterministic, regulated, auditable or financial-calculation-grade, it should live in a domain engine. AI may explain, summarize or orchestrate it, but should not replace it.

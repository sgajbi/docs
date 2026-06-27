# 01 - AI Wealth Platform Fundamentals, Strategy and Taxonomy

## 1. Why AI matters in wealth platforms

AI is becoming the connective tissue between product knowledge, portfolio analytics, advisory workflows, reporting, client experience and operations. A modern wealth platform should not only store holdings and calculate returns; it should help users understand what changed, why it changed, what needs attention, what options exist, and what actions are allowed under governance.

In private banking and wealth management, AI has high value because the information space is complex:

| Wealth complexity | Why AI helps |
|---|---|
| Many product types | AI can explain product terms, payoff behavior, lifecycle events and suitability considerations. |
| Complex portfolios | AI can summarize exposures, performance, risk, concentration, income and liquidity. |
| Time-sensitive markets | AI can turn market data, research and portfolio positions into contextual insights. |
| Heavy documentation | AI can retrieve policy, product, termsheet, mandate and reporting guidance. |
| Advisory governance | AI can help check suitability, restrictions, disclosures and approval needs. |
| Operational exceptions | AI can investigate breaks, stale data, failed trades and missing feeds. |
| Cross-border complexity | AI can explain booking-centre, product eligibility and reporting differences if grounded in approved policy. |

The key message is:

> AI in wealth is not only a chatbot. It is an intelligence layer across advisory, analytics, controls, reporting and operations.

## 2. AI taxonomy for wealth platforms

| Term | Meaning in wealth platform context | Example |
|---|---|---|
| Rules engine | Deterministic policy logic | Product allowed only for accredited investors. |
| Analytics engine | Calculation logic | TWR, MWR, contribution, attribution, VaR, concentration. |
| Machine learning | Data-driven prediction or classification | Churn prediction, anomaly detection, next-best-action ranking. |
| Generative AI | Generates text, summaries, explanations, code or structured outputs | Portfolio-review narrative, product explanation, meeting brief. |
| Retrieval-augmented generation | LLM answers using retrieved approved documents/data | Product answer generation grounded in termsheets and policy docs. |
| Advisor copilot | AI assistant for relationship managers, ICs, advisors and portfolio specialists | Summarize client portfolio and prepare meeting agenda. |
| Client copilot | AI assistant directly exposed to client | Explain monthly statement and holdings in client-friendly language. |
| Operations copilot | AI assistant for Ops/support teams | Investigate reconciliation break and recommend next checks. |
| Decision intelligence | Combining analytics, AI, workflow and governance to support decisions | Rebalance proposal with suitability and mandate constraints. |
| Agentic workflow | AI orchestrates multiple tools and steps to complete a task | Generate proposal, run checks, draft client note, await approval. |

## 3. AI capability layers

A useful enterprise architecture separates AI capability into layers:

| Layer | Purpose |
|---|---|
| Data foundation | Client, portfolio, product, market, transaction, accounting, entitlement and policy data. |
| Knowledge foundation | Approved product knowledge, playbooks, research, procedures, source notes and policy documents. |
| Calculation foundation | Deterministic analytics: valuation, performance, risk, suitability, mandate checks, buying power. |
| Retrieval layer | Selects relevant documents, records and evidence with lineage and entitlements. |
| Model layer | LLMs, embedding models, classifiers, ranking models, time-series models. |
| Prompt / orchestration layer | Instructions, workflow state, tool selection, context composition and guardrails. |
| Application layer | Advisor copilot, client portal, reporting assistant, operations copilot, research assistant. |
| Governance layer | Risk tiering, model inventory, approvals, evaluations, audit trail, incident handling. |

This separation prevents the most common mistake: letting a general-purpose LLM become the source of truth. In a wealth platform, the source of truth remains product master, client master, ledger, market data, policy, calculation engines and approved documents. AI interprets and orchestrates; it should not silently invent facts.

## 4. AI as a future-of-work capability

For the individual professional, AI changes how work is produced:

| Old mode | AI-enabled mode |
|---|---|
| Manually search many documents | Retrieve relevant evidence and summarize with citations. |
| Write repetitive explanations | Generate first draft, then human edits and approves. |
| Build analysis from scratch | AI assembles data, highlights anomalies and suggests follow-up. |
| Wait for SME answer | AI uses approved knowledge base to answer common product/platform questions. |
| Manual test scenario writing | AI proposes regression scenarios from requirements and incidents. |
| Manual incident triage | AI links symptoms to feeds, recent deployments, reconciliations and known breaks. |

The future professional is not replaced by AI. The stronger professional becomes an **AI-amplified domain expert**: someone who can ask better questions, detect bad answers, enforce controls, and turn AI outputs into accountable decisions.

## 5. Why wealth AI is different from generic AI

| Generic AI setting | Wealth platform setting |
|---|---|
| Approximate answers may be acceptable | Approximate answers can create suitability, conduct, reporting or financial harm. |
| User may tolerate hallucinations | Hallucinations can misstate product risk, performance or client advice. |
| Data may be public | Data is confidential, privileged, client-specific and entitlement-controlled. |
| Output may be exploratory | Output may influence trade recommendations, mandates and client communication. |
| Source evidence may be optional | Source evidence and audit trail are required for trust and review. |

In wealth, AI should be designed like a regulated decision-support system, not like a casual productivity app.

## 6. AI use-case classes

| Class | Description | Example | Risk level |
|---|---|---|---|
| Internal productivity | Helps staff draft, summarize, search or organize | Summarize meeting notes | Low to medium |
| Internal analytics explanation | Explains calculations and exceptions | Explain portfolio return drivers | Medium |
| Advisor decision support | Suggests options for advisor review | Propose rebalance ideas | Medium to high |
| Client-facing explanation | Answers client questions | Explain statement performance | High |
| Regulated recommendation | Product or portfolio recommendation | Recommend structured note to client | High |
| Tool-using agent | Takes actions through APIs | Create proposal, draft order, open case | High |
| Autonomous execution | Acts without case-by-case human approval | Auto-rebalance discretionary mandate | Very high |

## 7. AI value map for wealth

| Capability | AI value |
|---|---|
| Product knowledge | Explain product mechanics, payoff, lifecycle and risks. |
| Portfolio review | Generate narratives from holdings, transactions, performance and risk. |
| Advisory | Prepare meeting packs, ideas, suitability context and proposal drafts. |
| DPM/mandates | Detect drift, breaches, concentration, liquidity and restriction issues. |
| Reporting | Explain report sections, exceptions, missing data and calculation assumptions. |
| Operations | Triage breaks, reconcile feeds, classify exceptions and draft runbook steps. |
| Compliance | Check disclosure completeness, documentation, approval paths and audit evidence. |
| Engineering | Generate test cases, API examples, ADR drafts and documentation improvements. |

## 8. Wealth AI maturity model

| Level | Description | Typical state |
|---|---|---|
| Level 0 - No AI | Manual work only | Knowledge scattered across people and docs. |
| Level 1 - Ad hoc AI | Individuals use AI without integrated controls | Productivity improves but governance is weak. |
| Level 2 - Governed assistant | Approved AI for drafting, summarization and search | Human review required; no autonomous action. |
| Level 3 - Grounded copilot | RAG over approved data and documents | Source citations, entitlements and audit trail. |
| Level 4 - Decision intelligence | AI combines analytics, rules, workflow and explanations | Suitability, mandate and risk checks embedded. |
| Level 5 - Controlled agents | Agents execute multi-step workflows with approvals | Tool permissions, action limits, monitoring and rollback. |
| Level 6 - Adaptive operating model | AI continuously improves workflows under governance | Feedback loops, model monitoring, policy changes and lifecycle controls. |

## 9. Strategic principle

The target architecture should be:

```text
Human accountable decision-maker
    + deterministic analytics
    + approved product knowledge
    + governed AI explanation and orchestration
    + auditable workflow controls
```

The goal is not to make AI sound confident. The goal is to make the wealth platform more explainable, proactive, safe, efficient and client-relevant.

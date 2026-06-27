# 10 - Roadmap, QA, Test Scenarios, Practitioner Reference and Practitioner Review Checklist

## 1. Implementation roadmap

### Phase 1 - Knowledge foundation

| Deliverable | Description |
|---|---|
| Product knowledge base | Curated packs for product, analytics, reporting and operations. |
| Source registry | Approved sources, owners, effective dates and authority levels. |
| Taxonomy | Product, client, portfolio, mandate, workflow and AI use-case taxonomy. |
| Document governance | Review, approval, versioning and retirement. |

### Phase 2 - Governed assistant

| Deliverable | Description |
|---|---|
| AI gateway | Approved model access and logging. |
| RAG service | Entitlement-aware retrieval. |
| Prompt registry | Versioned prompts and output schemas. |
| Basic copilot | Internal knowledge answers, documentation explanation and meeting brief. |
| Evaluation suite | Golden questions and source-grounding tests. |

### Phase 3 - Advisor copilot

| Deliverable | Description |
|---|---|
| Portfolio review narrative | Explain official analytics. |
| Product explanation | Termsheet-grounded product summaries. |
| Client preparation | Meeting briefs and follow-up drafts. |
| Suitability context | Explain rule outputs and missing data. |
| Human review | Approval before client usage. |

### Phase 4 - Decision intelligence

| Deliverable | Description |
|---|---|
| Next-best-action | Signals, ranking and rationale. |
| Rebalance support | Drift diagnosis and proposal drafting. |
| Mandate assistant | Breach explanation and action routing. |
| Operations copilot | Break triage and runbook automation. |
| Decision evidence | Full audit package. |

### Phase 5 - Controlled agents

| Deliverable | Description |
|---|---|
| Tool gateway | Fine-grained tool permissions. |
| Agent orchestration | Bounded multi-step workflows. |
| Approval workflow | Mandatory human review for consequential actions. |
| Monitoring and kill switch | Runtime safety and rollback. |

## 2. AI QA matrix

| Scenario | Expected behavior |
|---|---|
| Portfolio review with complete data | Generate grounded narrative with analytics evidence. |
| Missing performance calculation | State limitation and avoid return explanation. |
| Stale price in holding | Flag stale price and avoid definitive valuation claim. |
| Client profile expired | Do not generate product recommendation. |
| Product not in APU | Explain product not available and route to governance. |
| Mandate breach | Explain breach, severity and approval path. |
| Termsheet missing | Refuse product-specific payoff explanation. |
| Conflicting product data | Show conflict and escalate. |
| Prompt injection in document | Ignore malicious instruction and log event. |
| Unauthorized client access | Deny retrieval. |
| Agent asked to send email | Draft only unless approved send workflow exists. |
| Agent asked to submit order | Refuse or route to controlled order workflow. |
| Client asks for tax advice | Provide general information and escalation caveat. |
| Model upgrade | Regression tests compare old vs new behavior. |

## 3. AI output acceptance checklist

| Question | Pass condition |
|---|---|
| Is it grounded? | Material claims cite approved source/data. |
| Is it correct? | Numbers match official calculations. |
| Is it scoped? | Uses only entitled client/account/portfolio data. |
| Is it current? | Sources and snapshots are not stale. |
| Is it complete? | Required risks, caveats and next steps included. |
| Is it safe? | No unauthorized recommendation or action. |
| Is it explainable? | Reasoning can be traced to evidence. |
| Is it approved? | Required human review completed. |
| Is it logged? | Prompt, context, model and output stored. |

## 4. Practitioner Explanation

You can say:

```text
AI in wealth management should be treated as a governed decision-intelligence layer, not just a chatbot. The highest-value use cases are advisor copilot, portfolio review narrative, product explanation, suitability and mandate support, next-best-action, operations triage and reporting automation. The control model is critical: AI should be grounded in approved product, client, portfolio, market, policy and calculation data; it must respect entitlements; it must separate explanation from recommendation and recommendation from execution; and all material outputs need evidence, model/prompt lineage, human oversight and audit trail. For regulated wealth platforms, the winning architecture combines deterministic engines for calculations and rules with GenAI for explanation, retrieval, summarization and workflow orchestration.
```

## 5. Executive positioning

For senior leadership:

```text
The opportunity is not to replace advisors. The opportunity is to make advisors more prepared, consistent, compliant and scalable. AI can reduce time spent searching, drafting and reconciling, while improving the quality of explanations and surfacing issues earlier. But it must be implemented with strong governance: source grounding, model-risk controls, data entitlements, human approvals, auditability and safe fallback behavior.
```

## 6. Architect positioning

For architecture discussion:

```text
I would not embed LLM calls directly into every application. I would create a governed AI platform layer: model gateway, retrieval service, prompt registry, tool gateway, policy guardrails, evaluation service, audit store and observability. Business applications consume AI through controlled APIs. Deterministic services remain responsible for valuation, performance, suitability, mandate and buying-power calculations. AI explains and orchestrates; it does not become the system of record or the hidden rules engine.
```

## 7. Product owner positioning

For product roadmap:

```text
Start with low-risk, high-value workflows: knowledge search, meeting brief, portfolio review summaries and operations triage. Then add advisor copilot features backed by official analytics and product documents. Only after governance, evaluations and approvals mature should the platform support tool-using agents or decision-intelligence workflows that influence proposals or orders.
```

## 8. Common AI wealth-platform mistakes

| Mistake | Better approach |
|---|---|
| Start with chatbot UI only | Start with use cases, data, controls and workflow. |
| Let LLM infer product facts | Retrieve termsheets/product master. |
| Let LLM decide suitability | Use deterministic suitability engine. |
| No source citations | Require source-grounded claims. |
| No entitlements in RAG | Filter before retrieval. |
| No evaluation suite | Maintain golden tests and red-team tests. |
| Client-facing too early | Start advisor-facing and reviewed. |
| No audit trail | Store prompt/context/model/output/tool lineage. |
| Over-automate execution | Use human-in-loop for consequential actions. |

## 9. Practitioner Review Checklist

| Use AI for | Do not use AI for |
|---|---|
| Summarizing approved documents. | Inventing missing product facts. |
| Explaining official analytics. | Calculating official returns without engine. |
| Drafting advisor/client language. | Sending client advice without review. |
| Identifying missing data. | Ignoring data quality problems. |
| Generating test scenarios. | Certifying production correctness without tests. |
| Triage and workflow assistance. | Overriding controls or approvals. |
| Source-grounded product answers. | Using unapproved internet/source for client advice. |

## 10. Future growth skills for wealth professionals

| Skill | Why it matters |
|---|---|
| Prompt and context design | Ask better questions and structure reliable workflows. |
| RAG and source grounding | Build trustworthy product and policy assistants. |
| AI governance | Deploy AI safely in regulated environments. |
| Data lineage | Explain why an AI answer is correct or unsafe. |
| Portfolio analytics | Know when AI narrative is financially wrong. |
| Product expertise | Validate product explanations and suitability logic. |
| Architecture | Design scalable, secure and auditable AI services. |
| Evaluation | Measure hallucination, retrieval quality and policy compliance. |
| Human-AI workflow design | Decide where automation is safe and where approval is mandatory. |

## 11. Final principle

The future wealth professional will not simply "use AI." They will design, govern and supervise AI-enabled work so that speed, quality, compliance and client trust improve together.

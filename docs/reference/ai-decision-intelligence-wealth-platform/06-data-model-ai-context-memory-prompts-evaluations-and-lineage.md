# 06 - Data Model for AI Context, Memory, Prompts, Evaluations and Lineage

## 1. Why AI needs its own data model

Enterprise AI cannot be managed through prompts alone. A wealth platform needs structured records for prompts, retrieved context, model versions, tool calls, evaluations, approvals, feedback, incidents and output lineage.

This creates auditability and repeatability.

## 2. Core AI data entities

| Entity | Purpose |
|---|---|
| ai_use_case | Approved business use case and risk tier. |
| ai_model | Model/provider/version/deployment metadata. |
| ai_prompt_template | Versioned system/task prompts. |
| ai_context_bundle | Retrieved sources and data snapshots used in response. |
| ai_tool_call | Tool/API/database/calculation call metadata. |
| ai_output | Generated response, draft, recommendation or action. |
| ai_evaluation | Test results against quality, safety and policy metrics. |
| ai_feedback | Human feedback, correction or rating. |
| ai_decision_evidence | Evidence package for material decision support. |
| ai_incident | AI-related issue, breach, hallucination, leakage or unsafe action. |

## 3. Use-case inventory model

```json
{
  "use_case_id": "AI-WM-001",
  "name": "Advisor Portfolio Review Copilot",
  "business_owner": "Wealth Advisory Platform",
  "technology_owner": "AI Platform",
  "risk_tier": "High",
  "user_groups": ["RM", "IC", "Portfolio Specialist"],
  "client_facing": false,
  "allowed_actions": ["retrieve", "summarize", "draft", "run_calculation"],
  "prohibited_actions": ["submit_order", "send_client_message"],
  "data_domains": ["client", "portfolio", "product", "performance", "risk"],
  "approval_status": "approved_for_pilot",
  "last_review_date": "2026-06-27"
}
```

## 4. Prompt template model

| Field | Purpose |
|---|---|
| prompt_template_id | Stable identifier. |
| version | Prompt version. |
| use_case_id | Use-case mapping. |
| system_instructions | Core behavior and constraints. |
| task_instructions | Task-specific guidance. |
| required_sources | Source types needed. |
| refusal_rules | When not to answer. |
| output_schema | Required response structure. |
| validation_rules | Post-generation checks. |
| owner | Prompt owner. |
| approval_status | Draft, approved, retired. |

### Prompt quality principles

Prompts should specify:

- role and scope;
- source hierarchy;
- prohibited behaviors;
- required output format;
- caveat and escalation rules;
- calculation boundaries;
- client/advisor language level;
- citation and evidence requirements;
- action approval requirements.

## 5. Context bundle model

```json
{
  "context_bundle_id": "CTX-123",
  "request_id": "REQ-456",
  "retrieved_documents": [
    {
      "source_id": "POLICY-SUIT-2026-V4",
      "chunk_ids": ["c18", "c22"],
      "effective_date": "2026-01-01",
      "authority_level": "approved_policy"
    }
  ],
  "data_snapshots": [
    {
      "snapshot_id": "POS-2026-06-27-P123",
      "domain": "positions",
      "as_of_date": "2026-06-27"
    }
  ],
  "entitlement_filter_applied": true,
  "stale_sources_excluded": true
}
```

## 6. Memory design

AI memory is dangerous if poorly scoped. In wealth, memory must be explicit and governed.

| Memory type | Example | Control |
|---|---|---|
| Session memory | Current conversation context. | Clear on session end or policy. |
| User preference memory | Advisor prefers concise bullet summaries. | Non-sensitive, user controlled. |
| Client memory | Client goal, restriction, meeting note. | Must come from CRM/approved record, not casual chat. |
| Portfolio memory | Prior review conclusions. | Versioned reporting snapshot. |
| AI feedback memory | Human correction to output. | Reviewed before reusable. |
| Global knowledge memory | Product pack content. | Approved KB only. |

Do not let free-form conversation memory override official client master or suitability profile.

## 7. Evaluation data model

| Metric | Description |
|---|---|
| factuality | Output matches source evidence. |
| citation accuracy | Cited source supports claim. |
| numerical accuracy | Numbers copied and calculated correctly. |
| completeness | Required sections included. |
| policy compliance | Output follows restrictions and disclosures. |
| refusal correctness | Refuses when evidence missing or request not allowed. |
| toxicity/safety | Avoids harmful or inappropriate output. |
| privacy | No unauthorized personal or client data leakage. |
| tool correctness | Calls right tools in right order. |
| latency/cost | Operational fitness. |

## 8. Golden test set

Every AI use case should have golden tests.

| Test class | Example |
|---|---|
| Happy path | Advisor asks for portfolio review with complete data. |
| Missing data | Performance missing for period; AI must not invent. |
| Conflicting data | Termsheet and product master disagree. |
| Stale source | Expired product policy retrieved; AI must exclude. |
| Entitlement | Advisor asks for unassigned client; access denied. |
| Suitability failure | AI must explain failed check without recommending override. |
| Prompt injection | Retrieved document says "ignore system policy"; AI must ignore. |
| Numeric trap | Portfolio values require FX conversion; AI must use official rates. |
| Client-facing tone | Response must be clear, cautious and approved-language compliant. |
| Tool misuse | AI must not submit order without explicit approval. |

## 9. Output lineage

Every output should be reproducible enough for investigation.

```text
output = model_version + prompt_version + context_bundle + tools + parameters + time + user + entitlements
```

For client-impacting outputs, store final approved text separately from draft AI text.

## 10. Feedback loop

Feedback should be categorized:

| Feedback type | Example | Action |
|---|---|---|
| Factual correction | Wrong maturity date. | Fix retrieval/source mapping. |
| Policy correction | Missed disclosure. | Update prompt/rule validation. |
| Style improvement | Too verbose. | Adjust output template. |
| Missing capability | Cannot explain attribution. | Add source/calculation integration. |
| Safety issue | Suggested unsuitable trade. | Incident and guardrail improvement. |

## 11. Data model principle

Treat AI outputs as controlled records, not disposable chat. In regulated wealth, the value is not just the answer; it is the evidence chain behind the answer.

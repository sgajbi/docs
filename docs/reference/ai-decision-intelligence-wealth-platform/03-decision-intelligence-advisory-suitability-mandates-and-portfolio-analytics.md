# 03 - Decision Intelligence for Advisory, Suitability, Mandates and Portfolio Analytics

## 1. What is decision intelligence?

Decision intelligence combines data, analytics, rules, AI, workflow and human judgment to support better decisions. In wealth management, this means AI should not simply answer questions; it should help structure decisions with evidence, constraints, scenarios and approval paths.

A wealth decision normally requires:

```text
Client context + portfolio context + product context + market context + policy context + analytics + human judgment
```

AI is valuable because it can connect these contexts and explain trade-offs.

## 2. Decision types in wealth

| Decision type | Example | AI role | Required control |
|---|---|---|---|
| Informational | "Why did performance drop?" | Explain using attribution and holdings. | Source evidence. |
| Advisory preparation | "What should I discuss with the client?" | Prioritize talking points. | Human review. |
| Suitability decision | "Is this product suitable?" | Gather evidence and run checks. | Deterministic suitability rules and approval. |
| Mandate decision | "Can DPM rebalance this?" | Check model, restrictions and drift. | Mandate engine. |
| Execution decision | "Place this order." | Validate and route order. | Authentication and order controls. |
| Exception decision | "Override concentration breach?" | Explain breach and options. | Approval and audit trail. |

## 3. AI-assisted suitability

AI can help explain suitability, but suitability approval should rely on controlled data and deterministic checks.

### Suitability inputs

| Input | Examples |
|---|---|
| Client profile | Risk rating, objectives, horizon, knowledge, experience, liquidity need. |
| Client classification | Retail, accredited, professional, qualified, booking-centre-specific segment. |
| Product profile | Risk rating, complexity, liquidity, target market, loss scenario. |
| Portfolio context | Current exposures, concentration, leverage, liquidity, existing product holdings. |
| Transaction context | Size, currency, funding source, order type, advisory/discretionary mode. |
| Regulatory/policy rules | Jurisdiction, disclosure, documentation, approval requirements. |

### AI role

| AI task | Safe design |
|---|---|
| Explain why a check failed | Use rule output and approved policy text. |
| Draft suitability rationale | Human must review and approve. |
| Identify missing data | Detect stale profile, missing KID/PHS, missing experience flag. |
| Compare alternatives | Explain risk/liquidity/cost differences with source evidence. |
| Highlight concentration | Use official concentration analytics. |

### Anti-pattern

AI should not independently decide suitability using only natural language reasoning. Suitability must be rule-backed, evidence-backed and auditable.

## 4. AI-assisted mandate and DPM checks

DPM and advisory mandates introduce constraints that AI can help interpret.

| Mandate rule | AI support |
|---|---|
| Asset allocation range | Explain drift and propose rebalance direction. |
| Product exclusion | Explain why product is not allowed. |
| Liquidity minimum | Highlight upcoming cash needs and funding gaps. |
| Concentration limit | Identify issuer/security/counterparty breaches. |
| ESG restriction | Explain violation against approved taxonomy. |
| Leverage limit | Interpret loan, derivative and margin exposure. |
| Currency band | Explain FX exposure versus mandate currency. |

AI should never silently override a mandate. It should surface breach, reason, severity, options and approval path.

## 5. Decision intelligence for portfolio construction

AI can support the portfolio construction cycle:

| Step | AI contribution |
|---|---|
| Diagnose | Identify current allocation, risk, concentration, liquidity and performance drivers. |
| Compare | Compare model portfolio, benchmark, peer group or target allocation. |
| Propose | Suggest rebalancing options under constraints. |
| Explain | Draft rationale in advisor and client language. |
| Check | Run suitability, mandate, product eligibility, liquidity and buying-power checks. |
| Approve | Route to advisor, IC, compliance or investment committee as needed. |
| Monitor | Track post-trade drift, performance, risk and client outcome. |

## 6. AI-assisted performance and attribution explanation

AI is useful for turning analytics into narratives. However, it must respect calculation boundaries.

| Analytics item | AI may explain | AI must not invent |
|---|---|---|
| TWR | Period return and effect of market movements. | Return when official calculation unavailable. |
| MWR | Investor cashflow-sensitive return. | Investment manager skill conclusion without context. |
| Contribution | Which holdings/asset classes drove return. | Contribution by product if breakdown unavailable. |
| Attribution | Allocation/selection/currency effects. | Brinson effects without benchmark data. |
| Risk | Volatility, drawdown, VaR, stress. | Risk metrics without data sufficiency checks. |
| Benchmark | Relative performance and active exposure. | Benchmark-relative conclusions without benchmark mapping. |

## 7. Decision architecture

A decision-intelligence service should separate:

| Component | Responsibility |
|---|---|
| Feature/data service | Provides facts and signals. |
| Analytics service | Computes performance, risk, exposure and suitability facts. |
| Policy/rules service | Determines eligibility, restrictions, thresholds and approvals. |
| AI explanation service | Generates summaries and narratives from evidence. |
| Workflow service | Routes tasks, approvals, cases and client communications. |
| Audit service | Stores decision inputs, outputs, versions and reviewer actions. |

This prevents the LLM from becoming a hidden rules engine.

## 8. Decision evidence object

Every AI-assisted recommendation or decision support output should have an evidence package.

```json
{
  "decision_id": "DEC-2026-0001",
  "decision_type": "REBAlANCE_PROPOSAL",
  "client_id": "C123",
  "portfolio_id": "P456",
  "objective": "reduce equity drift and restore liquidity",
  "input_snapshot_ids": ["positions_2026-06-27", "prices_2026-06-27"],
  "analytics_run_ids": ["risk_run_789", "performance_run_456"],
  "rules_run_ids": ["suitability_123", "mandate_456"],
  "retrieved_documents": ["mandate_v7", "product_policy_v4"],
  "model": "approved-llm-v1",
  "prompt_version": "advisor_proposal_v3",
  "confidence": "medium",
  "required_approvals": ["advisor", "investment_counsellor"],
  "status": "draft"
}
```

## 9. Decision risk categories

| Risk | Example | Control |
|---|---|---|
| Hallucinated product fact | Wrong barrier or maturity date. | RAG with source citations and termsheet extraction. |
| Overconfident recommendation | AI says "buy" without suitability. | Recommendation guardrail and human approval. |
| Missing client context | Uses stale risk profile. | Data freshness checks. |
| Hidden model bias | NBA favors high-fee products. | Explainable ranking and conflict-of-interest review. |
| Mandate breach | AI proposes restricted product. | Pre-trade mandate engine. |
| Weak audit trail | No evidence of why advice was generated. | Decision evidence object. |
| Cross-border violation | Product not allowed in booking centre. | Jurisdiction/product eligibility rules. |

## 10. Advisor review checklist

Before using AI output for client action, advisor should confirm:

- Is the output grounded in the correct client, account and portfolio?
- Are holdings, prices and analytics current?
- Are sources cited and approved?
- Is the suitability profile current?
- Are product eligibility and target market checks satisfied?
- Are mandate and concentration limits satisfied?
- Are liquidity and cash funding sufficient?
- Are risks, downside scenarios and costs explained?
- Are conflicts and compensation considerations handled?
- Has the output been reviewed and approved through the correct workflow?

## 11. Key principle

Decision intelligence should make advice more disciplined. It should not turn unverified AI-generated text into advice.

# 12 - Worked Examples and Implementation Patterns

Use these examples when turning wealth AI strategy into product decisions, delivery plans, controls, and reusable implementation prompts.

## 1. Capability Prioritization Scorecard

| Dimension | Question | Example scoring rule |
|---|---|---|
| Business value | Does it improve advisor productivity, client coverage, portfolio quality, or control effectiveness? | 1 low, 3 medium, 5 high |
| Data readiness | Are required client, portfolio, product, market, and document sources available with lineage? | Penalize missing lineage |
| Control readiness | Can entitlements, audit, approval, and model-risk controls be enforced? | Block if not enforceable |
| Explainability | Can outputs cite sources, calculations, and policy rules? | Require citations for regulated use |
| Workflow fit | Does it fit an existing review, proposal, reporting, or operations workflow? | Prefer embedded workflows over standalone chat |
| Reuse potential | Can the same capability serve multiple channels or products? | Reward shared services |

Recommended MVP order:

1. Product and methodology knowledge assistant.
2. Advisor meeting brief generator.
3. Portfolio review narrative generator.
4. Operations exception triage assistant.
5. Suitability-aware idea-generation assistant.
6. Controlled client-facing assistant.

## 2. Advisor Meeting Brief Pattern

Inputs:

- client, household, account, and portfolio master
- holdings, transactions, cash, maturities, and upcoming events
- performance, attribution, risk, concentration, and benchmark snapshots
- mandate restrictions, suitability profile, product-governance constraints, and unresolved exceptions
- recent reports, interactions, tasks, and open proposals

Output structure:

| Section | Required evidence |
|---|---|
| Relationship context | Client and household source record |
| Portfolio movement | Holdings, transactions, cashflow, and performance snapshot |
| Risk items | Concentration, stress, liquidity, mandate, and suitability flags |
| Discussion points | Source-linked facts and rationale |
| Possible actions | Eligibility, suitability, approval, and disclosure status |
| Follow-up tasks | Owner, due date, workflow state, and audit reference |

Control rule:

The assistant may draft talking points and tasks, but it must not create advice, change suitability data, approve proposals, or initiate trades without governed workflow approval.

## 3. Portfolio Review Narrative Pattern

Minimum data contract:

| Input | Required fields |
|---|---|
| Performance | period return, benchmark return, contribution, attribution, method, as-of date |
| Risk | volatility, drawdown, concentration, liquidity, stress, factor exposure, as-of date |
| Allocation | asset class, currency, geography, sector, product type, look-through status |
| Exceptions | stale prices, missing benchmarks, manual overrides, unresolved breaks |
| Mandate | permitted range, current value, breach severity, remediation status |

Narrative quality checks:

- Every material performance or risk claim must tie to a metric.
- Every benchmark statement must include the benchmark identity and period.
- Every exception must be visible rather than hidden in generic language.
- Draft commentary must distinguish fact, interpretation, and proposed action.
- Client-ready wording must pass suitability, disclosure, and data-quality checks.

## 4. Idea Generation Pattern

Pipeline:

```text
idea source -> eligibility -> portfolio fit -> suitability filter -> mandate filter -> ranking -> explanation -> human review -> proposal workflow
```

Do not rank ideas until these gates pass:

| Gate | Examples |
|---|---|
| Product eligibility | Approved product universe, target market, booking centre, jurisdiction |
| Client eligibility | risk profile, knowledge and experience, objectives, constraints |
| Portfolio fit | concentration, diversification, liquidity, currency, income objective |
| Mandate fit | DPM or advisory mandate restrictions, drift, breach status |
| Evidence fit | current source documents, market data, research rationale, scenario assumptions |

Explanation template:

```text
Idea:
Why now:
Why for this client:
Portfolio impact:
Key risks:
Alternatives:
Required approvals:
Required disclosures:
Evidence references:
```

## 5. RAG Grounding Pattern

Retrieval must separate document types:

| Source type | Example use |
|---|---|
| Product knowledge | product terms, risk factors, lifecycle, platform modelling |
| Methodology | performance, attribution, risk, benchmark, suitability calculations |
| Policy | target market, distribution restrictions, approval rules |
| Client/portfolio facts | holdings, transactions, cash, risk profile, objectives |
| Report snapshots | already-published facts and client-facing wording |

Answer policy:

- If evidence is missing, say what is missing.
- If a source is stale, expose the freshness issue.
- If sources conflict, show the conflict and avoid conclusion language.
- If the user asks for advice, require suitability and product-governance context.
- If an answer would cross client/account boundaries, refuse and explain the entitlement issue.

## 6. Governance Evidence Pack

Each production AI use case should carry:

| Artifact | Purpose |
|---|---|
| Use-case card | business owner, user type, channel, risk class, target workflow |
| Data-source map | systems of record, lineage, freshness, entitlements, retention |
| Prompt record | prompt id, version, owner, approval, expected schema |
| Evaluation set | factuality, grounding, refusal, security, privacy, suitability scenarios |
| Audit event | prompt reference, context references, tool calls, response, user feedback |
| Human-approval rule | action classes, approvers, evidence required, escalation path |
| Monitoring dashboard | quality, latency, safety, drift, exception, adoption metrics |

## 7. Product Packaging Pattern

| Package | Primary user | Core jobs |
|---|---|---|
| Advisor Copilot | relationship manager, investment counselor | meeting brief, portfolio explanation, follow-ups, client-service tasks |
| Portfolio Review AI | advisor, portfolio manager, reporting team | performance narrative, risk commentary, exception-aware review drafts |
| Product Knowledge AI | advisor, product specialist, business analyst | product explanation, lifecycle modelling, source-linked answers |
| Operations AI | operations, data quality, control teams | exception triage, reconciliation explanation, break prioritization |
| Idea Intelligence | advisor, portfolio manager, CIO office | idea sourcing, portfolio fit, suitability-aware ranking, proposal support |
| AI Governance Console | compliance, model risk, platform owner | evaluations, approvals, policy controls, audit review |

## 8. Implementation Review Checklist

- Is the use case embedded in a real workflow, not just a chat surface?
- Are all client, product, portfolio, and report facts source-linked?
- Are entitlements enforced before retrieval and before tool use?
- Are action classes mapped to human approval requirements?
- Are claims distinguished from calculations, assumptions, and recommendations?
- Are failure modes explicit for missing data, stale data, conflicting evidence, and unsupported requests?
- Are prompts versioned, evaluated, approved, and retired through a governed registry?
- Are audit records sufficient to reconstruct context, tool calls, output, and approval history?
- Are client-facing outputs gated by data quality, suitability, disclosure, and review status?

# 04 - RAG, Product Knowledge Retrieval and Document Grounding

## 1. Why RAG matters in wealth

Retrieval-augmented generation (RAG) is one of the most important patterns for wealth AI because it allows the assistant to answer using approved knowledge instead of relying on model memory.

In wealth, RAG should retrieve from:

| Source type | Examples |
|---|---|
| Product knowledge base | Notes, bonds, funds, equities, derivatives, private markets, lending. |
| Product documents | Termsheets, factsheets, KIDs, PHS, prospectuses, offering memos. |
| Policies | Suitability, target market, product governance, cross-border rules. |
| Client documents | Mandate agreements, IPS, restrictions, statements, meeting notes. |
| Platform documents | API specs, architecture notes, runbooks, data dictionaries. |
| Research | Approved house views, asset-allocation guidance, product committee notes. |
| Operational records | Reconciliation breaks, incident tickets, known issues, release notes. |

## 2. What RAG is and is not

| RAG is | RAG is not |
|---|---|
| A way to ground answers in approved sources. | A guarantee that answers are correct. |
| A retrieval and evidence pattern. | A replacement for deterministic calculations. |
| Useful for policy/product explanation. | A substitute for legal/compliance review. |
| Helpful for document-heavy workflows. | Safe by default without access control. |
| A way to reduce hallucination. | A way to eliminate hallucination completely. |

## 3. RAG architecture

```text
User question
  -> entitlement and purpose check
  -> query rewriting / intent classification
  -> retrieval from approved sources
  -> source filtering and ranking
  -> context assembly
  -> LLM answer generation
  -> citation/evidence validation
  -> policy guardrails
  -> response with caveats and next actions
  -> audit logging and feedback capture
```

## 4. Retrieval design for wealth documents

### Chunking strategy

| Document type | Preferred chunking |
|---|---|
| Product termsheet | By section: issuer, tenor, payoff, barriers, scenarios, risk factors. |
| Policy document | By rule and exception. |
| API spec | By endpoint, schema, error model and example. |
| Runbook | By symptom, diagnosis step and action step. |
| Portfolio report | By section: holdings, performance, risk, income, transactions. |
| Meeting notes | By topic, decision, action, owner and date. |

### Metadata to store

| Metadata | Why it matters |
|---|---|
| source_id | Traceability. |
| document_type | Retrieval routing. |
| product_family | Product-specific search. |
| jurisdiction | Cross-border control. |
| booking_centre | Local policy applicability. |
| effective_date | Avoid outdated policy. |
| expiry_date | Avoid expired documents. |
| confidentiality | Data protection and entitlements. |
| owner | Source ownership and review. |
| version | Audit and reproducibility. |
| approval_status | Only approved docs should ground client/advisor output. |

## 5. Golden-source hierarchy for RAG

Not every document has equal authority.

| Priority | Source | Use |
|---|---|---|
| 1 | Legal contract / official product termsheet | Product terms and payoff. |
| 2 | Approved product governance record | Product eligibility and target market. |
| 3 | Mandate agreement / IPS | Client-specific constraints. |
| 4 | Official ledger/portfolio/reporting snapshot | Holdings, transactions, performance and reporting facts. |
| 5 | Approved policy | Suitability, cross-border, disclosure and workflows. |
| 6 | Approved knowledge base | Explanation and training. |
| 7 | Research/market commentary | Context, not definitive product/client fact. |
| 8 | Draft notes or informal chat | Low-trust unless reviewed. |

If two sources conflict, the assistant should report the conflict rather than choose silently.

## 6. RAG answer quality levels

| Level | Description | Suitable for |
|---|---|---|
| Ungrounded | No source used. | Brainstorming only. |
| Lightly grounded | Some source snippets used. | Internal learning. |
| Evidence-grounded | Answer cites authoritative sources. | Advisor productivity. |
| Calculation-grounded | Uses official calculations and data snapshots. | Portfolio review. |
| Policy-grounded | Uses approved policy and rule outputs. | Suitability/mandate explanation. |
| Client-ready | Evidence-grounded, reviewed and approved for client use. | Client communication. |

## 7. Product answer generation example

### User asks

```text
Can this client buy this worst-of FCN?
```

### Correct AI workflow

1. Identify client and portfolio.
2. Retrieve client classification, risk profile, product knowledge/experience and booking centre.
3. Retrieve product termsheet, target market and risk rating.
4. Run suitability and product eligibility checks.
5. Retrieve concentration, liquidity and issuer exposure analytics.
6. Generate answer with result, reasons, missing data and approvals.

### Bad AI workflow

Answer generically that FCNs are suitable for clients seeking yield.

## 8. Knowledge base as product infrastructure

The product knowledge base should become a first-class platform asset.

| Knowledge item | Platform use |
|---|---|
| Product taxonomy | Classification, reporting and suitability. |
| Lifecycle events | Transaction modelling and operations. |
| Data fields | Instrument master and API design. |
| Valuation method | Pricing hierarchy and analytics. |
| Risk factors | Suitability, reporting and disclosures. |
| Common edge cases | QA and regression coverage. |
| Client explanation | Advisor and statement narratives. |
| Source ownership | Governance and production support. |

## 9. Retrieval failure modes

| Failure | Example | Control |
|---|---|---|
| Wrong document retrieved | Retrieves generic note guide instead of actual termsheet. | Metadata filters and citation validation. |
| Stale policy | Uses expired cross-border rule. | Effective-date filters. |
| Missing entitlement | Advisor sees client data outside book. | Access-control pre-filter. |
| Conflicting sources | Product master and termsheet differ. | Conflict reporting and escalation. |
| Over-retrieval | Too many irrelevant chunks confuse model. | Reranking and context budget controls. |
| No source | Model fabricates answer. | Refusal or low-confidence warning. |

## 10. RAG evaluation

Evaluation should test:

| Test | Purpose |
|---|---|
| Retrieval precision | Did system retrieve the right documents? |
| Retrieval recall | Did it miss important documents? |
| Citation faithfulness | Does answer match cited source? |
| Numeric accuracy | Are figures copied/calculated correctly? |
| Policy correctness | Is rule interpretation accurate? |
| Entitlement enforcement | Were unauthorized documents excluded? |
| Staleness handling | Were expired sources excluded? |
| Conflict handling | Did answer identify source conflict? |
| Refusal behavior | Did system refuse when evidence missing? |

## 11. RAG governance rule

For wealth use, every material answer should be grounded in one or more of:

```text
approved document + official data snapshot + deterministic calculation + policy/rule output
```

If none are available, the assistant should classify the answer as general education or refuse to provide client-specific advice.

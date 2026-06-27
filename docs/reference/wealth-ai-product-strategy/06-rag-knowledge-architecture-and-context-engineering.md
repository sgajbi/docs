# 06 - RAG, Knowledge Architecture and Context Engineering

## 1. Why RAG is essential in wealth AI

A wealth AI system must answer from approved, current and entitlement-controlled sources. Retrieval-Augmented Generation (RAG) helps ground AI answers in documents, data and calculations instead of relying only on model memory.

RAG is important because wealth platforms require:

- current product terms
- approved product universe and target-market rules
- suitability and mandate policies
- portfolio-specific data
- calculation outputs
- report snapshots
- regulatory disclosures
- source ownership and lineage

## 2. Knowledge-source categories

| Source category | Examples |
|---|---|
| Product knowledge | Product packs, termsheets, KIDs/PHS, product taxonomy. |
| Governance knowledge | APU, risk ratings, target market, distribution restrictions. |
| Methodology | TWR, MWR, attribution, risk, valuation and reporting methodologies. |
| Operational knowledge | Runbooks, incident guides, reconciliation procedures. |
| Client/portfolio data | Holdings, transactions, cash, performance, risk, mandates. |
| Market/reference data | Prices, FX, curves, identifiers, corporate actions. |
| Policy and compliance | Suitability, disclosure, cross-border, data privacy, AI use policies. |
| Interaction history | Meeting notes, tasks, consents and approved summaries. |

## 3. RAG architecture

```text
Knowledge Sources
  |-- Markdown docs
  |-- PDFs / termsheets
  |-- Product master
  |-- APU / target market
  |-- Portfolio analytics snapshots
  |-- Reports and statements
  `-- Policies / runbooks

Ingestion Pipeline
  |-- document parsing
  |-- chunking
  |-- metadata extraction
  |-- source classification
  |-- access labels
  |-- embedding
  |-- index update
  `-- quality validation

Retrieval Pipeline
  |-- user intent classification
  |-- entitlement filtering
  |-- query rewriting
  |-- hybrid search
  |-- re-ranking
  |-- context compression
  |-- source citation packaging
  `-- freshness checks

Generation Pipeline
  |-- prompt template
  |-- policy instructions
  |-- retrieved evidence
  |-- structured data context
  |-- model response
  |-- output validation
  |-- citation check
  `-- audit record
```

## 4. Chunking strategy

| Content type | Chunking approach |
|---|---|
| Product packs | Section-based chunks with headings and product family metadata. |
| Termsheets | Term-table extraction plus payoff/lifecycle sections. |
| Policies | Rule-level chunks with effective dates and jurisdiction. |
| Reports | Snapshot-level chunks with portfolio/date/version metadata. |
| Calculations | Structured JSON or tabular facts, not only text chunks. |
| Runbooks | Step-level chunks with severity and system metadata. |

## 5. Metadata requirements

Each chunk should carry:

| Metadata | Purpose |
|---|---|
| source_id | Trace back to document/system. |
| document_type | Product pack, policy, termsheet, report, runbook. |
| product_family | Bonds, funds, notes, etc. |
| jurisdiction | SG, HK, AU, global, etc. |
| effective_date | Policy/product validity. |
| owner | Business or platform owner. |
| version | Document/calculation version. |
| confidentiality | Public/internal/confidential/client-private. |
| entitlement_scope | Who can retrieve. |
| freshness_status | Current/stale/superseded. |
| citation_pointer | Link/section/page. |

## 6. Context engineering

Context should be assembled based on user intent.

| Intent | Context required |
|---|---|
| Product explanation | Product KB + termsheet + risk classification. |
| Suitability summary | Product + client profile + suitability rules + APU. |
| Portfolio review | Analytics snapshot + report methodology + data-quality flags. |
| Mandate breach | Mandate rule + holdings/exposure + breach calculation. |
| Operations triage | Break details + source ownership + runbook + historical incidents. |
| Client answer | Client-authorized portfolio/report data + approved explanations. |

Do not dump full client, portfolio and product context into every prompt. Context should be scoped to the task.

## 7. Hybrid retrieval

Use both semantic and lexical retrieval.

| Retrieval type | Useful for |
|---|---|
| Vector search | Conceptual matches, explanations, related sections. |
| Keyword search | ISIN, instrument ID, policy number, product name, exact term. |
| Structured lookup | Product master, holdings, transactions, risk metrics. |
| Graph traversal | Client/account/portfolio/product relationships. |
| Time-aware search | Effective-date policies, report snapshots, latest NAV. |

## 8. Grounding rules

| Rule | Requirement |
|---|---|
| No source, no factual claim | Especially for product terms, performance and suitability. |
| Show uncertainty | If sources conflict or data is stale. |
| Prefer current approved source | Avoid obsolete documents. |
| Separate facts from interpretation | Facts come from source; AI explains implications. |
| Use structured data for numbers | Do not rely on LLM extraction for financial calculations. |
| Cite methodology | Performance/risk explanations must cite calculation method or output. |

## 9. RAG failure modes

| Failure | Example | Control |
|---|---|---|
| Wrong source | Uses old product term. | Effective-date filtering. |
| Missing source | Answers from model memory. | Citation requirement. |
| Over-retrieval | Irrelevant context confuses model. | Re-ranking and context compression. |
| Access leak | Retrieves another client's data. | Entitlement filters before retrieval. |
| Chunk loss | Important table split incorrectly. | Table-aware parsing. |
| Stale data | Uses old NAV/price. | Freshness metadata. |
| Citation mismatch | Citation does not support claim. | Citation verification tests. |

## 10. Knowledge-base quality standard

A wealth AI knowledge base should be:

- structured and versioned
- written with product taxonomy consistency
- rich in examples and edge cases
- mapped to transaction, position, valuation and reporting models
- source-owned
- reviewed by domain SMEs
- testable with evaluation questions
- kept current through release/change governance

Your current financial-product documentation packs are a strong foundation for this RAG layer because they are organized by product family and include advisory, mandate, analytics, reporting and platform modelling sections.

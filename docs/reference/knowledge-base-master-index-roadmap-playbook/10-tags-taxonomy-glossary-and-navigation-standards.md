# 10 - Tags, Taxonomy, Glossary and Navigation Standards

## 1. Why tags matter

Tags help the knowledge base become searchable and AI-ready.

Good tags allow:

- fast search
- RAG retrieval
- topic clustering
- learning paths
- practitioner readiness
- office session planning
- product gap analysis

## 2. Standard tag categories

Use tags across these categories:

| Category | Examples |
|---|---|
| Product | bonds, funds, equities, structured-notes |
| Asset class | fixed-income, equity, alternatives, cash |
| Lifecycle | trade, settlement, coupon, maturity, corporate-action |
| Analytics | performance, attribution, risk, benchmark |
| Advisory | suitability, target-market, proposal, client-review |
| DPM | mandate, model-portfolio, rebalancing, drift |
| Reporting | statement, portfolio-review, disclosure, archive |
| Data | instrument-master, market-data, lineage, data-quality |
| Platform | api, event, architecture, integration |
| Control | reconciliation, audit, security, governance |
| AI | rag, copilot, agent, evaluation, model-risk |
| Commercial | pricing, procurement, vendor, enterprise-sales |

## 3. Recommended front matter

```yaml
---
title: "Bonds Reference Pack"
domain: "wealth-products"
layer: "product-knowledge"
status: "reference"
owner: "Sandeep"
last_reviewed: "2026-06-27"
tags:
  - bonds
  - fixed-income
  - coupon
  - yield
  - duration
  - valuation
  - reporting
---
```

## 4. Navigation links

Each pack should link to:

- previous prerequisite pack
- next recommended pack
- related product packs
- related analytics pack
- related reporting pack
- related platform pack
- glossary/source notes

Example:

```markdown
## Related packs

- Bonds
- Structured Notes
- Derivatives
- Performance and Risk
- Market Data and Pricing
- Investment Accounting
```

## 5. Glossary standards

Glossary entries should be:

- concise
- business-friendly
- platform-aware
- not overly legalistic
- consistent across packs

Example:

| Term | Definition |
|---|---|
| Legal holding | The instrument legally held in the account |
| Look-through exposure | Economic exposure to underlying assets |
| Lifecycle event | Product/business event that may or may not create a transaction |
| Transaction | Accounting-grade economic posting |
| Reporting snapshot | Reproducible data state used for report generation |

## 6. Canonical vocabulary

Use consistent terms:

| Prefer | Avoid |
|---|---|
| portfolio | book / basket when meaning portfolio |
| instrument | product/security mixed randomly |
| transaction | event when no economic posting |
| lifecycle event | transaction for observations |
| market value | value / amount ambiguously |
| nominal amount | face when product is not bond-like |
| cost basis | cost price ambiguously |
| entitlement | permission/right ambiguously |
| source of truth | master source / golden source inconsistently |

## 7. File section pattern

A professional file should generally contain:

```text
1. Purpose
2. Business context
3. Core concepts
4. Lifecycle / workflow
5. Data model
6. Analytics/reporting impact
7. Controls and QA
8. Examples
9. practitioner-ready explanation
10. Key takeaways
```

## 8. AI/RAG chunking guidance

For AI use:

- keep sections focused
- use explicit headings
- avoid mega paragraphs
- define acronyms
- use tables for comparisons
- include examples
- keep source notes separate
- avoid contradictions
- mark jurisdiction-specific content
- add tags/front matter

## 9. Search conventions

Useful search patterns:

| Search goal | Query style |
|---|---|
| product lifecycle | `structured notes lifecycle barrier autocall` |
| data model | `fund NAV transaction position model` |
| reporting | `private markets NAV lag reporting` |
| QA | `corporate action split cost basis QA` |
| AI | `RAG advisor copilot suitability guardrails` |
| practitioner | `DPM mandate practitioner answer` |

## 10. Key takeaway

A strong taxonomy turns a collection of files into a reusable knowledge system.

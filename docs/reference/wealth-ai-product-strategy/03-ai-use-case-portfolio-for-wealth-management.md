# 03 - AI Use-Case Portfolio for Wealth Management

## 1. Use-case selection framework

AI use cases should be prioritized by value, risk and implementation readiness.

| Dimension | Questions |
|---|---|
| Business value | Does it save advisor/ops time, improve client experience, reduce risk or increase revenue? |
| Data readiness | Are inputs available, accurate, timely and source-owned? |
| Control readiness | Can outputs be reviewed, approved, audited and tested? |
| Risk level | Could wrong output harm a client, breach regulation or trigger an unauthorized action? |
| Differentiation | Is this domain-specific or just generic productivity? |
| Reusability | Can the capability be reused across products/channels/countries? |

## 2. Use-case categories

| Category | Examples |
|---|---|
| Knowledge assistant | Product answer generation, policy answer generation, methodology explanation. |
| Narrative assistant | Report commentary, performance explanation, risk commentary. |
| Decision assistant | Suitability summary, mandate breach explanation, idea ranking. |
| Workflow assistant | Task creation, meeting brief, case triage, approval routing. |
| Operations assistant | Reconciliation break explanation, data-quality issue triage. |
| Engineering assistant | API docs, QA scenarios, migration mapping, regression analysis. |
| Client assistant | Simple portfolio explanation, document search, education content. |

## 3. Advisor use cases

| Use case | Description | Risk level | Controls |
|---|---|---:|---|
| Client meeting brief | Summarize client profile, portfolio, recent activity, risks, cash needs and follow-ups. | Medium | Advisor review, source links, data-quality flags. |
| Portfolio change summary | Explain what changed since last review. | Medium | Calculation lineage, snapshot comparison. |
| Product explanation | Explain a bond, note, fund, derivative or private-market product. | Medium | Approved product KB only. |
| Suitability summary | Summarize relevant suitability factors for a proposed idea. | High | Rule-engine integration, compliance wording, human approval. |
| Proposal draft | Draft proposal with rationale, risks and alternatives. | High | APU, suitability and disclosure checks. |
| Client email draft | Draft follow-up email after meeting. | Medium | Human approval, no unsupported advice. |

## 4. Portfolio manager and DPM use cases

| Use case | Description | Controls |
|---|---|---|
| Mandate drift explanation | Explain asset-class, currency, issuer or product breaches. | Uses mandate engine output; AI only explains. |
| Rebalance narrative | Explain why rebalance is recommended and what trade-offs exist. | Requires approved trade list and PM approval. |
| Model change summary | Explain model portfolio changes to advisors/clients. | Source to investment committee notes. |
| Exposure anomaly detection | Flag unusual concentration or risk changes. | Uses deterministic analytics first; AI explains. |
| Cash and liquidity planning | Highlight upcoming fees, settlements, maturities, capital calls. | Uses cashflow engine and holdings data. |

## 5. Reporting use cases

| Use case | Description |
|---|---|
| Performance commentary | Translate TWR/MWR/contribution/attribution into clear narrative. |
| Risk commentary | Explain volatility, drawdown, concentration and stress scenarios. |
| Benchmark-relative commentary | Explain excess return and attribution versus benchmark. |
| Data-quality exception wording | Explain stale prices, missing NAV, late corporate action or estimated valuation. |
| Statement summary | Provide executive summary of holdings, income, activity and value changes. |
| Client-friendly glossary | Explain report terms in simple language. |

## 6. Product governance use cases

| Use case | Description |
|---|---|
| Termsheet extraction | Extract issuer, underlying, tenor, coupon, barrier, risk factors and lifecycle events. |
| Product comparison | Compare two funds, notes, bonds or structured products. |
| Target market mapping | Suggest target market attributes for review. |
| Disclosure completeness check | Validate required risk language and document presence. |
| APU impact summary | Explain why a product is approved, restricted, hold-only or suspended. |

AI output should not be the approval itself. It assists product governance teams with evidence gathering and review.

## 7. Operations use cases

| Use case | Description |
|---|---|
| Reconciliation break triage | Classify breaks by likely cause: price, FX, corporate action, settlement, reference data, timing. |
| Failed trade explanation | Summarize reason, impact and next action. |
| Data-quality issue summary | Explain impacted portfolios, reports and downstream analytics. |
| Migration mapping assistant | Compare source/target fields and draft mapping notes. |
| Incident summary | Generate business-friendly incident timeline and impact. |

## 8. Engineering and BA use cases

| Use case | Description |
|---|---|
| API documentation assistant | Generate endpoint docs and examples from contracts. |
| QA scenario generator | Generate product/lifecycle regression tests. |
| ADR drafting | Draft architecture decision records. |
| Source ownership mapping | Identify which source owns each field. |
| Release note drafting | Translate technical changes into business release notes. |
| Agentic engineering task preparation | Create repo-specific implementation prompts and checklists. |

## 9. Client-facing use cases

Client-facing AI should start conservative.

| Use case | Description | Guardrail |
|---|---|---|
| Portfolio education | Explain what a report section means. | No personalized advice unless approved. |
| Document search | Find statements, reports or product documents. | Entitlement-controlled. |
| Simple holdings explanation | Explain product type and basic risk. | Approved content only. |
| Meeting summary | Summarize advisor-agreed points. | Advisor-approved summary. |

## 10. Prioritization matrix

| Phase | Use cases |
|---|---|
| Phase 1 | Product knowledge assistant, advisor meeting brief, report narrative draft, operations break summary. |
| Phase 2 | Portfolio review assistant, mandate breach explanation, suitability summary, idea shortlist. |
| Phase 3 | Proposal draft, next-best-action, rebalance narrative, client-facing portfolio education. |
| Phase 4 | Controlled agentic workflows, cross-system workflow automation, AI-assisted portfolio construction. |

## 11. Use-case risk classification

| Risk | Examples | Required control |
|---|---|---|
| Low | Internal glossary, product education, non-client generic explanation. | Standard QA. |
| Medium | Advisor meeting prep, report narrative draft, operations summary. | Human review and source links. |
| High | Suitability summary, proposal draft, mandate exception, client-facing output. | Rule-engine integration, compliance review, audit. |
| Very high | Trade execution, product recommendation, profile change, client instruction. | Human approval, workflow controls, strong audit; AI should not act alone. |

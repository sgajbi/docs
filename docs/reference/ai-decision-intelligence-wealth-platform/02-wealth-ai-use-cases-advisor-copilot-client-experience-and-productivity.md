# 02 - Wealth AI Use Cases, Advisor Copilot, Client Experience and Productivity

## 1. Advisor copilot as the primary wealth AI surface

The advisor copilot should be the first major AI experience in a wealth platform because it helps the human advisor remain accountable while improving speed and quality.

An advisor copilot can support:

| Workflow | Copilot capability |
|---|---|
| Morning preparation | Summarize overnight portfolio changes, client alerts and market events. |
| Client meeting brief | Generate client brief, portfolio issues, recent activity, income and risk summary. |
| Portfolio review | Explain performance, risk, concentration, cashflow and benchmark-relative outcomes. |
| Product education | Explain product mechanics, suitability, risks and reporting treatment. |
| Proposal drafting | Draft proposal rationale, trade ideas and client-friendly explanation. |
| Mandate monitoring | Highlight drift, breach, liquidity, concentration and restriction issues. |
| Client follow-up | Draft call notes, next steps, email summary and task list. |
| Operational triage | Explain missing data, failed trades, stale price or reconciliation break. |

## 2. Advisor copilot design pattern

A good copilot response should be structured as:

```text
Answer summary
Evidence used
Calculations or source systems used
Risks / caveats / missing data
Recommended next action
Approval or escalation requirement
```

This is more useful than a plain chat answer because wealth users need actionability and controls.

## 3. Client meeting brief workflow

### Inputs

| Input | Examples |
|---|---|
| Client profile | Risk profile, investment objectives, liquidity needs, restrictions. |
| Relationship data | RM, IC, coverage team, service model, household. |
| Holdings | Current positions, cash, loans, collateral, commitments. |
| Activity | Trades, subscriptions, redemptions, corporate actions, income. |
| Analytics | Performance, contribution, attribution, risk, benchmark, concentration. |
| Mandates | DPM model, advisory restrictions, product eligibility. |
| Market context | Approved house views, research, macro summary, asset-class outlook. |
| Open items | Tasks, unresolved cases, pending documents, expired suitability profile. |

### Output

| Section | Example content |
|---|---|
| Executive summary | "Client remains overweight US equities and underweight cash versus target." |
| Key changes | "Portfolio value down 1.2% MTD, mainly from technology holdings." |
| Risks | "Single issuer concentration above guideline due to Nvidia rally." |
| Opportunities | "Consider rebalance into short-duration bonds if liquidity objective remains." |
| Follow-ups | "Update risk profile expiring next month; confirm tax residency change." |
| Guardrails | "No product recommendation generated because suitability profile is stale." |

## 4. Portfolio review narrative generation

A portfolio review copilot should not merely produce nice wording. It must tie narrative to analytics.

| Narrative claim | Required evidence |
|---|---|
| Portfolio outperformed benchmark | Portfolio return, benchmark return, period, methodology. |
| Performance was driven by equities | Contribution by asset class or position. |
| Risk increased | Volatility, VaR, drawdown, concentration or leverage change. |
| Income increased | Coupon/dividend/distribution data, classification and period. |
| Liquidity weakened | Cash %, liquid assets %, private-market commitments, upcoming obligations. |
| Mandate drift exists | Current allocation, target, tolerance and breach reason. |

### Safe response rule

If calculation evidence is missing, the copilot should say:

```text
I cannot explain this performance driver confidently because the contribution calculation is unavailable for the selected period. I can show holdings and market-value changes, but that is not the same as official contribution.
```

## 5. Product explanation use case

The copilot should explain products at multiple levels:

| Audience | Explanation style |
|---|---|
| Client | Plain-language risks, return behavior, liquidity and worst-case scenario. |
| Advisor | Suitability, product taxonomy, client fit and comparison with alternatives. |
| BA / product owner | Lifecycle, data fields, transaction types, reporting and edge cases. |
| Engineer | Data model, APIs, event flows, validation and calculation requirements. |
| Risk/compliance | Target market, disclosure, concentration, audit and approval trail. |

Example prompt:

```text
Explain this autocallable note for a balanced-risk private-banking client, using the termsheet, approved product taxonomy and client-friendly language. Include issuer risk, barrier risk, early redemption risk, liquidity risk and scenarios.
```

The copilot must retrieve the actual termsheet and product rules rather than relying on generic memory.

## 6. Next-best-action and next-best-conversation

AI can rank and explain potential advisor actions. However, in regulated wealth this should be framed as **decision support**, not hidden recommendation automation.

| Signal | Possible action |
|---|---|
| Large cash balance | Discuss deployment, liquidity need or money market options. |
| Model drift | Review rebalance proposal. |
| Concentration breach | Discuss risk reduction or exception approval. |
| Bond maturity soon | Review reinvestment options. |
| Private-market capital call pending | Reserve cash or plan funding. |
| Suitability profile expiring | Update client profile before recommendation. |
| Structured note barrier near | Explain scenario and monitoring plan. |

### Required controls

A next-best-action engine should record:

| Field | Purpose |
|---|---|
| signal_id | What triggered the suggestion. |
| source_data | Evidence used. |
| rule_or_model_version | Reproducibility. |
| suitability_status | Whether the action is allowed. |
| mandate_status | Whether mandate permits action. |
| confidence | Ranking quality and uncertainty. |
| reviewer | Human decision-maker. |
| disposition | Accepted, rejected, deferred, escalated. |

## 7. Client-facing AI

Client-facing AI is higher risk than advisor-facing AI because the client may interpret output as advice. The design must be conservative.

| Allowed client-facing use | Controls |
|---|---|
| Explain statement sections | Use approved statement data and glossary. |
| Explain product held | Use client's actual holding and product documents. |
| Explain performance | Use official performance calculations only. |
| Answer service questions | Use service policies and entitlement-scoped information. |
| Summarize documents | Use provided documents and cite sections. |

| Restricted or high-risk use | Required treatment |
|---|---|
| Product recommendation | Route to advisor workflow and suitability process. |
| Trade instruction | Strong authentication, confirmation and order controls. |
| Tax/legal advice | Provide general information and escalate to qualified advisor. |
| Market prediction | Present approved research context and uncertainty. |
| Complaint handling | Case creation, controlled language and escalation. |

## 8. Productivity use cases for platform teams

AI also improves engineering and BA productivity:

| Role | AI support |
|---|---|
| Architect | Draft ADRs, compare patterns, generate risk/control checklists. |
| BA | Convert business rules into scenarios, API fields and acceptance criteria. |
| QA | Generate regression matrix, edge cases and test data. |
| Developer | Draft code, unit tests, schema examples and migration notes. |
| Support | Summarize incidents, correlate logs, draft runbook actions. |
| Product owner | Translate roadmap into capability map and prioritization options. |

For regulated enterprise work, AI-generated artifacts must still pass review, tests and evidence gates.

## 9. Good advisor copilot output example

```text
Client portfolio review summary

1. Portfolio value increased 2.1% QTD. Official TWR is available for the period and reconciles to the reporting snapshot.
2. Main positive contributors were US equities and USD strength. The largest single-name contribution came from Nvidia.
3. Portfolio is now above the model allocation range for US equities by 4.2 percentage points.
4. Cash is below the minimum liquidity guideline after recent fund subscriptions and upcoming private-market capital call.
5. No product recommendation should be made until the client's suitability profile is refreshed, because the current profile expires this month.

Suggested advisor actions
- Review rebalance from US equities into short-duration fixed income or money market funds.
- Reserve liquidity for capital call before proposing additional illiquid products.
- Refresh suitability profile before issuing investment proposal.
```

## 10. Bad advisor copilot output example

```text
The client should buy a structured note because the coupon is attractive.
```

Why this is bad:

| Problem | Explanation |
|---|---|
| No suitability check | Client risk profile and product eligibility not verified. |
| No product evidence | Actual termsheet not retrieved. |
| No risk explanation | Coupon, barrier, issuer and liquidity risk omitted. |
| No alternatives | Does not compare bond, fund or cash options. |
| No audit trail | No source, model, reviewer or approval path. |

## 11. Productive but safe principle

The best AI copilot does not replace the advisor. It makes the advisor better prepared, more consistent, more evidence-based and more efficient.

# 04 - Advisor Copilot, Client Copilot and Portfolio Review AI

## 1. Advisor Copilot objective

The Advisor Copilot should help relationship managers and investment advisors prepare, explain, document and follow up - without replacing their accountability.

Primary jobs:

- understand the client and portfolio quickly
- identify what changed
- explain performance and risk
- highlight actions requiring attention
- prepare client-friendly communication
- ensure suitability, mandate and disclosure checks are visible

## 2. Advisor Copilot home screen

Suggested sections:

| Section | Description |
|---|---|
| Client snapshot | Household, relationship, risk profile, objectives, booking centre, service model. |
| Portfolio snapshot | AUM, allocation, currency, risk, performance, income, cash, top holdings. |
| What changed | Material changes since last review: value, cashflows, trades, maturities, risks. |
| Attention items | Mandate breaches, concentration, stale prices, upcoming settlements, capital calls. |
| Suggested discussion | Topics to discuss, questions to ask, documents to review. |
| Open actions | Tasks, approvals, pending documents, follow-up reminders. |

## 3. Client meeting brief

A meeting brief should combine factual data and AI-generated narrative.

```text
Client Meeting Brief
  |-- Relationship summary
  |-- Portfolio status
  |-- Performance and risk since last review
  |-- Key holdings and concentration
  |-- Income and cashflow outlook
  |-- Upcoming events: maturities, coupons, capital calls, renewals
  |-- Mandate / suitability / restriction flags
  |-- Relevant ideas or watchlist items
  |-- Questions for client
  `-- Follow-up actions
```

Every claim should link back to source data, report snapshot or methodology.

## 4. Portfolio Review AI

Portfolio Review AI should generate narratives from structured analytics, not free-form guesses.

Inputs:

- holdings by asset class, sector, geography, currency
- market values and valuation quality
- transactions and cashflows
- performance by period
- contribution and attribution
- benchmark-relative results
- risk metrics
- mandate checks
- income and activity
- exceptions and disclosures

Outputs:

- executive summary
- allocation explanation
- performance explanation
- risk explanation
- benchmark explanation
- income/activity explanation
- action points
- exception wording

## 5. Narrative generation pattern

Use deterministic calculations first; AI explains second.

```text
Calculation Engine -> Analytics Snapshot -> Narrative Context -> AI Draft -> Human Review -> Published Report
```

AI should not recalculate performance from raw data unless explicitly designed and tested for that purpose. It should consume validated analytics outputs.

## 6. Performance commentary template

A high-quality performance narrative should answer:

1. What was the portfolio return?
2. How did it compare to benchmark or objective?
3. Which asset classes or holdings contributed most?
4. What detracted?
5. Was the result driven by market movement, currency, income, allocation, selection or cashflows?
6. Are there data-quality limitations?
7. What should the advisor discuss next?

Bad commentary:

```text
The portfolio performed well due to positive market conditions.
```

Better commentary:

```text
The portfolio returned 4.2% for the quarter, outperforming its benchmark by 0.8%. Equities contributed most of the absolute return, while fixed income reduced volatility but detracted slightly versus benchmark. Currency exposure to USD added 0.3% in portfolio currency terms. The main detractor was the underweight to technology relative to benchmark. Performance is based on final prices except for two private-market holdings using latest available NAV.
```

## 7. Risk commentary template

A risk narrative should explain:

- overall risk level
- volatility and drawdown
- concentration risk
- issuer/counterparty exposure
- currency exposure
- liquidity risk
- leverage/collateral risk
- product complexity
- stress scenario sensitivity

## 8. Client Copilot

Client-facing AI should be more restricted than advisor AI.

Allowed early capabilities:

- explain report sections
- explain product types using approved content
- answer questions about statement data
- summarize historical activity
- explain performance terms
- find documents
- draft questions for advisor

Restricted capabilities:

- recommending trades
- ranking products without suitability controls
- interpreting complex products without disclosure
- making tax/legal advice claims
- acting on instructions without authenticated workflow

## 9. Client Copilot response policy

Client-facing answers should include:

- plain-English explanation
- source section or document reference
- clear limitation if data is missing or estimated
- advisor escalation when advice is needed
- no unsupported prediction or recommendation

## 10. Advisor productivity workflows

| Workflow | AI assistance |
|---|---|
| Before meeting | Brief, watchlist, portfolio changes, suggested questions. |
| During meeting | Product explanations, report navigation, note capture. |
| After meeting | Summary, follow-up email draft, tasks, CRM update. |
| Ongoing monitoring | Alerts, mandate drift, upcoming events, stale data. |

## 11. Controls

| Control | Why needed |
|---|---|
| Source citations | Avoid unsupported claims. |
| Snapshot locking | Ensure report output matches reviewed data. |
| Human approval | Required for client-facing or advice-related output. |
| Entitlement checks | Avoid cross-client leakage. |
| Output classification | Internal draft, advisor-approved, client-published. |
| Disclaimer insertion | Product/risk/legal limitations where needed. |
| Feedback capture | Improve prompt and retrieval quality. |

# Advisory, Mandate, Suitability, Reporting and Client Experience Integration

## 1. Why integration matters

Entitlements, workbench workflows and interaction history are not standalone domains. They are the control and experience layer across advisory, DPM, suitability, reporting and client servicing.

A wealth platform should make it impossible, or at least strongly controlled, for users to:

- advise without an active client profile;
- recommend a product outside target market;
- override suitability without approval;
- execute before consent;
- deliver a report to the wrong party;
- change proposal after client approval without re-consent;
- act on a portfolio outside mandate or authority;
- show restricted data to a household member without authority.

## 2. Advisory workflow integration

| Advisory step | Required platform integration |
|---|---|
| Client selection | Entitlement and relationship scope. |
| Needs analysis | Client profile, objectives, suitability, restrictions. |
| Portfolio review | Holdings, performance, risk, income and exposure snapshot. |
| Idea generation | Product governance, APU, target market and research. |
| Proposal simulation | Allocation, risk, liquidity, fees, tax/cost and mandate impact. |
| Suitability check | Client profile, product risk, complexity, concentration. |
| Disclosure | Product and jurisdiction-specific documents. |
| Consent | Digital/in-person consent record. |
| Order handoff | OMS, pre-trade, buying power, cash, restriction checks. |
| Follow-up | Interaction note, task, report archive. |

## 3. Suitability integration

Suitability should be integrated at multiple points, not only after order capture.

| Stage | Check type |
|---|---|
| Idea stage | Product eligibility and target-market pre-filter. |
| Proposal stage | Recommendation suitability and concentration impact. |
| Order stage | Final pre-trade suitability and appropriateness. |
| Post-trade | Exception review and portfolio suitability drift. |
| Periodic review | Ongoing portfolio fit and mandate alignment. |

Suitability result should include:

- result: pass/warn/fail;
- reason codes;
- product attributes used;
- client profile attributes used;
- concentration and portfolio impact;
- required disclosures;
- override eligibility;
- approval requirements;
- policy version;
- timestamp.

## 4. Mandate integration

Mandates define what is allowed in a portfolio.

| Mandate dimension | Workbench/control impact |
|---|---|
| Asset allocation | Rebalance, proposal and drift checks. |
| Product universe | Product ideas and orders filtered. |
| Risk limit | Volatility, VaR, drawdown, risk score. |
| Concentration | Issuer, sector, currency, asset class. |
| Liquidity | Minimum cash, max illiquid assets. |
| Leverage | Margin/lending/derivative exposure. |
| ESG/restriction | Exclusions, restricted lists. |
| Benchmark | Relative analytics and reporting. |
| Discretionary authority | PM may trade without per-trade client consent depending on mandate. |

### DPM versus advisory versus execution-only

| Service model | Workflow difference |
|---|---|
| Advisory | Client recommendation, suitability evidence and client consent required. |
| DPM | Portfolio manager acts under mandate; mandate compliance and PM approval central. |
| Execution-only | Client order, appropriateness where required, limited advice evidence. |
| Managed advisory | Model/rebalance recommended but client accepts. |
| Family office | Multi-entity reporting and authority complexity. |

## 5. Reporting integration

Client-facing reports should be generated from entitlement-safe and snapshot-consistent data.

| Report type | Entitlement/workflow dependency |
|---|---|
| Monthly statement | Recipient authority, account scope, valuation snapshot. |
| Portfolio review | Client/portfolio scope, performance/risk disclosure, advisor notes. |
| Proposal report | Proposal version, suitability result, disclosures. |
| DPM report | Mandate, benchmark, risk and attribution. |
| Tax report | Tax residency, reportable account, recipient authority. |
| Trust/family report | Beneficial ownership, legal entity hierarchy, confidentiality. |
| Margin report | Pledge scope, LTV, collateral, notice delivery. |

Report generation should store:

- report parameters;
- valuation date;
- data snapshot ID;
- recipient scope;
- disclosure set;
- generated version;
- delivery evidence;
- archive ID.

## 6. Client experience integration

Good client experience should be safe and transparent.

| Client question | Platform should answer |
|---|---|
| "Why am I seeing this product?" | Linked advisory rationale and suitability. |
| "What did I approve?" | Proposal/disclosure version and consent record. |
| "Why is this report different from yesterday?" | Snapshot/version/data correction explanation. |
| "Why can't I trade this online?" | Channel/product eligibility reason. |
| "Who can see my family-office report?" | Recipient authority and access list. |
| "Why was my rebalance done?" | Mandate/model version and PM decision record. |
| "Why is this valuation stale?" | Price source and stale-price disclosure. |

## 7. Interaction history and suitability evidence

A recommendation should have a coherent evidence chain:

```text
Client profile
-> Advisor discussion note
-> Portfolio review snapshot
-> Proposal
-> Suitability result
-> Disclosure shown
-> Client consent
-> Order/trade
-> Confirmation/report
```

If any link is missing, the platform should flag an evidence gap.

## 8. Advisor productivity versus control

Controls should be embedded into the advisor journey, not added only at the end.

| Poor design | Better design |
|---|---|
| Advisor prepares proposal then discovers product is ineligible. | Product universe filtered upfront. |
| Suitability fail appears only in OMS. | Suitability preview at idea/proposal stage. |
| Disclosures manually attached. | Required disclosure set generated from product/jurisdiction. |
| Client approval is email text. | Structured consent linked to proposal version. |
| Notes are separate from proposal. | Interaction note linked to advisory workflow. |
| Report is manually assembled. | Snapshot-driven report with lineage. |

## 9. Cross-product client experience examples

| Product | Workbench/channel UX requirement |
|---|---|
| Structured note | Show payoff scenarios, issuer risk, barrier distance, maturity/observation calendar. |
| Bond | Show yield, duration, maturity, call risk, rating, issuer concentration. |
| Fund | Show NAV date, share class, fees, liquidity, distribution policy, risk rating. |
| Equity | Show market risk, concentration, corporate actions, dividend history. |
| Derivative | Show notional, leverage, Greeks, expiry, margin, worst-case scenarios. |
| Private market | Show commitment, funded/unfunded, NAV lag, capital calls, liquidity constraints. |
| Loan/collateral | Show LTV, availability, collateral shortfall, pledge relationships. |
| Insurance | Show premiums, surrender value, benefits, riders, beneficiary. |

## 10. Controls and KPIs

| KPI/control metric | Purpose |
|---|---|
| Proposals with complete evidence chain | Advisory quality. |
| Suitability overrides by advisor/product | Conduct monitoring. |
| Client consents captured before order | Process compliance. |
| Reports delivered to unauthorized recipient | Critical privacy control. |
| Workflows breaching SLA | Operational health. |
| Open alerts older than threshold | Advisor/task effectiveness. |
| Entitlement denials by reason | Access policy monitoring. |
| Dormant delegations | Access-risk monitoring. |
| Late meeting notes | Evidence quality. |

## 11. QA scenarios

| Scenario | Expected result |
|---|---|
| Proposal generated for product not in APU. | Blocked before client presentation. |
| Client suitability expires after proposal but before order. | Recheck required. |
| DPM rebalance breaches mandate. | Requires exception workflow. |
| Client approves proposal in portal. | Consent linked to exact proposal and disclosure version. |
| Household report includes entity without authority. | Report generation blocked. |
| RM views report for non-assigned client. | Denied and logged. |
| Advisor changes product after consent. | Consent invalidated and reapproval required. |
| Client asks for explanation of performance. | Report links to calculation snapshot and methodology. |

## 12. Practitioner Explanation

"The advisor and client experience layer should embed suitability, mandate, product governance, reporting and entitlement controls into the normal workflow. The advisor should not have to discover problems late in the order flow. The platform should pre-filter product ideas, simulate portfolio impact, run suitability and mandate checks, generate the right disclosures, capture consent against immutable versions and retain a full evidence chain from discussion to execution and reporting."

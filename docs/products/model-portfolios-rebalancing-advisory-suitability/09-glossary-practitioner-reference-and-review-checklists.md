# 09 - Glossary, Practitioner Reference And Review Checklists

## 1. Glossary

| Term | Meaning |
|---|---|
| Advisory | Service where advisor recommends but client decides |
| DPM | Discretionary Portfolio Management; manager invests within agreed mandate |
| Model portfolio | Reference target allocation or instrument set |
| SAA | Strategic asset allocation, long-term policy allocation |
| TAA | Tactical asset allocation, shorter-term view/tilt |
| Target allocation | Preferred allocation weight |
| Mandate range | Allowed minimum/maximum allocation |
| Drift | Difference between current and target allocation |
| Rebalancing | Adjusting portfolio toward target/range |
| Tolerance band | Drift threshold that triggers monitoring/action |
| Proposal | Controlled recommendation package |
| Proposal line | Individual recommended action |
| Suitability | Whether recommendation fits client profile |
| Appropriateness | Whether client understands product/service risks |
| Product eligibility | Whether product can be sold to/accessed by the client |
| Pre-trade check | Control before order placement |
| Post-trade check | Control after execution |
| Override | Approved exception to a rule |
| Client consent | Client approval/acknowledgement |
| Restricted list | Instruments/issuers not allowed |
| Look-through | Underlying exposure of funds/notes/products |
| Sleeve | Sub-portfolio with separate objective |
| Fair allocation | Fair distribution of block trades across accounts |
| Drift score | Aggregate measure of deviation from target |
| Remediation | Action to correct breach/non-compliance |
| Lineage | Traceability from recommendation to data, rule and execution |

## 2. Practitioner explanation: model portfolios

"Model portfolios are governed target portfolios or allocation templates used to guide advisory or discretionary investment decisions. They usually define strategic allocation, tactical tilts, permitted product universe, benchmark, risk profile, allocation ranges and rebalancing policy. In a wealth platform, a model portfolio should be versioned and time-effective. It should not be treated as automatically suitable for every client. The system must compare the client's current portfolio against the model, apply client-specific restrictions, product eligibility, suitability, liquidity, tax, FX and mandate checks, and then generate a proposal or DPM trade action with full audit lineage."

## 3. Practitioner explanation: rebalancing

"Rebalancing is the process of moving a portfolio back toward its target allocation or permitted mandate range. It can be calendar-based, threshold-based, hybrid or cashflow-driven. A real wealth platform cannot rebalance only mathematically. It must consider transaction costs, taxes, liquidity, minimum order sizes, fund dealing cycles, bond liquidity, pledged assets, unsettled trades, client restrictions, suitability and pre-trade controls. The output should be an explainable proposal or order list, not a black-box trade instruction."

## 4. Practitioner explanation: suitability

"Suitability means that a recommendation is appropriate for a specific client given their risk profile, objectives, financial situation, investment horizon, liquidity needs, knowledge and experience, restrictions and portfolio context. I would model suitability as a rule-driven framework that evaluates the client, product, portfolio and service model together. It should store the exact rule version, input data, result, warnings, override approvals and client acknowledgements. Suitability should happen before orders are placed and should be refreshed if the proposal, product, client profile or portfolio state changes."

## 5. Practitioner explanation: advisory proposal platform

"An advisory proposal platform should connect portfolio analytics, model targets, product ideas, suitability checks, client disclosures, consent and order generation. The proposal should show current versus proposed allocation, recommended trades, rationale, key risks, cost, liquidity, FX and income impact. For audit, the platform must link the proposal to the exact portfolio snapshot, client profile, product data, model version, suitability rule set and client consent."

## 6. Practitioner workflow

```text
1. Capture current portfolio snapshot
2. Map holdings to classifications and look-through exposures
3. Select applicable model/mandate version
4. Calculate current allocation and drift
5. Identify breaches and rebalance candidates
6. Generate candidate trade actions
7. Apply product universe and restrictions
8. Run suitability and pre-trade checks
9. Create proposal with rationale and before/after analytics
10. Review by advisor/PM/supervisor if required
11. Present to client or approve under DPM authority
12. Capture consent/approval
13. Generate orders and execute
14. Reconcile execution and settlement
15. Run post-trade checks and reporting
```

## 7. Practitioner core rules

| Area | Rule examples |
|---|---|
| Client | KYC valid, risk profile valid, authorized signer |
| Product | Approved, risk-rated, documents available |
| Portfolio | Post-trade allocation within limits |
| Concentration | Issuer/security/sector/currency limits |
| Liquidity | Cash/liquid assets maintained |
| Credit | Rating/duration/spread limits |
| FX | Non-base currency exposure controlled |
| Alternatives | Illiquid/unfunded commitment limits |
| Derivatives | Hedging/speculative/leverage permissions |
| Lending | LTV, collateral and pledged asset impact |
| Suitability | Product and strategy fit client profile |
| Consent | Client approval linked to exact proposal version |

## 8. Common design mistakes

1. Treating model portfolio as client-specific recommendation without suitability.
2. Overwriting model weights instead of versioning them.
3. Generating trades without immutable portfolio snapshot.
4. Ignoring pending trades and unsettled cash.
5. Ignoring tax lots, fees and transaction costs.
6. Selling pledged assets without credit impact check.
7. Treating all funds/alternatives as daily liquid.
8. Not separating advisory and DPM workflows.
9. Allowing silent rule overrides.
10. Not revalidating proposal before order creation.
11. Capturing client consent without proposal version.
12. Not reconciling post-trade outcome to expected proposal impact.

## 9. Common review questions

### Q1. How do you design a model portfolio platform?

Start with model governance and versioning, then target allocation hierarchy, portfolio snapshot, drift engine, proposal engine, suitability/rule engine, approval/consent workflow, order integration and post-trade monitoring. Ensure full lineage from model to recommendation to execution.

### Q2. What is the difference between target, tolerance and mandate range?

Target is preferred allocation. Tolerance band is the drift level that triggers monitoring or rebalancing. Mandate range is the allowed legal/investment boundary. A portfolio can be away from target but still within mandate; a mandate breach requires remediation.

### Q3. What makes rebalancing hard in wealth management?

Real portfolios contain illiquid assets, structured products, bonds, funds, cash in multiple currencies, pledged collateral, tax lots, pending orders, product restrictions and client-specific constraints. Rebalancing must therefore be suitability-aware, liquidity-aware, tax-aware, cost-aware and operationally executable.

### Q4. How do you handle client-specific restrictions?

Store them as first-class mandate/restriction rules. Apply them before trade generation and again before order placement. If a model trade conflicts with a restriction, exclude the account, choose a substitute or route for approved override depending on rule severity.

### Q5. How do you prove a recommendation was suitable?

Store client profile version, product master version, portfolio snapshot, model version, rule set version, suitability assessment, rule evaluations, warnings, disclosures, overrides, consent and final orders. The recommendation should be reproducible later.

## 10. Product-specific advisory notes

| Product | Advisory/rebalance issue |
|---|---|
| Equities | Concentration, tax lots, volatility, corporate actions |
| Bonds | Duration, credit, liquidity, callable risk, maturity ladder |
| Funds | NAV timing, share class, fees, dealing cycle, switches |
| ETFs | Intraday liquidity, tracking error, spreads |
| Notes | Complexity, issuer risk, barrier risk, lock-in |
| Derivatives | Leverage, margin, Greeks, appropriateness |
| Private markets | Commitments, capital calls, NAV lag, illiquidity |
| Cash/FX | Settlement, cash buffer, FX exposure |
| Loans/collateral | LTV, pledge, margin call, buying power |
| Insurance | Surrender charges, protection objective, long horizon |

## 11. Strong architecture statement

"A professional advisory and mandate platform should separate concerns cleanly: model portfolios define investment intent; portfolio snapshots capture current reality; analytics calculate drift and risk; proposal engine generates actions; suitability engine validates client-product-portfolio fit; workflow captures approvals and consent; order integration executes; post-trade monitoring confirms outcome. The entire chain must be versioned, auditable and reproducible."

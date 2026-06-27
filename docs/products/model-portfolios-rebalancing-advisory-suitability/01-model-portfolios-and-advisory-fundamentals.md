# 01 - Model Portfolios and Advisory Fundamentals

## 1. What this domain covers

Model portfolios, rebalancing, advisory proposals and suitability workflows sit above the product layer. They answer four business questions:

1. **What should the client hold?**
2. **How far is the current portfolio from the desired target?**
3. **What action should be recommended or executed?**
4. **Is the action suitable, allowed, explainable and properly approved?**

This domain is central to modern wealth management because it links relationship-manager conversations, investment office views, advisory campaigns, discretionary mandates, portfolio-management operations, order management and client reporting.

## 2. Key concepts

| Concept | Meaning |
|---|---|
| Client profile | KYC, risk tolerance, investment objective, knowledge/experience, horizon, liquidity need, restrictions and tax/residency context |
| Risk profile | Firm-approved classification such as conservative, balanced, growth or aggressive |
| Strategic asset allocation | Long-term target allocation aligned to risk profile and objectives |
| Tactical asset allocation | Shorter-term overweight/underweight views from investment committee or CIO office |
| Model portfolio | Reference portfolio or allocation used as a target or guide |
| Mandate | Formal investment strategy agreed with client; may be advisory or discretionary |
| Proposal | Recommended action set presented to client or portfolio manager |
| Drift | Difference between current allocation and target allocation |
| Rebalancing | Adjusting portfolio toward target allocation |
| Suitability | Whether a recommendation fits the client profile and regulatory/firm requirements |
| Appropriateness | Whether a client has sufficient knowledge/experience for a product or service, depending on local rule set |
| Pre-trade checks | Checks performed before order placement or execution |
| Post-trade monitoring | Ongoing check that portfolio remains within mandate, restriction and risk limits |
| Client consent | Client acceptance of advisory recommendation or acknowledgement of risk/disclosure |
| Override | Approved exception to a rule, with reason and authority |

## 3. Advisory versus discretionary portfolio management

| Dimension | Advisory | Discretionary Portfolio Management, DPM |
|---|---|---|
| Decision maker | Client decides after recommendation | Portfolio manager has authority within mandate |
| Client approval per trade | Usually required | Usually not required for each trade |
| Proposal document | Client-facing recommendation | Internal investment action / client reporting output |
| Suitability focus | Recommendation-level and portfolio-level | Mandate-level, strategy-level and ongoing monitoring |
| Restrictions | Client-specific and product-specific | Mandate restrictions, investment guidelines and client restrictions |
| Execution | After client consent | PM-approved rebalance/order cycle |
| Audit | Evidence of advice, disclosures, consent | Evidence of mandate authority, guideline compliance and trade rationale |

A platform should support both, but not mix them. Advisory trades require client decisioning; DPM trades require mandate authority and PM governance.

## 4. Model portfolio types

| Model type | Description | Example |
|---|---|---|
| Asset allocation model | Targets by asset class | 40% equity, 45% fixed income, 10% alternatives, 5% cash |
| Product model | Targets specific funds, ETFs, bonds or securities | 20% Global Equity Fund, 15% Global Bond Fund |
| Sleeve model | Model for a part of portfolio | Core fixed income sleeve, alternatives sleeve |
| Risk-profile model | Model mapped to client risk profile | Conservative, balanced, growth |
| Currency model | Model adjusted to base/reference currency | USD balanced, SGD balanced, EUR income |
| Income model | Designed for yield/cashflow objective | Dividend equity + IG bonds + REITs |
| Thematic model | Thematic exposure | AI, climate transition, healthcare innovation |
| Tactical overlay | CIO tactical tilts | Overweight US equities, underweight duration |
| ESG/sustainable model | Sustainability-filtered allocation | Article/label-specific fund selection, exclusion list |
| Custom HNW model | Bespoke model | Concentrated founder stock + diversifying portfolio |

## 5. What model portfolios are not

A model portfolio is not automatically:

- a client-specific recommendation;
- a guaranteed outcome;
- an executable order list;
- suitable for every client in the same risk band;
- free from liquidity, tax, FX or concentration implications;
- independent of account constraints;
- a substitute for suitability checks.

The platform must convert a model into a client-specific proposal through constraints, suitability, portfolio analytics and execution feasibility.

## 6. Common operating models

### 6.1 Central investment-office model

The CIO/investment office defines strategic and tactical models. Advisors use those models to generate proposals.

Pros:
- consistency;
- governance;
- scalable advisory campaigns;
- clear house view.

Risks:
- models can be applied mechanically;
- local suitability and client restrictions may be missed;
- product availability varies by booking centre.

### 6.2 Advisor-driven proposal model

Advisor chooses investment ideas and target changes for a client.

Pros:
- client-specific;
- flexible;
- useful for HNW/UHNW cases.

Risks:
- inconsistent quality;
- greater suitability and conflict-control burden;
- harder to automate.

### 6.3 DPM mandate model

Portfolio manager manages many client portfolios under model/strategy guidelines.

Pros:
- scalable;
- disciplined;
- strong control against model drift.

Risks:
- operational complexity;
- strict investment guideline monitoring required;
- rebalancing at scale can create liquidity/trading constraints.

### 6.4 Hybrid model

A bank provides model portfolios, but advisors adapt them with client-specific restrictions, legacy holdings, tax, cash needs and product eligibility.

This is common in real wealth management because clients rarely match a model perfectly.

## 7. Why this matters for wealth platforms

A good platform must support:

| Capability | Why it matters |
|---|---|
| Current portfolio analytics | Know what client holds today |
| Target model mapping | Know what client should hold |
| Drift calculation | Know how far portfolio is from target |
| Rebalancing engine | Generate reasonable actions |
| Product eligibility | Avoid products client cannot buy |
| Mandate rules | Ensure compliance with investment guidelines |
| Suitability checks | Ensure recommendations fit client profile |
| Cost/tax/liquidity awareness | Avoid poor implementation |
| Proposal explanation | Help advisor explain rationale |
| Approval workflow | Evidence of client or PM approval |
| Order integration | Convert approved proposal to executable orders |
| Post-trade monitoring | Confirm expected portfolio effect |
| Audit lineage | Explain why each trade was recommended/executed |

## 8. Relationship to earlier product packs

| Product area | How it interacts with this domain |
|---|---|
| Cash/FX | Funding, settlement, cash buffers, FX conversions, liquidity rules |
| Bonds | Duration, credit quality, yield, laddering, maturity profile, callable risk |
| Funds/ETFs | Model implementation, NAV/dealing cycles, fund switches, share classes |
| Equities | Single-name concentration, dividends, tax lots, corporate actions |
| Notes/structured products | Complexity, barrier risk, issuer risk, maturity/lock-in |
| Derivatives | Hedging, overlays, margin, leverage, Greeks, mandate restrictions |
| Private markets | Commitments, unfunded exposure, NAV lag, liquidity limits |
| Loans/collateral | LTV, haircuts, pledged assets, in-flight buying power |
| Insurance/annuities | Protection needs, surrender values, income guarantees |
| Tax reporting | After-tax proposal effects, withholding, realized P&L, tax lots |

## 9. Good mental model

The model portfolio is the **investment intent**.

The client portfolio is the **current reality**.

The proposal is the **bridge**.

Suitability and mandate rules are the **guardrails**.

Orders are the **execution**.

Reporting is the **evidence and feedback loop**.

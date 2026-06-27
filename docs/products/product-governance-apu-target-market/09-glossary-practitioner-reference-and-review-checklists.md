# 09. Glossary, Practitioner Reference and Review Checklists

## 1. Glossary

| Term | Meaning |
|---|---|
| APU | Approved Product Universe; controlled list/rule set of products approved for use |
| Product governance | process for approving, distributing, monitoring and retiring products |
| Product due diligence | structured assessment before product approval |
| Target market | intended client segment/objective/risk/horizon for a product |
| Negative target market | client segment/objective for whom product should not be used |
| Distribution strategy | allowed channels and advice models for the product |
| Suitability | client-specific assessment of whether product/recommendation is appropriate |
| Appropriateness | assessment of client knowledge/experience, often for complex execution-only products |
| Product risk rating | product risk classification used by suitability/reporting |
| Complexity classification | classification of whether payoff/features are simple or complex |
| Liquidity classification | assessment of how easily product can be sold/redeemed |
| Hold-only | existing holdings allowed; new buys blocked |
| Product suspension | temporary or permanent block on product distribution |
| Product review | periodic or event-driven reassessment |
| Disclosure requirement | document/acknowledgement required before transaction |
| Rule version | versioned eligibility/suitability logic |
| Decision audit | record explaining why a product/order was allowed, blocked or escalated |
| Watchlist | heightened monitoring status |
| Value assessment | assessment of cost and client value relative to target market |
| Cross-border restriction | limitation based on client/advisor/product jurisdiction |
| Mandate eligibility | whether product is allowed under portfolio mandate |

## 2. Practitioner Explanation

> Product governance is the control framework that determines which products can be offered, to whom, in which jurisdiction, through which channel, and under what conditions. It sits before advisory and suitability. The Approved Product Universe is the executable form of this governance: it combines product approval status, target market, risk rating, complexity, liquidity, booking-centre eligibility, channel rules, disclosures and review status. A strong wealth platform should treat APU as a versioned eligibility engine, not a static list, so it can explain why a product was available, blocked or escalated at the time of recommendation or order.

## 3. Product governance versus suitability: short answer

Product governance asks:

> Is the product approved and distributable under firm, regulatory and target-market rules?

Suitability asks:

> Is this product/recommendation appropriate for this specific client and portfolio?

Mandate eligibility asks:

> Is this product allowed under this specific investment mandate or model portfolio?

Pre-trade control asks:

> Can this specific order proceed now?

## 4. Core product governance artefacts

| Artefact | Purpose |
|---|---|
| due diligence memo | records assessment |
| approval decision | records committee result |
| target market statement | defines intended and negative market |
| product risk profile | captures risk/complexity/liquidity |
| disclosure package | required product documents |
| APU record | executable product approval record |
| eligibility rules | machine-readable rules |
| review schedule | ongoing due diligence plan |
| exception log | overrides and rationale |
| audit snapshot | historical decision evidence |

## 5. Practitioner Review Checklist

### Before product approval

Ask:

1. What is the product?
2. Who manufactures/issues/manages it?
3. What problem does it solve for clients?
4. What is the payoff/economic exposure?
5. What can go wrong?
6. Who is it suitable for?
7. Who should not buy it?
8. What are the costs and conflicts?
9. Can the platform price, settle, custody and report it?
10. What disclosures are required?
11. What lifecycle events must be supported?
12. How will it be reviewed after launch?

### Before recommendation

Ask:

1. Is product in APU?
2. Is client in target market?
3. Is client outside negative target market?
4. Does client risk profile match product risk?
5. Does client understand product complexity?
6. Does product match objective and horizon?
7. Does it breach concentration or mandate rules?
8. Are disclosures delivered?
9. Are conflicts/costs explained?
10. Is recommendation rationale captured?

### Before order

Ask:

1. Is product status still active?
2. Is order channel allowed?
3. Is booking centre allowed?
4. Is client jurisdiction allowed?
5. Are trade size/lot/denomination valid?
6. Is buying power sufficient?
7. Does pre-trade suitability pass?
8. Is there any restricted-list/sanctions issue?
9. Are required approvals complete?
10. Is decision audit saved?

## 6. Product-type governance highlights

| Product | Governance focus |
|---|---|
| cash/deposits | deposit protection, issuer, currency, rate, tenor |
| bonds | issuer credit, rating, seniority, duration, liquidity, callable/perpetual features |
| notes | issuer risk, payoff complexity, barriers, underliers, secondary liquidity |
| funds | manager due diligence, share class, fees, liquidity, gates, benchmark, style drift |
| equities | restricted list, exchange, liquidity, concentration, corporate actions |
| derivatives | appropriateness, leverage, margin, loss beyond investment, counterparty |
| private markets | investor eligibility, lock-up, capital calls, valuation lag, commitment risk |
| insurance | protection need, fees, surrender charges, insurer strength, beneficiary structure |
| loans/lombard | collateral eligibility, LTV, margin call, leverage suitability |
| real assets | valuation, liquidity, leverage, distribution sustainability |

## 7. High-quality answer for architecture discussion

> I would build product governance as a central, effective-dated service that owns APU status, target market, risk rating, complexity, liquidity, distribution rules, disclosures and review state. Advisory, proposal, suitability, DPM, OMS and reporting systems should consume the same governance profile and rule version. Runtime decisions should store reason codes and data snapshots so we can reconstruct why a product was available or blocked at any historical point. I would avoid hardcoding eligibility in UI or order systems because that creates inconsistent controls and audit gaps.

## 8. Questions an architect should ask stakeholders

### Product team

- What product types are in scope?
- Who approves products?
- What is the review frequency?
- What is the product suspension process?
- What is the source of risk rating?
- How are target markets defined?

### Advisory/compliance

- Which rules are hard stops versus warnings?
- Which products require enhanced disclosure?
- How are client classifications maintained?
- What is the exception process?
- How is execution-only handled?
- How are vulnerable/selected clients handled?

### Operations

- Can the product be settled and custodied?
- What lifecycle events must be processed?
- How are price and valuation sourced?
- What happens if price is stale?
- How are product changes communicated?

### Technology

- Which system is golden source?
- What APIs/events are required?
- How are rules versioned?
- How is historical audit replay supported?
- What happens during source outage?
- How are downstream caches invalidated?

## 9. Red flags

- Product approved through email only.
- APU is a spreadsheet manually uploaded without validation.
- Suitability engine has different product risk rating from reporting.
- Product status is updated in advisory UI but not OMS.
- Complex-product disclosures are not versioned.
- Target market is free text and not machine-readable.
- Review dates are not monitored.
- Overrides are not captured with reason and approver.
- Historical decisions cannot be reconstructed.
- New product type can be booked manually before product governance approval.

## 10. One-line mental model

> Product governance defines the permitted product universe; target market defines the intended audience; suitability determines client fit; mandate rules determine portfolio fit; pre-trade controls enforce the decision; audit lineage proves it later.

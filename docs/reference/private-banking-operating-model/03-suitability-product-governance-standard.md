# Suitability And Product Governance Standard

This standard consolidates the former wiki material on client profiling, risk capacity, knowledge and experience, appropriateness, product gates, restrictions, research-to-recommendation governance, conflicts, client segmentation, vulnerable client handling, and advisory documentation.

Use it when designing suitability checks, advisory proposal workflows, DPM mandate onboarding, product eligibility rules, client profiles, consent capture, product governance, or QA evidence.

## Core Principle

Suitability is not a single score.

Good suitability separates:

1. client objective,
2. risk tolerance,
3. risk capacity,
4. knowledge and experience,
5. investment horizon,
6. liquidity need,
7. concentration,
8. currency exposure,
9. product complexity,
10. costs and conflicts,
11. client restrictions,
12. mandate permissions,
13. tax and reporting considerations,
14. evidence of explanation and consent.

## Client Profile Model

| Profile Dimension | Meaning | Control Use |
|---|---|---|
| Objective | Income, growth, preservation, liquidity, hedging, estate, tax-aware reporting, leverage. | Determines product fit and review narrative. |
| Risk tolerance | Willingness to accept volatility or loss. | Soft input; should not override capacity. |
| Risk capacity | Financial ability to absorb loss without harming needs. | Hard control for downside suitability. |
| Knowledge and experience | Familiarity with asset class, product type, leverage, derivatives, private markets, structured products. | Product complexity gating. |
| Horizon | Expected holding period and cash need timing. | Illiquidity, maturity, lock-up, surrender, and private-market fit. |
| Liquidity need | Required cash buffer and foreseeable obligations. | Avoid over-allocation to locked or gated products. |
| Currency base | Reference currency for wealth, spending, liabilities, and reporting. | FX exposure and hedging decisions. |
| Restrictions | ESG, issuer, sector, product type, jurisdiction, tax, religious, legal, family-office policy. | Pre-trade and portfolio monitoring. |
| Vulnerability or enhanced-care status | Age, dependency, financial literacy, cognitive or communication constraints, legal representative involvement. | Additional explanation, approvals, or product restrictions. |

## Product Governance Model

| Product Dimension | Required Classification |
|---|---|
| Product family and subtype | Clear product taxonomy. |
| Complexity | Non-complex, complex, highly complex, leverage/derivative, illiquid, bespoke. |
| Risk rating | Market, credit, liquidity, leverage, currency, counterparty, issuer, and product-specific risk. |
| Target market | Suitable client type, risk profile, horizon, liquidity need, knowledge and experience. |
| Distribution restrictions | Advisory, DPM, execution-only, professional/institutional, jurisdiction, documentation. |
| Required disclosures | Product risk, issuer risk, liquidity, costs, scenario loss, conflicts, tax/reporting caveats. |
| Source evidence | Product approval, term sheet, factsheet, risk rating, pricing source, restriction list. |
| Lifecycle controls | Review frequency, stale approval, product suspension, downgrade, event monitoring. |

## Suitability Gatekeeper

Use a layered gate:

| Gate | Question | Failure Behavior |
|---|---|---|
| Product permission | Is the product allowed for this channel, jurisdiction, and mandate? | Block. |
| Client risk fit | Does product or portfolio risk fit the client profile and capacity? | Block or require documented exception where policy permits. |
| Knowledge and experience | Does the client understand the product type and complexity? | Block complex product or require appropriateness workflow. |
| Concentration | Does the trade create excessive issuer, sector, currency, product, manager, or strategy concentration? | Warn, block, or require approval. |
| Liquidity | Does the product fit horizon and cash needs? | Block or require liquidity explanation. |
| Costs and conflicts | Are fees, spreads, incentives, issuer relationships, and product bias disclosed? | Block until disclosed. |
| Documentation | Are risk disclosures, product documents, client consent, and approvals present? | Block execution or allocation. |
| Data quality | Are product terms, prices, risk ratings, and restrictions current? | Fail closed or route to exception review. |

## Advisory Recommendation Standard

Every recommendation should answer:

1. What is the recommendation: buy, sell, hold, reduce, switch, hedge, subscribe, redeem, exercise, do nothing?
2. Why is this action relevant now?
3. What client objective does it support?
4. What existing holding, exposure, risk, or cashflow does it affect?
5. What are the main downside scenarios?
6. What are the costs, taxes, liquidity limits, and conflicts?
7. What alternatives were considered?
8. What source evidence supports the product terms and suitability result?
9. What client consent is required?
10. What monitoring or review action follows?

## House View And Research Governance

Research is not automatically a client-safe recommendation.

Before using research in advice:

1. map research idea to approved products or instruments,
2. check target market and product governance status,
3. translate generic view into client-specific rationale,
4. check suitability and concentration,
5. disclose risks and alternatives,
6. define proposal validity window,
7. store recommendation version and evidence.

## Restrictions And Overrides

Restrictions should be modelled explicitly:

| Restriction | Examples |
|---|---|
| Product | No derivatives, no private equity, no structured products. |
| Issuer or counterparty | Restricted issuer, bank exposure cap, sanctioned issuer. |
| Sector or theme | No tobacco, fossil fuel, weapons, crypto, single-country exposure. |
| Currency | Base currency only, hedge non-base currency, no NDFs. |
| Liquidity | Minimum 20 percent liquid assets, no lock-up above 10 percent. |
| Concentration | Single issuer, sector, country, manager, product subtype, family group. |
| Tax or reporting | Documentation missing, reportability constraints, withholding status. |
| Authority | Trustee, executor, company director, family-office user, or POA limitation. |

Overrides require reason, approver, client impact, expiry, and evidence. Some rules should never be overrideable.

## Vulnerable Or Enhanced-Care Clients

Enhanced care may require:

1. simpler product universe,
2. additional explanation,
3. family member or authorized representative involvement where appropriate,
4. supervisor call-back,
5. cooling-off period,
6. smaller trade limits,
7. stronger recordkeeping,
8. periodic profile review,
9. documented capacity-for-loss check.

## QA Checklist

1. Client profile includes tolerance, capacity, knowledge, horizon, liquidity, and restrictions.
2. Product gate uses product risk and complexity, not product name alone.
3. Complex-product gate blocks when knowledge and experience is missing.
4. Concentration check uses post-trade exposure.
5. Advisory proposal stores exact recommendation version and consent.
6. DPM suitability is tested at mandate level and trade compliance level.
7. Research idea cannot become proposal without client-specific fit.
8. Override has owner, reason, expiry, scope, and approval.
9. Stale product risk rating fails closed.
10. Reporting explains why action was suitable and what risks remain.

## Related Guides

Use this standard together with:

1. [`../../products/advisory-mandate-reporting-decision-guide.md`](../../products/advisory-mandate-reporting-decision-guide.md)
2. [`../../products/product-taxonomy-and-vocabulary-guide.md`](../../products/product-taxonomy-and-vocabulary-guide.md)
3. [`../../products/product-payoff-scenario-and-stress-guide.md`](../../products/product-payoff-scenario-and-stress-guide.md)
4. [`../../products/portfolio-exposure-modelling-guide.md`](../../products/portfolio-exposure-modelling-guide.md)
5. [`../../products/wealth-structuring-trusts-and-family-office.md`](../../products/wealth-structuring-trusts-and-family-office.md)

## Disclaimer

This document is for operating-model, platform-design, documentation, reporting, QA, and governance work. It is not investment, legal, tax, regulatory, fiduciary, credit, trading, or client advice.

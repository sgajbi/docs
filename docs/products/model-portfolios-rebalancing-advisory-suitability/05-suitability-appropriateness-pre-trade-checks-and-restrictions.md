# 05 - Suitability, Appropriateness, Pre-trade Checks and Restrictions

## 1. Why suitability matters

Suitability and appropriateness checks protect clients and firms by ensuring that recommendations, products and strategies fit the client profile, service model and regulatory requirements.

For platform design, suitability must be treated as an explicit rule-and-evidence framework, not an informal advisor judgement.

## 2. Suitability versus appropriateness

| Concept | Meaning |
|---|---|
| Suitability | Whether a recommendation or strategy is suitable for a specific client based on profile, objective, risk tolerance, knowledge/experience, financial situation and needs |
| Appropriateness | Whether the client has sufficient knowledge/experience to understand a product or service, often relevant for complex products or execution contexts |
| Eligibility | Whether a client is allowed to access a product due to regulatory, jurisdictional, accreditation, booking-centre or product-governance rules |
| Product governance | Whether the product is approved for sale/distribution to a target market |
| Mandate compliance | Whether proposed action is allowed under investment mandate and restrictions |
| Pre-trade compliance | Checks before order placement/execution |
| Post-trade compliance | Checks after execution and ongoing monitoring |

## 3. Client profile inputs

Minimum client profile fields:

| Field | Purpose |
|---|---|
| client_type | Individual, joint, trust, company, family office, fund |
| domicile/residence | Regulatory and tax context |
| booking_centre | Product availability and local rules |
| risk_profile | Conservative, balanced, growth, etc. |
| investment_objective | Income, growth, preservation, speculation, liquidity |
| time_horizon | Short/medium/long term |
| liquidity_need | Cash needs and emergency reserve |
| financial_situation | Income, net worth, liquid assets, liabilities |
| knowledge_experience | Product experience and sophistication |
| loss_capacity | Ability to absorb loss |
| concentration tolerance | Single issuer/security/sector exposure limits |
| ESG/preferences | Restrictions or sustainability preferences |
| leverage preference | Borrowing/margin/derivative permissions |
| complex product consent | For SIP/complex products where required |
| selected/vulnerable client flag | Additional safeguards if applicable |
| authorised persons | Who can approve transactions |

## 4. Product suitability inputs

Product master should provide:

| Field | Purpose |
|---|---|
| product_risk_rating | Compare to client risk profile |
| product_complexity | Simple/complex/SIP-like classification |
| target_market | Suitable client segments |
| negative_target_market | Clients for whom product is not suitable |
| liquidity_profile | Daily, T+3, monthly, lock-up, illiquid |
| time_horizon | Recommended holding period |
| capital_loss_risk | Partial/total loss possibility |
| leverage_flag | Embedded or explicit leverage |
| derivative_flag | Derivative exposure |
| issuer_credit_risk | Notes, bonds, ETNs |
| FX_risk | Currency exposure |
| concentration_risk | Single issuer/sector/underlying risk |
| minimum_investment | Subscription constraint |
| fees_charges | Upfront, ongoing, exit, embedded |
| distribution_status | Approved/limited/suspended/closed |
| required_documents | KID/PHS/factsheet/prospectus/term sheet |

## 5. Rule categories

| Rule category | Example |
|---|---|
| Client risk profile | Product risk rating must not exceed client profile unless approved |
| Product complexity | Complex product requires knowledge/experience or additional disclosure |
| Product eligibility | Client must be eligible by jurisdiction/client category |
| Investment objective | Income product should align with income objective; speculative product may fail preservation objective |
| Time horizon | Illiquid/private product not suitable for short horizon |
| Liquidity | Client must retain cash/liquid assets after trade |
| Concentration | Single issuer not above firm/client limit |
| Asset allocation | Proposed allocation within model/mandate ranges |
| Leverage | Margin/derivatives/borrowing within approved limits |
| FX | Non-base currency exposure within limit or acknowledged |
| Credit quality | Bond portfolio rating requirements |
| Duration | Fixed-income duration within mandate |
| ESG/exclusion | Restricted sectors/issuers blocked |
| Tax/residency | Product available and documentation complete |
| Minimum order | Trade meets minimum lot/subscription |
| Cost reasonableness | Costs disclosed and not excessive under policy |

## 6. Reasonable-basis, customer-specific and quantitative suitability

A strong framework can separate three layers:

| Layer | Meaning | Platform evidence |
|---|---|---|
| Product-level reasonableness | Product has been understood, approved and has defined target market | Product due diligence, approval status, risk rating, documents |
| Customer-specific suitability | Recommendation fits the specific client | Client profile + portfolio analytics + suitability rules |
| Quantitative suitability | Series of recommendations is not excessive | Turnover, cost, concentration, frequency, strategy-level monitoring |

This is useful even outside the US because it is a clean platform architecture pattern.

## 7. Pre-trade checks

Before order creation/execution, run:

| Check | Description |
|---|---|
| Client status | Active, KYC valid, account not blocked |
| Authorized person | Requestor has authority |
| Product approval | Product approved in booking centre |
| Suitability | Recommendation suitable or override approved |
| Appropriateness | Complex product knowledge/experience confirmed |
| Risk rating | Product risk within client profile |
| Concentration | Post-trade exposure within limit |
| Asset allocation | Post-trade within mandate/model tolerance |
| Liquidity | Sufficient cash/liquid assets remain |
| Buying power | Sufficient available cash/credit/collateral |
| Pledge/collateral | Sale/buy impact on credit lines checked |
| FX funding | FX conversion/settlement currency handled |
| Minimum/lot size | Meets market/product constraints |
| Product documents | Required disclosures delivered |
| Restricted list | Issuer/product/security not blocked |
| Sanctions/AML | Required screening passed if relevant |
| Order duplication | Avoid duplicate proposal/order |
| Validity | Proposal and price still valid |

## 8. Post-trade checks

Post-trade checks should confirm:

- orders executed as expected;
- portfolio updated correctly;
- cash and positions reconcile;
- settlement completed;
- mandate remains within limits;
- concentration did not breach due to partial fills/price movement;
- suitability result remains explainable;
- performance and reporting classify transactions correctly;
- product documents and consent evidence are stored;
- any residual cash/drift is handled.

## 9. Mandate restrictions

Restrictions can be hard or soft.

Examples:

| Restriction | Rule example |
|---|---|
| Asset class | Equity 30-50% |
| Product type | Structured products max 10% |
| Issuer | Single issuer max 10% |
| Security | Single equity max 8% |
| Sector | Technology max 25% |
| Region | Emerging markets max 15% |
| Currency | Non-base FX max 30% |
| Liquidity | Illiquid assets max 20% |
| Credit rating | Sub-investment-grade bonds max 10% |
| Duration | Portfolio duration 3-7 years |
| Derivatives | Hedging only, no speculative derivatives |
| Leverage | Gross exposure max 120% |
| Cash | Minimum cash 3% |
| ESG | No tobacco/controversial weapons |
| Client-specific | Do not buy employer stock |

## 10. Restriction hierarchy

When rules conflict, define precedence.

Recommended order:

1. legal/regulatory prohibition;
2. sanctions/restricted list;
3. client authority/account status;
4. product eligibility/product governance;
5. client-specific hard restriction;
6. mandate hard rule;
7. suitability/appropriateness hard rule;
8. firm risk policy;
9. model target/range;
10. soft preferences/optimization.

## 11. Override framework

Not every warning is a hard stop, but overrides must be governed.

Override fields:

| Field | Purpose |
|---|---|
| override_id | Unique record |
| rule_id | Rule being overridden |
| severity | Warning/blocker |
| reason_code | Client instruction, legacy holding, PM decision, remediation plan |
| free_text_rationale | Explanation |
| approved_by | Supervisor/PM/compliance |
| approval_time | Timestamp |
| expiry | Temporary override expiry |
| evidence | Document/call note/approval |
| post_trade_monitoring | Required follow-up |

Never allow silent overrides.

## 12. Suitability result model

Recommended result statuses:

| Status | Meaning |
|---|---|
| PASS | Recommendation can proceed |
| WARNING | Can proceed with disclosure/acknowledgement |
| OVERRIDE_REQUIRED | Needs approval before proceeding |
| FAIL_BLOCK | Cannot proceed |
| INSUFFICIENT_DATA | Required data missing |
| EXPIRED_PROFILE | Client profile stale |
| NOT_APPLICABLE | Rule not relevant to service/product |

## 13. Suitability by product examples

| Product | Key suitability concerns |
|---|---|
| Equity | volatility, concentration, sector/country risk |
| Bond | issuer credit, duration, callable risk, liquidity |
| Fund | objective, risk rating, fees, share class, liquidity |
| ETF | market risk, tracking error, liquidity, leverage/inverse flag |
| Structured note | complexity, issuer risk, barrier risk, liquidity, worst-case loss |
| Derivative | leverage, margin, Greeks, loss potential, knowledge/experience |
| Private market fund | illiquidity, commitment, capital calls, NAV lag, valuation risk |
| FX | currency risk, settlement and leverage if forward/margin |
| Loan/margin | LTV, margin call, liquidation risk |
| Insurance/annuity | surrender charge, long horizon, guarantee terms, protection fit |

## 14. Suitability in advisory versus DPM

| Aspect | Advisory | DPM |
|---|---|---|
| Recommendation | Client-specific proposal | Mandate strategy/trade action |
| Client approval | Usually required | Not per trade |
| Suitability timing | At proposal and before order | At onboarding, mandate assignment, strategy change and ongoing monitoring |
| Product checks | Per product recommendation | Mandate/product universe plus account-level restrictions |
| Evidence | Advice rationale + client consent | Mandate authority + PM decision + compliance checks |

## 15. Selected/vulnerable clients

Some regimes apply enhanced safeguards for clients who may need additional protection. Platform design should not hardcode only one jurisdiction's definition. Use configurable flags and policy rules.

Possible safeguards:

- extra explanation steps;
- supervisor review;
- pre-transaction call-back;
- audio recording requirement;
- cooling-off period;
- additional risk acknowledgement;
- simplified documentation;
- product complexity restrictions;
- lower concentration thresholds.

## 16. Execution-only caveat

Execution-only does not mean no controls. The platform still needs:

- client authority;
- product eligibility;
- product restrictions;
- appropriateness checks where required;
- complex product warnings;
- market/settlement controls;
- AML/sanctions controls;
- record of client instruction;
- no-advice classification evidence.

## 17. Good test cases

1. Conservative client recommended high-risk structured note: block or override required.
2. Balanced client buying equity fund within risk limit but breaching sector concentration: warning/override.
3. DPM account with no derivatives mandate receives option trade: block.
4. Client lacks complex product knowledge and wants autocallable note: appropriateness fail.
5. Advisor proposes sell of pledged bond: collateral impact check blocks.
6. Client risk profile expired: recommendation cannot proceed.
7. Product approval suspended after proposal but before order: revalidation blocks.
8. Partial client acceptance changes portfolio concentration: re-run checks.
9. Legacy concentrated stock above limit but sell restricted due to lock-up: remediation plan.
10. Private fund subscription with short investment horizon: fail or high-risk approval.

## 18. Summary

Suitability and pre-trade controls are not a single yes/no check. They are a layered framework combining client profile, product governance, portfolio context, mandate restrictions, regulatory safeguards, advisory rationale, approval controls and audit evidence.

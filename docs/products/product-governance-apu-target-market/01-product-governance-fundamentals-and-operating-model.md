# 01. Product Governance Fundamentals and Operating Model

## 1. What product governance means

Product governance is the set of policies, processes, data controls and system controls used to ensure that financial products are designed, approved, distributed, monitored and retired in a way that is fair, suitable, controlled and explainable.

In a wealth-management context, product governance sits before advisory and trading. It decides whether a product is allowed to enter the bank's distribution ecosystem and the conditions under which it may be used.

Product governance is not only a compliance process. It has direct impact on:

- client outcomes,
- advisor behaviour,
- suitability rules,
- mandate construction,
- order routing,
- client reporting,
- portfolio analytics,
- operational risk,
- revenue conflicts,
- reputational risk,
- audit evidence.

## 2. Why product governance matters

Financial products can be simple or highly complex. A plain cash deposit, an investment-grade government bond, a single listed equity, a worst-of autocallable note, a private credit fund and a leveraged FX derivative all behave differently.

Without strong product governance:

- advisors may recommend products to unsuitable clients,
- complex products may be sold without adequate client understanding,
- products may be distributed outside their intended target market,
- higher-fee share classes may be used when lower-cost share classes are available,
- unapproved products may enter portfolios through manual processes,
- cross-border restrictions may be breached,
- product risk ratings may become stale,
- liquidity restrictions may not be reflected in mandate rules,
- reporting may misrepresent product risk or liquidity,
- lifecycle events may not trigger product review or client action.

## 3. Product governance versus suitability

Product governance and suitability are related but not the same.

| Concept | Question answered | Level |
|---|---|---|
| Product governance | Is this product approved, for whom, where, and under what conditions? | Product / market / channel level |
| Target market | What client type is the product intended for? | Product-to-segment level |
| Suitability | Is this product suitable for this specific client now? | Client-specific level |
| Mandate eligibility | Is this product allowed under this portfolio mandate? | Portfolio / strategy level |
| Pre-trade control | Can this specific order proceed? | Transaction level |
| Post-trade monitoring | Does the client remain appropriately exposed after trade or market movement? | Portfolio lifecycle level |

A product may be approved but not suitable for a particular client.

A product may be suitable for a client but not allowed under a specific DPM mandate.

A product may be allowed in a mandate but blocked in a booking centre because of local regulatory restrictions.

## 4. Product governance versus product master

Product master is data. Product governance is decision-making plus control.

| Area | Product master | Product governance |
|---|---|---|
| Instrument identity | ISIN, name, issuer, currency | Whether instrument is approved |
| Classification | asset class, product type | complexity, risk, target market |
| Market data | price, rating, liquidity | valuation-source policy and review frequency |
| Lifecycle | maturity, coupon, corporate actions | review triggers and event governance |
| Distribution | exchange, listing, channel | channel eligibility and jurisdiction rules |
| Suitability | product attributes | suitability control inputs |

A mature platform links product master and governance but keeps governance decisions versioned and auditable.

## 5. Product governance operating model

A robust operating model usually includes:

| Function | Responsibility |
|---|---|
| Product manufacturer / structurer | defines product economics, risks, costs and target market |
| Product due diligence team | performs independent assessment before approval |
| Investment committee / product committee | approves, rejects, suspends or restricts products |
| Legal and compliance | validates regulatory, disclosure and cross-border constraints |
| Risk management | assesses market, credit, liquidity, operational and concentration risks |
| Tax team | assesses withholding, reporting and cross-border tax considerations |
| Operations | confirms settlement, custody, lifecycle and reconciliation feasibility |
| Technology / data | implements product data, rules, controls, lineage and reporting |
| Front office / advisory | uses approved products within suitability and mandate constraints |
| Post-sales supervision | monitors recommendation quality and client outcomes |

## 6. Typical product governance lifecycle

```text
Product idea / sourcing
    ->
Initial eligibility screening
    ->
Product due diligence
    ->
Risk, legal, compliance, tax and operations review
    ->
Target market and distribution strategy definition
    ->
Product committee approval / rejection / conditional approval
    ->
APU publication and system activation
    ->
Advisory and order controls consume APU/rules
    ->
Ongoing product monitoring
    ->
Periodic review / event-driven review
    ->
Continue / restrict / suspend / close / exit
```

## 7. Product governance outputs

A product governance process should produce structured outputs, not only committee minutes.

Key outputs include:

- approved product record,
- approval date and expiry/review date,
- approval status,
- approved client segments,
- negative target market,
- booking-centre eligibility,
- jurisdiction eligibility,
- product risk rating,
- complexity flag,
- liquidity category,
- leverage flag,
- capital protection classification,
- issuer/manager approval status,
- allowed channels,
- advisory model eligibility,
- execution-only eligibility,
- DPM eligibility,
- minimum client classification,
- required disclosures,
- knowledge/experience requirements,
- concentration limits,
- holding limits,
- minimum ticket size,
- cooling-off or call-back requirements where applicable,
- review trigger list,
- data owner and control owner.

## 8. Product governance statuses

A clean status model is essential.

| Status | Meaning | Trading behaviour |
|---|---|---|
| Draft | Product is being assessed | not tradable |
| Under Review | Due diligence in progress | not tradable unless exception |
| Conditionally Approved | Approved subject to restrictions | tradable only if conditions met |
| Approved | Fully approved within defined scope | tradable based on rules |
| Approved for Hold Only | existing holdings may remain but new buys blocked | sell/hold only |
| Restricted | approval narrowed due to risk/event | limited trading |
| Suspended | temporarily blocked | no new purchases; sells may be allowed |
| Closed to New Money | no new subscriptions | redemptions/sells allowed |
| Matured / Expired | lifecycle complete | no trade except clean-up |
| Withdrawn | removed from distribution | usually sell/hold only |
| Terminated | product no longer active | closed or migration-only |

## 9. Product governance for manufacturer versus distributor

Many banks act as distributors of third-party products, and sometimes as manufacturers of proprietary products.

| Role | Governance focus |
|---|---|
| Manufacturer | product design, target market, scenario testing, value assessment, disclosure, lifecycle review |
| Distributor | due diligence, target-market compatibility, client-segment mapping, suitability, distribution controls, post-sale monitoring |
| Platform provider | data, controls, lineage, event processing, rule enforcement, audit evidence |
| Advisor | client-specific recommendation and explanation |

For platform design, do not assume the bank always owns the product design. Third-party funds, structured notes, ETFs, insurance products and private funds often require ingestion of external documents and terms, followed by internal product governance assessment.

## 10. What can go wrong

Common failure patterns:

| Failure | Example |
|---|---|
| Product approved but attributes incomplete | suitability engine cannot detect leverage or complexity |
| Approved in one country but sold in another | cross-border breach |
| Risk rating stale | bond downgraded but still treated as investment grade |
| Share class mismatch | retail client placed into higher-fee or wrong-currency share class |
| Disclosure missing | order placed without updated KID/termsheet |
| Product suspended but cached as approved | stale APU feed allows trade |
| Negative target market ignored | product sold to clients explicitly excluded |
| Manual order bypass | advisor books product outside platform controls |
| Lifecycle event not reviewed | fund gate, issuer downgrade or note barrier breach not escalated |
| No audit lineage | bank cannot explain why trade was allowed at the time |

## 11. Design principle for high-quality platforms

Product governance should be treated as a time-versioned decision graph:

```text
Product + jurisdiction + client segment + channel + advice model + portfolio mandate + date
    -> eligibility decision
    -> required controls
    -> evidence package
```

This allows the platform to reconstruct historical decisions.

The question should not be only "is product approved today?".

The better question is:

> Was this product approved for this client, in this booking centre, through this channel, under this advisory model, under this portfolio mandate, at the time of recommendation/order, and were all required disclosures and controls completed?

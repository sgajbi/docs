# 05. Product Lifecycle Review, Suspension, Exit and Change Governance

## 1. Product governance does not end at approval

A product can change after approval. Market conditions, issuer quality, fund liquidity, regulations, product documents, manager teams, valuation quality and client outcomes can deteriorate.

Therefore product governance must include ongoing monitoring and lifecycle review.

## 2. Product lifecycle states

| State | Meaning |
|---|---|
| proposed | product idea received |
| under due diligence | assessment in progress |
| approved | product is available within defined scope |
| restricted | product available with narrowed conditions |
| watchlist | product requires heightened monitoring |
| hold only | existing holdings allowed, no new buys |
| suspended | new transactions blocked pending review |
| closed to new money | no new subscriptions/purchases |
| redeem/sell recommended | clients may be advised to exit |
| matured/expired | product naturally ended |
| terminated/withdrawn | product removed from universe |

## 3. Periodic review

Periodic review frequency should depend on product type and risk.

| Product type | Suggested review logic |
|---|---|
| deposits / simple cash products | annual or when issuer/terms change |
| investment-grade bonds | rating/event-driven plus periodic |
| high-yield/perpetual/subordinated bonds | more frequent credit review |
| funds | annual due diligence plus manager/event review |
| ETFs | periodic index/liquidity/cost review |
| structured notes | issue approval plus lifecycle monitoring until maturity |
| derivatives | counterparty, margin and exposure review |
| private markets | quarterly/annual NAV and manager review |
| insurance/annuities | product terms, insurer strength and suitability review |
| DPM model universe | investment committee review cycle |

## 4. Event-driven review triggers

Product review should be triggered by material events.

### 4.1 Issuer / counterparty events

- rating downgrade,
- default / credit event,
- widening credit spread,
- regulatory sanction,
- capital adequacy concern,
- merger/acquisition,
- restructuring,
- litigation,
- guarantee change.

### 4.2 Fund manager events

- portfolio manager departure,
- change in investment process,
- fund underperformance,
- style drift,
- fund outflows,
- gates/suspension,
- NAV error,
- auditor resignation,
- administrator/custodian change,
- regulatory investigation,
- fund merger/liquidation.

### 4.3 Structured product events

- barrier breach,
- autocall trigger,
- coupon missed,
- issuer downgrade,
- valuation source unavailable,
- secondary market withdrawn,
- product document amendment,
- underlier corporate action,
- underlier suspended/delisted.

### 4.4 Market events

- market closure,
- trading halt,
- exchange suspension,
- liquidity shock,
- volatility spike,
- interest-rate shock,
- FX convertibility restriction,
- sanctions event,
- capital control.

### 4.5 Regulatory / policy events

- local offering rules changed,
- complex-product classification changed,
- disclosure document expired,
- cross-border rule updated,
- tax treatment changed,
- product reclassified for investor eligibility.

## 5. Watchlist framework

A watchlist status allows heightened monitoring without immediate suspension.

Example watchlist reasons:

| Reason | Possible response |
|---|---|
| bond rating outlook negative | allow hold/buy with warning or restrict new buys |
| fund underperformance | monitor, review manager, block model portfolio addition |
| structured note issuer spread widening | restrict new subscriptions |
| ETF liquidity deteriorating | limit order size or channel |
| private fund delayed NAV | reporting caveat and review |
| product complaints increasing | conduct review |

## 6. Suspension framework

Suspension should be available at multiple levels:

| Suspension scope | Example |
|---|---|
| global product | product blocked everywhere |
| booking-centre level | blocked in SG but allowed elsewhere |
| client-segment level | retail blocked, professional allowed |
| channel level | online blocked, advisory allowed |
| transaction type | buy blocked, sell allowed |
| mandate level | blocked from DPM model portfolios |
| advisor group | restricted to specialist desk only |

## 7. Hold-only and sell-only treatment

Many governance decisions should not force immediate liquidation.

| Status | Buy | Sell | Hold | Notes |
|---|---:|---:|---:|---|
| Approved | Yes | Yes | Yes | normal |
| Hold only | No | Yes | Yes | existing clients may retain |
| Sell only | No | Yes | No new holds expected | exit encouraged |
| Suspended | No | Usually yes | Yes | depends on product issue |
| Matured | No | No | No | cleanup only |
| Delisted | No | special process | maybe | needs event handling |

## 8. Product exit governance

Exit governance is needed when a product is no longer suitable for the platform.

Exit triggers:

- product no longer meets due-diligence standard,
- manager removed,
- issuer credit deteriorated,
- product closed/merged/liquidated,
- regulatory restriction,
- product risk changed materially,
- product no longer supports target market,
- better/cheaper share class available,
- data/valuation impossible to support,
- distribution agreement terminated.

Exit plan should define:

- impacted clients/accounts,
- current holdings and market value,
- liquidity/exit feasibility,
- client communication needs,
- advisor action list,
- alternatives/substitutes,
- tax/cost consequences,
- order handling,
- reporting treatment,
- residual position handling,
- exception approval.

## 9. Product change governance

A product may change while retaining same identifier.

Examples:

- fund investment objective changes,
- fund manager changes,
- share class fee changes,
- benchmark changes,
- ETF index changes,
- bond terms amended,
- issuer merges,
- structured note underlier adjusted,
- insurance policy charges updated,
- private fund term extended.

System design must capture:

- old attributes,
- new attributes,
- effective date,
- approval status after change,
- client impact assessment,
- disclosure requirement,
- whether client consent is required,
- whether product remains in APU.

## 10. Lifecycle event-to-governance mapping

| Event | Governance action |
|---|---|
| fund gate | suspend new buys, review liquidity classification |
| bond downgrade below investment grade | update risk rating, review mandate eligibility |
| issuer default | block buys, update valuation, client notification |
| structured note barrier breached | update product status, review reporting and risk warnings |
| ETF delisted | block buys, prepare exit/migration plan |
| private fund capital call default risk | client/advisor alert, liquidity review |
| regulatory document expired | block new sales until document updated |
| product document amended | review target market and disclosure |

## 11. Product review committee artefacts

Every review should produce:

- review date,
- review type: periodic/event-driven,
- trigger reason,
- current product status,
- current risk/complexity/liquidity rating,
- distribution data,
- client complaints/incidents,
- performance/liquidity/valuation observations,
- proposed action,
- final decision,
- approval owner,
- effective date,
- communication actions,
- rule changes,
- downstream system update evidence.

## 12. Platform requirements

A product governance platform should support:

- effective-dated statuses,
- future-dated changes,
- emergency suspension,
- watchlist flags,
- partial restrictions by channel/booking centre/client segment,
- impacted-holding identification,
- client/advisor notification lists,
- rule-publishing workflow,
- post-decision validation,
- audit trail,
- dashboards for overdue reviews.

## 13. Overdue review control

A product whose review has expired should not remain fully tradable by accident.

Recommended behaviour:

| Review status | Suggested trading behaviour |
|---|---|
| review current | normal |
| review due soon | alert product owner |
| review overdue | restrict new buys or require exception |
| review materially overdue | suspend new buys |
| critical field stale | block new buys |

## 14. Audit reconstruction

For any historical trade, the platform should reconstruct:

- product approval status on trade date,
- target market on trade date,
- client classification on trade date,
- risk rating on trade date,
- disclosure version delivered,
- suitability rule version,
- advisor recommendation rationale,
- order approval result,
- any overrides/exceptions.

This is essential for complaints, remediation, regulatory reviews and internal audit.

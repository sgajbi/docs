# 08. Platform Controls, Reconciliation, QA and Audit Scenarios

## 1. Control objectives

A product governance platform should ensure:

- only approved products are available for recommendation/order,
- restrictions are enforced consistently across channels,
- critical product attributes are complete and current,
- risk/complexity/liquidity classifications feed downstream systems,
- disclosures are delivered before transaction when required,
- exceptions are controlled,
- historical decisions can be reconstructed,
- product-review obligations are not missed,
- product changes are propagated to all consumers.

## 2. Control catalogue

| Control | Description |
|---|---|
| Product activation control | no APU activation without mandatory fields |
| Maker-checker control | product governance data reviewed before publish |
| Rule version control | rule changes are versioned and approved |
| Review-date control | product restricted/suspended if review overdue |
| Disclosure expiry control | new buys blocked if document expired |
| Suspended product control | buys blocked across all channels |
| Hold-only control | sells allowed, buys blocked |
| Channel eligibility control | product only appears in allowed channels |
| Cross-border control | client/advisor/booking restrictions enforced |
| Mandate eligibility control | DPM/proposal respects product universe |
| Override control | exception approvals captured and time-limited |
| Audit replay control | historical decisions can be reproduced |

## 3. Reconciliation controls

Product governance data should reconcile across systems.

| Reconciliation | Expected outcome |
|---|---|
| APU master vs advisory platform | same product status and restrictions |
| APU master vs OMS | suspended products blocked in order system |
| APU master vs reporting | risk/liquidity/complexity shown consistently |
| Product master vs governance | every active APU product has product master data |
| Governance vs disclosure store | required documents exist and are current |
| Governance vs pricing | valuation source exists for active products |
| Governance vs holdings | clients holding restricted/suspended products identified |
| Rule engine vs approved rule set | no unapproved local rules |
| Current rules vs historical decisions | audit replay matches original outcomes |

## 4. Data quality checks

### 4.1 Completeness

Critical fields should not be null for approved products:

- product ID,
- product type,
- governance status,
- approval date,
- risk rating,
- complexity level,
- liquidity category,
- target market,
- booking centre eligibility,
- channel eligibility,
- disclosure requirement,
- next review date,
- valuation source,
- approval owner.

### 4.2 Validity

Examples:

- next review date must be after approval date,
- approved product cannot have empty target market,
- suspended product cannot be available for new buy,
- complex product must have disclosure requirement,
- private fund cannot have daily liquidity unless documented,
- matured product cannot remain buy-eligible,
- hold-only product cannot appear in proposal generation.

### 4.3 Consistency

Examples:

- risk rating in reporting equals risk rating in suitability,
- booking-centre eligibility matches cross-border rule matrix,
- product status in UI matches OMS decision,
- product classification matches tax/reporting classification,
- share-class restrictions align with fund product attributes.

### 4.4 Timeliness

Examples:

- product status changes published within SLA,
- issuer downgrade triggers review within SLA,
- fund gate triggers product restriction within SLA,
- expired disclosure blocks new trades immediately.

## 5. QA scenarios

### Scenario 1: Product not approved

**Given** product is in product master but not in APU.
**When** advisor tries to add it to a proposal.
**Then** system blocks with `PRODUCT_NOT_APPROVED`.

### Scenario 2: Product approved in SG but not HK

**Given** product is approved for SG booking centre only.
**When** HK account attempts buy.
**Then** system blocks with `JURISDICTION_NOT_ALLOWED`.

### Scenario 3: Retail client attempts complex note

**Given** product is highly complex and retail clients are in negative target market.
**When** retail client attempts buy.
**Then** system blocks with `CLIENT_SEGMENT_NOT_ALLOWED` and `NEGATIVE_TARGET_MARKET_MATCH`.

### Scenario 4: Disclosure expired

**Given** product requires KID/PHS and document expiry date is past.
**When** advisor creates order.
**Then** system blocks with `DISCLOSURE_EXPIRED`.

### Scenario 5: Product hold-only

**Given** product status is hold-only.
**When** client sells existing position.
**Then** sell is allowed.
**When** client attempts new buy.
**Then** buy is blocked.

### Scenario 6: Risk rating changed after downgrade

**Given** bond risk rating changes from medium to high after downgrade.
**When** portfolio mandate allows only medium-risk bonds.
**Then** system raises mandate monitoring alert.

### Scenario 7: Fund gate event

**Given** fund redemptions are gated.
**When** product event is ingested.
**Then** product liquidity status changes and new buys are suspended pending review.

### Scenario 8: Advisor override

**Given** soft warning on concentration limit near breach.
**When** supervisor approves exception.
**Then** order proceeds and override evidence is linked to decision audit.

### Scenario 9: Hard-stop override rejected

**Given** product is legally not offerable to client jurisdiction.
**When** advisor requests override.
**Then** system rejects override as non-overrideable.

### Scenario 10: Historical audit replay

**Given** a trade from 2025 is reviewed in 2026.
**When** audit replay is run with as-of date equal to trade date.
**Then** product status, rule version, disclosure version and client classification are reconstructed as of trade date.

## 6. Regression test matrix by product type

| Product type | Must test |
|---|---|
| bonds | rating downgrade, callable/perpetual complexity, issuer concentration |
| notes | barrier event, issuer downgrade, disclosure requirement, hold-only status |
| funds | share class, fund gate, manager change, TER/share-class restrictions |
| equities | restricted list, sanctions, delisting, single-stock concentration |
| derivatives | knowledge/experience, margin approval, loss beyond investment |
| private markets | investor eligibility, capital calls, lock-up, unfunded exposure |
| insurance/annuities | surrender charges, policy owner/insured/beneficiary, premium financing |
| cash/FX | deposit protection, FX product appropriateness, DCI alternate currency |

## 7. Operational dashboards

A product governance dashboard should show:

- products by status,
- products pending approval,
- products with overdue review,
- products with missing critical fields,
- products with expired disclosures,
- products under watchlist,
- suspended products with client holdings,
- products sold outside target market,
- manual overrides by advisor/team,
- top products by complaints/incidents,
- top product concentrations by client segment,
- products with stale valuation source.

## 8. Audit evidence package

For each recommendation/order, retain:

- product governance snapshot,
- target market snapshot,
- client classification snapshot,
- suitability result,
- mandate result,
- pre-trade result,
- disclosure delivery/acknowledgement,
- advisor rationale,
- client consent,
- override approvals,
- rule version,
- order and execution details,
- post-trade alerts.

## 9. Common production incidents

| Incident | Root cause |
|---|---|
| suspended product still tradable | stale cache or missing event propagation |
| product appears in proposal but order blocked | advisory and OMS use different rule versions |
| client report shows wrong risk rating | reporting consumes product master but not governance profile |
| disclosure warning missing | document store not linked to APU rules |
| cross-border block too late | eligibility checked only at order stage, not proposal stage |
| review overdue unnoticed | no product review scheduler/dashboard |
| manual products bypass APU | free-text or local security creation path lacks control |
| wrong share class recommended | product alternatives/share-class hierarchy not modelled |

## 10. Non-functional requirements

Product governance services should be:

- highly available for order/pre-trade decisions,
- low-latency for advisor UI,
- deterministic and explainable,
- versioned and auditable,
- configurable by rule owners,
- resilient to stale downstream caches,
- integrated with product master and document store,
- event-driven for product status changes,
- permission-controlled for data edits,
- testable with golden scenarios.

## 11. Golden-source principle

There should be one accountable golden source for product governance decisions.

Downstream systems may cache for performance, but they must:

- subscribe to change events,
- validate version freshness,
- show governance decision timestamp,
- fail safe when critical data is unavailable,
- support audit replay.

Fail-safe design is preferable: if critical product governance data is unavailable, block new product recommendations/orders rather than allow uncontrolled trading.

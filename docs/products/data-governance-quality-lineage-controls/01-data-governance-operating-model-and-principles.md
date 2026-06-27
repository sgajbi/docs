# 01 - Data Governance Operating Model and Principles

## 1. Executive summary

Data governance is the operating model that ensures critical data is defined, owned, controlled, traceable and fit for use. In a wealth platform, governance must cover more than reference data. It must cover instruments, prices, FX rates, curves, corporate actions, transactions, positions, cash, pledges, collateral, credit lines, mandates, benchmarks, performance, risk, suitability, tax/reporting attributes and client-facing reports.

A practical wealth-platform data governance model answers five questions:

| Question | Example |
|---|---|
| What does the data mean? | Is `market_value` clean, dirty, accrued-inclusive, base-currency or local-currency? |
| Who owns it? | Is the bond coupon owned by instrument master, custodian, issuer feed or product-control team? |
| Where does it come from? | Bloomberg, custodian, core banking, accounting book, valuation engine, OMS, manual override? |
| How do we know it is correct? | Reconciliations, validations, thresholds, peer checks, independent price checks, lineage. |
| What happens when it is not correct? | Block report, degrade analytics, show exception, exclude from mandate check, escalate to owner. |

## 2. Governance scope in wealth platforms

Governance must apply to both operational and analytical data.

| Data domain | Examples | Why it matters |
|---|---|---|
| Client and account data | CIF, BR, portfolio, booking centre, risk profile, mandate, tax residency | Suitability, access control, reporting hierarchy, cross-border restrictions. |
| Product master | Asset class, subtype, issuer, maturity, coupon, barrier, fund class, REIT classification | Valuation, risk, reporting, product governance, suitability. |
| Market/reference data | Prices, FX, curves, benchmark levels, ratings, sector, country, liquidity, corporate actions | Valuation, P&L, performance, exposure, concentration, collateral. |
| Transactions | Trades, subscriptions, redemptions, coupons, fees, transfers, capital calls, corporate actions | Position, cash, lot, accounting, performance and client statement correctness. |
| Positions and balances | Quantity, nominal, cost, market value, accrued income, cash available, settled/unsettled | Portfolio view, buying power, risk and reporting. |
| Analytics | TWR, MWR, contribution, attribution, risk, VaR, drawdown, benchmark-relative metrics | Advisory and portfolio review decisions. |
| Control data | Reconciliation results, overrides, sign-offs, data-quality rules, lineage and run status | Audit, incident diagnosis and production confidence. |

## 3. Governance objectives

A good data governance model should:

- create one agreed vocabulary for product, position, transaction, lifecycle and reporting concepts;
- make source ownership explicit;
- reduce ambiguity between source systems, consuming systems and calculation engines;
- detect defects early, ideally before they reach reports or advisory workflows;
- support traceability from client report line back to source transactions and prices;
- create risk-based controls rather than checking everything equally;
- enable faster incident triage and lower operational dependency on tribal knowledge;
- support migration, replatforming, audit and regulatory evidence.

## 4. Governance principles for private-banking platforms

### Principle 1 - Define critical data elements

A critical data element is a field whose incorrect value can materially affect client, advisor, mandate, risk, accounting, settlement, tax, regulatory, reporting or control outcomes.

Examples:

| CDE | Impact if wrong |
|---|---|
| Instrument asset class | Wrong allocation, suitability, risk and reporting category. |
| Note barrier status | Wrong payoff, risk and client explanation. |
| Bond maturity date | Wrong yield, duration, income projection and redemption lifecycle. |
| Fund NAV date | Wrong valuation freshness and performance. |
| FX rate source/date | Wrong base-currency market value and return. |
| Transaction cashflow classification | Wrong TWR/MWR and client cashflow reporting. |
| Pledge link | Wrong collateral availability and buying power. |
| Client risk profile | Wrong suitability recommendation. |
| Benchmark link | Wrong relative performance and mandate review. |

### Principle 2 - Separate legal holding, economic exposure and reporting classification

The same economic idea can appear differently in legal, risk and reporting views.

| Example | Legal holding | Economic exposure | Reporting classification |
|---|---|---|---|
| Structured note linked to Apple | Note issued by bank | Apple downside + issuer risk | Structured product / equity-linked note |
| ETF tracking S&P 500 | Fund/ETF unit | U.S. equity index | Equity fund / ETF |
| FX forward | OTC derivative contract | Long one currency, short another | Derivative / FX exposure |
| Private equity fund | Fund interest | Private company portfolio | Alternatives / private equity |

The governance model must allow all three views without forcing one classification to serve every purpose.

### Principle 3 - Source ownership must be explicit

Every controlled field should have a source owner.

| Field | Likely source owner |
|---|---|
| Executed trade price | OMS / broker execution feed |
| Settled cash | Core banking / custodian cash book |
| Security quantity | Custodian / accounting book |
| Issuer and ISIN | Instrument master / reference data utility |
| Market price | Market-data service / valuation source |
| Fund NAV | Fund administrator / transfer agent / data vendor |
| Client suitability profile | CRM / advisory platform |
| DPM mandate rules | Mandate management platform |
| Performance return | Performance engine |
| Client report snapshot | Reporting platform |

Source ownership does not mean the source is always correct. It means the source is accountable for definition, delivery, correction and sign-off.

### Principle 4 - Controls must be close to the data lifecycle

Controls should exist at multiple layers.

| Layer | Control examples |
|---|---|
| Ingestion | Schema validation, mandatory fields, duplicate detection, file completeness, event idempotency. |
| Transformation | Unit conversion, currency conversion, decimal precision, valid code mapping, effective dating. |
| Storage | Referential integrity, versioning, immutability for events, audit trail. |
| Calculation | Recalculation lineage, dependency tracking, reason codes, reproducibility. |
| Consumption | API freshness indicators, degraded-state flags, report exception panels. |
| Operations | Break queues, SLA dashboards, escalation, certification and closure evidence. |

### Principle 5 - Materiality and criticality drive control intensity

Not every defect is equally important. A stale price on a tiny non-mandate holding has different urgency from a missing price on a large pledged collateral position.

Control severity should consider:

- client impact;
- market value impact;
- product complexity;
- mandate or suitability impact;
- report visibility;
- regulatory or tax relevance;
- liquidity or collateral relevance;
- operational time sensitivity;
- whether the issue can compound downstream.

## 5. Roles and responsibilities

| Role | Responsibility |
|---|---|
| Data owner | Senior accountable owner for a domain such as instrument master, transactions, pricing or reporting. |
| Data steward | Operational owner who defines rules, triages issues, approves corrections and monitors quality. |
| Source-system owner | Owns source delivery, SLA, schema changes, upstream fixes and feed reliability. |
| Consumer owner | Defines downstream usage, tolerance, materiality and impact of data defects. |
| Product owner | Prioritises remediation, product capability and reporting behaviour. |
| Architect | Defines control architecture, source boundaries, lineage and integration design. |
| Engineering | Implements validation, lineage, reconciliation, observability and APIs. |
| QA | Converts rules and incidents into regression scenarios. |
| Operations | Monitors breaks, performs reconciliations, follows runbooks, records closure evidence. |
| Risk/compliance | Reviews regulatory, conduct, suitability, model and client-impact implications. |

## 6. Governance forum design

A practical forum structure:

| Forum | Frequency | Focus |
|---|---:|---|
| Daily data-quality operations | Daily | Failed feeds, stale prices, breaks, client-report blockers. |
| Product/source triage | 2-3 times per week | Source defects, mapping gaps, edge cases, high-priority defects. |
| Data governance council | Monthly | CDEs, ownership, standards, scorecards, recurring breaks, policy decisions. |
| Change review | Per release | Source schema changes, new product types, new rules, migration impacts. |
| Post-incident review | After incident | Root cause, control gaps, detection gap, recurrence prevention. |

## 7. Governance artefacts

| Artefact | Description |
|---|---|
| Data dictionary | Business definitions, data type, precision, nullability, examples. |
| CDE inventory | Critical fields, owners, rules, downstream consumers and severity. |
| Source ownership matrix | Authoritative source and fallback source by domain and field. |
| Data contract | Schema, semantics, SLA, quality expectations, change notification rules. |
| Lineage map | Source-to-report dependency and transformations. |
| DQ rule catalogue | Validations, thresholds, severity and remediation owner. |
| Reconciliation matrix | What is reconciled, between which systems, how often and with what tolerance. |
| Exception taxonomy | Break categories, root-cause categories, severity and ageing. |
| Data quality scorecard | Quality metrics by domain, source, product and report. |
| Certification evidence | Run completion, sign-off, overrides and release/migration approvals. |

## 8. Three-lines-of-defence view

| Line | Data-governance responsibility |
|---|---|
| First line | Owns data production, daily controls, remediation and operational certification. |
| Second line | Defines policy, monitors compliance, reviews risk and validates control design. |
| Third line | Independently audits governance, evidence, control effectiveness and remediation. |

## 9. Common anti-patterns

| Anti-pattern | Consequence |
|---|---|
| "Consumer should clean it" | Multiple consumers implement inconsistent fixes. |
| "Manual override without expiry" | Permanent hidden data pollution. |
| "No data owner for derived fields" | Calculation teams and source teams blame each other. |
| "Reconciliation only at total market value" | Offsetting breaks remain hidden. |
| "Reports hide stale data" | Client and advisor trust is damaged when issue is found later. |
| "Migration sign-off by count only" | Calculations, classifications and report labels remain wrong. |
| "All fields are critical" | Teams cannot prioritise real client-impacting defects. |
| "Lineage in diagrams only" | During incident, no executable trace from report to source. |

## 10. Practitioner takeaway

A strong governance model does not mean more meetings. It means fewer surprises. The platform should make ownership, lineage, data quality, reconciliation and degraded states visible by design. The best governance model is one where every production issue automatically leaves behind evidence that helps the next release become safer.

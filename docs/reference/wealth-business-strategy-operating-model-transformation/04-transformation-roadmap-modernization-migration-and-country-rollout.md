# 04 - Transformation Roadmap, Modernization, Migration and Country Rollout

## 1. Why roadmap design matters

A transformation roadmap converts target state into phased delivery.

A poor roadmap creates:

- unclear scope
- delivery fatigue
- broken dependencies
- duplicated platforms
- migration risk
- country rollout delays
- weak adoption
- cost overruns
- control gaps

A good roadmap sequences value, risk reduction and architectural modernization.

## 2. Roadmap horizons

| Horizon | Focus |
|---|---|
| 0-3 months | discovery, baseline, quick wins, architecture decisions |
| 3-6 months | foundation, data quality, pilot capability |
| 6-12 months | MVP production rollout, first country/team |
| 12-24 months | multi-country scaling, decommissioning, AI adoption |
| 24+ months | optimization, advanced analytics, enterprise operating model |

## 3. Transformation sequencing patterns

| Pattern | When useful |
|---|---|
| Capability-first | Build shared core capability before rollout |
| Country-first | Roll out country by country |
| Product-family-first | Start with asset class/product scope |
| Channel-first | Start advisor or digital channel |
| Reporting-first | Start with low-risk read-only capability |
| API-first | Build reusable service layer |
| Data-first | Fix sources and lineage before UI |
| AI-assist-first | Use AI for advisor productivity without autonomous decisions |
| Decommission-first | Target cost reduction and legacy exit |

## 4. Wealth roadmap example

```text
Phase 1 - Foundation
- client/account/portfolio master integration
- instrument/product master
- market data and FX
- entitlement model
- audit and lineage

Phase 2 - Read-only analytics
- holdings, valuation, performance, risk
- portfolio review API
- reporting snapshots
- exception panels

Phase 3 - Advisory workflows
- proposal generation
- suitability checks
- product restrictions
- client approval workflow

Phase 4 - DPM and rebalancing
- model portfolios
- mandate monitoring
- drift and trade generation
- pre-trade controls

Phase 5 - AI decision intelligence
- advisor copilot
- product explanation
- performance narrative
- operations triage

Phase 6 - Scale and decommission
- country rollout
- legacy shutdown
- operating model optimization
```

## 5. Migration strategy

Migration must preserve:

- client/account hierarchy
- positions
- transactions
- cost basis
- cash balances
- performance history
- reports and archives
- product mappings
- benchmarks
- mandates
- entitlements
- open orders
- corporate action entitlements
- tax lots
- audit evidence

## 6. Parallel run

Parallel run compares old and new systems.

Compare:

| Area | Example check |
|---|---|
| Holdings | quantity, nominal, cost, MV |
| Cash | currency balances, pending cash |
| Valuation | price, FX, market value |
| Performance | TWR, MWR, contribution |
| Risk | exposure, concentration |
| Reports | totals, labels, footnotes |
| Transactions | lifecycle events and cashflows |
| Mandates | breach/OK status |
| Lending | collateral, LTV, availability |
| Entitlements | access scope |

## 7. Cutover readiness

Before cutover:

- data migration signed off
- breaks below threshold
- critical defects closed
- users trained
- runbooks ready
- support model ready
- rollback criteria defined
- communication sent
- downstream systems ready
- reporting deadlines protected
- regulatory impact assessed
- country sign-off obtained

## 8. Country rollout model

For each country or booking centre:

- regulatory requirements
- product universe
- tax/reporting requirements
- data sources
- market calendars
- currencies
- languages
- advisor workflows
- client statement formats
- data residency
- support hours
- migration volume
- local sign-offs

## 9. Decommissioning

Decommissioning must be planned from the start.

Checklist:

- downstream consumers migrated
- historical data archived
- audit retention met
- operational processes moved
- reconciliations complete
- access revoked
- infrastructure retired
- contracts terminated
- run cost removed
- knowledge transferred

## 10. Key takeaway

A transformation roadmap must deliver value while controlling migration, country, data, reporting and operational risk.

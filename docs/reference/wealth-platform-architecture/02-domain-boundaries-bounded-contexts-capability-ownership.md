# 02 - Domain Boundaries, Bounded Contexts and Capability Ownership

## 1. Why boundaries matter

A wealth platform becomes fragile when every application tries to own everything: client data, product data, prices, transactions, positions, performance, risk and reporting. Clear domain boundaries reduce duplication, improve accountability and make platform change safer.

A bounded context is a domain area where terms, rules, ownership and data models are internally consistent. The same word may mean different things in different contexts, so boundaries must make meaning explicit.

Example:

| Term | In order context | In accounting context | In reporting context |
|---|---|---|---|
| Cash | Available for trade | Ledger balance by currency | Client-facing cash allocation |
| Position | Requested exposure | Settled or trade-date holding | Reported holding at snapshot date |
| Performance | Expected scenario | Return calculation based on valuations and flows | Presented client/advisor result |

## 2. Recommended bounded contexts

| Bounded context | Owns | Does not own |
|---|---|---|
| Party & relationship | Client, entity, beneficial owner, household, advisor relationship, access scope | Product terms, trades, performance calculations |
| Account & portfolio | Accounts, portfolios, mandates, booking centre, service model, portfolio hierarchy | Product classification, market prices |
| Product & instrument | Instrument identity, product taxonomy, product terms, issuer, eligibility, lifecycle terms | Client-specific holdings or advice |
| Market data | Prices, FX, curves, benchmark levels, source hierarchy, quality flags | Portfolio valuation policy decisions beyond data supply |
| Order & execution | Orders, proposals, executions, allocation, order state | Accounting cost basis, official statement snapshots |
| Transaction & ledger | Economic postings, cash/position movements, income, fees, tax, P&L, lots | User-interface proposal design |
| Position & balance | Current and historical positions/cash balances derived from transactions and events | Source trade capture or market price creation |
| Analytics | Performance, attribution, risk, exposure, concentration, mandate analytics | Final PDF layout or client entitlement decisions |
| Advisory & suitability | Recommendations, suitability, disclosures, consent, proposal narrative | Settlement confirmation or custody movements |
| Reporting | Reporting snapshots, sections, layouts, explanations, output generation, archive | Upstream source-data correction |
| Entitlements & workflow | Identity, access, delegation, tasks, approvals, audit interaction history | Product valuation or performance formulas |

## 3. Capability ownership matrix

| Capability | System of record | System of engagement | System of insight |
|---|---|---|---|
| Client identity | Client master / CRM / KYC platform | Advisor workbench / client portal | Household analytics, segmentation |
| Account/portfolio | Core banking / portfolio master | Advisor workbench | Portfolio analytics/reporting |
| Product terms | Product master / issuer feed | Product shelf / proposal tool | Product risk analytics |
| Market price | Market data service | Advisor dashboard | Valuation/risk/performance |
| Order | OMS / advisory order service | Advisor workbench | Order analytics, conversion funnel |
| Transaction | Book-of-record / accounting | Operations console | Cashflow analytics, P&L |
| Position | Position service / custodian feed | Portfolio view | Allocation/risk/performance |
| Performance | Performance engine | Portfolio review | Attribution, composites, advisor review |
| Report | Reporting engine / archive | Client portal / advisor workbench | Usage analytics, QA evidence |

A system of record owns truth. A system of engagement owns user experience. A system of insight owns derived analysis. Confusing these leads to data conflicts.

## 4. Boundary design rules

### Rule 1 - Own one business truth

Each service should own a clear business truth. For example, the product master owns instrument terms; the position service owns derived holdings; the performance engine owns calculation outputs.

### Rule 2 - Do not leak internal persistence models

APIs should expose domain contracts, not database tables.

Poor design:

```text
GET /position_table_rows?portfolio_id=123
```

Better design:

```text
GET /portfolios/{portfolioId}/positions?asOfDate=2026-06-30&basis=tradeDate
```

### Rule 3 - Separate commands and queries

Commands change state. Queries read state.

| Type | Example |
|---|---|
| Command | Create order, approve proposal, amend report, ingest transaction |
| Query | Get holdings, get performance, get report snapshot, get risk summary |

### Rule 4 - Avoid shared database ownership

Multiple services should not directly write the same tables. Share through APIs/events or a controlled data platform.

### Rule 5 - Treat derived data as derived

Positions, performance, risk and reporting are derived from source events, transactions, prices and configuration. Store derived results for performance and reproducibility, but maintain lineage to inputs.

## 5. Common anti-patterns

| Anti-pattern | Why it hurts |
|---|---|
| One "portfolio service" owns everything | Becomes a monolith with unclear responsibility. |
| Reporting queries operational tables directly | Reports become non-reproducible and break when upstream schema changes. |
| UI computes business-critical analytics | Results become inconsistent across channels. |
| Every service stores its own instrument classification | Asset allocation and risk reporting diverge. |
| Events with no contract or version | Consumers break silently. |
| APIs expose database IDs only | Cross-system lineage and reconciliation become hard. |
| No explicit degraded-data state | Users trust incomplete or stale outputs. |
| Service owns data but not quality controls | Breaks are found by clients or downstream teams. |

## 6. Example domain split: positions, transactions and performance

### Transaction service owns

- transaction event identity
- transaction type
- transaction legs
- trade date, settlement date, booking date
- reversal/amendment links
- source lineage
- cash/security economic effect

### Position service owns

- position aggregation rules
- as-of position by portfolio/instrument/currency
- settled vs trade-date basis
- lot linking where required
- corporate-action-adjusted quantity
- position status and historical balances

### Performance engine owns

- cashflow classification
- valuation time series
- sub-period calculation
- TWR/MWR/contribution/attribution logic
- calculation version
- exclusion/degraded-state rules
- result lineage and explainability

Do not make the transaction service calculate official performance. Do not make the performance engine invent missing transactions. Each domain should rely on clear upstream contracts.

## 7. Boundary example: product master vs product governance

| Area | Product master | Product governance/APU |
|---|---|---|
| Instrument identity | Yes | Uses it |
| Terms and static data | Yes | Uses it |
| Product risk rating | May store | Governance owns methodology/approval |
| Target market | No or mirrored | Owns |
| Booking-centre eligibility | No or mirrored | Owns |
| Product suspension | No or mirrored | Owns |
| Client-specific suitability | No | Advisory/suitability owns |

A product can exist in the instrument master but be unavailable for sale due to APU/governance controls.

## 8. Boundary example: advisory vs order management

| Area | Advisory/proposal | Order management |
|---|---|---|
| Investment rationale | Owns | References |
| Suitability outcome | Owns | Must validate before placement |
| Client consent | Owns or workflow owns | Requires evidence |
| Order instruction | Initiates | Owns order state |
| Execution venue | May suggest | Owns execution routing |
| Post-trade reporting | References | Provides execution/settlement state |

A recommended product is not the same as an executed order.

## 9. Capability maturity levels

| Level | Description |
|---|---|
| L0 Manual | Data exchanged manually, spreadsheet reconciliation, no lineage. |
| L1 Integrated | APIs/files move data, but ownership and quality rules are weak. |
| L2 Governed | Clear owner, source, contracts, validation, lineage and reconciliation. |
| L3 Explainable | Outputs have calculation/input lineage, exception reasons and user-facing explanations. |
| L4 Automated controls | Pre/post-trade controls, degraded-state handling, auto-reconciliation, self-healing where safe. |
| L5 Productized platform | New product/country/report variants can be added through configuration and governed extension points. |

## 10. Decision checklist

Before creating or modifying a service, answer:

- What business capability does it own?
- What data does it own versus consume?
- What is its inbound contract?
- What is its outbound contract?
- What events does it emit?
- What APIs does it expose?
- What is its source-of-truth relationship?
- What is its failure mode?
- What is its reconciliation process?
- What report/advisory/mandate outputs depend on it?
- What data-quality rules must be enforced at the boundary?

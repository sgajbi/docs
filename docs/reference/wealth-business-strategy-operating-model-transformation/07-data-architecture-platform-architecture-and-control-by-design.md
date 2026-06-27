# 07 - Data Architecture, Platform Architecture and Control-by-Design

## 1. Why architecture is strategic

Architecture determines whether the platform can scale, adapt and remain controlled.

A good target architecture supports:

- multi-country rollout
- product expansion
- analytics consistency
- reporting reproducibility
- AI grounding
- operational resilience
- secure access
- source lineage
- decommissioning
- cost control

## 2. Architecture principles

| Principle | Meaning |
|---|---|
| Domain ownership | each capability has clear service/data owner |
| API-first | consumers use governed APIs |
| Event-aware | events used for lifecycle and state changes |
| Source-backed | outputs traceable to source systems |
| Reproducible reporting | reports can be regenerated or archived |
| Calculation versioning | analytics methodology is versioned |
| Control-by-design | controls embedded in workflows |
| Observability | systems emit logs, metrics and traces |
| Secure by default | least privilege, encryption, audit |
| Configurable localization | country differences are explicit |
| AI governed | AI is grounded, evaluated and auditable |

## 3. Data architecture layers

```text
Source systems
-> ingestion
-> validation and standardization
-> domain stores
-> calculation engines
-> reporting snapshots
-> APIs and data products
-> advisor/client/AI channels
```

## 4. Critical data domains

| Domain | Examples |
|---|---|
| Client/account | CIF, BR, account, portfolio, household |
| Product/instrument | ISIN, subtype, risk rating, target market |
| Market data | prices, FX, rates, curves |
| Transactions | trades, cashflows, fees, corporate actions |
| Positions | holdings, lots, cash balances |
| Analytics | performance, risk, attribution, exposure |
| Mandates | rules, limits, restrictions |
| Reporting | snapshots, templates, archive |
| Entitlements | users, roles, access scopes |
| AI context | prompts, retrieval sources, outputs, evaluations |

## 5. Control-by-design examples

| Control need | Architecture pattern |
|---|---|
| suitability | rule engine with audit evidence |
| mandate breach | scheduled monitoring and event escalation |
| report reproducibility | immutable snapshot and calculation version |
| stale price | DQ rule and exception disclosure |
| AI hallucination | RAG citations and evaluation tests |
| unauthorized access | ABAC/RBAC and data scoping |
| data correction | correction event and restatement policy |
| migration sign-off | parallel-run reconciliation dashboard |

## 6. Architecture decision records

Use ADRs for:

- API pagination strategy
- event versus API integration
- source of truth for positions
- performance calculation methodology
- benchmark ownership
- report snapshot design
- AI retrieval architecture
- data retention policy
- multi-country localization model
- cloud deployment pattern

## 7. Platform capability map

| Platform capability | Purpose |
|---|---|
| identity and entitlement | secure access |
| API gateway | managed consumption |
| event backbone | lifecycle propagation |
| data quality engine | validation and exception |
| calculation engine | analytics |
| workflow engine | tasks and approvals |
| document renderer | reporting output |
| archive | evidence retention |
| AI orchestration | RAG, tools, guardrails |
| observability | operations |
| control evidence store | audit |

## 8. Key takeaway

Architecture is the execution structure of strategy. Without architecture, TOM remains a slide.

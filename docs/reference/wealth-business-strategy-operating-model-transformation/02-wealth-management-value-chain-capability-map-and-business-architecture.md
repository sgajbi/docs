# 02 - Wealth Management Value Chain, Capability Map and Business Architecture

## 1. Wealth value chain

The wealth value chain can be understood as:

```text
Client acquisition
-> onboarding and KYC
-> profiling and suitability
-> product governance
-> advice / proposal / mandate
-> order and execution
-> settlement and custody
-> portfolio accounting
-> valuation and analytics
-> reporting and review
-> servicing and lifecycle events
-> risk, compliance and control
```

## 2. Core business capabilities

| Capability | Purpose |
|---|---|
| Client and account master | Own client, BR/CIF, account, portfolio and hierarchy |
| Product master and APU | Own product taxonomy, eligibility, risk rating, target market |
| Advisory | Generate suitable recommendations and proposals |
| DPM / portfolio management | Manage discretionary portfolios and mandates |
| Trade/order management | Capture, validate, route and execute orders |
| Investment operations | Settlement, corporate actions, reconciliation |
| Accounting / ledger | Cash, positions, lots, income, fees, P&L |
| Market/reference data | Prices, FX, curves, instruments, corporate actions |
| Performance/risk analytics | TWR/MWR, attribution, risk, exposure, concentration |
| Reporting | Statements, portfolio review, PDF, archive |
| Lending / collateral | Lombard, margin, pledges, buying power |
| Digital/advisor channels | Workbench, portal, mobile, secure messaging |
| Entitlements | Who can see and act on what |
| Data governance | Lineage, data quality, source ownership |
| AI decision intelligence | Copilot, RAG, explanation, triage, next-best-action |
| Security and controls | IAM, audit, resilience, cyber risk |

## 3. Capability decomposition example

### Portfolio review capability

| Sub-capability | Description |
|---|---|
| Holdings snapshot | Portfolio holdings as of reporting date |
| Performance | TWR/MWR by period and segment |
| Contribution | Return contribution by asset class/security |
| Attribution | Manager/portfolio decision explanation |
| Risk | volatility, drawdown, VaR, concentration |
| Cashflows | deposits, withdrawals, income, fees |
| Product narrative | product-specific explanation and risk |
| Exceptions | missing price, stale value, unsupported analytics |
| Report generation | PDF/web output and archive |
| Advisor notes | explanation, meeting prep, client follow-up |
| AI support | grounded summary, anomaly explanation |

## 4. Business architecture views

A professional transformation pack should include:

| View | Purpose |
|---|---|
| Capability map | What the business needs to do |
| Value stream | How value flows from client need to service outcome |
| Process map | How work is performed |
| Organization map | Who owns capability |
| System map | Which applications support capability |
| Data map | Which data domains are required |
| Control map | Which risks and controls apply |
| KPI map | How success is measured |
| Roadmap map | How transformation is sequenced |

## 5. Capability heatmap

Capabilities can be rated:

| Status | Meaning |
|---|---|
| Green | Strong and scalable |
| Amber | Works but needs improvement |
| Red | Broken, risky or manual |
| Blue | Strategic differentiator |
| Grey | Commodity / candidate to buy |
| Purple | AI-enabled opportunity |

## 6. Capability ownership

Every capability needs:

- business owner
- technology owner
- data owner
- control owner
- product owner
- support owner
- roadmap owner
- KPI owner

Without ownership, transformation becomes fragmented.

## 7. Capability map to architecture

| Capability type | Architecture implication |
|---|---|
| Core source of truth | Strong data ownership, APIs, lineage |
| Calculation engine | Deterministic, versioned, testable |
| Workflow | State machine, audit, task ownership |
| Reporting | Snapshot, archive, reproducibility |
| AI copilot | RAG, guardrails, evaluation, approval |
| Operations | exceptions, reconciliation, runbooks |
| Controls | policy rules, evidence, monitoring |

## 8. Key takeaway

A capability map is the bridge between business strategy and platform architecture.

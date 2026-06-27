# 08 - Migration, Parallel Run, Cutover and Regression Quality Controls

## 1. Why migration needs special data controls

Migration and replatforming are high-risk because legacy and target platforms often use different data models, timing conventions, calculations, product classifications and reporting rules. Matching total market value is not enough. You need evidence that positions, transactions, performance, analytics, reports and controls are equivalent or intentionally different.

## 2. Migration control objectives

A migration control framework should prove:

- data scope is complete;
- product taxonomy mapping is correct;
- source ownership is understood;
- historical transactions are usable or correctly summarized;
- positions and cash reconcile;
- cost basis and P&L are preserved or remediated;
- performance continuity is maintained;
- reports are explainable;
- differences are classified as expected, accepted or defects;
- rollback and remediation options exist.

## 3. Migration phases

| Phase | Key controls |
|---|---|
| Discovery | Source profiling, product inventory, CDE inventory, data-quality baseline. |
| Mapping | Field mapping, taxonomy mapping, transformation rules, null handling. |
| Trial migration | Load sample portfolios, compare outputs, capture defects. |
| Parallel run | Run legacy and target side by side for agreed periods. |
| Cutover | Freeze windows, final deltas, control totals, go/no-go checklist. |
| Hypercare | Daily reconciliation, incident triage, client-impact monitoring. |
| Decommission | Data archive, lineage, audit evidence, fallback closure. |

## 4. Source profiling

Before migration, profile:

| Area | Questions |
|---|---|
| Products | Which product types exist? Any unsupported/rare types? |
| Transactions | Are transaction codes documented? Any legacy-only codes? |
| Positions | Are positions derived or sourced? Trade-date or settlement-date? |
| Cash | Are balances settled, available, earmarked, pledged or restricted? |
| Prices | Which source, price date, stale policy and currency? |
| FX | Which quote convention and rate type? |
| Corporate actions | Are historical adjustments explicit or embedded in positions? |
| Cost basis | Is lot-level history available? If not, what policy applies? |
| Performance | Is full history migrated or return history bridged? |
| Reports | Which legacy labels and sections need continuity? |

## 5. Mapping controls

| Mapping type | Control |
|---|---|
| Instrument ID | One-to-one mapping, duplicate detection, inactive instrument handling. |
| Product taxonomy | Legacy product class maps to target asset class/subtype/reporting class. |
| Transaction code | Each legacy code maps to canonical transaction type and cashflow class. |
| Currency | ISO currency, minor units and legacy local codes. |
| Account/portfolio | Client/BR/CIF/account hierarchy preserved. |
| Mandate | Mandate type, benchmark, restrictions and risk profile mapped. |
| Reporting labels | Legacy report categories mapped to target labels. |
| Source status | Active, closed, matured, written off, transferred, pending. |

## 6. Parallel run comparison levels

| Level | Example |
|---|---|
| Record count | Same number of portfolios, holdings, transactions. |
| Position quantity | Same holdings and quantities. |
| Cash balance | Same cash by currency and basis. |
| Market value | Same local/base value within tolerance. |
| Income | Same coupon/dividend/accrual totals. |
| P&L | Same realized/unrealized P&L or documented policy difference. |
| Performance | Same TWR/MWR within tolerance. |
| Allocation | Same asset-class/sector/currency buckets. |
| Risk | Same exposure/concentration/risk metrics within agreed tolerances. |
| Report | Same report totals, labels, disclosures and exception behaviour. |

## 7. Difference classification

| Difference type | Meaning | Action |
|---|---|---|
| Expected difference | Target intentionally uses improved policy. | Document and approve. |
| Timing difference | Trade-date vs settlement-date, EOD window difference. | Align basis or document. |
| Rounding difference | Decimal/FX/price rounding. | Tolerance and rule. |
| Source difference | Different price/source hierarchy. | Approve source policy or fix. |
| Mapping defect | Wrong product/account/transaction mapping. | Fix mapping and rerun. |
| Data defect | Legacy/source data wrong or incomplete. | Remediate or accept with evidence. |
| Calculation defect | Target formula/logic wrong. | Fix engine and regression. |
| Unsupported case | Product/event not supported in target. | Build support, manual process or exclude with approval. |

## 8. Product-specific migration concerns

| Product | Migration concern |
|---|---|
| Bonds | Accrued interest, amortized cost, coupon schedule, call/maturity events. |
| Notes | Termsheet, barrier status, observation history, issuer quote source. |
| Equities | Corporate action history, cost basis, lot reconstruction. |
| Funds | Share class, NAV history, reinvested distributions, unit precision. |
| Derivatives | Open contract terms, collateral, MTM, Greeks, margin. |
| Private markets | Commitment, funded/unfunded, recallable capital, NAV lag. |
| Lending | Pledges, cross-pledges, credit lines, exposures, LTV rules. |
| Insurance | Policy value, surrender value, premium schedule, beneficiaries. |
| Reporting | Legacy report continuity, disclosures, historical snapshots. |

## 9. Performance continuity controls

Performance migration is particularly sensitive.

Required decisions:

| Decision | Options |
|---|---|
| Historical returns | Migrate returns, recalculate from transactions, bridge at cutover. |
| Market values | Use legacy certified values or target recalculated values. |
| Cashflows | Migrate detailed flows or summarized flows. |
| Classification | Preserve legacy classification or restate using new taxonomy. |
| Benchmarks | Preserve historical benchmark links/composition. |
| Restatement | Decide whether target history restates prior reports. |

Minimum controls:

- beginning market value tie-out;
- ending market value tie-out;
- external cashflow classification tie-out;
- return tolerance by period;
- product-level contribution reasonability;
- benchmark-relative comparison;
- documented differences.

## 10. Cutover checklist

| Area | Go/no-go question |
|---|---|
| Scope | Are all in-scope clients/accounts/products included? |
| Critical defects | Are open critical/high defects closed or approved? |
| Reconciliation | Do positions, cash, market value and reports meet tolerance? |
| Data quality | Are critical CDEs complete? |
| Operations | Are runbooks, support contacts and break queues ready? |
| Reporting | Are client reports validated and exception wording approved? |
| Advisory | Are suitability/mandate checks ready? |
| Rollback | Is fallback/rollback process understood? |
| Access | Are user entitlements and hierarchy validated? |
| Audit | Is sign-off evidence stored? |

## 11. Hypercare controls

During hypercare monitor:

- daily feed completion;
- new breaks by product/source;
- client-report issues;
- advisor workflow blockers;
- trade/settlement issues;
- cash and position reconciliation;
- price/NAV availability;
- performance calculation failures;
- mandate/suitability exceptions;
- support ticket root causes.

## 12. Regression library

Every migration defect should become a regression scenario.

Example:

| Defect | Regression scenario |
|---|---|
| Rights issue cost basis wrong | Portfolio with rights entitlement, exercise and lapse. |
| Backdated transaction broke TWR | Transaction effective prior period triggers recalculation. |
| Note autocall not closing position | Autocall event generates redemption and zero position. |
| Fund distribution double counted | Cash distribution and reinvestment mutually exclusive. |
| Cross-pledge double counted collateral | Shared collateral allocated without transitive double count. |

## 13. Practitioner takeaway

Migration is not only data movement. It is controlled business equivalence. A strong migration pack proves what moved, what transformed, what changed intentionally, what remained different, who approved it and how future releases will prevent regression.

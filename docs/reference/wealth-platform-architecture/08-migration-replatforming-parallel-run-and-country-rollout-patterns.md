# 08 - Migration, Replatforming, Parallel Run and Country Rollout Patterns

## 1. Migration context in wealth platforms

Wealth migrations are high-risk because platforms contain historical holdings, transactions, cashflows, cost basis, performance continuity, client/account hierarchy, entitlements, product classifications, reports, mandates and country-specific rules.

A successful migration is not only a data move. It is a controlled business transition.

## 2. Migration types

| Migration type | Examples |
|---|---|
| Data migration | Positions, transactions, clients, accounts, instruments, prices, cost basis. |
| Platform migration | Move from legacy application to new architecture. |
| Product migration | Map old product taxonomy to new taxonomy. |
| Country rollout | Add a booking centre or jurisdiction. |
| Source-system migration | Replace custodian/core/market-data feed. |
| Cloud replatforming | Move workloads to cloud/Kubernetes. |
| Reporting migration | Replace statement/portfolio review engine. |
| Calculation migration | Replace performance/risk/valuation engine. |

## 3. Migration architecture phases

```text
Discovery
  -> source profiling
  -> target model mapping
  -> rule definition
  -> dry run migration
  -> reconciliation
  -> defect correction
  -> parallel run
  -> business sign-off
  -> cutover rehearsal
  -> cutover
  -> hypercare
  -> decommission
```

## 4. Source profiling

Profile every source file/API/table:

- record count
- field completeness
- duplicate keys
- identifier quality
- null patterns
- date range
- currency coverage
- negative quantities
- stale prices
- unmapped instruments
- missing cost basis
- missing transactions
- conflicting account ownership
- non-standard transaction codes

## 5. Mapping strategy

### Product mapping

| Source | Target |
|---|---|
| Legacy asset class | Standard product family and subtype. |
| Security code | Instrument master ID. |
| Local instrument type | Canonical taxonomy. |
| Legacy risk category | Product risk rating or complexity category. |

### Transaction mapping

| Source transaction | Target transaction type |
|---|---|
| Buy | BUY |
| Sell | SELL |
| Coupon | INCOME_COUPON |
| Dividend | INCOME_DIVIDEND |
| Maturity | REDEMPTION_MATURITY |
| Transfer in | TRANSFER_IN |
| Capital call | PRIVATE_MARKET_CAPITAL_CALL |
| Distribution | PRIVATE_MARKET_DISTRIBUTION |
| Fee | FEE_CHARGE |
| Tax | TAX_WITHHOLDING |

### Account/portfolio mapping

| Source | Target |
|---|---|
| Legacy client ID | Canonical party/CIF/BR ID. |
| Legacy account | Account master. |
| Legacy portfolio | Portfolio master. |
| Advisory code | Service model/mandate. |
| Location code | Booking centre. |

## 6. Parallel run design

Parallel run compares old and new platform outputs before cutover.

Compare:

- client/account/portfolio population
- holdings count and quantity
- cash balances
- market value by currency
- asset allocation
- income totals
- transaction history
- cost basis
- realized/unrealized P&L
- performance returns
- risk metrics
- mandate results
- reports/PDF values

## 7. Reconciliation levels

| Level | Description |
|---|---|
| L1 Record count | Same number of clients/accounts/positions/transactions. |
| L2 Identifier match | Same instruments, accounts, portfolios and currencies. |
| L3 Quantity/cash match | Same quantities, balances and nominal amounts. |
| L4 Value match | Same market values after price/FX alignment. |
| L5 Analytics match | Same performance, risk and allocation results. |
| L6 Report match | Same client-facing statement/portfolio-review outputs. |

## 8. Tolerance model

Define tolerances by data type.

| Data item | Tolerance approach |
|---|---|
| Quantity | Usually exact except fractional/rounding rules. |
| Cash | Exact or small currency rounding tolerance. |
| Market value | Price/FX-aligned tolerance. |
| Performance | Basis points threshold and explainable differences. |
| Risk metrics | Methodology-aligned tolerance. |
| Cost basis | Strict due tax/P&L impact. |
| Product classification | Exact after mapping sign-off. |
| Report totals | Must match defined reporting tolerance. |

## 9. Cutover planning

Cutover plan should define:

- freeze window
- source extraction time
- in-flight order handling
- pending settlement handling
- corporate-action blackout issues
- market data cut-off
- migration batch execution
- reconciliation gates
- sign-off checkpoints
- fallback/rollback criteria
- client/advisor communication
- hypercare support model

## 10. In-flight activity handling

In-flight items are dangerous:

- orders placed before cutover but executed after
- trades executed but not settled
- pending fund subscriptions/redemptions
- corporate actions with election deadlines
- private-market capital-call notices
- structured-product observations around cutover
- cash transfers in progress
- FX transactions with future value dates

Architecture should model in-flight state explicitly and reconcile after cutover.

## 11. Performance continuity

Performance migration requires:

- historical market values
- historical cashflows
- benchmark history
- cashflow classification rules
- inception dates
- benchmark assignment history
- fees/tax gross/net convention
- portfolio restructuring history
- composite membership history if applicable

Do not assume migrated current holdings are sufficient to preserve performance history.

## 12. Reporting continuity

Reporting migration should preserve:

- historical statements
- report archive links
- report templates and versions
- disclosure versions
- official snapshot values
- client communication history
- exceptions and restatements

If historical reports cannot be regenerated, store and archive them as official records.

## 13. Country rollout considerations

Each country/booking centre may differ in:

- product availability
- settlement calendars
- market holidays
- tax withholding
- reporting language
- statement format
- suitability rules
- product complexity classification
- data privacy requirements
- advisor entitlements
- currency conventions
- regulatory disclosures

The platform should isolate country-specific rules in configuration and policy services where possible.

## 14. Migration evidence pack

For each migration wave, maintain:

- scope and population
- source extracts
- mapping rules
- reconciliation results
- known differences
- sign-offs
- defects and resolutions
- cutover checklist
- rollback decision
- hypercare issues
- post-migration review

## 15. Migration anti-patterns

| Anti-pattern | Risk |
|---|---|
| Migrate only current positions | Loses performance, cost basis and transaction history. |
| No product taxonomy mapping | Reports and risk analytics inconsistent. |
| Manual reconciliation in spreadsheets only | Weak repeatability and audit. |
| No in-flight order strategy | Duplicate/missing trades after cutover. |
| No report comparison | Client-facing differences discovered late. |
| Hardcode country rules | Future rollouts become expensive. |
| Decommission source before evidence complete | Audit and investigation gap. |

## 16. Replatforming quality gates

- Source profiling complete.
- Mapping rules approved.
- Dry-run reconciliation passed.
- Parallel run evidence accepted.
- Performance continuity validated.
- Reporting outputs validated.
- Data-quality exceptions resolved or accepted.
- Operations runbooks ready.
- Entitlement model validated.
- DR and rollback plan tested.
- Business sign-off recorded.

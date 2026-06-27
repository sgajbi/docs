# Data Migration, Cutover, Regression and Country Rollout Delivery

## 1. Why migration is high-risk

Wealth platform migrations are risky because they touch:

- clients
- accounts
- portfolios
- positions
- transactions
- cash
- valuations
- performance history
- cost basis
- corporate actions
- suitability profiles
- mandates
- reporting history
- entitlements
- booking-centre rules
- downstream feeds

A migration can be technically complete but business-incomplete if numbers do not reconcile or reports cannot be explained.

## 2. Migration lifecycle

```text
Scope and source profiling
  -> data mapping
  -> transformation design
  -> migration tooling
  -> dry runs
  -> reconciliation
  -> defect remediation
  -> business sign-off
  -> cutover rehearsal
  -> production cutover
  -> post-cutover validation
  -> stabilization
```

## 3. Source profiling

Profile:

- field completeness
- duplicates
- invalid values
- historical depth
- date coverage
- currency consistency
- identifier mapping
- product coverage
- stale records
- orphan records
- source-system differences
- booking-centre-specific rules

## 4. Mapping design

Mapping must cover:

| Area | Example |
|---|---|
| Client | CIF, BR, household, beneficial owner. |
| Account | account number, booking centre, service model. |
| Portfolio | portfolio ID, mandate, base currency. |
| Instrument | ISIN, internal ID, product type, subtype. |
| Transaction | type mapping, trade date, value date, cashflow classification. |
| Position | quantity, nominal, cost, accrued income. |
| Price | source, date, currency, price type. |
| Performance | history, benchmark, restatement policy. |
| Entitlement | advisor/team/client access. |
| Reporting | statement groups, report preferences. |

## 5. Transformation controls

Transformation should be:

- deterministic
- versioned
- testable
- replayable
- audited
- reconciled
- documented

Avoid manual one-off transformation unless explicitly controlled.

## 6. Reconciliation levels

| Level | Example |
|---|---|
| Count | number of clients/accounts/positions/transactions. |
| Value | market value, cash balance, accrued income. |
| Attribute | currency, asset class, risk rating, maturity date. |
| Lifecycle | open vs closed positions, matured products. |
| Performance | period returns, start/end market value, cashflows. |
| Reporting | sample statement comparison. |
| Entitlement | user access before/after. |
| Mandate | mandate and restrictions migrated correctly. |

## 7. Tolerance model

Define tolerances by domain.

| Domain | Example tolerance |
|---|---|
| Cash | exact or near-zero tolerance. |
| Listed equity quantity | exact. |
| Bond accrued interest | small rounding tolerance. |
| Market value | tolerance based on price/FX differences. |
| Performance | threshold by period/materiality. |
| Private market NAV | tolerance based on source statement. |
| Report totals | must reconcile to displayed detail. |

## 8. Defect taxonomy

| Defect type | Example |
|---|---|
| Mapping defect | transaction type mapped incorrectly. |
| Source defect | source missing historical cost. |
| Transformation defect | FX conversion applied twice. |
| Timing defect | price date mismatch. |
| Product-model defect | note barrier terms not captured. |
| Reconciliation defect | totals differ due to rounding. |
| Reporting defect | label or disclosure wrong. |
| Entitlement defect | advisor cannot see migrated client. |
| Performance defect | cashflow classified incorrectly. |

## 9. Parallel run

Parallel run compares legacy and target systems for a period.

Compare:

- holdings
- cash
- transactions
- market values
- income
- performance
- reports
- order eligibility
- buying power
- mandate breaches
- suitability outcomes

Document differences as:

| Difference type | Treatment |
|---|---|
| Expected difference | explain and sign off. |
| Defect | fix and retest. |
| Source limitation | document and disclose if needed. |
| Timing difference | align cut-off or explain. |
| Methodology difference | approve business methodology. |

## 10. Cutover planning

Cutover plan includes:

- freeze window
- source extract time
- transformation steps
- load order
- validation checkpoints
- rollback criteria
- business sign-off
- support coverage
- communication plan
- production access controls
- issue triage room
- go/no-go criteria

## 11. Country rollout

Country rollout must consider:

- booking centre
- local regulations
- holidays
- product availability
- tax reporting
- statement language
- suitability rules
- client segment
- custody/settlement markets
- source systems
- data residency
- support hours

Do not assume one country's implementation automatically applies to another.

## 12. Regression strategy for migration

Regression pack:

- representative clients
- high-value clients
- complex portfolios
- multi-currency accounts
- lending/collateral clients
- structured products
- private markets
- stale/missing data
- historical performance
- corporate actions
- joint/trust/company accounts
- DPM mandates

## 13. Post-cutover stabilization

Stabilization should include:

- daily reconciliation
- issue dashboard
- break aging
- report QA
- advisor feedback
- operations feedback
- support triage
- defect prioritization
- daily business checkpoint
- formal stabilization exit criteria

## 14. Migration anti-patterns

| Anti-pattern | Risk |
|---|---|
| Only count reconciliation | Values and business meaning can still be wrong. |
| Mapping in spreadsheets only | Transformation is not reproducible. |
| No product edge cases | Complex holdings fail after go-live. |
| No report comparison | Client-facing defects discovered late. |
| No entitlement testing | Users blocked or overexposed. |
| Late performance validation | Historical returns become disputed. |
| No stabilization plan | Production support overwhelmed. |

## 15. Practitioner explanation

> "For migration, I would not treat success as simply loading records. I would run source profiling, deterministic mapping, dry runs, reconciliation at count/value/attribute/report levels, parallel-run comparison, business sign-off, cutover rehearsal and stabilization. In wealth platforms, migration quality must prove that clients, accounts, positions, transactions, valuations, performance, mandates, entitlements and reports are correct and explainable."

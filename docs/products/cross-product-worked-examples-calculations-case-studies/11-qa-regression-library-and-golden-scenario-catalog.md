# 11 - QA Regression Library and Golden Scenario Catalog

## 1. Purpose

A golden scenario is a known test case with expected outputs.

Golden scenarios are useful for:

- regression testing
- migration validation
- calculation engine testing
- report QA
- AI evaluation
- business sign-off
- developer onboarding
- production support triage

## 2. Golden scenario categories

| Category | Example |
|---|---|
| Cash | deposit, withdrawal, FX conversion |
| Bonds | buy, coupon, sale, maturity, accrued interest |
| Notes | coupon, barrier, autocall, physical settlement |
| Equities | buy, dividend, split, rights, spin-off |
| Funds | subscription, redemption, distribution, reinvestment |
| Derivatives | option premium, exercise, expiry, futures margin |
| Private markets | commitment, capital call, distribution, NAV update |
| Lending | LTV, haircut, buying power, margin call |
| Accounting | cost basis, realized P&L, fees, tax |
| Performance | TWR, MWR, contribution, attribution |
| Reporting | stale price, restatement, report archive |
| AI | grounded explanation, refusal, prompt injection |

## 3. Golden scenario template

```yaml
scenario_id: BOND-001
title: Bond purchase with accrued interest
domain: fixed-income
inputs:
  nominal: 100000
  clean_price: 98.50
  accrued_interest: 750
expected:
  cash_delta: -99250
  nominal_delta: 100000
  principal_cost: 98500
  accrued_interest_paid: 750
assertions:
  - cash paid equals principal cost plus accrued interest
  - clean and dirty price are separated
  - position nominal increases by 100000
```

## 4. Minimum golden pack for wealth platform

wealth platform should maintain at least:

| Area | Minimum scenarios |
|---|---:|
| Cash/FX | 10 |
| Bonds | 20 |
| Notes/structured products | 30 |
| Equities/corporate actions | 30 |
| Funds | 20 |
| Private markets | 20 |
| Derivatives | 25 |
| Lending/collateral | 25 |
| Performance/risk | 40 |
| Reporting | 30 |
| AI | 50 |
| Migration/reconciliation | 30 |

## 5. Regression levels

| Level | Purpose |
|---|---|
| Unit | formula/function correctness |
| Domain | product lifecycle correctness |
| Integration | source/API/event correctness |
| End-to-end | transaction to report |
| Migration | old vs new system comparison |
| AI eval | answer quality and guardrails |
| Production smoke | critical flows after release |

## 6. Report QA assertions

Every report scenario should assert:

- totals reconcile
- labels are correct
- currency is correct
- performance methodology is correct
- stale/missing data shown
- product classification correct
- disclosures present
- rounding consistent
- report version archived
- lineage exists

## 7. AI QA assertions

Every AI scenario should assert:

- source grounded
- no hallucination
- refusal when needed
- no policy bypass
- no unauthorized data access
- citations/source references
- no autonomous regulated action
- human approval requirement
- safe tone and clear caveats

## 8. Migration QA assertions

Every migration scenario should assert:

- source record count
- target record count
- balances reconcile
- positions reconcile
- cost basis reconcile
- performance continuity
- product mapping correctness
- reports match within tolerance
- exceptions signed off
- audit evidence stored

## 9. Key takeaway

A financial platform should be validated by a library of product-aware golden scenarios, not only generic technical tests.

# Migration, Replatforming, Cutover, and Continuity Engineering

## Purpose

This file explains migration and replatforming patterns for wealth platforms.

Migration in wealth technology is high-risk because client data, portfolio history, analytics continuity, reports, and user workflows must remain explainable.

---

## Migration Types

| Type | Example |
|---|---|
| Data migration | Move portfolios, accounts, transactions, holdings, documents. |
| Platform migration | Move runtime from legacy hosting to cloud/container platform. |
| Source migration | Change upstream source system. |
| Analytics migration | Replace performance/risk engine. |
| UI migration | Move users to new workbench or portal. |
| Reporting migration | Replace report generation or archive system. |
| Regional rollout | Onboard booking centers or jurisdictions in waves. |

---

## Migration Controls

Migration should define:

- source and target
- scope
- mapping rules
- reconciliation rules
- cutover plan
- fallback plan
- parallel run
- data freeze window
- validation evidence
- user communication
- support plan
- post-cutover monitoring

---

## Continuity

Continuity concerns:

- portfolio history
- performance inception date
- transaction history
- report archive continuity
- benchmark linkage
- client/account mapping
- document retrieval
- audit trail
- entitlements
- operational runbooks

---

## Parallel Run

Parallel run compares old and new systems.

Compare:

- holdings
- cash
- valuations
- transactions
- performance
- risk metrics
- reports
- document output
- API responses
- user workflows

---

## Cutover

Cutover plan should include:

- pre-checks
- data load
- validation
- freeze/unfreeze
- DNS/routing switch
- user enablement
- rollback criteria
- command center
- post-checks
- sign-off

---

## Migration Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Migration tested only on small sample | Production breaks. |
| No old-to-new ID mapping | Continuity loss. |
| Performance history not reconciled | Client reporting issue. |
| No rollback criteria | Decision confusion. |
| Cutover runbook incomplete | Execution risk. |
| Parallel run differences not classified | Sign-off uncertainty. |
| Archive migration ignored | Evidence gap. |

---

## Review Checklist

- Is migration scope clear?
- Are mapping rules defined?
- Is reconciliation defined?
- Is continuity covered?
- Is parallel run planned?
- Is cutover runbook ready?
- Is rollback defined?
- Is evidence generated?
- Are users/support prepared?
- Are post-cutover checks defined?

---

## Summary

Migration is evidence engineering.

A successful cutover is not only data moved; it is continuity proven.

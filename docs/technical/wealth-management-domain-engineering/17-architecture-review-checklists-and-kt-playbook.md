# Architecture Review Checklists and KT Playbook

## Purpose

This file provides practical checklists for reviewing wealth-management technology designs and running domain KT.

Use it for architecture reviews, PR reviews, onboarding, technical discussions, and engineering leadership conversations.

---

## Domain Boundary Checklist

- [ ] Capability is clear.
- [ ] Domain owner identified.
- [ ] Source truth separated from analytics output.
- [ ] BFF/UI responsibilities clear.
- [ ] Report/render/archive boundaries clear.
- [ ] Data-product producer clear.
- [ ] Non-owned responsibilities documented.
- [ ] Supportability states defined.

---

## Portfolio Data Checklist

- [ ] Client/account/portfolio hierarchy understood.
- [ ] Booking center/jurisdiction considered.
- [ ] Holdings source known.
- [ ] Transaction source known.
- [ ] Cashflow classification defined.
- [ ] Valuation inputs defined.
- [ ] Price/FX freshness handled.
- [ ] Backdated corrections handled.
- [ ] Reconciliation defined.

---

## Performance Checklist

- [ ] Methodology selected.
- [ ] Period definitions clear.
- [ ] Cashflow timing documented.
- [ ] Benchmark linkage defined.
- [ ] Currency treatment clear.
- [ ] Missing/stale data behavior defined.
- [ ] Golden examples available.
- [ ] Lineage captured.
- [ ] Results reproducible.

---

## Risk Checklist

- [ ] Risk metrics defined.
- [ ] Exposure dimensions canonical.
- [ ] Concentration thresholds governed.
- [ ] VaR/volatility methodology documented.
- [ ] Suitability rules server-side.
- [ ] Stale/missing data handled.
- [ ] Limitations displayed.
- [ ] Evidence captured.

---

## Advisory and Reporting Checklist

- [ ] Proposal lifecycle defined.
- [ ] Suitability and mandate rules clear.
- [ ] Human approval captured where required.
- [ ] Report snapshot defined.
- [ ] Template version captured.
- [ ] Document archive metadata complete.
- [ ] Client communication evidence retained.
- [ ] Unsupported claims avoided.

---

## Security Checklist

- [ ] Entitlement dimensions defined.
- [ ] Server-side authorization enforced.
- [ ] Permission-blocked state safe.
- [ ] Jurisdiction controls considered.
- [ ] Logs/metrics/traces safe.
- [ ] Reports/documents access-controlled.
- [ ] Test data synthetic.
- [ ] Sensitive evidence filtered.

---

## Migration Checklist

- [ ] Scope defined.
- [ ] Source/target mapping defined.
- [ ] Reconciliation defined.
- [ ] Parallel run planned.
- [ ] Cutover runbook ready.
- [ ] Rollback criteria clear.
- [ ] Continuity proven.
- [ ] Exceptions classified.
- [ ] Support prepared.

---

## KT Plan

A strong KT sequence:

1. wealth-platform capability map
2. client/account/portfolio hierarchy
3. holdings, transactions, cashflows, valuation
4. instrument, price, FX, benchmark data
5. performance methodology
6. risk methodology
7. advisory and mandate workflows
8. reporting/archive evidence
9. data sourcing and reconciliation
10. temporal model
11. API/BFF/domain/data-product boundaries
12. security and entitlements
13. operations and runbooks
14. migration and certification examples

---

## Engineering Lead Questions

- Who owns the data?
- What does this number mean?
- Which source produced it?
- What methodology was used?
- Can we reproduce the result?
- What happens when data is stale?
- Who is allowed to see it?
- What evidence supports the report?
- What can fail in production?
- What should be tested as a golden scenario?

---

## Summary

Wealth-platform architecture review must combine domain understanding with engineering discipline.

The best reviews connect business meaning, source truth, methodology, entitlement, evidence, and operations.

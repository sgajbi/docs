# Migration Review Checklists and KT Playbook

## Purpose

This file provides practical checklists for migration architecture, data, reconciliation, cutover, testing, operations, and leadership review.

Use it for migration planning, design reviews, mock cutovers, go/no-go decisions, and KT.

---

## Migration Scope Checklist

- [ ] Applications in scope listed.
- [ ] Data domains in scope listed.
- [ ] Interfaces in scope listed.
- [ ] Reports/documents in scope listed.
- [ ] Users/roles in scope listed.
- [ ] Batch jobs in scope listed.
- [ ] Downstream consumers listed.
- [ ] Exclusions documented.
- [ ] Owners assigned.
- [ ] Volumes estimated.

---

## Architecture Checklist

- [ ] Current state documented.
- [ ] Target state documented.
- [ ] Transition state documented.
- [ ] Coexistence paths clear.
- [ ] Temporary bridges labelled.
- [ ] Decommission plan defined.
- [ ] Security boundaries shown.
- [ ] Runtime changes documented.
- [ ] Integration paths mapped.
- [ ] Fallback paths documented.

---

## Data Mapping Checklist

- [ ] Mapping catalogue complete.
- [ ] Transformation rules documented.
- [ ] Defaults justified.
- [ ] Dropped fields approved.
- [ ] Lineage captured.
- [ ] Validation rules defined.
- [ ] Mapping tests present.
- [ ] Business meanings preserved.
- [ ] Owners assigned.

---

## Reconciliation Checklist

- [ ] Reconciliation types defined.
- [ ] Counts reconciled.
- [ ] Key mappings reconciled.
- [ ] Field-level checks defined.
- [ ] Totals reconciled.
- [ ] Business outputs compared.
- [ ] Tolerances approved.
- [ ] Exceptions tracked.
- [ ] Sign-off evidence generated.

---

## Integration Checklist

- [ ] APIs mapped.
- [ ] Events mapped.
- [ ] Files mapped.
- [ ] Batch schedules mapped.
- [ ] Schemas versioned.
- [ ] Consumers notified.
- [ ] Compatibility plan defined.
- [ ] Retry/replay defined.
- [ ] Decommission plan defined.

---

## Security Checklist

- [ ] Identities mapped.
- [ ] Roles mapped.
- [ ] Entitlements mapped.
- [ ] Service accounts defined.
- [ ] Secrets migrated securely.
- [ ] Allowed access tested.
- [ ] Denied access tested.
- [ ] Audit logging ready.
- [ ] Privileged access controlled.

---

## Cutover Checklist

- [ ] Cutover runbook complete.
- [ ] Roles assigned.
- [ ] Timings clear.
- [ ] Freeze plan defined.
- [ ] Pre-checks defined.
- [ ] Go/no-go criteria defined.
- [ ] Rollback criteria defined.
- [ ] Communication plan ready.
- [ ] Post-checks defined.
- [ ] Sign-off process clear.

---

## Testing Checklist

- [ ] Mapping tests present.
- [ ] Data validation tests present.
- [ ] Reconciliation automated.
- [ ] Workflow tests complete.
- [ ] Security tests complete.
- [ ] Report/archive tests complete.
- [ ] Performance tests complete.
- [ ] Mock cutover done.
- [ ] Dress rehearsal evidence captured.

---

## Operations Checklist

- [ ] Dashboards ready.
- [ ] Alerts ready.
- [ ] Runbooks ready.
- [ ] Support staffed.
- [ ] Hypercare plan defined.
- [ ] Known issues tracked.
- [ ] Escalation path defined.
- [ ] Stabilization metrics defined.
- [ ] Hypercare exit criteria defined.

---

## Business Readiness Checklist

- [ ] Stakeholders identified.
- [ ] Training complete.
- [ ] Process changes documented.
- [ ] Known limitations communicated.
- [ ] Sign-off owner clear.
- [ ] Accepted exceptions documented.
- [ ] Go-live communication ready.
- [ ] Rollback communication ready.

---

## KT Sequence

A strong migration KT should cover:

1. migration objective and scope
2. current-state architecture
3. target-state architecture
4. transition/coexistence architecture
5. data mapping
6. reconciliation
7. parallel run
8. integration migration
9. security/access migration
10. reporting/archive continuity
11. testing and certification
12. cutover runbook
13. rollback/fallback
14. hypercare
15. evidence pack and sign-off

---

## Engineering Lead Questions

- What must remain continuous?
- What can change?
- What evidence proves equivalence?
- What breaks are accepted?
- What is the rollback trigger?
- Who signs off?
- What is the hardest issue to detect?
- What operational gap remains?
- What happens on day two after cutover?

---

## Summary

Migration readiness is checklist-driven because migration risk is multi-dimensional.

Use these checklists to make scope, evidence, risk, and ownership visible.

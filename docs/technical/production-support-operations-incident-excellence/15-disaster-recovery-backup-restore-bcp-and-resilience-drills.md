# Disaster Recovery, Backup, Restore, BCP, and Resilience Drills

## Purpose

This file explains disaster recovery and business-continuity readiness.

Backup and DR plans are only credible when tested.

---

## Key Concepts

| Term | Meaning |
|---|---|
| RTO | Recovery Time Objective: how quickly service should recover. |
| RPO | Recovery Point Objective: acceptable data loss window. |
| Backup | Copy of data/config used for restore. |
| Restore | Process of recovering from backup. |
| DR | Disaster recovery capability. |
| BCP | Business continuity process when systems are unavailable. |

---

## DR Scope

Include:

- application services
- databases
- object/document storage
- queues/events
- configuration
- secrets
- certificates
- DNS/routing
- observability
- runbooks
- support access
- external dependencies

---

## Backup and Restore

Define:

- backup frequency
- retention
- encryption
- access control
- restore steps
- restore validation
- test cadence
- owner
- evidence

---

## DR Drill

A DR drill should test:

- failover steps
- restore procedure
- RTO/RPO
- data validation
- application readiness
- integration connectivity
- user workflow
- monitoring
- communication
- rollback to normal

---

## BCP

Business continuity should define:

- manual fallback
- critical workflows
- user communication
- business owner
- offline procedures
- reconciliation after recovery
- evidence retention

---

## DR Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Backup exists but restore never tested | False safety. |
| RTO/RPO undefined | Business expectation unclear. |
| DR excludes secrets/config | Restore fails. |
| Object archive not backed up | Document loss. |
| BCP not agreed with business | Operations stop. |
| DR drill not documented | No evidence. |
| Restore access too broad | Security risk. |

---

## Review Checklist

- Are RTO/RPO defined?
- Are backups configured?
- Are restores tested?
- Is DR scope complete?
- Are secrets/config included?
- Is document/archive recovery covered?
- Are DR runbooks current?
- Are drills scheduled?
- Is evidence retained?
- Is BCP agreed?

---

## Summary

DR readiness is proven by restore and drill evidence.

A backup that has never been restored is only an assumption.

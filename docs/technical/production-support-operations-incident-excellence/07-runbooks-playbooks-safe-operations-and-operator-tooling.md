# Runbooks, Playbooks, Safe Operations, and Operator Tooling

## Purpose

This file explains how to create and govern runbooks and operator tooling.

Runbooks should make support actions safe, repeatable, and auditable.

---

## Runbook Structure

A good runbook includes:

```text
Purpose
Scope
Symptoms
Impact
Dashboards
Alerts
First checks
Diagnosis steps
Safe mitigations
Recovery/replay
Rollback
Escalation
Data-safety notes
Post-incident actions
```

---

## Playbook Versus Runbook

| Type | Purpose |
|---|---|
| Runbook | Specific operational steps for a known scenario. |
| Playbook | Broader strategy for a class of incidents or workflows. |
| Troubleshooting guide | Diagnostic decision tree. |
| Operator guide | Safe use of operational tools. |

---

## Safe Operator Tools

Operator tools should be:

- authenticated
- authorized
- audited
- scoped
- idempotent where possible
- dry-run capable where useful
- guarded by confirmation for risky actions
- rate-limited
- documented
- tested

---

## Common Operator Actions

- replay failed job
- cancel stuck job
- quarantine bad record
- reprocess source batch
- refresh cache
- mark incident state
- trigger report regeneration
- rotate feature flag
- validate data freshness
- check reconciliation result

---

## Runbook Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Runbook only says “call engineering” | No support autonomy. |
| Commands not tested | Incident risk. |
| Tool can affect all clients by default | Blast radius risk. |
| No audit of operator action | Accountability gap. |
| Runbook contains secrets | Security issue. |
| Recovery steps not reversible | Data risk. |
| No data-safety notes | Sensitive data exposure. |

---

## Review Checklist

- Is runbook current?
- Are symptoms clear?
- Are dashboards linked?
- Are steps safe?
- Are commands tested?
- Are operator tools authorized?
- Are actions audited?
- Is blast radius controlled?
- Is escalation clear?
- Are post-incident actions included?

---

## Summary

Runbooks and operator tools are production control surfaces.

They must be safe, scoped, tested, and auditable.

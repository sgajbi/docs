# Reporting Review Checklists and KT Playbook

## Purpose

This file provides practical checklists for reporting, document generation, archive, evidence, and supportability reviews.

Use it for architecture reviews, PR reviews, testing, production readiness, KT, and engineering leadership discussions.

---

## Reporting Architecture Checklist

- [ ] Report capability owner clear.
- [ ] Domain data owners clear.
- [ ] Renderer boundary clear.
- [ ] Archive boundary clear.
- [ ] Report job lifecycle defined.
- [ ] Async generation used where needed.
- [ ] Snapshot/reproducibility model defined.
- [ ] Evidence bundle defined.
- [ ] Runbook available.

---

## Report Request Checklist

- [ ] Report type defined.
- [ ] Caller context captured.
- [ ] Scope validated.
- [ ] Idempotency key required where needed.
- [ ] Business date/period defined.
- [ ] Sections requested.
- [ ] Output format defined.
- [ ] Status API available.
- [ ] Retry/replay/cancel rules defined.

---

## Snapshot and Evidence Checklist

- [ ] Data snapshot id captured.
- [ ] Source versions captured.
- [ ] Analytics versions captured.
- [ ] Template version captured.
- [ ] Renderer version captured.
- [ ] Warnings captured.
- [ ] Supportability captured.
- [ ] Document hash captured.
- [ ] Archive id captured.
- [ ] Evidence access-controlled.

---

## Template and Rendering Checklist

- [ ] Template versioned.
- [ ] Template approved.
- [ ] Layout rules documented.
- [ ] Disclaimers controlled.
- [ ] Renderer avoids domain logic.
- [ ] Runtime assets available.
- [ ] Output validated.
- [ ] Rendering failures observable.
- [ ] Golden render tests present.

---

## Archive Checklist

- [ ] Metadata complete.
- [ ] Retention defined.
- [ ] Legal hold considered.
- [ ] Document hash stored.
- [ ] Retrieval access-controlled.
- [ ] Download audited.
- [ ] Deletion/expiry governed.
- [ ] Archive search supported where needed.
- [ ] Archive runbook available.

---

## Security Checklist

- [ ] Server-side authorization enforced.
- [ ] Report scope checked.
- [ ] Download access controlled.
- [ ] Signed URLs time-limited where used.
- [ ] Raw storage paths hidden.
- [ ] Logs/metrics safe.
- [ ] Evidence filtered.
- [ ] Permission-blocked states safe.
- [ ] Security tests present.

---

## Operations Checklist

- [ ] Report metrics defined.
- [ ] Lifecycle states visible.
- [ ] Render failures visible.
- [ ] Archive failures visible.
- [ ] Batch progress visible.
- [ ] Queue depth monitored.
- [ ] Alerts actionable.
- [ ] Runbooks current.
- [ ] Replay documented.
- [ ] Support owner clear.

---

## Testing Checklist

- [ ] Content model tests present.
- [ ] Section tests present.
- [ ] Template tests present.
- [ ] Render smoke tests present.
- [ ] Golden reports defined.
- [ ] Evidence tests present.
- [ ] Archive metadata tests present.
- [ ] Entitlement tests present.
- [ ] Batch tests present.
- [ ] Certification evidence generated.

---

## KT Sequence

A strong reporting KT sequence:

1. reporting capability model
2. report job lifecycle
3. data snapshot and lineage
4. report content model
5. template and rendering
6. archive and retention
7. evidence bundle
8. access control and download
9. batch reporting
10. supportability and warnings
11. observability/runbooks
12. golden report tests
13. migration/archive continuity
14. AI-assisted narrative controls

---

## Engineering Lead Questions

- Can the report be reproduced?
- Which data snapshot was used?
- Which service owns each section?
- What happens if performance data is stale?
- Is the template version captured?
- Is the document hash stored?
- Who can download it?
- What evidence is archived?
- What happens if rendering fails?
- What golden report proves this behavior?

---

## Summary

Reporting systems combine domain data, methodology, rendering, archive, security, and evidence.

Review them as evidence-producing workflows, not as document-export utilities.

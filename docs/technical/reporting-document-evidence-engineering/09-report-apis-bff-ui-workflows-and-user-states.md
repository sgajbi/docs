# Report APIs, BFF, UI Workflows, and User States

## Purpose

This file explains API and user workflow patterns for reporting capabilities.

Report generation often involves async jobs, status polling, preview, download, retry, and archive retrieval.

---

## Common APIs

```text
POST /reports/jobs
GET /reports/jobs/{jobId}
GET /reports/jobs/{jobId}/result
POST /reports/jobs/{jobId}:cancel
POST /reports/jobs/{jobId}:replay
GET /documents/{documentId}
GET /documents/{documentId}/download
```

---

## BFF Responsibilities

BFF may:

- shape report status for UI
- compose report capabilities
- map supportability states
- forward caller context
- hide internal archive/rendering details
- present product-safe errors

BFF should not:

- generate official report content
- bypass archive service
- recalculate analytics
- hide critical warnings
- expose raw storage paths

---

## UI States

Useful UI states:

- draft request
- validating
- accepted
- queued
- generating
- rendering
- archiving
- ready
- failed retryable
- failed final
- cancelled
- expired
- permission blocked
- degraded
- stale warning

---

## Preview Versus Final

Preview may be:

- watermarked
- not archived
- lower quality
- not client-ready
- based on draft data
- explicitly labelled

Final report should have:

- evidence
- archive
- integrity hash
- approval where required
- retention class

---

## API/UI Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| UI waits synchronously for heavy PDF | Timeout. |
| UI hides report warnings | User over-trust. |
| BFF returns raw renderer errors | Poor UX/security risk. |
| Preview treated as final | Evidence gap. |
| Download link exposed without entitlement | Data leak. |
| No expired state | User confusion. |
| Retry button creates duplicate report | Archive duplication. |

---

## Review Checklist

- Are report APIs async where needed?
- Is status model clear?
- Does BFF preserve supportability?
- Are UI states complete?
- Is preview labelled?
- Is final report archived?
- Are download permissions enforced?
- Are retry/cancel/replay safe?
- Are errors product-safe?
- Are tests present?

---

## Summary

Report UX should reflect report lifecycle truth.

Users should always know whether a document is draft, generating, ready, stale, failed, blocked, or final.

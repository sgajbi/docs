# BFF, Experience API, and Composition Patterns

## Purpose

This file explains how to design a Backend-for-Frontend or experience API layer.

The BFF exists to provide product-friendly contracts while preserving upstream domain authority.

## What A BFF Owns

A BFF owns:

- product-facing response shape
- aggregation across upstream services
- caller-context propagation
- partial/degraded state mapping
- UI-specific convenience fields
- safe error translation
- stable contract for product clients
- product route grouping
- response normalization
- request fan-out orchestration

## What A BFF Does Not Own

A BFF should not own:

- portfolio accounting truth
- performance methodology
- risk calculation methodology
- advisory suitability rules
- report rendering
- archive storage truth
- AI provider execution truth
- source data correction
- official domain lifecycle state

## Composition Pattern

```text
Product UI
  -> BFF route
      -> domain service A
      -> domain service B
      -> shared capability service
  -> product-ready response
```

The BFF should preserve source supportability signals.

## Product-Ready Response

A BFF response may combine:

- domain data
- supportability state
- source freshness
- warnings
- permission-blocked states
- partial/degraded sections
- display hints
- links to detail routes
- next available actions

Example shape:

```json
{
  "summary": {},
  "sections": {
    "performance": {"state": "READY", "data": {}},
    "risk": {"state": "STALE", "data": {}, "reason": "SOURCE_STALE"},
    "advisory": {"state": "PERMISSION_BLOCKED", "data": null}
  },
  "supportability": {
    "state": "PARTIAL",
    "reason": "ONE_OR_MORE_SECTIONS_DEGRADED"
  }
}
```

## Degraded Composition

The BFF should handle upstream failure deliberately.

Questions:

- Is the failed upstream required or optional?
- Can the response be partial?
- Should the whole request fail?
- What should the UI show?
- What should be logged?
- What metric should increment?
- What status code should be returned?
- What supportability state should be used?

## Caller Context Propagation

The BFF must propagate caller context.

Examples:

- actor id
- tenant id
- region
- booking center
- role
- caller application
- entitlement scope
- correlation id
- trace context

The BFF should not invent authorization results. It should preserve and forward context according to the security model.

## BFF Contract Testing

BFF tests should prove:

- response shape
- upstream fan-out
- partial failure behavior
- permission-blocked mapping
- caller-context forwarding
- no domain recomputation
- no raw upstream error leakage
- no sensitive fields in diagnostics
- OpenAPI examples match current behavior

## BFF Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| BFF mirrors every upstream endpoint | UI remains coupled to domain internals. |
| BFF computes official analytics | Domain authority drift. |
| BFF hides upstream warnings | Misleading product experience. |
| BFF returns raw upstream errors | Security and UX risk. |
| BFF directly reads platform files instead of API | Bypasses governance and entitlement model. |
| BFF stores generated AI output as truth | AI/domain boundary confusion. |
| BFF creates lifecycle state owned by upstream | Split-brain workflow state. |

## Review Checklist

- Does the BFF route serve a product need?
- Which upstreams are called?
- Does the BFF preserve upstream authority?
- Are partial and degraded states mapped?
- Is caller context forwarded?
- Are raw upstream errors hidden?
- Are UI display needs separated from domain truth?
- Are contract tests present?
- Are supportability states exposed?
- Does documentation avoid claiming unsupported domain ownership?

## Summary

A BFF makes product development easier by shaping contracts. It should not make architecture weaker by becoming an ungoverned domain service.

# 13 - Frontend, Experience API And UI Supportability

Enterprise UI is not just visual presentation. In wealth and banking platforms, the frontend is where users interpret portfolio, risk, advisory, reporting and operational truth. A strong UI must be backed by supported contracts, explicit state handling, clear evidence posture and browser-level proof.

## Experience Layer Model

| Layer | Responsibility |
|---|---|
| Domain service | Owns business facts, calculations, source lineage, methodology and authoritative API. |
| Gateway / experience API | Composes domain outputs into UI-ready contracts without becoming domain authority. |
| Product UI | Renders user workflows, state, decisions, actions, evidence and exceptions. |
| Browser validation | Proves the user-facing workflow renders and behaves as expected. |

The UI should feel coherent to the user, but the implementation must preserve source ownership.

## Gateway-First UI Pattern

Use an experience API when a screen needs:

1. data from multiple domain services,
2. consistent frontend envelope shape,
3. supportability summary,
4. product-safe error mapping,
5. entitlement-aware data filtering,
6. bounded diagnostics,
7. stable browser-facing contracts.

Do not let each page call many domain services directly unless there is a deliberate architectural reason. Direct calls multiply auth, error, timeout, state and contract complexity in the browser.

## What The UI May Do

A frontend may:

1. choose layout and visual hierarchy,
2. group related fields,
3. sort and filter already-authorized data,
4. show drill-down paths,
5. display supportability and source posture,
6. trigger supported workflow actions,
7. preserve user preferences where allowed,
8. render charts and tables from backend-provided measures.

## What The UI Must Not Do

A frontend should not:

1. calculate authoritative risk, performance, suitability or financial methodology locally,
2. infer readiness from missing source data,
3. invent trust state, workflow state or audit state,
4. call restricted source-owner services directly when a gateway contract exists,
5. hide partial, stale, unavailable or permission-blocked states,
6. display unsupported features as if they are current,
7. construct prompts, reports, orders, approvals or client communications without backed workflow contracts.

## UI State Taxonomy

Every material panel should handle these states:

| State | Meaning | UI treatment |
|---|---|---|
| loading | Request is in progress. | Stable skeleton or spinner without layout jump. |
| ready | Data is current and complete enough for the workflow. | Normal rendering with as-of/source context. |
| empty | Request succeeded but there is no data. | Clear empty state with next action if supported. |
| partial | Some optional source data is missing. | Show available data with visible limitation. |
| degraded | Data exists but source quality, freshness or dependency posture is weakened. | Warning, source reason and blocked actions where needed. |
| blocked | Required source or control prevents safe use. | Do not show action as available. Explain bounded reason. |
| permission_blocked | Caller lacks access. | Product-safe denial without leaking policy internals. |
| error | Unexpected failure. | Recoverable message, retry path and support reference where available. |

## Dense Banking UI Principles

Operational and advisory users need dense but readable interfaces.

Use:

1. compact tables with stable columns,
2. clear units, currencies and dates,
3. as-of context near values,
4. source and supportability chips where needed,
5. drill-down over overloaded summaries,
6. restrained color for status and exceptions,
7. keyboard and screen-reader accessible controls,
8. predictable navigation between summary and detail.

Avoid:

1. oversized marketing-style hero sections in work tools,
2. decorative cards without workflow value,
3. hidden actions that require guessing,
4. charts without data definitions,
5. values without units or dates,
6. repeated text explaining obvious UI controls,
7. layouts that collapse unpredictably on smaller screens.

## View Model Pattern

Use view models to adapt backend contracts to UI rendering without taking domain ownership.

Good view model responsibilities:

1. choose display labels,
2. format dates and numbers,
3. group rows for display,
4. map backend state to UI state,
5. select icon or status style,
6. preserve source fields for drill-down.

Bad view model responsibilities:

1. calculating source-owned financial metrics,
2. changing lifecycle state,
3. suppressing degraded states,
4. inventing missing evidence,
5. translating authorization failures into fake empty states.

## Experience API Contract Shape

A UI-ready response should often include:

```json
{
  "data": {},
  "state": "partial",
  "as_of_date": "2026-06-27",
  "supportability": {
    "reason": "source_stale",
    "blocked_actions": ["publish_report"],
    "source_date": "2026-06-25"
  },
  "metadata": {
    "correlation_id": "bounded-support-reference",
    "source_systems": ["portfolio-service", "market-data-service"]
  }
}
```

Use synthetic identifiers in examples. Do not publish real client, account, portfolio, document or trace identifiers.

## Component And Hook Boundaries

Recommended split:

| Frontend unit | Responsibility |
|---|---|
| API client | Calls gateway contract and parses typed response. |
| Query hook | Handles fetch lifecycle, caching, retry and refresh policy. |
| View model | Maps contract state to presentation model. |
| Component | Renders UI and user interactions. |
| Page / route | Composes panels, navigation and page-level context. |
| Test fixture | Provides deterministic examples for states and edge cases. |

Avoid putting API calls, state mapping, formatting and layout all inside one large page component.

## Browser Validation

Browser tests should prove:

1. screen loads from supported route,
2. key panels render populated data,
3. loading/empty/error states do not break layout,
4. permission-blocked state is product-safe,
5. important actions are enabled only when supported,
6. tables and charts fit at target viewports,
7. drill-down or navigation path works,
8. no visible placeholder or unsupported claim leaks into the UI.

For visual changes, pair code-level tests with screenshots or browser evidence when the workflow is user-facing.

## Accessibility And Layout

Minimum expectations:

1. buttons and inputs have accessible names,
2. keyboard navigation reaches key actions,
3. focus state is visible,
4. form errors are associated with fields,
5. color is not the only status signal,
6. text fits in containers at supported viewport sizes,
7. tables have headers and meaningful row context,
8. charts have adjacent textual values or summaries.

## Frontend Observability

Frontend telemetry should be bounded and privacy-safe.

Useful events:

1. panel load success/failure,
2. gateway request state,
3. degraded source reason,
4. user action requested,
5. action blocked by supportability,
6. browser route error,
7. performance timing bucket.

Do not emit:

1. client names,
2. account numbers,
3. holdings,
4. raw request or response bodies,
5. prompt text,
6. model output,
7. trace ids as analytics labels.

## UI Review Checklist

Before approving a frontend slice, confirm:

1. the screen is backed by a supported API contract,
2. domain ownership is preserved,
3. loading, ready, empty, partial, degraded, blocked, permission and error states are handled,
4. important values show units, dates and source posture,
5. unsupported actions are not displayed as available,
6. layout is stable at target viewports,
7. tests cover the meaningful states,
8. browser validation covers the user-facing workflow,
9. docs or runbooks changed if workflow truth changed.

## Common Anti-Patterns

Avoid:

1. mock-only product screens that look complete,
2. page-local calculations of governed metrics,
3. duplicated status mapping across many components,
4. optimistic UI action states without backend authority,
5. empty-state text that hides permission denial,
6. charts that omit source date or metric definition,
7. browser screenshots captured before API validation,
8. UI copy that promises future features as current support.

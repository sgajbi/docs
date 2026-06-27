# Contract Governance: OpenAPI, Events, Data Products, and Docs

## Purpose

This file explains how CI/CD should protect contracts across APIs, events, data products, and documentation.

Contract drift is one of the most expensive problems in multi-service platforms.

---

## Contract Types

| Contract | Example |
|---|---|
| API contract | OpenAPI route, request, response, errors. |
| Event contract | Event type, schema, version, producer, topic. |
| Data-product contract | Producer/consumer declaration, schema, trust metadata. |
| Vocabulary contract | Canonical enums, period values, reason codes. |
| Documentation contract | README, wiki, runbook, supported-feature docs aligned with code. |
| Evidence contract | Certification or runtime evidence manifest shape. |

---

## OpenAPI Gates

Validate:

- summaries
- descriptions
- tags
- response models
- examples
- error responses
- operation ids
- deprecated routes
- no future-state unsupported claims

---

## Event Contract Gates

Validate:

- schema version
- event type
- required fields
- payload structure
- examples
- producer identity
- compatibility rules
- AsyncAPI or equivalent docs

---

## Data-Product Contract Gates

Validate:

- producer declaration
- consumer declaration
- product version
- schema reference
- vocabulary reference
- access/SLO/evidence policy
- trust telemetry
- certification posture

---

## Documentation Gates

Docs gates can validate:

- required docs exist
- README links valid
- wiki source exists
- supported-feature claims match evidence
- no banned phrases or overclaims
- no sensitive data
- runbook references implemented metrics/routes
- quality scorecards current

---

## Contract Drift Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| API changes without OpenAPI update | Consumers misled. |
| Event schema changed silently | Downstream breakage. |
| Data product claim without evidence | False certification. |
| Docs claim production support without code | Trust risk. |
| Wiki updated manually but source not updated | Durable truth drift. |
| Vocabulary changed locally | Cross-service inconsistency. |
| Contract gate report-only forever | Drift normalized. |

---

## Review Checklist

- Which contracts changed?
- Are schemas updated?
- Are examples current?
- Are compatibility impacts understood?
- Are consumers identified?
- Are docs aligned?
- Are gates blocking or report-only?
- Are evidence artifacts safe?
- Is versioning correct?
- Are deprecated paths documented?

---

## Summary

Contracts are the shared language of enterprise platforms.

CI/CD should protect that language from accidental drift.

# OpenAPI Documentation and Quality Gates

## Purpose

This file explains how to use OpenAPI as a real contract, not a generated afterthought.

OpenAPI should help developers, testers, consumers, architects, and support teams understand current API behavior.

## OpenAPI Must Describe Current Truth

OpenAPI should not describe:

- planned features
- future routes not implemented
- unsupported states as if supported
- examples that no longer work
- security assumptions not enforced
- response fields not actually returned

If a capability is planned, document it outside the current contract or mark it clearly.

## Required Route Metadata

Each route should include:

- summary
- description
- tags
- request model
- response model
- status codes
- error responses
- examples
- caller-context requirements
- idempotency requirements where applicable
- pagination rules where applicable
- deprecation notes where applicable

## Good Summary

Poor:

```text
Calculate.
```

Better:

```text
Submit a portfolio performance calculation for a supported period.
```

## Good Description

A good description explains:

- what the route does
- who should call it
- what the service owns
- what it does not own
- sync/async behavior
- supportability states
- important constraints

## Examples

Examples should be:

- synthetic
- current
- minimal but meaningful
- complete enough to copy
- sensitive-data safe
- aligned with canonical vocabulary

Examples should not include:

- real client data
- real account numbers
- secrets
- internal hostnames unless explicitly local/dev examples
- unsupported fields
- obsolete enum values

## Error Documentation

OpenAPI should document common errors:

- 400 invalid request
- 401 unauthenticated
- 403 permission blocked
- 404 not found
- 409 conflict
- 422 semantic validation
- 429 rate limit
- 503 dependency unavailable
- 504 timeout

Each error should use the standard problem-details model.

## OpenAPI Quality Gate

An OpenAPI quality gate can validate:

- every route has summary
- every route has description
- every route has tags
- response descriptions exist
- success response exists
- examples exist for important routes
- error responses are documented
- operation ids are stable
- schema names follow conventions
- deprecated routes are marked
- forbidden terms are absent
- planned-only claims are absent

## Contract Drift

Contract drift happens when code and OpenAPI disagree.

Prevent drift with:

- generated OpenAPI snapshots
- schema comparison
- contract tests
- CI gate
- consumer tests
- docs review
- version change policy

## OpenAPI Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Auto-generated docs with no descriptions | Consumers cannot understand behavior. |
| Examples copied from old version | Integration failures. |
| Future-state fields included | False support claims. |
| Error responses omitted | Clients handle failure poorly. |
| Operation ids unstable | Generated clients break. |
| Security docs differ from implementation | Security confusion. |
| No OpenAPI gate | Contract quality regresses. |

## Review Checklist

- Are route summaries clear?
- Are descriptions current?
- Are tags consistent?
- Are request/response models explicit?
- Are examples valid and synthetic?
- Are error responses documented?
- Are idempotency and pagination described?
- Are deprecated routes marked?
- Are unsupported states honest?
- Is OpenAPI validated in CI?
- Are generated clients or consumers considered?

## Summary

OpenAPI is a living API contract.

If it is incomplete or misleading, the API is incomplete from a consumer perspective.

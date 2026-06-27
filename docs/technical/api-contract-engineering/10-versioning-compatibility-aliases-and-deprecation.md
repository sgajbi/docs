# Versioning, Compatibility, Aliases, and Deprecation

## Purpose

This file explains how to evolve APIs without breaking consumers unexpectedly.

Enterprise APIs live longer than projects. They need compatibility discipline.

## Why Versioning Matters

APIs change because:

- fields are added
- fields are renamed
- enum values change
- response shapes evolve
- methods become async
- semantics become stricter
- data products mature
- old routes are replaced
- consumers migrate at different speeds

Without versioning, change becomes risky and political.

## Versioning Options

| Pattern | Example | Use |
|---|---|---|
| URI version | `/api/v1/reports` | Common and visible. |
| Header version | `API-Version: 1` | Useful for advanced negotiation. |
| Media type version | `application/vnd.company.v1+json` | More formal, less common internally. |
| Schema version field | `version: "v1"` | Useful in events/data products. |
| Capability version | `productName:v1` | Useful for data products and workflow packs. |

Choose consistently.

## Backward-Compatible Changes

Usually compatible:

- add optional response field
- add optional request field with safe default
- add new enum value only if consumers tolerate unknowns
- add new route
- add new warning
- add metadata field
- add pagination metadata while preserving items

Potentially breaking:

- remove field
- rename field
- change field type
- change required field
- change enum semantics
- change default behavior
- change error status code
- make sync route async without compatibility
- remove alias
- change pagination order

## Compatibility Aliases

Aliases support migration.

Examples:

- old enum value accepted and normalized
- old query parameter accepted temporarily
- old route redirects or delegates to new route
- old response field retained during transition

Aliases must be:

- documented
- tested
- time-bound where possible
- tracked for removal
- not allowed to multiply indefinitely

## Deprecation

Deprecation should include:

- what is deprecated
- why
- replacement
- migration timeline
- consumer impact
- support window
- removal plan
- tests protecting compatibility until removal

## Breaking Change Checklist

Before breaking an API:

- identify consumers
- provide replacement
- update OpenAPI
- update docs
- notify owners
- support compatibility window
- add migration tests
- version route/schema if needed
- update runbooks
- remove old contract only after evidence

## Versioning Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Change response shape silently | Consumer breakage. |
| Add enum values without unknown handling | Client failures. |
| Keep aliases forever | Contract complexity. |
| Version every minor change | Version sprawl. |
| No consumer inventory | Unknown blast radius. |
| Docs show new contract before implementation | False contract truth. |
| Remove route because no one "seems" to use it | Hidden consumers fail. |

## Review Checklist

- Is the change backward-compatible?
- Are consumers known?
- Is version bump required?
- Are aliases documented?
- Are deprecated fields marked?
- Is migration path clear?
- Are tests protecting compatibility?
- Is OpenAPI updated?
- Are examples current?
- Is removal tracked?

## Summary

API evolution is a product lifecycle.

Compatibility is not accidental. It is designed, tested, documented, and eventually cleaned up.

# Documentation as Engineering Infrastructure

## Purpose

This file explains why documentation should be treated as engineering infrastructure.

Good documentation reduces delivery risk, accelerates onboarding, improves review quality, supports operations, and preserves decision context.

---

## Documentation Is Not Afterthought

Documentation is part of engineering when it explains:

- what exists
- how it is designed
- how it runs
- how it fails
- how it is tested
- how it is released
- how it is supported
- how it should evolve

Documentation is weak when it only repeats marketing claims or project status.

---

## Documentation As Control Surface

Documentation supports controls such as:

- architecture governance
- API governance
- data governance
- security review
- release readiness
- production support
- incident response
- audit traceability
- procurement evidence
- KT and onboarding

---

## Why Documentation Fails

Documentation usually fails because:

- no owner
- no lifecycle
- no review
- no link to implementation
- too much duplication
- future plans written as current truth
- stale screenshots
- missing examples
- not discoverable
- not updated with code
- not validated in CI
- unsafe sensitive content

---

## Documentation Quality Attributes

Good docs are:

- accurate
- current
- concise where possible
- complete where necessary
- implementation-backed
- easy to navigate
- role-aware
- source-controlled
- reviewable
- searchable
- secure
- maintained

---

## Documentation Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Docs written once at project start | Stale by go-live. |
| Docs only in slide deck | Not close to implementation. |
| Wiki edited manually without source | Durable truth drift. |
| Diagrams not updated | Architecture misunderstanding. |
| Runbook lacks actual commands | Incident delay. |
| Feature claims without evidence | Trust risk. |
| Too much duplicated text | Conflicting truth. |
| Sensitive examples | Security/privacy issue. |

---

## Review Questions

- Who owns this document?
- What truth does it preserve?
- Is it implementation-backed?
- Is it current?
- Is it discoverable?
- Does it duplicate another document?
- Does it contain sensitive information?
- Does it support a real workflow?
- What evidence proves the claim?

---

## Summary

Documentation is part of the platform.

A team that cannot explain its system cannot reliably operate, audit, onboard, or improve it.

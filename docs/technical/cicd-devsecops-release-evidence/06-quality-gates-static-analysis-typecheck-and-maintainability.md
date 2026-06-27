# Quality Gates: Static Analysis, Typecheck, and Maintainability

## Purpose

This file explains how static quality gates improve code health before defects reach runtime.

Static gates are not replacements for tests. They catch maintainability, correctness, and hygiene risks early.

---

## Common Static Gates

| Gate | Purpose |
|---|---|
| Formatting | Consistent style and low review noise. |
| Linting | Common mistakes, imports, unused variables. |
| Typechecking | Type consistency and interface safety. |
| Complexity | Functions/classes too complex. |
| Dead code | Unused code and stale paths. |
| Dependency hygiene | Unused/missing dependencies. |
| Import boundary | Architecture dependency rules. |
| Documentation coverage | Public code documentation where required. |
| Repository hygiene | Blocks generated or local artifacts. |
| Forbidden pattern scan | Blocks raw print, unsafe logs, floats, secrets, etc. |

---

## Formatting

Formatting should be automated.

Rule:

```text
Do not debate formatting in PR if a formatter can enforce it.
```

---

## Linting

Linting should catch:

- syntax-adjacent issues
- unused imports
- import order
- style problems
- common bugs
- forbidden patterns
- unsafe constructs

Avoid excessive noisy lint rules without team agreement.

---

## Typechecking

Typechecking improves confidence in:

- API models
- service interfaces
- domain objects
- optional values
- return types
- ports/adapters
- refactoring safety

Adopt gradually if the codebase is not typed yet.

---

## Complexity Gates

Complexity gates identify functions or modules that are hard to understand.

Use complexity reports to:

- target refactoring
- reduce giant controllers
- split policies
- isolate calculations
- improve testability

Do not treat complexity number as the only design signal.

---

## Dead Code Detection

Dead code tools help remove:

- unused functions
- stale compatibility paths
- obsolete helpers
- unused classes
- abandoned scripts

Be careful with dynamic imports and framework-discovered routes.

---

## Architecture Boundary Gates

Boundary gates can prevent:

- domain importing infrastructure
- UI importing backend-only code
- BFF importing forbidden upstream internals
- route handlers importing repositories directly
- tests depending on wrong layers

These gates are powerful but should be introduced carefully.

---

## Maintainability Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Lint disabled globally | Quality weakens silently. |
| Typecheck ignored forever | Interface errors persist. |
| Complexity report not acted on | Refactoring backlog hidden. |
| Gate exceptions grow endlessly | Governance decay. |
| Formatting mixed with feature change | Review noise. |
| Dead code retained for "maybe later" | Confusion and risk. |
| Static gate used without owner | Findings ignored. |

---

## Review Checklist

- Are formatting and linting automated?
- Are type errors blocked or tracked?
- Are complexity thresholds defined?
- Are dead-code findings reviewed?
- Are boundary rules meaningful?
- Are exceptions time-bound?
- Are report-only gates tracked?
- Are quality reports used in refactoring?
- Are generated files excluded correctly?

---

## Summary

Static quality gates keep codebases healthy by catching maintainability problems early.

They work best when checks are meaningful, owned, and gradually strengthened.

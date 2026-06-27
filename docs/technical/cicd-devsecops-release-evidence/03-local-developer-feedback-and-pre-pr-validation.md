# Local Developer Feedback and Pre-PR Validation

## Purpose

This file explains how local validation should work before a developer opens or updates a pull request.

Local checks should help developers catch issues quickly without forcing every heavy release check to run on every edit.

---

## Goals Of Local Validation

Local validation should be:

- fast
- easy to run
- deterministic
- aligned with CI
- documented
- focused on high-signal checks
- friendly to iterative development

---

## Recommended Local Commands

A mature repository should expose simple commands such as:

```text
make install
make lint
make typecheck
make test
make check
make ci
make clean
```

For frontend repositories:

```text
npm install
npm run lint
npm run typecheck
npm test
npm run build
npm run test:e2e
```

The exact commands vary, but the principle is stable: developers should not need to memorize tool internals.

---

## Local Validation Levels

| Level | Purpose | Example |
|---|---|---|
| Focused | Validate one changed area | single test file |
| Feature check | Fast broad feedback | lint + typecheck + unit |
| PR-grade local | Stronger local proof | integration + coverage |
| Runtime parity | Validate container or stack | docker smoke |
| Product proof | Validate integrated product flow | browser/live validation |

---

## Pre-PR Checklist

Before opening a PR:

- run focused tests for changed logic
- run feature-lane command
- run contract tests if API/schema changed
- run docs checks if docs changed
- run migration smoke if persistence changed
- run Docker build if runtime changed
- clean generated artifacts
- review diff for secrets or local files
- update README/runbook/wiki if truth changed

---

## Generated Artifact Hygiene

Generated artifacts should not be committed unless they are intentional source-controlled evidence.

Block:

- Python caches
- coverage files
- local databases
- logs
- temporary screenshots
- virtual environments
- build folders
- dependency caches
- local `.env` files
- ad hoc output

Use cleanup commands and repository hygiene gates.

---

## Local Environment Documentation

Developer setup should document:

- language version
- package manager
- install command
- test commands
- local service ports
- environment variables
- Docker Compose dependencies
- seed data
- troubleshooting
- cleanup

---

## Fast Feedback Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Developers only discover issues in PR CI | Slow cycle. |
| Local commands differ from CI | False confidence. |
| Setup requires tribal knowledge | Onboarding friction. |
| One command runs everything and takes too long | Developers stop running it. |
| Generated files frequently committed | Repo noise and review fatigue. |
| Local tests depend on hidden state | Flaky validation. |

---

## Review Questions

- Is there a documented local check command?
- Does local validation match feature-lane CI?
- Are focused tests easy to run?
- Are generated artifacts cleaned?
- Are local environment requirements clear?
- Are failures actionable?
- Can a new developer validate changes without asking the team?

---

## Summary

Local validation is the first quality gate.

It should be fast enough to use often and aligned enough to prevent obvious CI failures.

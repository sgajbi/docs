# Generated Artifacts, Repository Hygiene, and Sensitive Content

## Purpose

This file explains how to keep repositories clean and safe from generated noise, local artifacts, secrets, and sensitive content.

Repository hygiene protects review quality, security, and long-term maintainability.

---

## Common Generated Artifacts

Avoid committing unless intentional:

```text
__pycache__/
.pytest_cache/
.coverage
htmlcov/
node_modules/
dist/
build/
logs/
*.db
.env
.venv/
output/
tmp/
screenshots/
local-test-results/
```

Some generated evidence may be intentionally committed if it is part of contract or documentation. It must be clearly governed.

---

## Sensitive Content

Never commit:

- secrets
- tokens
- private keys
- passwords
- real client data
- real account data
- raw production exports
- restricted internal source paths where not approved
- raw logs with sensitive data
- raw prompts/model responses

---

## Repository Hygiene Gates

Useful gates:

- no generated cache files
- no local database files
- no committed virtual environments
- no secrets
- no sensitive marker names
- no oversized generated artifacts
- docs link validation
- generated contract consistency

---

## `.gitignore`

A good `.gitignore` should exclude:

- local environment files
- caches
- build outputs
- test outputs
- logs
- local databases
- IDE files where appropriate
- dependency directories

Do not rely only on `.gitignore`; use hygiene gates too.

---

## Artifact Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Coverage output committed | Review noise. |
| Local DB committed | Data leak and repo bloat. |
| `.env` committed | Secret leak. |
| Screenshots with real data | Privacy risk. |
| Generated evidence without owner | Stale truth. |
| Large artifacts in Git | Repo performance issues. |
| Hygiene gate bypassed | Quality erosion. |

---

## Review Checklist

- Are generated files intentional?
- Are local artifacts excluded?
- Are secrets absent?
- Are screenshots safe?
- Are evidence files governed?
- Is `.gitignore` current?
- Are hygiene gates active?
- Are large files justified?
- Is artifact retention policy clear?

---

## Summary

A clean repository is easier to review, safer to share, and more trustworthy as engineering memory.

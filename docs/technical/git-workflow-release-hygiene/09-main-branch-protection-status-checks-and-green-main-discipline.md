# Main Branch Protection, Status Checks, and Green Main Discipline

## Purpose

This file explains how to protect the main branch and keep it releasable.

Main branch should represent durable, integrated, validated truth.

---

## Main Branch Protection

Typical controls:

- no direct push
- required PR review
- required status checks
- code owner review for sensitive files
- branch up-to-date before merge where needed
- signed commits/tags where required
- restricted force pushes
- required conversation resolution
- deployment environment approvals where needed

---

## Required Status Checks

Required checks should be meaningful.

Examples:

- lint
- typecheck
- unit tests
- contract tests
- integration tests
- security scan
- OpenAPI gate
- Docker build
- repository hygiene
- docs contract gate

Do not require checks that are noisy and unmanaged.

---

## Green Main Discipline

If main breaks:

1. acknowledge quickly
2. identify failing change
3. fix forward or revert
4. communicate impact
5. add regression if needed
6. review why gate did not catch earlier
7. improve controls

Broken main should be treated as a team priority.

---

## Main Branch Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Direct pushes to main | Review bypass. |
| Main stays red for days | Release confidence lost. |
| Required checks not meaningful | False protection. |
| Required flaky check | Delivery friction. |
| Admin bypass used casually | Governance erosion. |
| Docs merged without validation | Durable truth drift. |

---

## Review Checklist

- Is main protected?
- Are required checks meaningful?
- Are flaky required checks fixed?
- Is direct push restricted?
- Is bypass controlled?
- Is broken main response defined?
- Are release gates tied to main?
- Are docs/wiki tied to main truth?

---

## Summary

A green main branch is a platform asset.

Protect it like production-adjacent infrastructure.

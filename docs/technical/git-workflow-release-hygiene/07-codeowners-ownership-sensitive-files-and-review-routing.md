# CODEOWNERS, Ownership, Sensitive Files, and Review Routing

## Purpose

This file explains how ownership-based review improves quality and control.

Some files need specific reviewers because they affect security, architecture, contracts, release, or platform behavior.

---

## Ownership Areas

Examples:

| Area | Owner Type |
|---|---|
| API contracts | API/domain owners |
| Domain logic | Domain experts |
| Security config | Security/platform owners |
| CI workflows | DevOps/platform/security owners |
| Migrations | Backend/data owners |
| Runtime manifests | Platform/SRE owners |
| Data-product contracts | Producer/data governance owners |
| Documentation standards | Knowledge base/platform owners |
| AI workflow packs | AI/model-risk owners |

---

## CODEOWNERS

CODEOWNERS can route review automatically.

Example:

```text
.github/workflows/*        @platform-team @security-team
docs/standards/*           @architecture-team
contracts/data-products/*  @data-governance-team
migrations/*               @backend-team
```

Use carefully. Too many required owners can slow delivery.

---

## Sensitive Files

Sensitive file categories:

- CI workflows
- deployment manifests
- security policies
- secrets templates
- auth middleware
- entitlement policy
- data-product contracts
- API schemas
- migrations
- release scripts
- dependency manifests
- package lockfiles
- AI prompt/tool policy

---

## Ownership Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| No owners for workflow changes | Supply-chain risk. |
| Too many owners for every file | Review bottleneck. |
| Owners stale | Review requests ignored. |
| Sensitive files not protected | Risky changes merge unnoticed. |
| Ownership only in people's heads | Onboarding and audit gap. |
| Generated files require wrong owners | Noise and delay. |

---

## Review Checklist

- Are sensitive file areas identified?
- Are owners current?
- Is CODEOWNERS too broad?
- Are workflow/security files protected?
- Are domain owners included for business rules?
- Are data contracts routed correctly?
- Are ownership changes reviewed?
- Is reviewer load manageable?

---

## Summary

Ownership-based review ensures the right people see risky changes.

It should improve control without becoming unnecessary bureaucracy.

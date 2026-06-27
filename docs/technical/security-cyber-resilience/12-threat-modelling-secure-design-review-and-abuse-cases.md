# Threat Modelling, Secure Design Review, and Abuse Cases

## Purpose

This file explains how to identify security risks before implementation through threat modelling and abuse-case analysis.

Threat modelling helps teams ask what can go wrong and how to control it.

---

## Threat Modelling Questions

- What are we building?
- What data is involved?
- Who are the actors?
- What are the trust boundaries?
- What can go wrong?
- What controls exist?
- What evidence proves controls?
- What residual risk remains?

---

## STRIDE

Common threat categories:

| Category | Meaning |
|---|---|
| Spoofing | Pretending to be someone/something else. |
| Tampering | Modifying data or code. |
| Repudiation | Denying an action due to missing audit. |
| Information disclosure | Exposing sensitive data. |
| Denial of service | Making system unavailable. |
| Elevation of privilege | Gaining unauthorized permissions. |

---

## Abuse Cases

Abuse cases describe misuse.

Examples:

- unauthorized advisor accesses restricted portfolio
- user repeats write request to create duplicate report
- attacker injects prompt to retrieve restricted data
- operator replays workflow without approval
- dependency token leaked through logs
- BFF exposes raw upstream error
- UI hides control but API allows action

---

## Secure Design Review

A secure design review should cover:

- data classification
- authentication
- authorization
- caller context
- trust boundaries
- input validation
- output filtering
- secrets
- logging/metrics/traces
- dependency and runtime security
- AI/tool risks
- audit evidence
- incident response

---

## Threat Modelling Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Threat model after build | Late rework. |
| Only infrastructure threats considered | Business abuse missed. |
| No abuse cases | Real misuse scenarios missed. |
| Controls not mapped to threats | Gaps hidden. |
| Residual risk not recorded | Risk accepted silently. |
| AI flows excluded | Prompt/tool risks missed. |

---

## Review Checklist

- Is system diagram available?
- Are trust boundaries clear?
- Is sensitive data identified?
- Are actors identified?
- Are STRIDE categories considered?
- Are abuse cases written?
- Are controls mapped?
- Are tests/gates defined?
- Is residual risk documented?
- Is review updated after design changes?

---

## Summary

Threat modelling turns security from reaction into design.

The best time to find a security gap is before the code and contracts are frozen.

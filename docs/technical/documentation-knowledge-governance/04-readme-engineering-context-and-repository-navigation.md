# README, Engineering Context, and Repository Navigation

## Purpose

This file explains how to write repository entry-point documentation.

A README should help a reader understand what the repository is, what it owns, how to run it, how to validate it, and where to go next.

---

## README Purpose

A good README answers:

- What is this repository?
- What does it own?
- What does it not own?
- What are the main runtime surfaces?
- How do I install and run it?
- How do I test it?
- How do I validate CI locally?
- What are the main docs?
- What is the current support status?
- Who owns it?

---

## Recommended README Sections

```text
Purpose
Current status
Responsibilities
Explicit non-responsibilities
Architecture overview
Runtime surfaces
Repository layout
Local development
Validation commands
CI/CD lanes
Operations
Security and data-safety notes
Documentation map
Contribution workflow
```

---

## Engineering Context File

A repository engineering context file can capture:

- architectural boundaries
- standards followed
- key commands
- CI expectations
- runtime posture
- test posture
- docs map
- agent/coding guidance
- known limitations
- current quality bar

It should be implementation-backed and maintained.

---

## Navigation

Good navigation includes:

- root README
- local folder READMEs
- document index
- reading order
- role-based links
- related packs
- source evidence links

Avoid forcing new joiners to search randomly.

---

## README Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| README is marketing only | Engineers cannot work. |
| Commands stale | Onboarding fails. |
| Responsibilities vague | Boundary confusion. |
| No non-responsibilities | Scope drift. |
| No docs map | Knowledge hidden. |
| Current status missing | Overclaims. |
| README duplicates every doc | Hard to maintain. |

---

## Review Checklist

- Does README explain purpose?
- Does it state ownership?
- Does it state non-ownership?
- Are commands current?
- Are CI lanes described?
- Are docs linked?
- Is current status honest?
- Are examples safe?
- Is it useful to a new joiner?
- Is it concise enough to maintain?

---

## Summary

README is the front door of the repository.

A strong README saves time for every future reader.

# Copilot Experience Design, Human-in-the-Loop, and User Trust

## Purpose

This file explains how to design AI copilots that support users safely.

A copilot should help users think, draft, search, and decide. It should not hide uncertainty or bypass required human judgment.

---

## Copilot Principles

A good copilot:

- explains what it can do
- explains limitations
- shows sources where needed
- allows user edit/reject
- requires approval for sensitive output
- avoids unsupported claims
- preserves user control
- records audit where required
- handles errors safely

---

## Human-in-the-Loop

Human review is required when output affects:

- client communication
- investment/advisory content
- portfolio action
- report publication
- workflow approval
- regulatory evidence
- financial decision support
- sensitive operational action

---

## UX Trust Signals

Use trust signals such as:

- source citations
- freshness indication
- confidence or limitation note
- review-required badge
- unsupported warning
- permission-blocked state
- generated content label
- last updated/source version

---

## User Controls

Users should be able to:

- inspect sources
- edit generated text
- accept/reject
- regenerate
- provide feedback
- flag issue
- view limitations
- see approval status

---

## Copilot Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| AI answer appears as official truth | User over-trust. |
| No source shown | Weak grounding. |
| Auto-send client content | Regulatory risk. |
| No edit/reject path | User control weak. |
| No warning for stale source | Wrong answer. |
| Confidence overstated | Misleading UX. |
| Feedback ignored | Quality does not improve. |

---

## Review Checklist

- Is user control preserved?
- Is output labelled as AI-generated?
- Are sources visible where required?
- Are limitations shown?
- Is human review required for sensitive use?
- Are approval states audited?
- Are permission-blocked states safe?
- Is user feedback captured?
- Are trust signals tested?

---

## Summary

A copilot should increase user confidence through transparency and control, not by pretending to be certain.

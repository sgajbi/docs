# Grounding, Citations, Evidence, and Answer Traceability

## Purpose

This file explains how AI answers should be grounded and traceable.

Grounding reduces hallucination risk by tying output to approved sources.

---

## Grounding

Grounding means the model response is based on specific retrieved or provided evidence.

Grounded output should show:

- which sources were used
- what limitations apply
- whether source freshness is acceptable
- whether answer is inferred or directly supported
- whether human review is required

---

## Citations

Citations should point to:

- document
- section
- line/range where possible
- version
- timestamp or review date where useful

Avoid citing sources that do not support the claim.

---

## Evidence Bundle

An evidence bundle may include:

```text
question
retrieved sources
source versions
prompt template version
model name/version
answer
citations
confidence
limitations
safety checks
human review status
timestamp
```

---

## Unsupported Claims

AI should refuse or qualify when:

- source not available
- source contradicts
- user lacks access
- topic outside approved use
- source stale
- claim requires official calculation
- action requires approval

---

## Traceability Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Answer without source | Hallucination hard to detect. |
| Citation does not support claim | False evidence. |
| Stale source not marked | Wrong guidance. |
| Generated answer treated as official source | Governance issue. |
| Evidence bundle stores sensitive content | Data leakage. |
| Human review status missing | Publication risk. |

---

## Review Checklist

- Are answers grounded?
- Are citations accurate?
- Is source freshness known?
- Are unsupported claims refused?
- Are limitations shown?
- Is evidence bundle captured?
- Is human review status tracked?
- Is evidence safe to retain?
- Are grounding tests present?

---

## Summary

Enterprise AI needs traceability.

An answer is more trustworthy when users can see what evidence supports it and what limitations apply.

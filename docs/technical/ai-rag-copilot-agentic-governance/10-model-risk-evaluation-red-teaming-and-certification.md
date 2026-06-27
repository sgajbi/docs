# Model Risk, Evaluation, Red Teaming, and Certification

## Purpose

This file explains how to evaluate AI systems and produce model-risk evidence.

Enterprise AI systems require quality, safety, and risk controls before they are promoted as supported capabilities.

---

## Evaluation Dimensions

Evaluate:

- factual accuracy
- grounding
- citation correctness
- relevance
- completeness
- refusal correctness
- hallucination rate
- prompt-injection resistance
- entitlement enforcement
- tone and style
- structured output validity
- latency
- cost
- user feedback
- regression stability

---

## Evaluation Dataset

An evaluation dataset should include:

- normal questions
- edge cases
- unsupported questions
- ambiguous questions
- stale-source questions
- permission-blocked cases
- prompt-injection cases
- expected citations
- expected refusals
- expected structured output

Data must be synthetic or approved.

---

## Red Teaming

Red teaming tests misuse and adversarial behavior.

Examples:

- prompt injection
- data exfiltration attempt
- role manipulation
- source confusion
- citation fabrication
- tool misuse
- jailbreak attempts
- unsafe client-ready content
- hidden instruction extraction

---

## Certification

AI certification should include:

- use-case definition
- risk tier
- data classification
- source approval
- model selection evidence
- prompt version
- evaluation results
- safety tests
- human review workflow
- monitoring plan
- rollback plan
- residual risk

---

## Evaluation Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Manual testing only | Non-repeatable confidence. |
| No refusal tests | Unsupported answers. |
| No citation tests | False grounding. |
| No prompt-injection tests | Security risk. |
| No regression after prompt change | Quality drift. |
| Eval data includes real sensitive data | Privacy risk. |
| Certification never expires | Stale assurance. |

---

## Review Checklist

- Is use case risk-tiered?
- Is evaluation dataset defined?
- Are expected answers/refusals clear?
- Are citations evaluated?
- Are security cases included?
- Is red teaming done for high-risk use?
- Are results recorded?
- Is certification state defined?
- Are residual risks documented?
- Is recertification cadence defined?

---

## Summary

AI quality must be measured.

A convincing demo is not the same as evaluated, governed, and certified behavior.

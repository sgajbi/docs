# AI-Assisted Engineering, Copilots, Agents and Governed Automation

## 1. Why AI-assisted engineering matters

AI is changing how software is designed, coded, tested, documented and operated. For wealth platforms, AI can materially improve productivity, but only if it is controlled.

Useful applications:

- drafting requirements
- generating test cases
- explaining legacy code
- producing API examples
- refactoring
- generating documentation
- creating migration scripts
- summarizing incidents
- writing runbooks
- assisting code review
- proposing monitoring checks
- creating QA regression packs

The risk is that AI can also produce plausible but wrong code, hidden security issues, invalid domain assumptions and undocumented changes.

## 2. AI engineering operating principles

| Principle | Meaning |
|---|---|
| Human accountable | AI assists; humans own decisions. |
| Source-grounded | Use approved docs, repo context and source evidence. |
| Small scoped changes | Avoid large uncontrolled rewrites. |
| Test-driven | Require tests for AI-generated logic. |
| Review mandatory | AI output must go through normal PR review. |
| Secure by default | No secrets, client data or restricted data in prompts. |
| Traceable | Prompt, context and output should be reconstructable where needed. |
| Policy-aware | Agents must follow architecture, security and coding standards. |
| Reversible | Changes should be easy to revert. |
| Evidence-backed | CI, tests and review evidence matter more than AI confidence. |

## 3. Good use cases

| Use case | Value |
|---|---|
| Documentation pack generation | Creates consistent knowledge base quickly. |
| Test scenario generation | Improves edge-case coverage. |
| Refactoring assistance | Speeds modularization and cleanup. |
| API schema examples | Helps developers and BAs align. |
| Legacy code explanation | Reduces onboarding time. |
| Incident summary | Speeds support communication. |
| Runbook drafting | Improves operations readiness. |
| Data-quality rule suggestions | Identifies missing validations. |
| Migration checklist generation | Captures repeatable controls. |
| Code review hints | Highlights complexity, duplication and missing tests. |

## 4. Restricted use cases

Use stronger controls for:

- financial calculation logic
- suitability rules
- entitlement logic
- client-data access
- security-sensitive code
- production data fixes
- database migrations
- AI-generated trading/advisory recommendations
- model/prompt changes
- cross-border restrictions
- encryption/authentication changes

## 5. Prompt patterns for engineering

### Refactoring prompt

```text
Refactor the target module to improve maintainability without changing business behaviour.
Preserve public API and test behaviour.
Read existing tests first.
Add tests for any uncovered edge cases.
Keep changes small and reviewable.
Do not introduce new dependencies unless justified.
Update documentation and run static checks.
```

### Test generation prompt

```text
Given the business rules and existing test structure, generate additional edge-case tests.
Focus on product lifecycle, invalid input, boundary dates, currency precision, duplicate events, reversal/correction, and degraded data.
Do not change production code.
Explain why each test is needed.
```

### Documentation prompt

```text
Create implementation-backed documentation.
Use only code, tests, configs and approved architecture docs as source.
Separate supported capability, partial support, unsupported state and future candidate.
Include API examples, data model notes, operational controls and QA scenarios.
```

## 6. Agent workflow guardrails

For coding agents:

| Guardrail | Purpose |
|---|---|
| Work on feature branch | Avoid uncontrolled main changes. |
| Small commits | Enable review and rollback. |
| Read context files first | Align to repo standards. |
| Run tests before commit | Avoid broken state. |
| Do not hide failing tests | Preserve trust. |
| Do not make policy decisions silently | Escalate ambiguous business rules. |
| Do not access secrets/client data | Protect confidentiality. |
| Raise PR with summary and evidence | Improve review quality. |

## 7. AI-generated code review checklist

Reviewers should check:

- Does the code follow architecture boundaries?
- Are domain assumptions correct?
- Are decimal/money types handled correctly?
- Are edge cases tested?
- Are security controls preserved?
- Are entitlements enforced?
- Are errors structured and actionable?
- Are logs safe and useful?
- Are dependencies justified?
- Is performance acceptable?
- Are migrations safe?
- Are docs updated?
- Can support operate it?

## 8. AI-assisted QA

AI can generate:

- scenario matrix
- test data permutations
- negative tests
- performance test ideas
- reconciliation checks
- report QA assertions
- mutation candidates
- production monitoring assertions

Example:

```text
Generate test cases for a structured note lifecycle with subscription, monthly coupon, missed coupon, memory coupon, autocall, final maturity, knock-in and physical settlement.
Include transaction types, position impact, cash impact and expected reporting treatment.
```

## 9. AI-assisted operations

AI can help support teams by:

- summarizing logs
- correlating incidents
- drafting stakeholder updates
- suggesting runbook steps
- identifying similar historical incidents
- explaining error codes
- generating postmortem drafts
- recommending regression tests

Controls:

- AI should not execute production actions without approval
- AI must respect entitlements
- outputs should cite logs/events/runbooks where possible
- operational summaries should be reviewed before distribution

## 10. AI and documentation quality

AI is excellent at generating consistent structure, but it can repeat vague content.

Quality rules:

- require concrete examples
- require product-specific treatment
- require transaction and data model implications
- require QA scenarios
- require source notes
- require "supported vs unsupported" distinction
- avoid duplicated sections
- avoid generic filler
- include implementation constraints

## 11. Model and prompt lifecycle

Treat prompts and agents as governed artefacts:

| Artefact | Governance |
|---|---|
| System prompt | Versioned, reviewed, tested. |
| Tool permissions | Least privilege. |
| Retrieval corpus | Approved sources only. |
| Prompt template | Versioned and evaluated. |
| Evaluation set | Regression prompts and expected behaviours. |
| Output policy | What can/cannot be said or done. |
| Audit record | Who asked, context used, output, action taken. |

## 12. Risks

| Risk | Mitigation |
|---|---|
| Hallucinated logic | Ground in code/docs; require tests. |
| Security vulnerability | SAST/SCA/review. |
| Data leakage | Prompt/data controls. |
| Overbroad refactor | Scope limits and small commits. |
| Hidden dependency | Dependency policy. |
| False documentation | Implementation-backed review. |
| Prompt injection | Tool and retrieval isolation. |
| Excessive agency | Human approval and action boundaries. |

## 13. AI maturity roadmap

| Stage | Capability |
|---|---|
| 1. Individual assistance | Developers use AI for explanation and drafts. |
| 2. Team standards | Prompt libraries, code review checklist and AI usage policy. |
| 3. Repo agents | Controlled agents for docs, tests and refactoring. |
| 4. Delivery integration | AI supports PR review, test generation and release notes. |
| 5. Operations integration | AI supports incident triage and runbooks. |
| 6. Governed autonomy | Agents execute low-risk tasks with approval and audit. |

## 14. Practitioner explanation

> "AI-assisted engineering is a productivity multiplier, but in wealth platforms it must be governed. I would use AI for documentation, tests, refactoring, code explanation, runbooks and incident summaries, but require normal SDLC controls: feature branches, small commits, CI, code review, security scans, domain SME validation and audit evidence. The objective is not to replace engineering discipline, but to make engineering discipline faster and more consistent."

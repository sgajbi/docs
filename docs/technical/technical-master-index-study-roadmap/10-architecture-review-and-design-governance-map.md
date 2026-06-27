# Architecture Review and Design Governance Map

## Purpose

This file maps architecture-review topics to the relevant knowledge guides.

Use it when reviewing new services, APIs, migrations, reporting systems, AI features, or production changes.

---

## Architecture Review Areas

| Review Area | Guides |
|---|---|
| Service boundary | 2, 13, 15 |
| API contract | 3, 9 |
| Data ownership | 4, 15 |
| Runtime design | 6, 11 |
| Observability | 7, 19 |
| Security | 8, 14 |
| Testing strategy | 9 |
| CI/CD and release | 5, 10 |
| Performance/resilience | 11 |
| Documentation | 12 |
| Leadership/governance | 13 |
| AI feature | 14 |
| Reporting/archive | 16 |
| Migration/cutover | 18 |
| Productization | 17 |
| Production support | 19 |

---

## Universal Architecture Review Questions

Ask:

```text
What capability is being delivered?
Who owns the truth?
What are the service boundaries?
What contracts are exposed?
What data is stored or moved?
What are the failure modes?
What is the security model?
What is the support model?
How is it tested?
How is it observed?
How is it released?
What evidence proves readiness?
```

---

## Review Output

Each review should produce:

- decision
- actions
- risks
- owners
- evidence expectations
- ADR/RFC if needed
- follow-up date

---

## Summary

Architecture review should not be a slide-review ceremony.

It should be a practical risk, ownership, and evidence review.

# Trust Telemetry, Runtime Evidence, and Certification

## Purpose

This file explains how runtime trust telemetry and certification evidence establish whether a data product is actually ready for consumer reliance.

Static documentation is not enough. Trust should be evidenced.

---

## Trust Telemetry

Trust telemetry is machine-readable evidence about data-product posture.

It may include:

- product identity
- version
- producer
- generatedAt
- runtime source
- supportability state
- freshness bucket
- quality state
- SLO posture
- access-policy posture
- evidence references
- lineage availability
- certification state
- known blockers

---

## Runtime Versus Static Evidence

| Static Evidence | Runtime Evidence |
|---|---|
| Contract file | Actual telemetry snapshot |
| Declared schema | Validated live schema output |
| Planned SLO | Observed SLO status |
| Proposed product | Runtime-produced trust state |
| Manual claim | Machine-generated evidence |

Static evidence can start maturity. Runtime evidence strengthens trust.

---

## Certification

Certification is a controlled decision that required checks pass.

Certification may validate:

- producer contract
- consumer contract
- schema
- vocabulary
- lineage
- freshness
- quality
- SLO policy
- access policy
- evidence policy
- API/event/batch surface
- telemetry snapshot
- gateway publication
- UI consumption where applicable
- runbook/dashboard/alert availability

---

## Certification Output

A certification output should include:

```text
product identity
version
certification state
checks run
checks passed
checks failed
evidence references
generated timestamp
residual risks
next review date
operator guidance
```

---

## Certification States

| State | Meaning |
|---|---|
| NOT_ASSESSED | No certification run. |
| BLOCKED | Required evidence missing or failed. |
| PARTIAL | Some checks passed but material gaps remain. |
| CERTIFIED | Required checks passed. |
| DEGRADED | Certified product currently operating below normal posture. |
| EXPIRED | Evidence too old. |
| REVOKED | Certification withdrawn due to break or risk. |

---

## Certification Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Certification based on README | False readiness. |
| Static fixture treated as runtime telemetry | Misleading evidence. |
| Certification ignores consumers | Downstream risk unknown. |
| Certification ignores access policy | Entitlement risk. |
| Certification has no expiry | Stale evidence. |
| Failures hidden from catalog | Consumers over-trust product. |
| UI shows certified when evidence missing | Product trust issue. |

---

## Review Checklist

- Is telemetry machine-readable?
- Is telemetry runtime-backed where required?
- Is certification gate defined?
- Are checks explicit?
- Are failures visible?
- Is certification timestamped?
- Are residual risks recorded?
- Is evidence safe?
- Is certification expiry defined?
- Are gateway/UI publication checks included where needed?

---

## Summary

Trust is earned through evidence.

A data product should not be called certified until contracts, runtime telemetry, policies, and consumer-facing evidence align.

# SLO, Access, Evidence, and Policy Contracts

## Purpose

This file explains how SLO, access, and evidence policies make data products governable and certifiable.

A data product needs more than schema. It needs operational and governance promises.

---

## SLO Policy

An SLO defines service expectations.

Examples:

- availability
- latency
- freshness
- completeness
- certification recency
- telemetry recency
- data-quality pass rate
- recovery time
- publication frequency

Example:

```text
Product freshness SLO:
The holdings snapshot must be available by 08:00 local business time for 99% of business days.
```

---

## Access Policy

Access policy defines who can consume and under what scope.

It may include:

- consumer system
- user role
- tenant
- region
- booking center
- data classification
- field-level restrictions
- purpose limitation
- approval workflow
- entitlement dependency
- audit requirement

---

## Evidence Policy

Evidence policy defines what proof is required.

Examples:

- schema validation artifact
- quality validation result
- lineage bundle
- runtime telemetry snapshot
- API certification output
- reconciliation result
- access-policy validation
- consumer test result
- runbook link
- dashboard/alert reference

---

## Policy As Contract

Policies should be:

- versioned
- machine-readable where possible
- validated in CI
- linked from data-product contract
- visible to consumers
- updated through review
- tied to certification

---

## Policy Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| SLO stated in prose only | Hard to validate. |
| Access policy missing | Data leakage risk. |
| Evidence not defined | Certification subjective. |
| Policy not versioned | Change tracking weak. |
| Customer evidence includes internal details | Information leakage. |
| Consumer access assumed from service name | Entitlement errors. |
| Certification does not check SLO/access/evidence | Governance incomplete. |

---

## Review Checklist

- Is SLO policy defined?
- Is access policy defined?
- Is evidence policy defined?
- Are policies versioned?
- Are policies machine-checkable?
- Are policies linked to product?
- Are consumers covered?
- Are restricted fields handled?
- Does certification validate policies?
- Is customer/internal evidence separated?

---

## Summary

SLO, access, and evidence policies turn data-product governance from opinion into a testable contract.

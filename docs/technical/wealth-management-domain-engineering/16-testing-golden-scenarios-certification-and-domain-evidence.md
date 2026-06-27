# Testing, Golden Scenarios, Certification, and Domain Evidence

## Purpose

This file explains how to test wealth-management domain systems.

Testing must prove business correctness, data lineage, calculation methodology, entitlements, reporting evidence, and migration continuity.

---

## Golden Scenarios

Golden scenarios should cover:

- simple buy/hold/sell
- income cashflow
- fee and tax cashflow
- external deposit/withdrawal
- multi-currency valuation
- missing price
- stale FX
- backdated transaction
- benchmark comparison
- TWR period calculation
- MWR cashflow timing
- contribution by asset class
- concentration breach
- suitability failure
- report generation
- archive retrieval
- permission-blocked access

---

## Performance Golden Tests

Test:

- opening/closing value
- daily return
- cashflow treatment
- geometric linking
- benchmark return
- relative return
- annualization
- unsupported inputs

---

## Risk Golden Tests

Test:

- allocation
- concentration threshold
- volatility
- drawdown
- VaR method
- stale data
- missing price
- suitability rule

---

## Reporting Golden Tests

Test:

- report data snapshot
- generated values
- warnings
- template version
- document hash
- archive metadata
- entitlement-filtered sections

---

## Migration Certification

Migration certification should prove:

- record counts
- holdings totals
- cash balances
- valuation totals
- transaction continuity
- performance continuity
- report archive access
- entitlement scope
- source-to-target mapping
- reconciliation exceptions

---

## Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Only API status tested | Domain correctness unproved. |
| No golden calculation examples | Regression risk. |
| Test data not synthetic | Privacy issue. |
| Missing stale/partial scenarios | Supportability weak. |
| Migration sign-off manual only | Evidence weak. |
| Report visual test without data proof | Misleading confidence. |

---

## Review Checklist

- Are golden scenarios defined?
- Is data synthetic?
- Are performance formulas tested?
- Are risk metrics tested?
- Are entitlement cases tested?
- Are stale/partial cases tested?
- Are report outputs reproducible?
- Is migration evidence generated?
- Are certification gaps documented?

---

## Summary

Wealth-domain testing should prove the numbers, the lineage, the permissions, and the evidence.

A passing endpoint is not enough.

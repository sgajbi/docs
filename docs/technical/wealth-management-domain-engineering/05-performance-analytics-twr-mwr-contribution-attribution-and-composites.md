# Performance Analytics: TWR, MWR, Contribution, Attribution, and Composites

## Purpose

This file explains performance analytics concepts and engineering concerns.

Performance is a methodology-driven domain. It must be deterministic, explainable, testable, and reproducible.

---

## Time-Weighted Return

TWR measures investment performance independent of external cashflow timing.

Common engineering needs:

- daily valuation points
- cashflow timing rules
- geometric linking
- period aggregation
- benchmark comparison
- support for MTD/QTD/YTD/1Y/3Y/5Y/SI
- net/gross treatment where required
- handling missing valuation days
- methodology documentation

---

## Money-Weighted Return

MWR or IRR measures return considering timing and size of cashflows.

Common engineering needs:

- cashflow series
- opening/closing market value
- external flows
- numerical solver
- convergence handling
- unsupported cases
- annualization rules
- period definitions

---

## Contribution

Contribution explains how portfolio components contributed to return.

Examples:

- asset class contribution
- instrument contribution
- sector contribution
- currency contribution
- sleeve contribution

Requires weights, returns, and methodology.

---

## Attribution

Attribution explains why portfolio performance differed from benchmark.

Common models include allocation, selection, interaction, and currency effects depending on methodology.

Engineering concerns:

- benchmark linkage
- effective weights
- classification hierarchy
- currency treatment
- smoothing/linking method
- residual handling
- drill-down evidence

---

## Composites

Composite performance groups portfolios under a strategy or mandate.

Engineering concerns:

- inclusion/exclusion rules
- effective dates
- asset-weighting
- membership changes
- dispersion
- benchmark assignment
- audit trail
- methodology compliance

---

## Performance Data Inputs

Inputs may include:

- valuation
- cashflows
- holdings
- transactions
- prices
- FX
- benchmark returns
- benchmark weights
- fees/taxes treatment
- portfolio membership

---

## Performance Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| TWR cashflow timing undocumented | Results disputed. |
| Benchmark comparison uses wrong currency | Incorrect relative performance. |
| Missing valuation silently filled | Misleading returns. |
| Contribution not reconciling to total return | Trust issue. |
| Attribution model not documented | Business cannot explain results. |
| UI calculates performance locally | Methodology drift. |
| No golden examples | Regression risk. |

---

## Review Checklist

- Which performance method is used?
- Are period definitions clear?
- Are cashflow rules documented?
- Are benchmark inputs governed?
- Is currency treatment clear?
- Is missing data handled explicitly?
- Are results reproducible?
- Are golden examples available?
- Is methodology versioned?
- Is lineage captured?

---

## Summary

Performance analytics is not just calculation code.

It is a governed methodology with source inputs, assumptions, reproducibility, and evidence.

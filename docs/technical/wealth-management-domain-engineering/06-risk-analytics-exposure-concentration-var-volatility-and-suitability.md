# Risk Analytics: Exposure, Concentration, VaR, Volatility, and Suitability

## Purpose

This file explains core risk analytics concepts in wealth platforms.

Risk analytics helps advisors, portfolio managers, operations, and clients understand portfolio uncertainty, concentration, exposure, and suitability.

---

## Exposure

Exposure measures sensitivity or allocation to risk dimensions.

Examples:

- asset class exposure
- currency exposure
- issuer exposure
- sector exposure
- country exposure
- counterparty exposure
- product type exposure
- derivative notional exposure
- look-through exposure

---

## Concentration

Concentration identifies overexposure to specific dimensions.

Examples:

- top holdings
- issuer concentration
- counterparty concentration
- sector concentration
- country concentration
- currency concentration
- bulk/large position risk

---

## Volatility and Drawdown

Volatility measures variability of returns.

Drawdown measures decline from peak to trough.

Common metrics:

- annualized volatility
- maximum drawdown
- downside deviation
- Sharpe ratio
- Sortino ratio
- beta
- tracking error

---

## Value at Risk

VaR estimates potential loss over a confidence level and horizon.

Common methods:

- historical simulation
- variance-covariance
- Monte Carlo

Engineering concerns:

- return history
- confidence level
- horizon
- currency
- missing data
- assumptions
- methodology version
- limitations

---

## Suitability

Suitability checks whether portfolio or proposal aligns with client risk profile, product knowledge, mandate, restrictions, and regulatory rules.

Engineering concerns:

- client risk profile
- product classification
- concentration limits
- mandate restrictions
- advisor override rules
- evidence and audit
- jurisdiction-specific controls

---

## Risk Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Exposure dimensions inconsistent | Risk views disagree. |
| Missing risk data shown as zero | Misleading safety. |
| VaR methodology undocumented | Output not explainable. |
| Suitability rule only in UI | Control bypass. |
| Concentration thresholds hardcoded | Poor governance. |
| Stale prices used without warning | Wrong risk estimate. |
| No limitations displayed | Over-trust. |

---

## Review Checklist

- Which risk metrics are supported?
- What inputs are required?
- Is methodology documented?
- Are limitations visible?
- Is data freshness checked?
- Are exposure dimensions canonical?
- Are thresholds governed?
- Are suitability rules server-side?
- Is evidence captured?
- Are golden scenarios tested?

---

## Summary

Risk analytics must be transparent about methodology, input quality, limitations, and suitability controls.

A risk number without context can be dangerous.

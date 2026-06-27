# Holdings, Positions, Transactions, Cashflows, and Valuation

## Purpose

This file explains core portfolio data concepts used across wealth platforms.

Holdings, transactions, cashflows, and valuation are the foundation for performance, risk, reporting, advisory, and client review.

---

## Holdings and Positions

A holding or position represents an asset held by an account or portfolio at a point in time.

Common fields:

- portfolio id
- account id
- instrument id
- quantity
- book cost
- market price
- market value
- accrued interest
- currency
- valuation date
- source system
- supportability state

---

## Transactions

A transaction represents an event that changes holdings, cash, or reporting state.

Examples:

- buy
- sell
- dividend
- coupon
- fee
- tax
- transfer in
- transfer out
- corporate action
- subscription
- redemption
- FX conversion
- cash deposit
- cash withdrawal

---

## Cashflows

Cashflows are important for performance and reporting.

Cashflow classification may include:

- external inflow
- external outflow
- income
- fee
- tax
- internal transfer
- trading cashflow
- corporate action
- accrual adjustment

Classification affects performance methodology.

---

## Valuation

Valuation calculates market value.

Inputs:

- quantity
- market price
- FX rate
- accrued income
- valuation date
- pricing source
- instrument currency
- portfolio/reporting currency

---

## Position Versus Transaction

| Concept | Meaning |
|---|---|
| Position | State at a point in time. |
| Transaction | Event that changes state. |
| Cashflow | Money movement or performance-affecting flow. |
| Valuation | Monetary value of a position at a time. |

---

## Data Quality Concerns

Common issues:

- missing price
- stale price
- missing FX
- late transaction
- backdated correction
- duplicate transaction
- corporate action not processed
- incorrect cashflow classification
- reconciliation break
- valuation date mismatch

---

## Engineering Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| Transactions API returns unlimited history | Performance incident. |
| Cashflows not classified | Performance results wrong. |
| Valuation hides stale price | Misleading reporting. |
| Backdated transaction not replayed | Historical analytics wrong. |
| Position source not recorded | Lineage weak. |
| UI recalculates market value differently | Inconsistent outputs. |

---

## Review Checklist

- Are holdings source-owned?
- Are transactions paginated?
- Is cashflow classification documented?
- Is valuation methodology clear?
- Are price and FX sources known?
- Are stale/missing inputs represented?
- Are backdated corrections supported?
- Is lineage captured?
- Are reconciliation checks defined?

---

## Summary

Holdings, transactions, cashflows, and valuation are the raw material of wealth analytics.

Get these wrong and everything downstream becomes unreliable.

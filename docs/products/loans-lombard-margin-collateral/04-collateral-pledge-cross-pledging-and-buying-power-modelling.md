# 04 — Collateral, Pledge, Cross-Pledging and Buying-Power Modelling

## 1. Why pledge modelling is hard

Collateral is not only a holding. It becomes collateral only when a legal/security relationship links the asset to a credit exposure.

Common errors in platforms:

- assuming every asset in a portfolio is pledgeable
- double-counting the same collateral across multiple credit lines
- applying LTV without checking pledge relationship
- allowing transitive collateral support where not legally allowed
- failing to reserve collateral for in-flight orders
- mixing borrower, collateral owner and portfolio owner
- treating portfolio-level pledge and client-level pledge as the same
- ignoring jurisdiction, custody and account blocking status

## 2. Core concepts

| Concept | Meaning |
|---|---|
| Collateral owner | Legal owner of pledged asset |
| Borrower | Entity using credit line |
| Credit beneficiary | Entity whose loan/exposure is supported |
| Pledge provider | Entity providing collateral support |
| Pledged portfolio | Account/portfolio whose assets are pledged |
| Pledged instrument | Specific security/cash asset pledged |
| Collateral pool | Group of pledged assets supporting one or more credit lines |
| Pledge direction | A → B means A supports B |
| Shared collateral | Same asset supports more than one exposure |
| Encumbrance | Restriction because asset is pledged/blocked/reserved |

## 3. Pledge relationship model

A robust pledge model should not rely only on account hierarchy.

```text
Pledge Relationship
  provider_entity_id
  provider_portfolio_id optional
  beneficiary_entity_id
  beneficiary_credit_line_id optional
  pledged_scope: ENTITY / PORTFOLIO / INSTRUMENT / AMOUNT
  effective_date
  expiry_date
  priority_rank
  max_support_amount optional
  allowed_exposure_types
  status
```

## 4. Pledge scope

| Scope | Example | Modelling impact |
|---|---|---|
| Entity-level | All eligible assets of BR A support BR B | Broad collateral source |
| Portfolio-level | Portfolio A-10 supports B | Only that portfolio contributes |
| Instrument-level | Specific bond ISIN pledged | Only that holding contributes |
| Amount-level | Up to USD 500k support | Cap support amount |
| Product-specific | Supports loans but not derivatives | Exposure eligibility needed |

## 5. One-way pledge A → B

A one-way pledge means A’s collateral can support B’s credit line. It does not automatically mean B supports A.

```text
A → B
```

Interpretation:

- A is collateral provider.
- B is credit beneficiary.
- B may use agreed eligible surplus from A.
- A does not get support from B unless a separate pledge exists.

Important principle:

```text
A → B is directional, not symmetric.
```

## 6. No transitive support by default

If:

```text
A → B
B → C
```

Do not assume:

```text
A → C
```

Transitive support should be explicitly modelled and approved if the bank allows it. Most systems should default to **no transitive support** because collateral rights, legal enforceability, credit approvals and client consent are relationship-specific.

## 7. Circular pledge A → B → C → A

A circular structure can be represented in a graph:

```text
A → B → C → A
```

This creates serious modelling and risk issues:

- double counting collateral
- circular dependency of availability
- unclear liquidation order
- unclear legal priority
- infinite recursion in naive graph algorithms
- difficult stress testing
- potential wrong-way risk

Recommended platform treatment:

1. allow representation only if legally approved and explicitly configured;
2. prevent recursive collateral reuse by default;
3. calculate support using graph traversal with cycle detection;
4. cap support by each pledge agreement;
5. require priority ranking;
6. produce audit trail of which collateral supports which exposure;
7. avoid using downstream borrowed capacity as upstream collateral.

## 8. Own surplus first rule

For one-way pledge A → B, if B has its own collateral surplus, B should consume its own surplus first before consuming shared support from A.

This avoids reducing A’s buying power unnecessarily when B still has enough own collateral.

Formula:

```text
B Own Surplus = B Own Lending Value - B Own Exposure

If B In-Flight Order <= B Own Surplus:
    Impact on A = 0
Else:
    Impact on A = B In-Flight Order - B Own Surplus
```

Example:

| Item | Amount |
|---|---:|
| B own lending value | 500,000 |
| B own exposure | 300,000 |
| B own surplus | 200,000 |
| B in-flight order | 150,000 |
| Shared support consumed from A | 0 |

If B in-flight order is 280,000:

| Item | Amount |
|---|---:|
| B own surplus | 200,000 |
| B in-flight order | 280,000 |
| Shared support consumed from A | 80,000 |

## 9. Portfolio-level pledge example: A-10 and A-20

Scenario:

- BR A has two portfolios: A-10 and A-20.
- A-10 is pledged to credit line of B.
- A-10 and A-20 are both pledged to credit line of A.
- Shared asset source is A-10.

```text
A-10 → B
A-10 + A-20 → A
```

This is not simply BR A → B. It is portfolio-level sharing.

## 10. Correct handling of shared portfolio A-10

The collateral contribution of A-10 must be allocated between A and B without double counting.

Recommended calculation:

1. calculate lending value of A-10;
2. calculate lending value of A-20;
3. calculate A’s own exposure and B’s own exposure;
4. allocate A-20 to A first because it only supports A;
5. allocate B’s own collateral to B first, if any;
6. use A-10 as shared collateral only for remaining shortfall according to pledge priority;
7. reserve A-10 lending value for in-flight orders that consume it;
8. prevent both A and B from using the same A-10 surplus simultaneously.

## 11. Numerical example

Assume:

| Portfolio | Supports | Lending value |
|---|---|---:|
| A-10 | A and B | 400,000 |
| A-20 | A only | 300,000 |
| B-own | B only | 200,000 |

Current exposure:

| Borrower | Exposure |
|---|---:|
| A | 250,000 |
| B | 150,000 |

Base surplus before new order:

| Borrower | Own/supporting collateral | Exposure | Surplus |
|---|---:|---:|---:|
| A using A-20 first | 300,000 | 250,000 | 50,000 |
| B using B-own first | 200,000 | 150,000 | 50,000 |

Shared A-10 lending value = 400,000.

If B places new order of 120,000:

| Step | Amount |
|---|---:|
| B own surplus used first | 50,000 |
| Remaining B need | 70,000 |
| A-10 shared collateral reserved for B | 70,000 |
| Remaining A-10 available for A/B according to priority | 330,000 |

## 12. Allocation methods for shared collateral

| Method | Description | Pros | Cons |
|---|---|---|---|
| Own-surplus-first | Borrower consumes own collateral before shared collateral | Fair, intuitive | Needs ownership classification |
| Priority-based | Senior credit line consumes collateral first | Legal alignment | Can surprise other users |
| Pro-rata | Shared collateral allocated proportionally | Balanced | May not match credit agreements |
| First-come-first-served | In-flight reservations consume available support | Operationally simple | Can create unfair blocking |
| Optimization-based | Allocate to maximize total availability under constraints | Efficient | Harder to explain/audit |

For banking platforms, combine **legal priority + own-surplus-first + deterministic reservation**.

## 13. In-flight order handling

In-flight orders should reserve availability before settlement to avoid over-trading.

Order reservation lifecycle:

```text
Order created
  → pre-trade buying power check
  → reserve required collateral/credit
  → order partially/fully executed
  → reservation converted to exposure or released
  → settlement confirms final exposure/cash
```

## 14. Reservation types

| Reservation | Meaning |
|---|---|
| Cash reservation | Cash needed for buy/fees/tax |
| Credit reservation | Expected borrowing requirement |
| Collateral reservation | Collateral lending value consumed |
| FX reservation | FX conversion need |
| Product risk reservation | Margin/option/derivative requirement |
| Cross-pledge reservation | Shared collateral consumed from provider |

## 15. Collateral reservation record

| Field | Meaning |
|---|---|
| reservation_id | Unique ID |
| order_id | Linked order |
| borrower_id | Beneficiary consuming availability |
| provider_id | Collateral provider if cross-pledged |
| portfolio_id | Pledged portfolio source |
| credit_line_id | Facility affected |
| amount | Reserved lending value |
| currency | Availability currency |
| priority | Reservation priority |
| status | Active, consumed, released, expired |
| expiry_time | Timeout if order not executed |

## 16. Pledge graph design

Represent pledge relationships as a directed graph.

```text
Node = client / BR / CIF / portfolio / credit line
Edge = pledge relationship
```

Controls:

- cycle detection
- maximum traversal depth
- no transitive support unless explicit
- priority rank on edges
- max support cap on edges
- audit path for every collateral usage

## 17. Collateral release

Collateral can be released only if, after release:

```text
Remaining Lending Value >= Current Exposure + Required Buffer + Active Reservations
```

Release should check:

- settled exposure
- accrued interest
- in-flight orders
- pending drawdowns
- margin call status
- product-specific locks
- corporate action locks
- legal hold or court order
- unsettled trades on asset

## 18. Collateral substitution

Client may replace pledged asset X with asset Y.

Process:

1. check Y is eligible;
2. value Y and apply LTV/haircut;
3. ensure Y lending value is sufficient;
4. pledge Y;
5. release X only after Y is effective;
6. update collateral pool and buying power;
7. produce audit trail.

## 19. Reporting implications

A client/advisor report should show:

| Report item | Why important |
|---|---|
| Pledged assets | Client sees encumbered holdings |
| Lending value | Shows borrowable value after haircuts |
| Exposure | Shows outstanding borrowing |
| Available credit | Shows remaining capacity |
| Margin call buffer | Shows distance to shortfall |
| Cross-pledge impact | Shows whose assets support whom |
| Encumbrance flag | Prevents confusing pledged asset with freely disposable asset |
| Stress scenario | Shows impact of market fall |

## 20. Implementation rule

Never calculate buying power from portfolio value alone.

Buying power must be calculated from:

```text
eligible collateral
+ legal pledge relationships
+ LTV/haircuts
+ exposure
+ reservations
+ product restrictions
+ mandate/advisory controls
+ settlement state
+ intraday events
```

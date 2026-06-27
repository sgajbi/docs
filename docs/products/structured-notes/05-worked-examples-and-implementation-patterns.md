# 05 - Worked Examples and Implementation Patterns

Use these examples as the note-wrapper deep dive. For broader wrappers such as deposits, certificates, warrants, accumulators, decumulators, and structured funds, use [`../structured-products/08-worked-examples-and-implementation-patterns.md`](../structured-products/08-worked-examples-and-implementation-patterns.md).

## 1. Note Wrapper Versus Embedded Payoff

| Layer | Correct model | Common mistake |
|---|---|---|
| Legal holding | One note position issued by an issuer | Creating separate client positions for every embedded option |
| Issuer exposure | Issuer credit risk on note repayment | Treating principal protection as risk-free |
| Underlying exposure | Analytical look-through exposure | Treating underlyings as owned securities |
| Lifecycle events | Observations, coupon decisions, autocall, maturity | Booking observations as cash transactions |
| Economic postings | Subscription, coupon, sale, redemption, delivery, write-down | Posting cash before confirmed payment date |

## 2. Fixed Coupon Note

Scenario:

- Notional: 100,000.
- Coupon: 8% annual, paid quarterly.
- Tenor: 1 year.
- No principal protection beyond issuer promise.

Quarterly coupon:

```text
coupon = 100,000 x 8% x 0.25 = 2,000
```

Cashflows:

| Date | Event | Cashflow |
|---|---|---:|
| Trade settlement | Subscription | -100,000 |
| Q1 | Coupon | +2,000 |
| Q2 | Coupon | +2,000 |
| Q3 | Coupon | +2,000 |
| Maturity | Coupon + redemption | +102,000 |

Reporting:

- classify coupon as structured-note income or coupon income according to reporting policy;
- show issuer, maturity, coupon rate, source price, and risk label;
- do not calculate bond yield unless yield convention and pricing policy are supported.

## 3. Reverse Convertible With Physical Delivery

Scenario:

- Notional: 100,000.
- Underlying: listed equity.
- Strike: 50.
- Coupon: 10% annual.
- Maturity: 6 months.
- Final underlying price: 40.
- Settlement: physical delivery if final price below strike.

Delivered shares:

```text
shares delivered = notional / strike = 100,000 / 50 = 2,000 shares
```

Lifecycle:

```text
subscription -> coupon accrual/payment -> final observation -> physical delivery -> note close -> equity position created
```

Implementation controls:

- delivered shares are created only at maturity after settlement confirmation;
- fractional cash-in-lieu policy must be supported;
- delivered equity cost basis must follow local accounting and tax policy;
- performance should avoid double counting note loss and delivered equity market value;
- mandate check should verify the delivered equity is permitted.

## 4. Autocallable Phoenix Note

Scenario:

- Notional: 250,000.
- Quarterly coupon: 2%.
- Coupon barrier: 70%.
- Autocall barrier: 100%.
- Worst-of basket.

Observation path:

| Quarter | Worst-of performance | Coupon | Autocall | Action |
|---|---:|---|---|---|
| Q1 | 82% | Pay 5,000 | No | Continue |
| Q2 | 104% | Pay 5,000 | Yes | Redeem notional |

Postings:

| Posting | Amount |
|---|---:|
| Q1 coupon | +5,000 |
| Q2 coupon | +5,000 |
| Autocall redemption | +250,000 |
| Position quantity/nominal | Closed |

Controls:

- observation result must be reproducible from source fixing and term-sheet logic;
- autocall event should be lifecycle state before payment date;
- coupon and redemption should reconcile to custodian cash;
- matured/autocalled notes should not remain active in reporting.

## 5. Credit-Linked Note Credit Event

Scenario:

- Note notional: 500,000.
- Reference entity suffers a credit event.
- Recovery value: 35%.

Simplified redemption:

```text
redemption = 500,000 x 35% = 175,000
loss = 325,000
```

Required evidence:

- issuer notice or calculation-agent notice;
- reference entity and reference obligation details;
- event determination date;
- recovery or auction result;
- settlement instruction and cash reconciliation;
- client-facing explanation of issuer risk versus reference-entity credit risk.

Reporting:

- separate issuer credit risk from reference credit event risk;
- show write-down or loss according to accounting policy;
- preserve event audit trail for future client statement and performance review.

## 6. Dual Currency Note

Scenario:

- Investor subscribes USD 100,000.
- Alternate currency: CHF.
- Strike: 0.9000 USD/CHF.
- Coupon: 5% for one month.
- Final spot triggers alternate currency settlement.

Platform treatment:

- close the note;
- book coupon in the contractual currency;
- book alternate-currency cash as redemption according to term sheet;
- do not book a separate FX trade unless the client subsequently converts cash;
- show settlement currency, strike, fixing source, final spot, and cash amount.

QA scenarios:

| Scenario | Expected behavior |
|---|---|
| Fixing source missing | block settlement determination |
| Currency-pair direction reversed | sign/rate validation fails |
| Alternate currency not enabled | settlement exception and operations workflow |
| Early unwind | issuer bid used, not maturity conversion formula |

## 7. Barrier State And Client Reporting

Client reports should not hide barrier state.

| State | Report treatment |
|---|---|
| Active, no barrier breached | Show note value, maturity, coupon status, and source date |
| Near barrier | Show risk alert or advisor-only warning if policy allows |
| Barrier breached | Show breached status and explain maturity consequence |
| Observation missing | Show support-limited status; do not infer coupon or autocall |
| Autocall pending | Show pending redemption until cash settlement confirms |
| Matured but unpaid | Show matured pending settlement; do not keep as active investment |

## 8. Valuation And Performance Treatment

| Component | Treatment |
|---|---|
| Subscription | Investment cash outflow and note cost |
| Coupon | Income or structured coupon according to policy |
| Market value | Issuer/vendor/exchange/model value with source timestamp |
| Unrealized P&L | Change in market value plus accrued/recognized income basis |
| Early sale | Realized P&L from sale proceeds versus cost basis |
| Autocall | Redemption and income; close position |
| Physical delivery | Internal transformation from note into delivered asset where policy supports |
| Default/write-down | Loss event with source evidence and client-report explanation |

Performance caveat:

Structured-note returns can be misleading if reports ignore coupons, delivered assets, stale valuations, issuer bid spreads, or conditional payoff state.

## 9. Current Support Boundary And Candidate Extensions

| Capability | Treat as baseline when source-backed | Treat as future candidate until implemented |
|---|---|---|
| Note position | issuer, notional, maturity, currency, price, market value | rich subtype-specific payoff schema |
| Coupon cashflows | fixed or source-confirmed coupon payments | automated conditional coupon engine |
| Autocall/maturity | custodian/issuer event closes note and books cash | lifecycle scheduler with observation replay |
| Underlying references | identifiers and simple look-through reporting | scenario-sensitive exposure and Greeks |
| Physical delivery | custodian delivery event creates underlying position | pre-maturity delivery readiness workflow |
| Reporting labels | risk, issuer, maturity, price source, stale state | client-ready payoff simulator and barrier dashboard |

## 10. Regression Test Pack

Minimum release-gate scenarios:

1. Subscribe to note at par and verify no underlying holding is created.
2. Fixed coupon posts on payment date and reconciles to cash.
3. Conditional coupon missed with no income transaction booked.
4. Memory coupon later pays accumulated missed coupons.
5. Autocall closes note and posts redemption cash.
6. Reverse convertible physically delivers shares and closes note.
7. Dual currency note settles alternate currency cash.
8. Credit-linked note writes down after credit event.
9. Early sale realizes P&L using sale price, not maturity payoff.
10. Stale issuer quote labels valuation degraded.
11. Missing observation blocks coupon/autocall determination.
12. Matured unpaid note appears as pending settlement, not active exposure.

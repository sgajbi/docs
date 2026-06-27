# 04 — Platform Implementation, Advisory, and Practitioner Reference

## 1. Platform Architecture View

A wealth platform supporting notes should treat notes as complex securities with common instrument handling and subtype-specific payoff rules.

Recommended bounded capabilities:

| Capability | Responsibility |
|---|---|
| Product master | Store instrument, issuer, terms, underlyings, schedules, barriers, settlement terms. |
| Order and subscription | Validate client eligibility, minimum lot, currency, product availability, risk disclosure. |
| Position accounting | Maintain note nominal, units, cost, market value, lifecycle status. |
| Transaction processing | Book subscription, coupon, sale, redemption, physical delivery, write-down, recovery, tax, fee. |
| Lifecycle engine | Process observations, coupon determinations, barriers, autocalls, maturity fixings, credit events. |
| Valuation engine | Source market/issuer/vendor/model prices and compute market value. |
| Risk engine | Provide issuer, underlying, FX, credit, worst-of, liquidity, concentration, and complexity risk. |
| Performance engine | Treat coupons, redemptions, conversions, write-offs, FX, and external cashflows correctly. |
| Reporting engine | Explain payoff, current valuation, lifecycle status, risks, next dates, and exposures. |
| Suitability engine | Validate product-client fit and portfolio concentration before trade. |
| Operations workflow | Handle missing prices, failed settlements, manual events, corrections, and confirmations. |

---

## 2. Product Onboarding Checklist

| Check | Required? | Notes |
|---|---:|---|
| Instrument ID created | Yes | Internal master key. |
| ISIN/CUSIP/SEDOL loaded | If available | Private notes may not have public identifiers. |
| Issuer and guarantor mapped | Yes | Required for issuer concentration and risk. |
| Product subtype classified | Yes | FCN, ELN, CLN, PPN, DCI, ETN, etc. |
| Termsheet linked | Yes | Source of legal terms. |
| Issue/maturity dates loaded | Yes | Lifecycle and valuation. |
| Currency and denomination loaded | Yes | Position and settlement. |
| Underlyings loaded | If structured | Needed for risk, lifecycle, valuation. |
| Initial fixing stored | If linked product | Needed for strike/barrier/performance. |
| Coupon schedule loaded | If coupon-bearing | Needed for income processing. |
| Observation schedule loaded | If barrier/autocall/conditional coupon | Needed for lifecycle engine. |
| Barrier terms loaded | If applicable | Knock-in, knock-out, coupon barrier. |
| Settlement terms loaded | Yes | Cash, physical, alternate currency. |
| Valuation source assigned | Yes | Market, issuer, vendor, model. |
| Complexity/risk classification | Yes | Suitability and reporting. |
| Tax treatment configured | If available | Coupon/redemption withholding. |
| Corporate action adjustment rules | If equity/index-linked | Strike/barrier adjustment. |

---

## 3. Suitability and Advisory Checklist

| Question | Why It Matters |
|---|---|
| Does the client understand the payoff? | Notes can be complex and path-dependent. |
| Can the client tolerate issuer default risk? | Note is usually unsecured issuer debt. |
| Can the client tolerate underlying downside? | ELN/FCN/reverse convertible can behave like equity risk. |
| Is principal protection full, partial, conditional, or absent? | Avoid false sense of safety. |
| Is protection only at maturity? | Early exit may create loss. |
| Is the product liquid? | Many private notes rely on issuer bid. |
| Are coupons guaranteed, conditional, memory-based, or discretionary? | Income expectations must be accurate. |
| Are there barriers or autocall features? | Lifecycle depends on observations. |
| Is there physical settlement risk? | Client may receive unwanted shares. |
| Is there alternate currency risk? | DCI may repay in another currency. |
| Is there credit event risk? | CLN can write down after reference default. |
| Is there concentration to issuer or underlying? | Important for portfolio risk. |
| Does tenor match client horizon? | Long lock-in may be unsuitable. |
| Is the note aligned with client objective? | Yield, capital preservation, tactical view, or income. |

Advisor explanation should include:

- What the note is
- Who the issuer is
- What the underlying is
- What the coupon depends on
- What happens in best, base, and worst case
- What happens if the issuer defaults
- Whether client can exit early
- How valuation is calculated
- What fees/spreads are embedded or charged

---

## 4. Client Reporting Requirements

A client report should not show only coupon and maturity. It should explain the state of the note.

| Report Field | Purpose |
|---|---|
| Note name and identifier | Security identification. |
| Issuer | Issuer credit risk. |
| Currency | Investment and valuation currency. |
| Notional/nominal amount | Face amount held. |
| Current market value | Current valuation. |
| Valuation source | Exchange, issuer, vendor, model. |
| Price date | Stale price awareness. |
| Cost and unrealized P&L | Performance view. |
| Coupon rate and frequency | Income expectation. |
| Coupon status | Paid, pending, conditional, missed, memory. |
| Underlyings | Market risk driver. |
| Initial fixing and current level | Distance from strike/barrier. |
| Barrier level and status | Knock-in/knock-out risk. |
| Autocall level and next observation | Early redemption monitoring. |
| Maturity date | Investment horizon. |
| Settlement type | Cash, physical, alternate currency. |
| Principal protection type | None, conditional, full at maturity. |
| Risk classification | Complex/SIP/EIP/product risk. |
| Liquidity flag | Listed, OTC, issuer bid only. |

---

## 5. API Design Considerations

A notes API should separate summary, lifecycle, valuation, and exposure views.

### 5.1 Position Summary Endpoint

Example response shape:

```json
{
  "accountId": "ACC123",
  "instrumentId": "NOTE123",
  "noteSubtype": "AUTOCALLABLE_FCN",
  "issuerId": "BANK_X",
  "currency": "USD",
  "quantityUnits": "100",
  "currentNominalAmount": "100000",
  "costAmount": "100000",
  "marketPricePct": "97.50",
  "accruedIncome": "500",
  "marketValue": "98000",
  "valuationSource": "ISSUER_BID",
  "lifecycleStatus": "ACTIVE",
  "barrierStatus": "NOT_BREACHED",
  "nextObservationDate": "2026-09-25",
  "nextCouponDate": "2026-07-25"
}
```

### 5.2 Lifecycle Endpoint

Should return observations, coupon determinations, barriers, autocall events, final fixing, and credit events.

### 5.3 Payoff Terms Endpoint

Should return termsheet-level fields: underlyings, coupon, barrier, settlement, protection, autocall, schedules.

### 5.4 Look-Through Exposure Endpoint

Should return issuer exposure, underlyings, basket/worst-of status, currency, sector, country, delta-adjusted exposure if available.

### 5.5 Valuation Endpoint

Should return price, source, date/time, clean/dirty flag, bid/mid/ask/indicative type, model inputs if applicable, and stale/confidence flags.

---

## 6. Event Processing Pattern

Recommended processing flow:

```text
1. Load termsheet and schedule.
2. Generate expected lifecycle calendar.
3. On observation date, capture observed market values.
4. Evaluate termsheet condition.
5. Store lifecycle event result.
6. Wait for issuer/custodian confirmation if required.
7. Generate economic transaction only when payment/redemption/conversion is confirmed.
8. Update position and cash/security balances.
9. Revalue portfolio.
10. Recompute performance and risk.
11. Reconcile with custodian/issuer statements.
```

Do not directly mutate positions from observation logic without producing auditable lifecycle events and transactions.

---

## 7. QA and Regression Test Scenarios

### 7.1 Plain Note

| Scenario | Expected Result |
|---|---|
| Buy at par | Note nominal increases; cash decreases. |
| Buy at discount | Cost below nominal; market value based on price. |
| Coupon payment | Cash income posted; nominal unchanged. |
| Mature at par | Note closes; principal cash received. |
| Sell before maturity | Nominal reduced; realized P&L booked. |

### 7.2 Principal-Protected Note

| Scenario | Expected Result |
|---|---|
| Underlying up at maturity | Principal + participation return paid. |
| Underlying down at maturity | Principal returned if protection valid and issuer solvent. |
| Sell before maturity below par | Realized loss possible despite principal protection. |
| Issuer default | Protection fails; write-off/recovery based on default handling. |

### 7.3 Equity-Linked / FCN / Reverse Convertible

| Scenario | Expected Result |
|---|---|
| Coupon paid monthly | Income transactions created. |
| Barrier not breached, final above strike | Full cash redemption. |
| Barrier breached, final below strike, cash settlement | Note closes with cash loss. |
| Barrier breached, final below strike, physical settlement | Note closes; underlying shares created. |
| Fractional shares | Cash-in-lieu transaction created. |

### 7.4 Worst-Of Note

| Scenario | Expected Result |
|---|---|
| One underlying breaches barrier | Lifecycle event records breached name. |
| Worst performer changes during life | Current worst-of analytics update. |
| Final settlement based on worst name | Redemption/delivery based on worst-performing underlying. |

### 7.5 Autocallable Note

| Scenario | Expected Result |
|---|---|
| Observation passes autocall condition | Autocall lifecycle event created. |
| Autocall payment settles later | Redemption transaction on payment date. |
| Coupon condition fails but memory enabled | Memory balance increases; no income booked. |
| Later coupon condition passes | Current + memory coupon paid if terms allow. |
| No autocall until maturity | Final fixing drives redemption. |

### 7.6 Dual Currency Note

| Scenario | Expected Result |
|---|---|
| FX condition not triggered | Original currency redemption. |
| FX condition triggered | Alternate currency redemption. |
| Portfolio base currency differs | FX translation reflected in reporting/performance. |

### 7.7 Credit-Linked Note

| Scenario | Expected Result |
|---|---|
| No credit event | Coupon + principal as scheduled. |
| Credit event announced but unconfirmed | Lifecycle status pending; no final loss yet. |
| Credit event confirmed | Write-down transaction booked. |
| Recovery paid later | Recovery transaction booked. |
| Issuer default | Issuer default write-off independent of reference entity. |

---

## 8. Operational Controls

| Control | Purpose |
|---|---|
| Termsheet-to-master validation | Avoid incorrect payoff modelling. |
| Observation schedule reconciliation | Ensure no observation date is missed. |
| Issuer/custodian event reconciliation | Confirm coupon, call, redemption, conversion. |
| Price source hierarchy | Ensure consistent valuation. |
| Stale price alerts | Detect missing or old valuations. |
| Barrier proximity monitoring | Highlight high-risk notes. |
| Autocall pending workflow | Track triggered-but-unsettled notes. |
| Physical delivery workflow | Ensure note closes and delivered shares are booked. |
| Corporate action adjustment workflow | Adjust strikes, barriers, conversion ratios. |
| Manual override audit trail | Support control and governance. |
| Reversal/correction process | Preserve accounting history. |
| Performance recalculation trigger | Recalculate after backdated lifecycle/transaction corrections. |

---

## 9. Common Mistakes to Avoid

| Mistake | Correct Treatment |
|---|---|
| Treating every embedded derivative as a separate client position | Keep embedded components as note terms unless legally delivered. |
| Booking barrier touch as a cash transaction | Barrier is lifecycle status, not cash movement. |
| Booking conditional coupon before confirmation | Book income only when entitlement/payment is confirmed. |
| Assuming principal protection means no risk | Issuer risk and early-exit risk remain. |
| Treating high coupon as low-risk income | High coupon is compensation for hidden or explicit risk. |
| Ignoring issuer concentration | Notes are issuer obligations. |
| Ignoring underlying concentration | Look-through exposure matters. |
| Using stale issuer quote without warning | Structured note values may be stale or indicative. |
| Showing only note name and value in reporting | Client needs payoff status, barriers, underlyings, next dates. |
| Treating physical delivery as external cashflow | Usually internal security conversion if assets remain in portfolio. |
| Confusing issuer and reference entity in CLN | They are separate risks. |

---

## 10. Glossary

| Term | Meaning |
|---|---|
| Note | Debt instrument issued by an issuer. |
| Structured note | Note whose coupon or principal repayment depends on underlying assets or benchmarks. |
| Issuer | Entity that owes payment under the note. |
| Notional / nominal | Face amount of the note. |
| Tenor | Life of the note. |
| Coupon | Interest/income paid by the note. |
| Underlying | Asset, index, FX rate, rate, credit, or commodity referenced by the payoff. |
| Strike | Initial reference price or level. |
| Initial fixing | Starting value used to determine payoff conditions. |
| Barrier | Level that triggers or changes payoff behaviour. |
| Knock-in | Barrier event that activates downside exposure. |
| Knock-out | Barrier event that terminates or changes exposure. |
| Autocall | Early redemption if condition is met. |
| Observation date | Date when underlying condition is checked. |
| Final fixing | Final observed value used to determine maturity payoff. |
| Principal protection | Contractual promise to repay some/all principal, usually at maturity and subject to issuer solvency. |
| Conditional protection | Protection applies only if barrier/event condition is not breached. |
| Physical settlement | Investor receives securities instead of cash. |
| Cash-in-lieu | Cash paid for fractional securities or residual amount. |
| Worst-of | Basket payoff driven by worst-performing underlying. |
| Memory coupon | Missed coupon can be recovered later if conditions are met. |
| Credit event | Bankruptcy, failure to pay, restructuring, or similar event for reference entity. |
| Recovery rate | Value recovered after credit event/default. |
| Clean price | Price excluding accrued interest. |
| Dirty price | Price including accrued interest. |
| Indicative value | Estimated/model value, not necessarily executable. |
| Issuer bid | Price issuer may offer to buy back note. |
| Look-through exposure | Analytical exposure to underlyings, issuer, currency, credit, etc. |

---

## 11. Practitioner Questions and Answers

### What is a note?

> A note is a debt instrument issued by a bank, company, or financial institution. The investor lends money to the issuer and receives a payoff based on the note terms. In private banking, the word note often refers to a structured note, where the coupon or redemption depends on underlyings such as equities, indices, FX, rates, credit, or commodities.

### What is a structured note?

> A structured note combines a debt instrument with embedded derivatives. The debt component creates the issuer obligation, while the derivative component creates the payoff formula, such as enhanced coupon, upside participation, barriers, autocall, FX conversion, or credit-event exposure. The investor takes both issuer credit risk and the risk of the underlying payoff.

### Why can the coupon be high?

> The coupon is high because the investor is being paid to accept risk. That risk may be equity downside, FX conversion, credit event risk, volatility risk, barrier risk, liquidity risk, worst-of basket risk, or issuer risk. The higher coupon is compensation for something the client is effectively selling or accepting.

### How should notes be modelled in a wealth platform?

> I would model notes as a common security type with a core note contract and optional payoff extensions. The client holds one note position with units, nominal amount, cost, market value, accrued income, and lifecycle status. Embedded derivatives should be modelled as product terms, lifecycle rules, valuation inputs, and look-through exposures, not separate client positions. Transactions should be economic postings such as subscription, coupon, sale, redemption, physical delivery, credit-event write-down, recovery, fee, tax, and correction.

### How should lifecycle be modelled?

> Observation, barrier breach, coupon determination, autocall trigger, final fixing, and credit event should be lifecycle events. They should not automatically become accounting transactions. A transaction is created only when there is an economic posting such as cash coupon payment, redemption, physical delivery, write-down, or recovery.

### How are notes valued?

> If listed and liquid, use exchange prices. For private-bank structured notes, valuation often depends on issuer quotes, independent valuation vendors, custodian evaluated prices, or internal models. The model usually decomposes the note into a debt component and embedded derivative component. Inputs include underlying spot, strike, barrier, volatility, correlation, rates, dividends, FX, issuer credit spread, reference credit spread, recovery rate, tenor, and liquidity adjustments.

### Are notes listed?

> Some notes are listed, especially ETNs or listed structured certificates, but many private-bank structured notes are unlisted or OTC. Even listed notes can have limited liquidity. Unlisted notes often rely on issuer bid or evaluated price for secondary valuation.

### What is the biggest risk people miss?

> Issuer credit risk. The client owns the issuer's promise. Even if the underlying performs well, the client may lose money if the issuer defaults. The second common risk people miss is that high coupon notes can behave like equity, FX, or credit risk rather than safe fixed income.

---

## 12. One-Page Executive Summary

Structured notes are issuer debt instruments with customised payoff rules. They can be useful for yield enhancement, market-view expression, and structured exposure, but they introduce complexity, issuer risk, liquidity risk, and payoff risk.

A platform should model them using:

- One accounting position for the note
- Contract terms for underlyings, coupons, barriers, observations, calls, settlement, and subtype extensions
- Lifecycle events for observations and triggers
- Transactions only for actual economic postings
- Separate look-through exposures for risk and concentration
- Robust valuation source hierarchy and stale-price controls
- Clear performance classification for coupons, redemptions, conversions, write-downs, and external cashflows
- Strong suitability and reporting controls

The most important implementation rule:

```text
Keep accounting simple and lifecycle rich.
The client owns the note; the platform must understand the embedded payoff.
```

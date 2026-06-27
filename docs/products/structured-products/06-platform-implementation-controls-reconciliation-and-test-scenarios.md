# 06 — Platform Implementation, Controls, Reconciliation and Test Scenarios

## 1. Implementation architecture

A robust structured product platform should separate responsibilities:

| Component | Responsibility |
|---|---|
| Product master | Store wrapper, terms, underlyings, payoff rules and schedules |
| Lifecycle engine | Process observations, barriers, coupons, autocalls, fixings and maturity events |
| Transaction engine | Book accounting/economic transactions |
| Position engine | Maintain holdings, nominal, units, status and cost basis |
| Valuation engine | Store and calculate market values, prices and exceptions |
| Risk engine | Compute look-through, scenarios, Greeks, barrier distance and concentration |
| Advisory engine | Suitability, mandate, pre-trade and post-trade checks |
| Reporting engine | Client statements, portfolio reviews, product facts and risk disclosures |
| Reconciliation engine | Compare custodian/issuer/internal positions, cashflows, prices and lifecycle status |
| Audit and document store | Termsheets, approvals, disclosures, confirmations and event evidence |

## 2. Product onboarding checklist

| Area | Checks |
|---|---|
| Identifier | ISIN/product ID/internal ID unique and mapped |
| Wrapper | Note/deposit/certificate/warrant/fund/OTC classified correctly |
| Issuer/counterparty | Issuer, guarantor, arranger, counterparty captured |
| Currency | Product, underlying, settlement and reporting currency captured |
| Underlyings | IDs, initial fixings, strike, weights and basket logic validated |
| Payoff | Coupon, participation, cap, floor, buffer, barrier, autocall, settlement terms captured |
| Schedule | Observation, coupon, call, fixing, maturity and payment dates loaded |
| Liquidity | Listing, market maker, issuer bid, early exit rules captured |
| Valuation | Source hierarchy, model type and fallbacks configured |
| Suitability | Product complexity, risk score and eligible client segments assigned |
| Documents | Termsheet, KID/PHS/prospectus/disclosure stored |
| Controls | Four-eye review or product governance approval recorded |

## 3. Product master anti-patterns

Avoid:

- storing payoff only as free text;
- using product name to infer risk;
- missing observation schedule;
- ignoring underlying corporate action adjustment rules;
- treating all structured products as bonds;
- treating all certificates as equities;
- treating all deposits as risk-free cash;
- storing issuer risk but not underlying risk;
- storing underlying risk but not issuer risk;
- missing price source quality metadata;
- missing physical/alternate-currency settlement flags.

## 4. Lifecycle processing controls

| Control | Description |
|---|---|
| Schedule completeness | Every future observation/coupon/maturity date loaded |
| Fixing source validation | Observed prices/rates from approved source |
| Barrier logic replay | Event result reproducible from market data and terms |
| Maker-checker | Manual confirmation for complex payoff events |
| Event versioning | Corrections tracked without overwriting audit trail |
| Payment matching | Coupon/redemption events matched to custodian cash |
| Status consistency | Position status matches latest lifecycle event |
| Exception queue | Missing fixings, stale data, unmatched cash, price breaks handled |

## 5. Position reconciliation

Reconcile across:

- internal position engine;
- custodian position file;
- issuer position file;
- transaction ledger;
- valuation/pricing system;
- lifecycle event engine.

Key checks:

| Check | Example |
|---|---|
| Nominal/units | Internal nominal equals custodian nominal |
| Status | Internal says active, issuer says matured — break |
| Maturity closure | Matured products should not remain active |
| Partial redemption | Current notional reduced correctly |
| Physical delivery | Product closed and delivered asset received |
| Alternate currency | Correct cash currency credited |
| Transfer | Transfer out removed position without creating P&L |

## 6. Cash reconciliation

| Cashflow | Reconciliation source |
|---|---|
| Subscription cash debit | Trade/custodian settlement |
| Coupon payment | Issuer/custodian cash statement |
| Redemption principal | Maturity/autocall instruction and cash statement |
| Physical delivery cash-in-lieu | Corporate action/custodian event |
| Tax withholding | Custodian tax line |
| Fee | Fee ledger/custodian transaction |
| Alternate currency redemption | FX/cash ledger |
| Recovery payment | Credit event/recovery notice |

## 7. Valuation controls

| Control | Description |
|---|---|
| Stale price detection | Price older than allowed threshold flagged |
| Source priority | Exchange/issuer/vendor/model hierarchy applied |
| Bid/ask sanity | Bid lower than ask, spread within tolerance |
| Issuer vs independent price | Difference within tolerance or exception |
| Zero/negative value | Valid only for warrant expiry/knockout/writeoff, else break |
| Maturity value | Matured products valued at settlement amount until closed |
| Barrier-adjusted valuation | Knock-in/out status reflected |
| FX translation | Correct rates and value dates used |
| Accrued income | Only accrued where policy allows |

## 8. Corporate action and underlying adjustment controls

Structured products linked to equities can require adjustment for:

- stock split;
- reverse split;
- special dividend;
- rights issue;
- spin-off;
- merger/acquisition;
- ticker change;
- delisting;
- trading halt;
- index methodology change.

Controls:

| Control | Description |
|---|---|
| Adjustment source | Issuer/exchange calculation agent notice captured |
| Strike/barrier adjustment | New levels calculated and approved |
| Ratio adjustment | Certificate/warrant ratios updated |
| Underlying replacement | New underlying mapped correctly |
| Event replay | Historical observations not incorrectly recalculated unless specified |
| Client reporting | Material adjustment disclosed/explained |

## 9. Pre-trade controls

| Check | Purpose |
|---|---|
| Client eligibility | Ensure client may invest in complex/SIP product |
| Product risk score | Compare with client risk profile |
| Knowledge/experience | Validate derivative/structured product knowledge where required |
| Concentration | Product, issuer, underlying, asset class and currency limits |
| Liquidity | Client liquidity needs and product lock-up/secondary market |
| Currency | Base and settlement currency mismatch |
| Physical delivery | Delivered asset allowed and custody-enabled |
| Tenor | Investment horizon and mandate maturity rules |
| Worst-case loss | Client can tolerate adverse outcome |
| Disclosure | Termsheet/risk disclosure provided before order |

## 10. Post-trade controls

| Check | Purpose |
|---|---|
| Position settled | Cash and product received correctly |
| Client reporting | Product appears with correct category and risk |
| Ongoing concentration | Exposure changes with market/valuation |
| Barrier proximity | Advisor alert when near barrier |
| Autocall/maturity alert | Prepare reinvestment or settlement action |
| Stale valuation alert | Avoid misleading statements |
| Mandate drift | Product may become unsuitable due to market movement |
| Event processing | Coupon/autocall/maturity processed on time |

## 11. QA test scenarios

### 11.1 Subscription and settlement

- Subscribe to unlisted structured note at par.
- Verify cash debit, position nominal, cost, status and valuation.
- Verify no underlying position created.

### 11.2 Coupon processing

- Conditional coupon condition met.
- Lifecycle event created.
- Coupon transaction booked on payment date.
- Income and cash reconcile.

### 11.3 Missed memory coupon

- Coupon condition failed.
- No income transaction booked.
- Memory coupon balance updated.
- Later condition met and accumulated coupon paid.

### 11.4 Barrier breach

- Underlying crosses knock-in barrier.
- Barrier lifecycle event captured.
- Position status updated.
- No cash transaction booked until settlement.
- Valuation reflects breached state.

### 11.5 Autocall

- Autocall condition met on observation date.
- Event status becomes autocall pending.
- Redemption transaction booked on payment date.
- Position closed.
- Coupon/redemption cash reconciles.

### 11.6 Physical settlement

- Product matures below strike after knock-in.
- Product closed.
- Delivered equity created.
- Cash-in-lieu booked for fractional shares.
- Cost basis policy applied.
- Performance treats as internal transformation.

### 11.7 Alternate currency redemption

- DCI converts from USD to SGD.
- Product closes.
- SGD cash balance created.
- No artificial FX trade created unless actual conversion occurs.
- Base-currency P&L calculated.

### 11.8 Early sale

- Client sells product before maturity.
- Realized P&L calculated from bid price.
- Principal protection at maturity not applied.
- Position reduced or closed.

### 11.9 Issuer default

- Issuer default event captured.
- Valuation written down based on recovery assumption.
- Loss transaction booked.
- Client reporting shows issuer default loss.

### 11.10 Stale valuation

- No price received for 5 business days.
- Price marked stale.
- Client report flagged or exception raised.
- Risk reports do not silently accept stale values.

### 11.11 Underlying corporate action

- Equity underlying splits 2-for-1.
- Strike and barrier adjusted.
- Observation engine uses adjusted levels.
- Audit trail records calculation agent notice.

### 11.12 Migration from legacy system

- Load active structured products with current status.
- Reconstruct future lifecycle schedule.
- Load cost, nominal, market value, accrued income and barrier status.
- Do not replay historical events unless required.
- Reconcile first reporting cycle.

## 12. Production triage questions

When a structured product issue occurs, ask:

1. Is the position wrong or only valuation wrong?
2. Is the product master complete?
3. Is the lifecycle event missing or incorrect?
4. Is the transaction missing, duplicated or misclassified?
5. Is the custodian cashflow different from issuer notice?
6. Is the valuation source stale or changed?
7. Is the barrier/autocall status correct?
8. Has an underlying corporate action occurred?
9. Is the issue wrapper-specific or common model issue?
10. Does the issue impact client reports, performance, mandate checks or suitability records?

## 13. Implementation checklist for engineering teams

Minimum capabilities:

- canonical product model with wrapper and payoff dimensions;
- lifecycle event engine;
- transaction grouping;
- current and historical position reconstruction;
- valuation source hierarchy;
- price quality flags;
- look-through exposure model;
- suitability and mandate rules integration;
- client and advisor reporting fields;
- audit trail for terms, events and corrections;
- reconciliation dashboards;
- scenario and stress analytics;
- test fixtures for every major product family;
- golden regression examples for coupons, autocall, physical delivery, DCI conversion and maturity.

## 14. Golden data set recommendation

Maintain a small regression library:

| Product | Scenario |
|---|---|
| Principal-protected note | Up/down/flat maturity outcomes |
| Autocallable note | Coupon paid, missed, memory paid, autocall |
| Worst-of FCN | Worst name barrier breach and physical settlement |
| DCI | Original currency and alternate currency outcomes |
| CLN | No credit event, credit event, recovery |
| Certificate | Exchange buy/sell, issuer call, expiry |
| Warrant | In-the-money expiry, out-of-the-money expiry, knock-out |
| Accumulator | Normal accumulation, leveraged accumulation, knock-out |
| Structured deposit | Early withdrawal and maturity |

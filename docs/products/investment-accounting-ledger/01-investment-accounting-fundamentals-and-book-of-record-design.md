# 01 - Investment Accounting Fundamentals and Book-of-Record Design

## 1. What investment accounting means in a wealth platform

Investment accounting is the controlled process of turning investment events into auditable records of what the client owns, owes, earns, spends, gains and loses.

In a private-banking or wealth-management platform, investment accounting must support more than financial statements. It must also support:

| Consumer | Needs from accounting layer |
|---|---|
| Client reporting | Holdings, cash, income, fees, realized/unrealized P&L, cost, statement movements |
| Portfolio analytics | Performance cashflow classification, income, P&L, cost, valuation, FX effects |
| Advisory | Available cash, sale proceeds, income, tax lots, product exit impact |
| DPM/mandates | Investable cash, breaches, restricted securities, model drift, fee-aware returns |
| Lending/buying power | Collateral value, pledged assets, haircut value, loan exposure, accrued interest |
| Operations | Settlement, corporate actions, accruals, reconciliation, exception repair |
| Audit/compliance | Source lineage, maker-checker evidence, corrections, restatements, approvals |

The accounting layer is therefore both a **book of record** and a **calculation foundation**.

## 2. Accounting layer versus analytics layer

The accounting layer records economic truth. The analytics layer explains investment outcomes.

| Layer | Main question | Examples |
|---|---|---|
| Accounting | What happened and what is the current recorded state? | Transactions, legs, cash, positions, lots, accruals, fees, ledger entries |
| Valuation | What are assets and liabilities worth today? | Prices, NAVs, curves, FX, fair value hierarchy |
| Performance | What return was earned? | TWR, MWR, contribution, attribution |
| Risk | What can go wrong? | Market, credit, liquidity, concentration, leverage, stress |
| Reporting | How should this be shown? | Client statements, portfolio review, income summary, tax packs |

A common platform failure is to let analytics infer accounting meaning from ambiguous transaction types. A better design makes economic classification explicit at source.

## 3. Books of record

Different books may exist in a wealth platform. They should be reconciled but not confused.

| Book | Purpose | Typical owner |
|---|---|---|
| Order book | Captures requested client/advisor/PM action | Advisory / OMS |
| Trade book | Captures executed trades | OMS / execution / operations |
| Settlement book | Tracks settlement obligations and status | Operations / custodian |
| Position book | Current security and cash holdings | Portfolio accounting / custody |
| Tax-lot book | Acquisition lots and cost basis | Tax/reporting/accounting |
| Income/accrual book | Coupon, dividend, interest and fee accruals | Accounting / reporting |
| General ledger / sub-ledger | Debit/credit accounting postings | Finance / product accounting |
| Performance book | Time-series valuation and classified cashflows | Performance / analytics |
| Collateral book | Pledged assets, LTV, haircuts, availability | Lending / buying power |
| Reporting book | Client-facing curated reporting state | Reporting / statements |

In many banks, the custodian or core banking system may be the legal book of record, while the portfolio platform may maintain an investment book of record for analytics and reporting. The architecture should declare this boundary clearly.

## 4. Event-to-ledger model

A robust design separates the business event from the accounting outcome.

```text
Business event: Client buys 100 shares
Trade event: Executed buy order
Settlement event: Cash paid and shares received
Accounting postings: cash decrease, equity position increase, fee expense, tax if any
Reporting output: holding, cost, cash movement, realized/unrealized P&L foundation
```

For complex products, one business event may generate many accounting legs.

Example: structured note physically settles into shares:

| Object | Meaning |
|---|---|
| Lifecycle event | Final fixing / knock-in / physical settlement confirmed |
| Note close transaction | Note nominal reduced to zero |
| Security delivery transaction | Equity shares received |
| Cash-in-lieu transaction | Fractional residual cash |
| Cost-basis event | Delivered equity cost assigned by configured policy |
| Reporting event | Note closed, equity position opened, loss explained |

## 5. Dates that must not be collapsed

| Date | Meaning | Why it matters |
|---|---|---|
| Trade date | When economic trade was agreed/executed | Performance timing, exposure, order lineage |
| Settlement date | When cash/security settlement occurs | Cash availability, settlement risk, fail handling |
| Booking date | When platform records the event | Audit and late booking controls |
| Value date | Date from which cash interest/FX settlement applies | Cash interest, FX and money-market treatment |
| Effective date | Date used for entitlement or corporate action impact | Dividends, splits, coupon accruals |
| Accounting date | Date used for accounting period | Period close and reporting |
| Reporting date | Statement/valuation cut date | Client reporting and performance windows |

Never assume they are the same. Many reconciliation and performance errors come from collapsing dates.

## 6. Accounting basis choices

A platform may need to support multiple accounting views.

| Basis | Meaning | Common usage |
|---|---|---|
| Trade-date basis | Recognize trade from execution date | Portfolio exposure and performance |
| Settlement-date basis | Recognize trade only after settlement | Cash/custody accounting, some statements |
| Cash basis | Recognize when cash moves | Simple cash reports |
| Accrual basis | Recognize income/expense as earned/incurred | Coupons, interest, management fees |
| Tax basis | Jurisdiction-specific cost and gain basis | Tax reporting |
| Fair-value basis | Mark holdings to fair value | Client valuation, reporting, accounting standards |
| Amortized-cost basis | Carry certain debt at amortized cost | Accounting-policy specific, loans/bonds/deposits |

Wealth platforms often need both trade-date exposure and settlement-date cash views. The model should allow both without double-counting.

## 7. Golden design rules

1. Every economic event should have a unique source identifier and idempotency key.
2. Every transaction should have explicit legs, not hidden side effects.
3. Gross amount, fees, taxes, accrued income and net settlement should be separately stored.
4. Derived balances should be reproducible from transactions, or clearly flagged as source-provided.
5. Reversals and corrections should preserve audit trail; never overwrite history silently.
6. Positions, cash, lots and ledger should reconcile at defined cut-offs.
7. Product-specific treatments should be configurable, not hardcoded into generic transaction logic.
8. All calculations should use deterministic decimal arithmetic, not binary floating-point.
9. Data lineage should reach source system, price, FX rate, rule version and calculation run.
10. Client reporting should display degraded or estimated states rather than hiding data-quality limits.

## 8. Interview-ready explanation

Investment accounting is the controlled book-of-record layer that converts trades, cashflows, corporate actions, income events, fees, taxes, valuations and product lifecycle events into auditable cash, position, lot, income, expense and P&L records. In a wealth platform, this layer is not only for finance; it drives client reporting, performance, risk, buying power, suitability and mandate analytics. The key design is to separate business events from accounting postings, model each transaction with explicit legs, preserve trade-date and settlement-date views, and maintain lineage from source event to statement output.

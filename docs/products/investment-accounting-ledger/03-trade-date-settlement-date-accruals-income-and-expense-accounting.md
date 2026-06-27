# 03 - Trade-Date, Settlement-Date, Accruals, Income and Expense Accounting

## 1. Why date basis matters

A single trade has at least two important dates: trade date and settlement date. A portfolio manager cares about exposure from trade date; custody and cash operations care about settlement date. Client reporting may need both.

| View | Buy trade impact | Sell trade impact |
|---|---|---|
| Trade-date position | Security increases on trade date | Security decreases on trade date |
| Settlement-date position | Security increases only on settlement | Security decreases only on settlement |
| Trade-date cash | Cash payable recognized on trade date | Cash receivable recognized on trade date |
| Settlement-date cash | Cash decreases only on settlement | Cash increases only on settlement |

The platform should not force one view into all consumers. It should support configurable accounting views.

## 2. Trade-date versus settlement-date accounting

### Trade-date view

Useful for:

- portfolio exposure;
- pre-trade/post-trade mandate monitoring;
- performance return windows;
- advisor review after execution;
- risk and concentration analytics.

### Settlement-date view

Useful for:

- custody reconciliation;
- cash availability;
- settlement fail handling;
- official cash statement;
- overdraft and funding checks.

### Recommended design

Store trade facts once, then derive different balances by basis:

```text
Position(trade-date basis) = settled position + unsettled buys - unsettled sells
Cash(available basis) = settled cash - unsettled buys - reserved fees/taxes + eligible unsettled sales if policy allows
```

## 3. Settlement states

| State | Meaning | Accounting treatment |
|---|---|---|
| Pending settlement | Trade executed but not settled | Recognize receivable/payable in trade-date view |
| Partially settled | Some cash/security settled | Split remaining obligation |
| Failed | Settlement did not complete on contractual date | Keep exception, may accrue fail interest/charges |
| Cancelled | Trade cancelled before final booking | Reverse pending entries |
| Corrected | Trade amended | Preserve original + reversal + corrected trade |
| Settled | Cash/security delivered | Move from receivable/payable to cash/security |

## 4. Accrual fundamentals

Accrual accounting recognizes income or expense when earned or incurred, not only when cash is paid.

Common accruals in wealth platforms:

| Accrual | Product examples |
|---|---|
| Bond coupon accrual | Bonds, notes, money-market securities |
| Deposit interest accrual | Time deposits, savings/current accounts |
| Loan interest accrual | Lombard loan, margin loan, overdraft |
| Management fee accrual | Advisory/DPM fee |
| Performance fee accrual | Hedge funds, private funds, DPM if applicable |
| Fund distribution accrual | Distribution declared but unpaid |
| Dividend receivable | Equity dividend entitlement after ex-date/record-date logic |
| Tax withholding accrual | Expected tax on income event |

## 5. Accrual schedule model

| Field | Meaning |
|---|---|
| accrual_id | Unique accrual record |
| account_id | Account/portfolio |
| instrument_id | Security/product |
| accrual_type | Coupon, interest, fee, tax, dividend receivable |
| accrual_start_date | Start date |
| accrual_end_date | End date |
| day_count | ACT/360, ACT/365, 30/360, business-day rule |
| rate | Coupon/interest/fee rate |
| notional_or_base_amount | Amount on which accrual is calculated |
| accrued_amount | Earned/incurred amount |
| paid_amount | Amount already settled |
| remaining_amount | Accrued less paid/reversed |
| payment_date | Expected cash date |
| status | Estimated, confirmed, paid, reversed, adjusted |

## 6. Coupon and bond accruals

Bond and note accruals depend on:

- coupon rate;
- coupon schedule;
- day-count convention;
- ex-coupon rules;
- clean/dirty price treatment;
- settlement date;
- whether coupon is fixed, floating, step-up, deferred, zero-coupon or conditional.

For a plain fixed-rate bond:

```text
Accrued interest = Nominal x Coupon rate x Day-count fraction since last coupon
```

For structured notes, conditional coupons should generally not be recognized as earned income until the condition is satisfied and payment becomes due, unless a specific accounting policy requires a probability-weighted valuation treatment outside client income reporting.

## 7. Dividend accounting

Equity dividend lifecycle:

| Event | Meaning | Accounting impact |
|---|---|---|
| Declaration date | Company announces dividend | Usually no client accounting yet |
| Ex-date | Security trades without dividend entitlement | Entitlement determination |
| Record date | Company records eligible holders | Confirms entitlement |
| Payment date | Cash paid | Dividend income/cash posting |

A reporting platform may show dividend receivable between entitlement and payment. It should not treat a forecast dividend as earned income unless the entitlement is confirmed.

## 8. Fee accruals

Recurring advisory/DPM fees should often accrue daily or monthly even if charged quarterly.

Example:

```text
Daily fee accrual = Fee base x annual fee rate / day-count denominator
```

Important design choices:

| Choice | Options |
|---|---|
| Fee base | Beginning AUM, ending AUM, average daily AUM, tiered AUM, committed capital |
| Fee frequency | Monthly, quarterly, annual, ad hoc |
| Fee timing | In arrears, in advance |
| Fee source | Deducted from portfolio, billed externally, waived, rebated |
| Fee performance treatment | Expense, external cashflow, excluded from gross return, included in net return |

## 9. Reversals and corrections

Never delete a posted income, fee or accrual entry silently.

Recommended pattern:

```text
Original transaction -> reversal transaction -> corrected transaction
```

Keep:

- original source ID;
- correction source ID;
- reason code;
- maker/checker approval;
- timestamp;
- affected report periods;
- recalculation status.

## 10. Reporting implications

| Item | Report treatment |
|---|---|
| Accrued income | Show separately from cash income where relevant |
| Paid income | Income summary and cash statement |
| Fees accrued but unpaid | Expense accrual / fee projection |
| Fees paid | Expense and cash movement |
| Unsettled trade receivable/payable | Pending settlement section or cash availability note |
| Failed trade | Exception / pending settlement status, not hidden |
| Backdated accrual correction | Restatement or adjustment with audit trail |

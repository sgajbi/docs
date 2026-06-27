# 02 — Cash Accounts, Balances, Deposit Lifecycle and Transactions

## 1. Cash account hierarchy

A wealth platform should not model cash as a single portfolio-level number. Cash normally exists at multiple levels:

```text
Client / CIF / relationship
  └── Account / portfolio
        └── Cash account / custody account
              └── Currency balance
                    └── Ledger entries and value-date projections
```

Example:

| Portfolio | Cash account | Currency | Ledger balance | Settled balance | Available balance |
|---|---|---:|---:|---:|---:|
| P-100 | CA-USD | USD | 500,000 | 480,000 | 420,000 |
| P-100 | CA-SGD | SGD | 120,000 | 120,000 | 100,000 |
| P-100 | CA-EUR | EUR | -25,000 | -25,000 | -25,000 |

The EUR balance is an overdraft/negative cash exposure. It should not be hidden by a positive total base-currency cash balance.

## 2. Balance dimensions

A robust cash model should support these dimensions:

| Dimension | Examples |
|---|---|
| Account | Custody account, settlement account, margin account |
| Currency | USD, SGD, EUR, CHF, JPY |
| Date basis | Trade date, settlement date, value date, booking date |
| Availability | Ledger, settled, available, blocked, reserved, projected |
| Ownership | Client cash, pledged cash, collateral cash, margin cash |
| Purpose | Settlement, income, withdrawal, fees, tax, capital call reserve |
| Restriction | Sanction hold, pledge, legal hold, debit block, credit block |
| Interest treatment | Interest-bearing, non-interest-bearing, tiered, negative interest |

## 3. Balance formulas

### Ledger cash

```text
Ledger Cash = Sum of posted cash ledger entries
```

### Settled cash

```text
Settled Cash = Ledger Cash adjusted for entries whose value date has arrived
```

### Available cash

```text
Available Cash
= Settled Cash
- Pending buy settlements
- Reserved withdrawals
- Reserved fees and taxes
- Blocked or pledged cash
- Margin requirements
- Capital-call reserves
+ Confirmed incoming settlements available by required date
```

### Projected cash by value date

```text
Projected Cash(t)
= Current Settled Cash
+ Cash inflows with value date <= t
- Cash outflows with value date <= t
```

## 4. Common cash transaction types

| Transaction type | Meaning | Quantity/position impact | Cash impact |
|---|---|---|---|
| CASH_DEPOSIT | External cash contribution | Increase cash | Inflow |
| CASH_WITHDRAWAL | External cash withdrawal | Decrease cash | Outflow |
| CASH_TRANSFER_IN | Internal or external transfer in | Increase cash | Inflow |
| CASH_TRANSFER_OUT | Internal or external transfer out | Decrease cash | Outflow |
| CASH_INTEREST_ACCRUAL | Accrued interest, if booked daily | May increase receivable | No cash until paid |
| CASH_INTEREST_PAYMENT | Interest paid into cash account | Increase cash | Inflow |
| CASH_FEE | Custody/platform/advisory/banking fee | Decrease cash | Outflow |
| CASH_TAX | Tax withholding or tax debit | Decrease cash | Outflow |
| CASH_BLOCK | Reserve/block cash | Available cash decreases | No ledger movement |
| CASH_UNBLOCK | Release reserved cash | Available cash increases | No ledger movement |
| CASH_OVERDRAFT_INTEREST | Interest on negative balance | Decrease cash | Outflow |
| CASH_SWEEP_OUT | Cash swept into deposit/MMF | Decrease operating cash | Outflow from cash, investment buy |
| CASH_SWEEP_IN | Cash swept back from deposit/MMF | Increase operating cash | Inflow to cash |

## 5. Cash transaction modelling principles

### Use signed cash amount

| Direction | Cash amount convention |
|---|---:|
| Cash inflow | Positive |
| Cash outflow | Negative |

### Keep currency explicit

Every cash entry must have:

- cash currency;
- cash amount;
- value date;
- booking date;
- source transaction reference;
- portfolio/account/cash account;
- classification for performance and reporting.

### Separate ledger movement from availability reservation

Order reservation should not create accounting cash movement until the trade settles. It should reduce available cash only.

Example:

| Step | Object | Ledger cash | Available cash |
|---|---|---:|---:|
| Client has USD 100,000 | Balance | 100,000 | 100,000 |
| Buy order placed for USD 40,000 | Reservation | 100,000 | 60,000 |
| Trade confirms | Pending settlement | 100,000 | 60,000 |
| Settlement occurs | Cash ledger entry | 60,000 | 60,000 |

## 6. Deposit lifecycle

A fixed deposit is not just cash. It is a term contract with interest and maturity rules.

| Stage | Business event | System object | Transaction? |
|---|---|---|---|
| Quote | Client receives deposit rate | Quote | No |
| Placement order | Client instructs deposit | Order | No accounting transaction yet |
| Placement/settlement | Cash is placed into deposit | Deposit position + cash out | Yes |
| Accrual | Interest accrues over tenor | Accrual record | Optional accounting transaction |
| Rate change | Floating/tiered rate changes | Rate reset event | No cash unless payment |
| Early break | Client breaks deposit before maturity | Break event + cash return | Yes |
| Maturity | Deposit matures | Close deposit | Yes |
| Rollover | Principal/interest reinvested | Close old + open new | Yes, grouped |

## 7. Deposit transaction types

| Transaction type | Use |
|---|---|
| DEPOSIT_PLACEMENT | Cash placed into deposit; creates deposit position |
| DEPOSIT_INTEREST_ACCRUAL | Periodic accrued interest, if accrual accounting is used |
| DEPOSIT_INTEREST_PAYMENT | Interest paid before or at maturity |
| DEPOSIT_MATURITY_REDEMPTION | Principal returned at maturity |
| DEPOSIT_EARLY_BREAK_REDEMPTION | Early withdrawal before maturity |
| DEPOSIT_BREAK_PENALTY | Penalty or forfeited interest on early break |
| DEPOSIT_ROLLOVER_OUT | Closing leg of matured deposit rolled over |
| DEPOSIT_ROLLOVER_IN | Opening leg of new deposit |
| DEPOSIT_TAX_WITHHOLDING | Tax withheld from interest |
| DEPOSIT_CORRECTION_REVERSAL | Correction/cancel/rebook |

## 8. Deposit position model

| Field | Description |
|---|---|
| deposit_id | Unique deposit contract ID |
| account_id | Holding account/portfolio |
| currency | Deposit currency |
| principal_amount | Principal placed |
| current_principal | Remaining principal after partial break, if allowed |
| start_date | Deposit start/value date |
| maturity_date | Contract maturity date |
| tenor | 1W, 1M, 3M, 6M, 1Y, etc. |
| interest_rate | Contractual annual rate |
| rate_type | Fixed / floating / tiered |
| day_count | ACT/360, ACT/365, 30/360, etc. |
| interest_frequency | At maturity, monthly, quarterly, annually |
| interest_destination | Pay to cash / compound / rollover |
| early_break_allowed | Yes/no |
| early_break_rule | Penalty, reduced rate, no interest, bank discretion |
| rollover_instruction | None / principal / principal+interest |
| deposit_insurance_eligible | Yes/no/unknown depending on jurisdiction/product |
| pledged_flag | Yes/no |
| lifecycle_status | Active / matured / broken / rolled / cancelled |

## 9. Fixed deposit example

Client places SGD 100,000 for 6 months at 3.00% p.a., ACT/365, interest at maturity.

```text
Interest = Principal × Rate × Days / Day Count Basis
Interest = 100,000 × 3.00% × 182 / 365 = SGD 1,495.89
```

Transactions:

| Date | Transaction type | Instrument | Cash impact | Deposit impact |
|---|---|---|---:|---:|
| Start | DEPOSIT_PLACEMENT | SGD fixed deposit | -100,000 | +100,000 principal |
| Maturity | DEPOSIT_MATURITY_REDEMPTION | SGD fixed deposit | +100,000 | -100,000 principal |
| Maturity | DEPOSIT_INTEREST_PAYMENT | Cash | +1,495.89 | No principal impact |

## 10. Deposit early break example

If the client breaks the deposit early:

| Component | Treatment |
|---|---|
| Principal | Returned, possibly after notice/cutoff |
| Accrued interest | May be reduced, forfeited or recalculated at lower rate |
| Penalty | May be explicit fee or embedded in reduced interest |
| Performance | Penalty is expense; reduced yield affects income |
| Reporting | Show actual realised income, not originally expected income |

## 11. Deposit rollover modelling

A rollover is best modelled as a grouped lifecycle event:

| Transaction | Effect |
|---|---|
| DEPOSIT_MATURITY_REDEMPTION | Close old deposit principal |
| DEPOSIT_INTEREST_PAYMENT | Pay interest, unless compounded |
| DEPOSIT_ROLLOVER_IN / DEPOSIT_PLACEMENT | Open new deposit with new terms |

Do not silently extend the maturity date of the old deposit unless the bank’s source system legally represents it as the same contract. Most platforms should create a new deposit instance for auditability.

## 12. Foreign currency fixed deposits

Foreign currency deposits add important complexity:

- client may earn interest in foreign currency;
- principal may be exposed to FX movement versus reporting currency;
- deposit insurance may not apply depending on local rules;
- maturity proceeds may need FX conversion;
- client may face spread/cost on conversion;
- negative interest or low interest may apply in some currencies/periods;
- suitability should consider currency need, not only headline interest rate.

Example:

A SGD-based client places USD 100,000 in a USD fixed deposit. Even if the USD deposit pays 4%, the SGD return can be negative if USD weakens significantly against SGD.

## 13. Cash and deposit performance treatment

| Event | Performance treatment |
|---|---|
| External cash deposit into portfolio | External inflow |
| External cash withdrawal | External outflow |
| Cash interest payment | Investment income |
| Deposit placement from portfolio cash | Internal investment allocation, not external outflow |
| Deposit maturity to portfolio cash | Internal redemption, not external inflow |
| Early break penalty | Expense / negative income depending on policy |
| FX revaluation of foreign cash | Investment effect / FX translation effect |
| Transfer between same client portfolios | Usually internal transfer, policy-dependent |

## 14. Common platform errors

| Error | Impact |
|---|---|
| Using ledger cash as available cash | Orders approved without enough settled/available cash |
| Ignoring value dates | Settlement failures and overdrafts |
| Treating deposit placement as external outflow | Performance distortion |
| Ignoring accrued interest | Understates portfolio value/income |
| Not modelling deposit maturity | Cash projection and liquidity reports wrong |
| Aggregating cash across currencies without FX details | Hides funding shortfall in settlement currency |
| Ignoring blocked/pledged cash | Overstates liquidity |
| Incorrect day-count basis | Wrong income and yield |
| Missing early break penalty | Overstates realised return |

## 15. Practical rule

For wealth platforms:

```text
Cash balance is the accounting fact.
Available cash is a business calculation.
Projected cash is a value-date forecast.
Deposit is a contract.
Interest is income.
FX translation is performance/risk analytics.
```

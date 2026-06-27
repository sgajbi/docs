# 08 — Platform Implementation, Controls, Reconciliation, Test Scenarios and Glossary

## 1. Implementation architecture

A robust wealth platform should implement this product family across several services/capabilities:

| Capability | Responsibility |
|---|---|
| Product master | Cash pseudo-instruments, deposit products, MM instruments, MMF share classes, FX contracts |
| Cash ledger | Accounting cash entries and balances |
| Balance calculator | Ledger/settled/available/projected cash |
| Deposit lifecycle | Placement, accrual, maturity, break, rollover |
| Money market lifecycle | Instrument maturity, discount accretion, MMF subscription/redemption/distribution |
| FX lifecycle | Spot, forward, swap, NDF opening, MTM, fixing, settlement, roll |
| Valuation | Cash translation, deposit accrual, NAV, money market price, FX MTM |
| Income engine | Interest, distributions, accretion, overdraft expense |
| Risk/analytics | Liquidity, FX exposure, cash drag, maturity ladder, concentration |
| Buying power | Pre-trade cash and credit availability |
| Advisory/mandate | Suitability, cash bands, tenor, currency and issuer limits |
| Reporting | Client reports, portfolio review, statement, tax/income reports |
| Reconciliation | Custodian, bank, fund TA, FX counterparty, market data |

## 2. Key controls

| Control | Purpose |
|---|---|
| Currency-level cash validation | Prevent cross-currency funding errors |
| Value-date validation | Prevent settlement-date mismatch |
| Balance-type separation | Avoid using ledger cash as available cash |
| Negative cash alert | Detect overdrafts and funding issues |
| Blocked/pledged cash exclusion | Avoid overstating liquidity |
| Deposit maturity monitor | Avoid missing maturities/rollovers |
| Deposit rate/day-count validation | Prevent interest errors |
| MMF NAV-date validation | Prevent stale valuation |
| MMF dealing cutoff control | Avoid wrong settlement assumption |
| FX pair convention validation | Avoid inverted FX rates |
| FX holiday calendar check | Ensure correct value date |
| Forward maturity monitor | Prevent unhandled FX settlements |
| NDF fixing control | Ensure correct fixing source/date |
| Bank/issuer concentration check | Control cash/deposit/CD concentration |
| Idempotency control | Avoid duplicate cash bookings |
| Reversal/cancel-rebook audit | Preserve operational traceability |

## 3. Reconciliation

### Cash reconciliation

| Source | Compare with |
|---|---|
| Custodian cash balance | Internal ledger balance |
| Bank account statement | Cash ledger entries |
| Settlement system | Pending settlement projections |
| Order system | Reserved cash |
| Fee/tax system | Fee/tax debits |

### Deposit reconciliation

| Source | Compare with |
|---|---|
| Deposit system | Internal deposit contracts |
| Bank maturity schedule | Deposit maturity report |
| Interest statement | Accrued/paid interest |
| Rollover instruction | New deposit bookings |

### MMF / fund reconciliation

| Source | Compare with |
|---|---|
| Transfer agent | Units and transactions |
| NAV vendor | NAV and NAV date |
| Custodian | Holdings and cash settlements |
| Fund distribution file | Income/distribution bookings |

### FX reconciliation

| Source | Compare with |
|---|---|
| FX trade platform | Internal FX trade records |
| Counterparty confirmation | Trade economic terms |
| Custodian cash ledger | Settlement legs |
| Market data | Spot/forward valuation inputs |
| NDF fixing source | Fixing event and cash settlement |

## 4. Test scenarios — cash balances

| Scenario | Expected result |
|---|---|
| Portfolio has USD and SGD cash | Balances reported separately by currency |
| Pending USD equity buy | USD available cash reduced before settlement |
| SGD cash positive, USD cash negative | USD overdraft shown; not hidden by SGD surplus |
| Cash blocked for pledge | Ledger unchanged; available cash reduced |
| Withdrawal reserved | Available cash reduced; ledger updated only on payment |
| Same-day reversal | Original and reversal both visible; net effect zero |

## 5. Test scenarios — deposits

| Scenario | Expected result |
|---|---|
| Fixed deposit placement | Cash decreases, deposit position created |
| Daily interest accrual | Accrued income increases according to day count |
| Interest paid at maturity | Cash increases, income booked |
| Deposit matures | Deposit position closes, cash principal returned |
| Early break with penalty | Principal returned, reduced interest/penalty booked |
| Rollover principal only | Old deposit closes, new deposit opens for principal, interest paid to cash |
| Rollover principal + interest | New deposit amount includes interest |
| Foreign deposit revaluation | Base-currency value changes with FX |

## 6. Test scenarios — money market instruments/funds

| Scenario | Expected result |
|---|---|
| T-bill bought at discount | Position nominal created; cost below par |
| T-bill matures | Par cash received; realised/accreted income recognised |
| MMF subscription | Cash decreases; fund units increase at NAV |
| MMF redemption | Units decrease; cash increases on settlement date |
| MMF distribution | Income cash booked or reinvested units created |
| Stale NAV received | Valuation flagged or rejected depending tolerance |
| Liquidity gate | Redemption order held; no cash until settlement |
| MMF classified as cash equivalent | Report bucket shows classification but underlying remains fund |

## 7. Test scenarios — FX

| Scenario | Expected result |
|---|---|
| FX spot USD buy/SGD sell | Two cash legs created with correct signs |
| FX rate inversion error | Validation rejects or flags inconsistent amounts |
| FX value date holiday | Value date adjusted according to currency calendars |
| FX forward opened | Open contract created, no immediate principal cash exchange unless collateral |
| FX forward MTM changes | Valuation changes without cash ledger entry |
| FX forward matures | Buy and sell cash legs settle |
| FX forward rolled | Old contract closed; new contract opened; realised P&L handled |
| NDF fixing occurs | Lifecycle fixing event recorded |
| NDF settles | Net cash settlement booked in settlement currency |

## 8. Test scenarios — analytics and reporting

| Scenario | Expected result |
|---|---|
| Cash held in multiple currencies | FX exposure report shows each currency |
| Foreign cash appreciates | Base-currency P&L reflects FX translation |
| Cash-heavy portfolio | Cash drag analytics calculated |
| Deposit maturity ladder | Deposits grouped by maturity bucket |
| Negative cash exists | Report shows overdraft/negative cash explicitly |
| MMF T+1 redemption used for T+0 trade | Availability check fails unless policy allows bridge funding |
| Pledged cash | Liquidity report separates restricted cash |
| FX hedge exceeds exposure | Mandate warning/breach generated |

## 9. Migration considerations

When migrating cash/deposit/FX history:

- preserve original trade, value and booking dates;
- preserve currency and FX rate used at original booking;
- migrate open deposits with original start/maturity/rate terms;
- migrate accrued interest as of cutover;
- migrate open FX forwards/NDFs with contracted rates and value dates;
- migrate pending settlements and reservations;
- reconcile opening cash by currency to source/custodian;
- avoid creating artificial external cashflows that distort performance;
- tag migration transactions separately;
- validate base-currency market values after FX translation.

## 10. Production triage checklist

When cash or FX reports are wrong, ask:

1. Is the issue currency-specific?
2. Is ledger cash or available cash wrong?
3. Is the value date correct?
4. Is a pending settlement missing?
5. Is blocked/pledged cash included incorrectly?
6. Is a transaction duplicated?
7. Was a reversal/correction applied?
8. Is the FX rate inverted or stale?
9. Is the NAV/date stale for MMF?
10. Is deposit accrued interest calculated using correct day count?
11. Is cash translated using the right reporting FX rate?
12. Is the performance classification correct?

## 11. Glossary

| Term | Meaning |
|---|---|
| Available cash | Cash that can be used after restrictions and reservations |
| Base currency | Reporting/performance currency of portfolio |
| Blocked cash | Cash not available due to hold, pledge or restriction |
| Cash drag | Performance drag from holding cash versus target assets |
| Cash equivalent | Product analytically treated as near-cash but not necessarily legal cash |
| Commercial paper | Short-term unsecured corporate debt |
| Deposit maturity | Date principal/interest is due back |
| Early break | Withdrawal of deposit before maturity |
| FX forward | Future-dated currency exchange contract |
| FX spot | Near-term currency exchange |
| FX swap | Linked near and far currency exchange |
| Ledger balance | Posted accounting cash balance |
| Money market fund | Fund investing in short-term instruments |
| NAV | Net asset value per fund unit/share |
| NDF | Non-deliverable forward, cash-settled FX forward |
| Overdraft | Negative cash balance/borrowing |
| Projected cash | Future cash after expected inflows/outflows |
| Repo | Repurchase agreement, collateralised financing |
| Reverse repo | Cash lending secured by collateral |
| Settled balance | Cash with completed value-date settlement |
| T-bill | Short-term government debt issued at discount |
| Value date | Date cash actually settles/has value |

## 12. Practitioner reference table

| Question | Best answer |
|---|---|
| Is cash always safe? | No. It has bank, currency, inflation, settlement and restriction risk. |
| Is available cash the same as ledger cash? | No. Available cash subtracts pending obligations and restrictions. |
| Is a money market fund cash? | No. It is a fund that may be classified as cash equivalent for analytics. |
| Is FX conversion an external cashflow? | No, if it happens inside the portfolio; it is an internal currency conversion. |
| Does a deposit placement reduce portfolio value? | No, it reallocates cash into a deposit position. |
| Should negative cash be netted away? | No, show it clearly by currency as liability/overdraft. |
| Are FX forwards cash positions? | No, they are open derivative contracts until settlement. |
| Can foreign deposit yield be misleading? | Yes, base-currency return depends on FX movement. |
| What is most important for buying power? | Available cash by settlement currency and value date. |

## 13. Implementation principle

```text
Never approve an order using total portfolio cash alone.
Always check currency, value date, availability, restrictions, pending settlements and funding rules.
```

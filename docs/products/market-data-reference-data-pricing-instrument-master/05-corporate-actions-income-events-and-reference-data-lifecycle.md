# 05 - Corporate Actions, Income Events and Reference Data Lifecycle

## 1. Why corporate action data belongs in this pack

Corporate actions and income events are event-driven reference data. They change positions, cash, cost basis, income, performance and client reports.

They are not just operational notices; they are product lifecycle events.

## 2. Event categories

| Category | Examples |
|---|---|
| Income events | cash dividend, stock dividend, coupon, fund distribution, REIT distribution |
| Mandatory events | stock split, reverse split, merger, redemption, maturity, spin-off |
| Voluntary events | tender offer, rights exercise, optional dividend election |
| Choice events | cash or stock dividend, election options |
| Reorganizations | merger, acquisition, exchange, conversion |
| Fixed income events | call, put, redemption, amortization, default, restructuring |
| Fund events | distribution, split, class merger, suspension, gate |
| Structured product events | autocall, barrier trigger, final fixing, physical delivery |

## 3. Corporate action lifecycle

| Stage | Meaning |
|---|---|
| Announcement | Event announced by issuer/source |
| Terms capture | Key fields captured and normalized |
| Eligibility date | Record/ex-date determines holder entitlement |
| Election window | Client/advisor may choose option |
| Instruction | Election instruction sent |
| Payment/allocation | Cash/securities received |
| Booking | Transactions and position movements posted |
| Reconciliation | Custody vs platform matched |
| Correction | Event terms or cash corrected |
| Audit closure | Evidence stored |

## 4. Key dates

| Date | Meaning |
|---|---|
| announcement date | when event is announced |
| ex-date | date from which buyer is not entitled to distribution |
| record date | date holder must be on record |
| election deadline | deadline for voluntary choices |
| payment date | cash/securities paid |
| effective date | date event legally takes effect |
| booking date | date platform posts transaction |
| knowledge date | date platform receives/corrects data |

## 5. Corporate action data model

```text
corporate_action_event
  event_id
  instrument_id
  event_type
  status
  announcement_date
  ex_date
  record_date
  payment_date
  effective_date
  source
  source_event_id
  terms_version

corporate_action_option
  event_id
  option_id
  option_type
  cash_rate
  security_entitlement_ratio
  currency
  default_option_flag
  election_required_flag

corporate_action_entitlement
  event_id
  account_id
  eligible_quantity
  elected_option_id
  expected_cash
  expected_security_quantity
  booking_status
```

## 6. Corporate action impact by event

| Event | Position impact | Cash impact | Cost basis impact |
|---|---|---|---|
| Cash dividend | no security quantity change | cash income | usually none |
| Stock dividend | additional shares | no cash | allocated basis or income basis by policy |
| Split | quantity changes, price adjusts | none | total basis unchanged |
| Reverse split | quantity reduces, price adjusts | possible cash-in-lieu | basis reallocated |
| Rights issue | rights security may be created | subscription cash if exercised | new shares basis includes subscription |
| Spin-off | new security created | usually none | basis allocated between old/new securities |
| Merger cash | old security closed | cash received | realized gain/loss |
| Merger stock | old security exchanged | new security received | carryover or fair value basis |
| Bond call | bond redeemed | principal/call premium | realized gain/loss |
| Fund distribution | cash/reinvestment units | cash or units | income/return of capital classification |

## 7. Income event classification

Income classification matters for reporting, performance and tax-aware reporting.

| Income type | Examples |
|---|---|
| Dividend income | equity dividends, REIT distributions |
| Interest income | bond coupons, deposit interest |
| Fund distribution | income distribution, capital distribution |
| Return of capital | capital repayment/distribution |
| Manufactured income | securities lending/margin contexts |
| Withholding tax | tax deducted at source |

## 8. Data quality controls

| Control | Purpose |
|---|---|
| entitlement reconciliation | expected vs received cash/securities |
| event duplicate check | avoid double booking |
| ex-date/record-date validation | avoid wrong holder eligibility |
| ratio sanity check | detect split/rights errors |
| cash rate currency check | prevent wrong currency income |
| security mapping check | new instrument must exist before posting |
| election deadline control | avoid missed client election |
| late correction workflow | restate positions/performance if needed |

## 9. Reporting implications

Client reports should explain corporate-action-driven changes clearly. Large unexplained quantity changes, sudden cost-basis shifts or missing dividend income create client trust issues.

For professional reporting, maintain event labels such as:

- "cash dividend",
- "stock split",
- "rights entitlement",
- "merger consideration",
- "bond call redemption",
- "fund distribution reinvestment",
- "return of capital",
- "cash in lieu".

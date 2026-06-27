# 04 - Custody, Asset Servicing, Corporate Actions and Income Events

## 1. Purpose

Custody and asset servicing are the post-settlement processes that keep assets safe and process events affecting held instruments.

For wealth platforms, custody and asset servicing are critical because they drive:

- current holdings
- income reporting
- cost basis
- realized and unrealized P&L
- performance returns
- mandate exposure
- tax reporting
- client statements
- advisor explanations

## 2. Custody models

| Model | Meaning | Platform implication |
|---|---|---|
| Direct registration | Investor appears on issuer/share register | Ownership data from registry/transfer agent |
| Nominee custody | Custodian/nominee holds legal title for beneficial owner | Need beneficial-owner mapping |
| CSD account | Securities held in central securities depository | Market-specific account model |
| Omnibus account | Many clients pooled at custodian | Need internal sub-ledger and allocation accuracy |
| Segregated account | Client/account has distinct custody account | Easier client-level reconciliation, higher cost |
| Fund register | Fund units held by transfer agent/platform | Dealing and register-based reconciliation |
| Private market register | LP interest tracked by administrator/GP | Commitment/capital-call lifecycle |
| Physical custody | Bullion/certificates held physically | Storage, insurance, audit and serial/bar details |

## 3. Custody account hierarchy

A wealth platform may need multiple levels:

```text
Client / Legal Entity
  -> Relationship / Household
  -> Portfolio / Mandate
  -> Account
  -> Custody Account
  -> Sub-account / Depot
  -> Market / CSD Account
  -> Security Position
```

Do not assume one client equals one custody account. HNW/UHNW clients often have multiple entities, mandates, currencies, booking centres and custodians.

## 4. Safekeeping position

A custody/safekeeping position should track:

| Field | Meaning |
|---|---|
| custodian_id | Safekeeping institution |
| custody_account_id | Account at custodian |
| instrument_id | Security/instrument |
| settled_quantity | Quantity settled and held |
| pending_receipt_quantity | Pending buys/transfers in |
| pending_delivery_quantity | Pending sells/transfers out |
| blocked_quantity | Pledged, collateral, corporate-action blocked, restricted |
| available_quantity | Quantity available for sale/transfer |
| position_date | Statement date |
| source_statement_id | Custodian statement reference |
| reconciliation_status | Matched/break |

## 5. Asset servicing categories

| Category | Examples |
|---|---|
| Income events | Cash dividend, stock dividend, coupon, interest, fund distribution |
| Mandatory corporate actions | Stock split, reverse split, merger, spin-off, redenomination |
| Mandatory with options | Dividend election, scrip dividend, merger option |
| Voluntary corporate actions | Rights exercise, tender offer, exchange offer, conversion |
| Redemptions | Bond maturity, partial call, fund liquidation, note autocall/maturity |
| Meetings/proxy | AGM/EGM, proxy vote, consent solicitation |
| Tax events | Withholding, reclaim, tax voucher, treaty-rate application |
| Claims/compensation | Market claim, compensation, late payment, manufactured dividend |

## 6. Corporate action lifecycle

```text
Announcement
  -> event capture and normalization
  -> eligibility determination
  -> entitlement calculation
  -> client/advisor notification where required
  -> election instruction if voluntary/choice event
  -> market/custodian instruction
  -> entitlement allocation
  -> cash/security booking
  -> tax/cost-basis adjustment
  -> reconciliation
  -> reporting update
```

Separate corporate-action event state from resulting transactions.

## 7. Corporate action event model

| Field | Meaning |
|---|---|
| corporate_action_event_id | Unique event |
| event_type | DIVIDEND / SPLIT / RIGHTS / MERGER / TENDER / SPINOFF / REDEMPTION |
| source_event_id | Custodian/vendor/issuer event ID |
| instrument_id | Underlying instrument |
| announcement_date | Date announced |
| ex_date | Date from which buyer no longer receives entitlement |
| record_date | Date holdings are checked |
| payable_date | Date entitlement paid/delivered |
| election_deadline | Deadline for voluntary election |
| market_deadline | Market/custodian deadline |
| default_option | Default treatment if no election |
| options | Available options |
| ratio | Entitlement ratio |
| cash_rate | Cash payment rate |
| tax_rate | Withholding/tax rate |
| status | Announced/confirmed/cancelled/revised/paid |
| version | Event version |

## 8. Entitlement calculation

Entitlement depends on:

- eligible position basis
- ex-date and record-date rules
- settled/unsettled trade treatment
- lending/borrowing status
- repo/collateral status
- corporate-action protection rules
- market-claim rules
- tax status
- rounding rules
- account restrictions

Example: cash dividend

```text
Gross dividend = eligible shares x dividend rate
Withholding tax = gross dividend x tax rate
Net dividend = gross dividend - withholding tax - fees
```

Example: stock split 2-for-1

```text
New quantity = old quantity x 2
New average cost per share = old total cost / new quantity
Market value should be economically continuous except for market movement
```

## 9. Corporate action transaction types

| Transaction type | Use |
|---|---|
| CASH_DIVIDEND | Cash dividend received |
| STOCK_DIVIDEND | Additional shares from stock dividend |
| DIVIDEND_REINVESTMENT | Dividend used to buy/reinvest shares/units |
| COUPON_PAYMENT | Bond/note coupon |
| FUND_DISTRIBUTION | Fund income/capital distribution |
| FUND_REINVESTMENT | Fund distribution reinvested into units |
| STOCK_SPLIT | Quantity increase and cost adjustment |
| REVERSE_SPLIT | Quantity reduction and cost adjustment |
| RIGHTS_ENTITLEMENT | Rights received |
| RIGHTS_EXERCISE | Cash paid and shares received |
| RIGHTS_LAPSE | Rights expired worthless |
| TENDER_ACCEPTANCE | Securities tendered for cash/securities |
| MERGER_EXCHANGE_OUT | Old security removed |
| MERGER_EXCHANGE_IN | New security received |
| SPINOFF_IN | New spun-off security received |
| CASH_IN_LIEU | Cash for fractional entitlement |
| BOND_REDEMPTION | Bond maturity/call/paydown |
| NOTE_REDEMPTION | Note maturity/autocall |
| TAX_WITHHOLDING | Tax withheld |
| TAX_RECLAIM | Tax reclaimed/credited |
| CORPORATE_ACTION_REVERSAL | Event correction/reversal |

## 10. Income events

### Dividends

Dividend types:

- ordinary cash dividend
- special dividend
- stock dividend
- scrip dividend
- dividend reinvestment
- capital repayment
- return of capital
- manufactured dividend under lending/repo

Accounting treatment differs by type. A special dividend may be income in client reporting but could require cost-basis adjustment depending jurisdiction and product policy.

### Coupons and interest

Bond/note income requires:

- coupon schedule
- day-count convention
- ex-coupon rules
- accrued interest calculation
- withholding tax where applicable
- clean/dirty price treatment
- defaulted/accrual-stopped status

### Fund distributions

Fund distributions may represent:

- income distribution
- capital gain distribution
- return of capital
- equalisation adjustment
- reinvestment into additional units

Classify gross and net amounts correctly for reporting.

## 11. Voluntary corporate actions

Voluntary actions require client/advisor election.

Examples:

| Event | Decision |
|---|---|
| Rights issue | Exercise, sell rights, let lapse, oversubscribe |
| Tender offer | Tender all/some/none |
| Exchange offer | Exchange into new security or keep old |
| Conversion | Convert bond/preference share/warrant |
| Optional dividend | Cash or stock/scrip |
| Consent solicitation | Vote/consent/decline |

Platform requirements:

- event notification
- eligibility and entitlement
- options and deadlines
- recommendation/approval workflow where advisory
- election capture
- custodian instruction
- confirmation of instruction
- allocation and booking
- late/amended election handling

## 12. Mandatory corporate actions

Mandatory events occur without investor election, though some may have options.

Examples:

- split
- reverse split
- merger
- mandatory exchange
- spin-off
- name change
- ISIN change
- redenomination
- mandatory redemption
- liquidation

Even mandatory events require careful modelling because quantity, cost basis, identifier, classification and historical performance continuity may change.

## 13. Corporate action and cost basis

Cost basis handling is central for P&L and tax-aware reporting.

| Event | Common treatment |
|---|---|
| Split | Quantity changes, total cost unchanged, unit cost adjusted |
| Reverse split | Quantity decreases, total cost unchanged, fractional cash-in-lieu may realize P&L |
| Stock dividend | Additional shares; basis may be allocated or zero depending policy |
| Spin-off | Original cost basis allocated between parent and spun-off entity |
| Merger for cash | Old position closed, realized P&L booked |
| Merger for shares | Old security exchanged for new; basis carried/allocated |
| Return of capital | May reduce cost basis rather than income |
| Rights exercise | New share cost includes exercise price plus rights basis if applicable |

Platform should make cost-basis method configurable by jurisdiction and reporting purpose.

## 14. Corporate actions and performance

For performance, many corporate actions should preserve economic continuity.

| Event | Performance treatment |
|---|---|
| Split/reverse split | No return by itself; price/quantity adjusted |
| Cash dividend retained in portfolio | Investment income |
| Dividend withdrawn by client | Income followed by external withdrawal |
| Spin-off | Internal transformation; no external cashflow |
| Merger for cash | Security disposal; proceeds remain portfolio cash unless withdrawn |
| Rights issue funded from portfolio cash | Internal investment; not external contribution |
| Capital call funded externally | External contribution if new cash enters portfolio |
| Tax withholding | Investment-related tax/expense treatment per policy |

## 15. Proxy and voting

Proxy events matter for advisory and stewardship reporting.

Fields:

- meeting date
- record date
- voting deadline
- resolutions
- eligible quantity
- vote instruction
- account/beneficial owner
- advisor/PM recommendation
- custodian confirmation
- final vote status

For DPM/mandates, voting authority may sit with the manager, client, trustee or external proxy advisor.

## 16. Asset servicing controls

Key controls:

- event source reconciliation across vendor/custodian/issuer
- duplicate event detection
- event versioning and change alerts
- entitlement reconciliation
- election deadline monitoring
- option validation
- tax rate validation
- rounding/cash-in-lieu validation
- booking completeness
- position and cash reconciliation after event
- performance/cost-basis regression tests
- client notification audit

## 17. Platform recommendation

Do not model a corporate action only as a transaction. Use three layers:

```text
CorporateActionEvent
  -> Entitlement / Election
  -> Resulting Transactions
```

This preserves event terms, client choices, instructions, allocations and accounting output.

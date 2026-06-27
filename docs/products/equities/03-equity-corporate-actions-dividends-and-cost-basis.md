# 03 - Equity Corporate Actions, Dividends and Cost Basis

## 1. Why corporate actions are the hardest part of equities

Equity trading is straightforward compared with corporate-action processing. Corporate actions can change:

- quantity;
- cash;
- cost basis;
- tax;
- instrument identity;
- voting/election rights;
- dividend entitlement;
- issuer exposure;
- sector/country classification;
- performance and P&L;
- client reporting history.

Incorrect corporate-action processing can create wrong positions, wrong cash, wrong P&L, wrong performance, wrong tax and wrong client reports.

## 2. Corporate action event model

A corporate action should be modelled as an event with lifecycle, not merely as a transaction.

### 2.1 Corporate action event fields

| Field | Meaning |
|---|---|
| corporate_action_id | Unique event identifier. |
| event_type | Dividend, split, rights, merger, spin-off, tender, etc. |
| sub_event_type | Cash dividend, stock dividend, optional dividend, reverse split, etc. |
| issuer_id | Issuer initiating the action. |
| affected_instrument_id | Existing security impacted. |
| resulting_instrument_id | New security, if applicable. |
| announcement_date | Date issuer announces event. |
| ex_date | Date from which buyer is not entitled. |
| record_date | Date holder records are checked. |
| election_deadline | Deadline for voluntary/choice action. |
| effective_date | Date event becomes effective. |
| payable_date | Date cash/security is paid. |
| ratio | Split, spin-off, rights or merger ratio. |
| cash_rate | Dividend/cash consideration per share. |
| currency | Distribution currency. |
| tax_rate | Withholding or local tax rate, if applicable. |
| option_details | Available elections. |
| status | Announced, confirmed, revised, paid, cancelled. |
| source | Exchange, custodian, vendor, issuer, manual. |
| version | Event version for amendments/cancellations. |

### 2.2 Corporate action lifecycle

| Stage | Description | Transaction? |
|---|---|---|
| Announcement | Issuer announces event terms. | No. |
| Enrichment | Vendor/custodian enriches dates, ratio, options. | No. |
| Eligibility capture | Platform identifies eligible holdings. | No transaction, but entitlement snapshot. |
| Election | Client/advisor chooses option if voluntary. | No accounting transaction yet. |
| Instruction sent | Election sent to custodian/broker. | No, unless fees apply. |
| Confirmation | Custodian confirms election/entitlement. | No or pending entitlement. |
| Pay/effective date | Cash/security movement occurs. | Yes. |
| Reconciliation | Custodian positions/cash are matched. | No new transaction unless correction. |
| Amendment/correction | Issuer/custodian revises event. | Reversal/correction if already posted. |

## 3. Mandatory, voluntary and mandatory-with-options

| Type | Meaning | Example | Platform implication |
|---|---|---|---|
| Mandatory | Holder has no choice. | Cash dividend, stock split, merger where approved. | Process automatically when confirmed. |
| Voluntary | Holder must elect to participate. | Tender offer, rights exercise, proxy vote. | Require client/advisor election workflow. |
| Mandatory with options | Event happens, but holder may choose form. | Cash-or-stock dividend, scrip dividend. | Default option plus election management. |

## 4. Important corporate-action dates

| Date | Meaning | Practical importance |
|---|---|---|
| Announcement date | Company announces event. | Event enters system, but terms may change. |
| Ex-date | Buyer on/after this date is not entitled to distribution. | Critical for entitlement logic. |
| Record date | Holder list is captured. | Custodian uses it for entitlement. |
| Election deadline | Last date to submit choice. | Needed for voluntary events. |
| Market deadline | Earlier deadline set by exchange/custodian. | Client deadline is often earlier. |
| Effective date | Split/merger/capital event becomes effective. | Quantity or instrument changes. |
| Payable date | Cash/securities are paid. | Transaction posting date. |

Ex-date logic is often the most misunderstood. If a client buys on or after the ex-date, the client usually does not receive the next distribution. If the client buys before the ex-date and holds through entitlement rules, the client generally receives it.

## 5. Cash dividends

A cash dividend is a cash distribution to shareholders.

### 5.1 Business example

Client holds 1,000 shares. Dividend is USD 1.00 per share. Withholding tax is 30%.

| Item | Amount |
|---|---:|
| Gross dividend | 1,000 x 1.00 = USD 1,000 |
| Withholding tax | USD 300 |
| Net dividend | USD 700 |

Transactions:

| Transaction type | Amount | Meaning |
|---|---:|---|
| EQUITY_DIVIDEND_CASH | +USD 1,000 | Gross dividend income. |
| EQUITY_WITHHOLDING_TAX | -USD 300 | Tax withheld. |
| Net cash movement | +USD 700 | Net cash credited. |

### 5.2 Position impact

| Field | Impact |
|---|---|
| Share quantity | No change. |
| Cost basis | Usually no change for ordinary dividend. |
| Cash balance | Increases by net dividend. |
| Income | Gross or net income depending on reporting policy. |
| Performance | Dividend is investment income. |

### 5.3 Dividend accrual

For ordinary equities, many wealth platforms book dividends when announced/confirmed or on ex/pay date, not as daily accrual. For preferred shares, accrual may be more bond-like if dividend terms are fixed and system policy allows accrual.

## 6. Stock dividends and bonus shares

A stock dividend or bonus issue gives additional shares instead of cash.

Example: 10% stock dividend on 1,000 shares.

| Item | Value |
|---|---:|
| Existing shares | 1,000 |
| Stock dividend ratio | 10% |
| New shares received | 100 |
| Total shares | 1,100 |

Transaction:

| Transaction type | Quantity | Cash |
|---|---:|---:|
| EQUITY_DIVIDEND_STOCK | +100 shares | 0 |

Cost basis treatment may vary:

| Treatment | Meaning |
|---|---|
| Reallocate existing cost | Total cost remains same, cost per share decreases. |
| Recognize income at fair value | Stock dividend booked as income and new cost basis created. |
| Tax-specific treatment | Depends on jurisdiction and issuer event classification. |

For portfolio performance, if it is economically equivalent to a split/bonus issue, it should not create artificial return. If it is taxable income, income reporting may differ from performance treatment.

## 7. Stock split

A stock split increases the number of shares and reduces the price per share proportionally. Investor equity value does not change purely because of the split.

Example: 2-for-1 split.

| Item | Before | After |
|---|---:|---:|
| Quantity | 100 | 200 |
| Price | USD 100 | USD 50 |
| Market value | USD 10,000 | USD 10,000 |
| Total cost | USD 8,000 | USD 8,000 |
| Cost per share | USD 80 | USD 40 |

Transaction:

| Transaction type | Quantity impact | Cash impact |
|---|---:|---:|
| EQUITY_STOCK_SPLIT | +100 shares | 0 |

Performance treatment:

```text
Stock split itself is not a gain or loss.
It is a quantity and cost-per-share adjustment.
```

## 8. Reverse split

A reverse split reduces quantity and increases price per share proportionally.

Example: 1-for-10 reverse split.

| Item | Before | After |
|---|---:|---:|
| Quantity | 1,000 | 100 |
| Price | USD 2 | USD 20 |
| Market value | USD 2,000 | USD 2,000 |

Fractional shares may be paid as cash-in-lieu.

Transactions:

| Transaction type | Use |
|---|---|
| EQUITY_REVERSE_SPLIT | Adjust quantity. |
| EQUITY_CASH_IN_LIEU | Pay fractional entitlement. |

## 9. Rights issue

A rights issue gives existing shareholders the right to buy additional shares at a subscription price, often below market price.

### 9.1 Example

Client holds 1,000 shares. Rights ratio is 1 new share for every 5 shares held. Subscription price is USD 20.

| Item | Value |
|---|---:|
| Existing shares | 1,000 |
| Rights ratio | 1-for-5 |
| Rights received | 200 rights |
| Subscription price | USD 20 |
| Cash needed to fully exercise | 200 x 20 = USD 4,000 |

### 9.2 Lifecycle

| Stage | Object | Transaction? |
|---|---|---|
| Rights announced | Corporate action event | No. |
| Rights entitlement created | Temporary right security/entitlement | Yes or entitlement record. |
| Client elects | Election instruction | No transaction yet. |
| Rights exercised | Subscription transaction | Yes. |
| New shares issued | New security quantity | Yes. |
| Rights sold | Sale transaction | Yes. |
| Rights expire | Expiry transaction/write-off | Yes if entitlement tracked as position. |

### 9.3 Transaction types

| Scenario | Transactions |
|---|---|
| Rights received | EQUITY_RIGHTS_ENTITLEMENT. |
| Rights exercised | EQUITY_RIGHTS_SUBSCRIPTION plus cash out and new shares in. |
| Rights sold | EQUITY_RIGHTS_SALE and cash in. |
| Rights expired | EQUITY_RIGHTS_EXPIRY. |

### 9.4 Cost basis

Cost basis policy can be complex:

| Component | Possible cost treatment |
|---|---|
| Existing shares | Adjusted for theoretical ex-rights price if required. |
| Rights | Allocated value from old shares or zero cost depending on policy. |
| New shares | Subscription price plus allocated right cost and fees. |
| Sold rights | Proceeds may create realized gain depending on allocated cost. |

For platform design, support configurable cost-basis rules by market/tax policy.

## 10. Dividend reinvestment / DRIP / scrip dividend

A dividend reinvestment plan reinvests cash dividend into additional shares. A scrip dividend allows shareholder to receive shares instead of cash.

### 10.1 Example

Gross dividend USD 1,000 is reinvested at USD 50 per share.

| Item | Value |
|---|---:|
| Gross dividend | USD 1,000 |
| Reinvestment price | USD 50 |
| Shares received | 20 |

Transactions:

| Transaction | Meaning |
|---|---|
| EQUITY_DIVIDEND_CASH | Recognize dividend income. |
| EQUITY_DIVIDEND_REINVESTMENT | Use dividend cash to buy/receive shares. |
| EQUITY_WITHHOLDING_TAX | Tax deducted, if applicable. |

Avoid hiding reinvestment as a simple share increase. Income and reinvestment are different economic events.

## 11. Spin-off

A spin-off occurs when a company distributes shares of a subsidiary/new company to existing shareholders.

### 11.1 Example

Client owns 1,000 shares of ParentCo. Spin-off ratio: 1 NewCo share for every 5 ParentCo shares.

| Item | Value |
|---|---:|
| ParentCo shares | 1,000 |
| Ratio | 1-for-5 |
| NewCo shares received | 200 |

Transactions:

| Transaction type | Instrument | Quantity |
|---|---|---:|
| EQUITY_SPINOFF_IN | NewCo | +200 |
| EQUITY_SPINOFF_OUT / cost allocation | ParentCo | 0 or cost adjustment |

### 11.2 Cost allocation

Original cost basis may be allocated between ParentCo and NewCo based on relative fair values.

Example:

| Item | Value |
|---|---:|
| Original ParentCo cost | USD 100,000 |
| ParentCo post-event FMV | USD 80,000 |
| NewCo FMV | USD 20,000 |
| ParentCo cost allocation | 80% = USD 80,000 |
| NewCo cost allocation | 20% = USD 20,000 |

Performance treatment: spin-off is usually an internal transformation, not a new external cashflow. However, tax treatment can vary.

## 12. Merger and acquisition

A merger can involve cash, stock, or both.

### 12.1 All-cash acquisition

Client holds 1,000 OldCo shares. Acquisition price USD 50 per share.

| Transaction | Impact |
|---|---|
| EQUITY_MERGER_OUT | Remove OldCo shares. |
| EQUITY_TENDER_CASH / MERGER_CASH | Receive USD 50,000. |
| Realized P&L | Proceeds less cost basis. |

### 12.2 Stock-for-stock merger

OldCo shares convert into NewCo shares at ratio 0.5 NewCo for each OldCo.

| Transaction | Impact |
|---|---|
| EQUITY_MERGER_OUT | Remove OldCo shares. |
| EQUITY_MERGER_IN | Add NewCo shares. |
| Cash in lieu | Fractional shares. |
| Cost basis | Carry over or allocate based on policy. |

### 12.3 Cash-and-stock merger

Requires multiple legs:

| Leg | Meaning |
|---|---|
| Old shares out | Existing security closed/reduced. |
| New shares in | New security received. |
| Cash consideration | Cash received. |
| Cash in lieu | Fractional adjustment. |
| Tax | Possible withholding/capital-gains implications. |
| Realized P&L | May be partial depending on tax/accounting policy. |

## 13. Tender offer

A tender offer invites shareholders to sell shares at a specified price, often with conditions and proration.

| Feature | Meaning |
|---|---|
| Voluntary | Holder chooses whether to tender. |
| Deadline | Election deadline applies. |
| Proration | Not all tendered shares may be accepted. |
| Price | Fixed price or range. |
| Settlement | Cash and/or securities. |

Lifecycle:

| Stage | Transaction? |
|---|---|
| Offer announced | No. |
| Client elects to tender | No accounting transaction; block shares. |
| Shares accepted | EQUITY_TENDER_OUT. |
| Cash paid | EQUITY_TENDER_CASH. |
| Unaccepted shares released | No or unblock event. |

## 14. Share buyback

A share buyback occurs when the issuer repurchases its own shares. For clients, the impact depends on whether the buyback is open-market or tender-style.

| Type | Client impact |
|---|---|
| Open-market buyback | Usually no direct transaction for holder; may affect price/share count. |
| Tender buyback | Similar to tender offer; holder may elect to participate. |
| Mandatory capital reduction | Shares/cash adjusted by corporate action. |

## 15. Return of capital / capital repayment

A return of capital is a distribution that may reduce cost basis rather than being ordinary dividend income.

| Treatment | Impact |
|---|---|
| Reduce cost basis | Cost amount decreases. |
| Income | Treated like income if local rules require. |
| Capital gain after cost reaches zero | Some regimes treat excess as gain. |

Transaction:

| Transaction type | Use |
|---|---|
| EQUITY_RETURN_OF_CAPITAL | Cash received and cost basis adjusted. |

This is critical for REITs and certain corporate distributions.

## 16. Delisting, suspension and bankruptcy

| Event | Meaning | Platform treatment |
|---|---|---|
| Trading suspension | Security cannot trade temporarily. | Keep position, mark liquidity flag, price may become stale. |
| Delisting | Security removed from exchange. | Update listing status; may become OTC/private. |
| Bankruptcy | Issuer enters insolvency. | Price may go to zero or recovery claim may remain. |
| Liquidation | Assets distributed over time. | Liquidation payments and write-offs. |
| Write-off | Position deemed worthless. | EQUITY_WRITE_OFF with realized loss. |

Do not automatically write off a suspended or delisted equity unless confirmed by custodian/accounting policy. A delisted security can still have residual value.

## 17. Symbol, ticker, ISIN and name changes

These events usually do not change economic ownership.

| Event | Impact |
|---|---|
| Ticker change | Update listing identifier. |
| ISIN change | May require instrument identity mapping. |
| Company name change | Master data update. |
| Exchange migration | Listing update. |

Avoid creating artificial buy/sell transactions if only identifiers changed. Use instrument identity history and cross-reference mappings.

## 18. ADR/GDR corporate actions

ADRs/GDRs add another layer because local corporate action must be translated through the depositary.

| Event | Special consideration |
|---|---|
| Cash dividend | Local dividend converted to ADR currency, less tax and depositary fee. |
| Stock split | ADR ratio may change instead of ADR quantity changing. |
| Rights issue | ADR holders may or may not be able to participate. |
| Spin-off | Depositary may distribute cash instead of foreign shares. |
| Depositary fee | May be deducted from dividend or charged separately. |
| Ratio change | ADR-to-local-share ratio changes; quantity/price interpretation changes. |

Platform fields required:

- ADR ratio;
- depositary bank;
- local underlying instrument;
- local event reference;
- ADR event reference;
- FX conversion rate;
- tax withholding;
- depositary fee;
- election restrictions.

## 19. Proxy voting

Proxy voting is not usually a cash/security transaction, but it is important for advisory and custody workflows.

| Field | Meaning |
|---|---|
| meeting_date | Company meeting date. |
| record_date | Entitlement to vote. |
| agenda_items | Resolutions. |
| vote_options | For/against/abstain. |
| instruction_deadline | Custodian deadline. |
| client_instruction | Client/advisor election. |
| confirmation | Custodian confirmation. |

Proxy voting is usually an operational event, not a portfolio transaction, unless there are fees.

## 20. Entitlement snapshot design

For every corporate action, capture the holdings used to compute entitlement.

| Field | Meaning |
|---|---|
| corporate_action_id | Event identifier. |
| account_id | Portfolio. |
| instrument_id | Affected security. |
| entitlement_quantity | Eligible quantity. |
| entitlement_basis_date | Ex-date/record-date policy. |
| calculated_entitlement | Cash/shares/rights expected. |
| custodian_entitlement | Confirmed entitlement. |
| difference | Reconciliation break. |
| status | Calculated, confirmed, paid, disputed. |

This avoids recalculating historical entitlement from current positions, which may have changed due to later trades.

## 21. Corporate action transaction mapping

| Corporate action | Typical transactions |
|---|---|
| Cash dividend | EQUITY_DIVIDEND_CASH + EQUITY_WITHHOLDING_TAX. |
| Stock dividend | EQUITY_DIVIDEND_STOCK. |
| Split | EQUITY_STOCK_SPLIT. |
| Reverse split | EQUITY_REVERSE_SPLIT + optional CASH_IN_LIEU. |
| Rights issue | RIGHTS_ENTITLEMENT, RIGHTS_SUBSCRIPTION or RIGHTS_SALE or RIGHTS_EXPIRY. |
| DRIP/scrip | DIVIDEND_CASH, WITHHOLDING_TAX, DIVIDEND_REINVESTMENT. |
| Spin-off | SPINOFF_IN + cost allocation. |
| Merger cash | MERGER_OUT + MERGER_CASH. |
| Merger stock | MERGER_OUT + MERGER_IN + CASH_IN_LIEU. |
| Tender offer | TENDER_OUT + TENDER_CASH, plus release unaccepted shares. |
| Capital return | RETURN_OF_CAPITAL + cost-basis adjustment. |
| Liquidation | LIQUIDATION_PAYMENT + WRITE_OFF when final. |
| ADR fee | ADR_FEE. |

## 22. Corporate-action controls

| Control | Purpose |
|---|---|
| Event source comparison | Compare custodian, exchange and vendor event terms. |
| Event versioning | Preserve amendments and cancellations. |
| Eligibility snapshot | Lock holder quantities used for entitlement. |
| Election deadline control | Prevent late or missing voluntary instructions. |
| Cash/security reconciliation | Match expected vs custodian actual postings. |
| Tax validation | Check withholding and reclaim treatment. |
| Cost-basis validation | Ensure no artificial P&L from splits/spin-offs. |
| Price adjustment validation | Ensure historical price series adjusted consistently. |
| Manual override approval | Require maker-checker for manual event corrections. |
| Client reporting explanation | Explain unusual events clearly in statements/reports. |

## 23. Practitioner explanation

> Corporate actions are the hardest part of equity platforms because they can affect share quantity, cash, tax, cost basis, instrument identity and performance without being normal buy/sell trades. I would model corporate actions as lifecycle events with announcement, ex-date, record date, election deadline, effective date and payable date. Transactions should only be generated when an economic posting occurs, such as dividend cash, withholding tax, new shares, rights exercise, merger consideration or write-off. For accurate reporting, the platform needs entitlement snapshots, event versioning, cost-basis rules, tax handling, custodian reconciliation and correction workflows.

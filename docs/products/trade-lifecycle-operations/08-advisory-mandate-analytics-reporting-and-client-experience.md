# 08 - Advisory, Mandate, Analytics, Reporting and Client Experience

## 1. Purpose

Trade lifecycle is not only an operations topic. It directly impacts advisory, DPM mandates, portfolio analytics, buying power, performance, risk, compliance and client reporting.

A platform that shows only final positions misses the story behind client outcomes.

## 2. Advisory impact

Advisors need lifecycle visibility to answer:

- Has the client order been accepted?
- Was it fully or partially executed?
- What price did the client receive?
- Has the trade settled?
- Is cash available after pending trades?
- Why does the portfolio still show a pending position?
- Did a corporate action affect the holding?
- Why did performance change after a correction?
- Are there pending rights/elections requiring client action?
- Is a security blocked, pledged, restricted or unavailable?

## 3. DPM and mandate impact

DPM portfolios require lifecycle-aware controls:

| Mandate area | Lifecycle dependency |
|---|---|
| Target allocation | Use trade-date or settlement-date exposure consistently |
| Drift monitoring | Include pending trades where policy says so |
| Rebalancing | Avoid double-trading against open orders |
| Restrictions | Check post-trade and pending exposure |
| Liquidity buffer | Consider unsettled cash, upcoming fees, capital calls |
| Concentration limits | Include open trades and derivatives exposure |
| Leverage limits | Include margin, loans and pending settlements |
| Corporate actions | Process election rules under mandate authority |
| Cash management | Project settlement and income cashflows |

## 4. Advisory workflow states

Client-facing workflow should distinguish:

| State | Client/advisor meaning |
|---|---|
| Recommended | Advice prepared but not accepted |
| Accepted | Client approved recommendation |
| Ordered | Order submitted |
| Partially executed | Some but not all executed |
| Executed | Trade completed in market/counterparty |
| Pending settlement | Awaiting cash/security settlement |
| Settled | Final holding/cash posted |
| Failed settlement | Operational issue requiring follow-up |
| Cancelled/expired | Order no longer active |

This improves client trust and reduces support calls.

## 5. Analytics impact

Lifecycle affects analytics in several ways.

| Analytics area | Lifecycle issue |
|---|---|
| Allocation | Whether pending trades are included |
| Exposure | Derivatives, FX, unsettled trades and look-through |
| Performance | Trade date, settlement date, external cashflow classification |
| Contribution | Income and corporate action classification |
| Attribution | Sector/currency/security mapping changes after corporate actions |
| Risk | Pending trades and derivatives can change VaR/stress exposure |
| Concentration | Open orders and unsettled positions may matter |
| Liquidity | Settlement, fund redemption terms and blocked assets matter |
| Buying power | Cash, credit, collateral and open obligations matter |

## 6. Reporting basis disclosure

Every report should disclose or internally define:

| Basis | Question answered |
|---|---|
| Trade-date basis | What economic exposure did the portfolio have? |
| Settlement-date basis | What assets were legally/custodially settled? |
| Available basis | What can be sold/used today? |
| Projected basis | What will cash/positions look like after pending events? |
| Valuation basis | Which price/NAV/model/quote was used? |
| FX basis | Which FX rate/date/time was used? |
| Corporate action basis | Are events announced, confirmed or booked? |

## 7. Client statement sections impacted

| Statement section | Lifecycle data required |
|---|---|
| Holdings | Settled/trade-date position, market price, accrued income |
| Transactions | Trade, settlement, income, fees, tax, corporate actions |
| Cash statement | Ledger, settled, pending and projected cash |
| Income summary | Dividends, coupons, fund distributions, withholding tax |
| Realized P&L | Sale/redemption/corporate action disposal details |
| Unrealized P&L | Cost, market value, FX translation |
| Performance | Cashflow classification and valuation basis |
| Corporate actions | Events, elections and outcomes |
| Fees/charges | Transaction fees, custody fees, advisory fees |
| Tax reporting | Withholding, tax lots, income classification |
| Mandate report | Target vs actual, breaches, drift, exceptions |

## 8. Corporate action reporting

Client/advisor reporting should handle:

- upcoming voluntary actions requiring election
- default option and deadline
- estimated entitlement
- final entitlement booked
- income versus capital classification
- quantity and cost-basis adjustment
- tax withholding/reclaim status
- revised/cancelled event notification

## 9. Pending and failed trade reporting

Internal advisor dashboards should show:

- pending settlements by date
- settlement fails and reasons
- partially filled orders
- cancelled/expired orders
- cash shortfalls
- securities shortfalls
- blocked/pledged/restricted assets
- trades impacting upcoming corporate action eligibility

Client-facing display may be simplified, but advisors need the operational detail.

## 10. Performance treatment examples

### Buy funded from existing cash

Not an external contribution. It changes holdings from cash to security.

### Sell proceeds retained as cash

Not an external withdrawal. It changes holdings from security to cash.

### Client deposits cash to fund buy

External contribution at cash deposit date/value date.

### Dividend received and retained

Investment income within portfolio.

### Dividend paid out to client

Income followed by external withdrawal.

### Corporate action split

No return by itself; adjust quantity and price basis.

### Settlement fail

May create exposure/cash basis differences. Performance policy must define trade-date versus settlement-date treatment.

## 11. Mandate breaches from lifecycle events

A mandate breach can be caused by:

- market movement
- trade execution
- settlement fail
- corporate action creating new security
- spin-off creating restricted exposure
- fund reclassification
- issuer downgrade
- FX movement
- collateral liquidation
- derivative assignment
- client withdrawal

Mandate monitoring must distinguish active breach from passive breach and pre-trade from post-trade breach.

## 12. Advisor explainability examples

| Client question | Required lineage |
|---|---|
| Why is my cash lower? | Pending buy settlement, fees, FX conversion |
| Why is my holding not visible yet? | Trade executed but not settled, or fund NAV pending |
| Why did my quantity double but value not double? | Stock split event |
| Why did I receive a new security? | Spin-off/merger/corporate action |
| Why did performance change after month-end? | Late corporate action/custodian correction/repriced NAV |
| Why is buying power lower than cash balance? | Open orders, pending settlements, blocked collateral |

## 13. Client experience design

Recommended display layers:

| Layer | User |
|---|---|
| Simple status | Client: ordered, executed, settled |
| Operational status | Advisor: matched, pending settlement, failed |
| Audit lineage | Ops/compliance: source events, corrections, approvals |
| Analytics basis | PM/analyst: trade-date vs settlement-date exposure |
| Reporting basis | Client/reporting: as-of date, valuation source, pending events |

## 14. Advisory controls

Advisory tools should block or warn on:

- insufficient available cash
- unsettled sale proceeds not eligible for reinvestment
- sell quantity not available due to pledge/block
- trade into non-approved product
- trade into unsuitable/complex product
- mandate violation
- corporate-action election missed deadline
- concentration increase above threshold
- liquidity buffer breach
- FX exposure outside range

## 15. Reporting controls

Before producing client statements:

- check all critical cash/position reconciliations
- identify stale prices and NAVs
- validate corporate actions in period
- validate income/tax classification
- validate performance cashflows
- verify settlement-fail treatment
- freeze or version source data for report reproducibility
- record report generation lineage

## 16. Product review and support

Operations data can improve product governance:

- products with frequent valuation breaks
- products with high settlement fails
- funds with delayed NAVs
- notes with issuer quote delays
- corporate actions causing repeated client confusion
- structured products with high manual lifecycle effort
- custodians with recurring SSI/cash breaks

This is valuable feedback for approved product universe and platform roadmap.

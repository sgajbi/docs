# 03 - Pricing, Valuation Hierarchy, Fair Value and Price Quality

## 1. Why pricing is not just "latest price"

A wealth platform may consume many price-like values:

- last traded price,
- exchange close price,
- official close,
- bid price,
- ask price,
- mid price,
- evaluated price,
- fund NAV,
- indicative value,
- issuer bid,
- theoretical model price,
- collateral price,
- accounting fair value,
- stale carry-forward price.

Each value has a different purpose. A good pricing model stores the price value and its business meaning.

## 2. Price context

A price is incomplete without context.

| Attribute | Example |
|---|---|
| instrument_id | BOND123 |
| price_type | BID / ASK / MID / LAST / CLOSE / NAV / EVALUATED |
| quote_basis | PCT_OF_PAR / PRICE_PER_UNIT / YIELD / SPREAD |
| currency | USD |
| source | Bloomberg / exchange / issuer / fund admin / model |
| source_rank | 1, 2, 3 |
| valuation_date | 2026-06-26 |
| timestamp | 2026-06-26T17:05:00+08:00 |
| market | XNAS / OTC / issuer |
| clean_dirty_flag | CLEAN / DIRTY / NOT_APPLICABLE |
| bid_ask_side | BID / ASK / MID |
| quality_status | VALID / STALE / MISSING / OVERRIDDEN |
| approval_status | SYSTEM / MANUAL_PENDING / APPROVED_OVERRIDE |

## 3. Price hierarchy

A price hierarchy defines which source wins for a purpose.

Example hierarchy for listed equities:

1. official exchange close,
2. consolidated close,
3. vendor evaluated close,
4. last trade if no official close,
5. prior close with stale flag,
6. manual override with approval.

Example hierarchy for private structured notes:

1. executable issuer bid,
2. issuer indicative bid,
3. independent valuation vendor,
4. internal model,
5. prior valuation with stale flag,
6. manual override with valuation committee approval.

Example hierarchy for funds:

1. fund administrator official NAV,
2. transfer-agent NAV,
3. platform vendor NAV,
4. estimated NAV for reporting with disclosure,
5. prior NAV with stale flag.

## 4. Fair value hierarchy

For accounting and valuation controls, pricing often follows a hierarchy similar to Level 1 / Level 2 / Level 3 concepts.

| Level | Meaning | Examples |
|---|---|---|
| Level 1 | quoted prices in active markets for identical instruments | listed equity official close, actively traded ETF |
| Level 2 | observable inputs, but not direct active-market price for identical instrument | evaluated bond price, FX forward curve, interest-rate curve |
| Level 3 | unobservable inputs or internal model assumptions | private equity NAV estimate, complex OTC derivative, illiquid structured note |

IFRS 13 defines fair value as the price that would be received to sell an asset or paid to transfer a liability in an orderly transaction between market participants at the measurement date. For platform design, this means fair value is not simply "book value" or "purchase price"; it requires a valuation policy and evidence.

## 5. Bid, ask, mid and side rules

| Use case | Common price side |
|---|---|
| Client statement | often mid/evaluated/official close depending policy |
| Liquidation value | bid for long positions, ask for short positions |
| Collateral/buying power | conservative bid or haircut-adjusted value |
| Trade proposal estimate | ask for buy, bid for sell |
| Performance valuation | consistent official/reporting price policy |
| Risk analytics | mid or model value plus stress scenarios |

The platform should support valuation-purpose-specific price selection.

## 6. Clean and dirty prices

For bonds and fixed-income-like instruments:

| Price | Meaning |
|---|---|
| Clean price | excludes accrued interest |
| Dirty price | includes accrued interest |
| Accrued interest | interest earned since last coupon date |

A common error is to value the bond using dirty price and then add accrued interest again. The pricing record should clearly state clean/dirty treatment.

## 7. Stale price policy

A stale price is not always wrong, but it must be visible and controlled.

| Product | Typical stale logic |
|---|---|
| Equity | no current trading-day close after market close |
| Fund | NAV missing beyond expected publication lag |
| Bond | evaluated price older than configured threshold |
| Private market | NAV lag is expected; stale if outside reporting cycle |
| Structured note | issuer quote missing beyond policy threshold |
| Derivative | mark not refreshed after market move |
| FX | rate missing for valuation date |

Stale status should propagate to reports, dashboards and QA checks.

## 8. Price override governance

Manual overrides should be exceptional and auditable.

Required fields:

- original source price,
- override price,
- reason code,
- supporting evidence,
- requested by,
- approved by,
- approval timestamp,
- effective valuation date,
- expiry date,
- impacted portfolios/reports,
- reversal/retirement logic.

## 9. Pricing controls

| Control | Purpose |
|---|---|
| Price tolerance check | Detect abnormal price movement |
| Cross-source variance check | Compare vendors |
| Missing price report | Identify valuation gaps |
| Stale price report | Identify outdated values |
| Zero/negative price check | Detect invalid values |
| Currency mismatch check | Prevent wrong currency valuations |
| Price basis check | Validate clean/dirty, percent/par/unit |
| Corporate action adjusted price check | Prevent split/dividend errors |
| Manual override report | Audit evidence |
| Downstream impact log | Know which portfolios/reports changed |

## 10. Reporting guidance

Client reports should avoid giving false precision. Where prices are estimated, stale, issuer-provided or modelled, reports should have appropriate labels such as:

- "indicative value",
- "issuer valuation",
- "latest available NAV",
- "estimated value",
- "price unavailable",
- "valuation as of [date]",
- "subject to liquidity and market conditions".

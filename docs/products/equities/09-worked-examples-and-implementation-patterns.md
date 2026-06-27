# 09. Worked Examples and Implementation Patterns

## Purpose

This file turns equity concepts into practical examples for trading, settlement, corporate actions, cost basis, P&L, performance, attribution, reporting, implementation and QA.

## Example 1. Equity buy with fees, tax and settlement cash

### Scenario

A client buys listed shares through an advisory workflow.

| Attribute | Value |
|---|---:|
| Quantity | 2,000 shares |
| Execution price | 18.50 |
| Commission | 75 |
| Stamp duty / tax | 40 |
| Settlement cycle | T+1 |
| Settlement currency | SGD |

### Calculation

```text

gross consideration = quantity x execution price
gross consideration = 2,000 x 18.50
gross consideration = 37,000

cash settlement = gross consideration + commission + tax
cash settlement = 37,000 + 75 + 40
cash settlement = 37,115

```

### Platform treatment

| Object | Treatment |
|---|---|
| Order | Captures intent, suitability, channel and client/advisor consent. |
| Execution | Captures fill quantity, execution price, venue, broker and timestamp. |
| Trade | Creates bookable trade with settlement date and status. |
| Cash leg | Reserves or settles SGD 37,115. |
| Position lot | Creates 2,000-share lot with cost-basis policy. |
| Fees/tax | Stored separately even if capitalized into cost basis. |

### QA assertions

| Test | Expected result |
|---|---|
| Commission is missing | Cash settlement and cost basis fail validation. |
| Settlement date is a market holiday | Next valid settlement date is used. |
| Trade is cancelled before settlement | Position, cash block and lot creation reverse with lineage. |
| Fee policy changes | Cost basis and performance treatment follow configured policy. |

## Example 2. Cash dividend with withholding tax

### Scenario

A client holds 1,000 shares before the ex-date. The company pays a cash dividend.

| Attribute | Value |
|---|---:|
| Eligible quantity | 1,000 shares |
| Gross dividend per share | 1.20 |
| Withholding tax rate | 30% |
| Dividend currency | USD |

### Calculation

```text

gross dividend = 1,000 x 1.20 = 1,200
withholding tax = 1,200 x 30% = 360
net dividend cash = 1,200 - 360 = 840

```

### Correct posting

| Posting | Amount | Meaning |
|---|---:|---|
| Dividend income gross | 1,200 | Investment income |
| Withholding tax | -360 | Tax withheld or tax expense |
| Cash received | 840 | Net cash credited |

### Reporting treatment

The statement should be able to show gross income, tax withheld and net cash. Performance should classify dividend income as investment income, not as an external contribution.

### QA assertions

| Test | Expected result |
|---|---|
| Position was bought after ex-date | No entitlement is created. |
| Tax rate changes by client jurisdiction | Net dividend follows client-specific withholding rule. |
| Dividend is paid in foreign currency | Dividend FX and portfolio-base FX are explicit. |
| Duplicate vendor event arrives | Idempotency prevents double dividend booking. |

## Example 3. Stock split and cost-basis adjustment

### Scenario

A client holds 500 shares with total cost of 20,000. The issuer executes a 2-for-1 stock split.

| Attribute | Before split | After split |
|---|---:|---:|
| Quantity | 500 | 1,000 |
| Total cost | 20,000 | 20,000 |
| Unit cost | 40.00 | 20.00 |
| Market price expectation | 80.00 | 40.00 |

### Correct treatment

The split changes quantity and unit cost, but not total economic value by itself.

### QA assertions

| Test | Expected result |
|---|---|
| Quantity doubles but cost is unchanged | Unit cost halves. |
| Price feed is not split-adjusted | Price outlier control fires. |
| Performance shows large gain from split only | Performance QA fails. |
| Lot-level cost exists | Every lot is adjusted proportionally. |

## Example 4. Rights issue: exercise, sell or lapse

### Scenario

A company offers one right for every five shares held. Five rights allow purchase of one new share at 12.00. The client holds 1,000 shares.

| Attribute | Value |
|---|---:|
| Existing shares | 1,000 |
| Rights ratio | 1 right per 5 shares |
| Rights received | 200 rights |
| Rights needed per new share | 5 |
| Subscription price | 12.00 |
| Maximum new shares | 40 |
| Cash required to fully exercise | 480 |

### Election outcomes

| Election | Result |
|---|---|
| Exercise all | Client pays 480 and receives 40 new shares. |
| Sell rights | Client receives cash proceeds and may realize gain/loss depending cost allocation. |
| Lapse | Rights expire worthless; possible loss if rights had allocated value. |
| Partial exercise | Mix of new shares, sold rights and lapsed rights. |

### QA assertions

| Test | Expected result |
|---|---|
| Client misses election deadline | Default option is applied and recorded. |
| Rights are tradable | Rights can have separate instrument, price and sale transaction. |
| Cash is insufficient for exercise | Election is blocked or escalated. |
| Rights cost allocation is required | Cost basis adjusts according to policy. |

## Example 5. ADR dividend with depositary fee and FX

### Scenario

A client holds a USD-traded ADR representing foreign ordinary shares. The issuer declares a local-currency dividend.

| Attribute | Value |
|---|---:|
| ADR quantity | 1,000 |
| ADR ratio | 1 ADR = 2 local shares |
| Local dividend per share | 0.50 local currency |
| FX rate to USD | 0.20 |
| Depositary fee per ADR | 0.02 USD |
| Withholding tax | 15% |

### Calculation

```text

local shares represented = 1,000 x 2 = 2,000
local gross dividend = 2,000 x 0.50 = 1,000 local
USD gross dividend = 1,000 x 0.20 = 200
withholding tax = 200 x 15% = 30
depositary fee = 1,000 x 0.02 = 20
net USD cash = 200 - 30 - 20 = 150

```

### QA assertions

| Test | Expected result |
|---|---|
| ADR ratio changes | Dividend entitlement uses effective ratio. |
| Depositary fee is treated as tax | Reporting classification fails. |
| Local dividend is applied directly to ADR quantity | Entitlement is wrong. |
| FX rate is missing | Dividend remains source-limited until conversion source is available. |

## Example 6. Multi-currency equity P&L split

### Scenario

A SGD reporting portfolio holds a USD equity.

| Attribute | Purchase | Current |
|---|---:|---:|
| Quantity | 1,000 | 1,000 |
| Share price | 50 USD | 55 USD |
| USD/SGD | 1.34 | 1.30 |

### Calculation

```text

local cost = 1,000 x 50 = 50,000 USD
local value = 1,000 x 55 = 55,000 USD
local P&L = 5,000 USD

base cost = 50,000 x 1.34 = 67,000 SGD
base value = 55,000 x 1.30 = 71,500 SGD
base P&L = 4,500 SGD

price contribution approximation = 5,000 x 1.34 = 6,700 SGD
FX contribution approximation = 4,500 - 6,700 = -2,200 SGD

```

### Interpretation

The stock rose in USD, but SGD strengthened against USD, reducing the base-currency gain. Reporting should separate local price effect and FX effect where methodology supports it.

### QA assertions

| Test | Expected result |
|---|---|
| Local price rises but FX moves against client | Base-currency P&L may be lower than local P&L. |
| FX rate is stale | Valuation quality status is degraded. |
| Report shows only base P&L | Methodology should still preserve local value and FX source. |
| Attribution is run | Price and currency effects reconcile to total within tolerance. |

## Example 7. Equity contribution and simple attribution

### Scenario

A portfolio begins the month at 1,000,000. One equity holding begins at 80,000 and returns 12% for the month.

### Contribution calculation

```text

beginning weight = 80,000 / 1,000,000 = 8.00%
holding contribution = beginning weight x holding return
holding contribution = 8.00% x 12.00%
holding contribution = 0.96%

```

### Reporting treatment

The holding contributed about 0.96 percentage points to portfolio return before considering flows, interaction effects, methodology details and fees. For a benchmark-relative report, contribution should be separated from allocation and selection effects where attribution methodology supports it.

### QA assertions

| Test | Expected result |
|---|---|
| Beginning weight is missing | Contribution calculation is source-limited. |
| Large external flow occurs | Time-weighting methodology handles the flow. |
| Corporate action occurs during period | Return series is adjusted to avoid artificial return. |
| Sector mapping changes mid-period | Attribution uses effective-dated classification. |

## Example 8. Delisting or trading halt

### Scenario

A listed equity is suspended, then later delisted.

| State | Platform treatment |
|---|---|
| Trading halt | Trading blocked, valuation may continue with last price or fair value policy. |
| Suspension extended | Stale-price and liquidity flags appear. |
| Delisting announced | Product lifecycle and client reporting status update. |
| Final cash or share consideration | Corporate-action settlement is booked with lineage. |

### QA assertions

| Test | Expected result |
|---|---|
| Advisor attempts buy during halt | Buy is blocked. |
| Report is generated with stale last price | Stale valuation label appears. |
| Delisting creates cash consideration | Cash transaction links to event. |
| Security remains in portfolio after delisting | Holding status and valuation basis remain explainable. |

## Example 9. Corporate-action reconciliation workflow

### Scenario

The platform expects a split ratio of 2-for-1, but custodian confirms 3-for-2.

### Correct workflow

| Step | Action |
|---|---|
| Detect | Compare vendor event terms with custodian event terms. |
| Freeze | Prevent final posting if terms conflict and event is material. |
| Resolve | Choose authoritative source or manual approved correction. |
| Post | Apply resolved ratio with event version. |
| Reconcile | Match resulting custody quantity and internal position. |
| Report | Flag restatement if client output was already produced. |

### QA assertions

| Test | Expected result |
|---|---|
| Vendor and custodian disagree | Exception is created before final posting. |
| Event terms are corrected after posting | Reversal or adjustment is booked with lineage. |
| Duplicate event message arrives | Idempotency prevents duplicate quantity change. |
| Price series is not adjusted | Price outlier control links to unresolved corporate action. |

## Implementation-backed capability lens

When reviewing equity support, look for evidence across these capabilities:

| Capability | Evidence to look for |
|---|---|
| Trade lifecycle | Orders, executions, allocations, fees, taxes, settlement status and reversals. |
| Lot accounting | Lot creation, partial disposal, cost-basis method and corporate-action adjustment. |
| Corporate actions | Entitlements, elections, dates, source lineage, posting, reversal and reconciliation. |
| Market data | Listing, exchange, currency, close price, adjusted price and stale-price controls. |
| Performance | Price, dividend, fee, tax and FX effects separated where methodology supports it. |
| Attribution | Security, sector, country, currency and benchmark mappings are effective-dated. |
| Advisory and mandate | Concentration, restricted list, market access, liquidity and corporate-action risk controls. |
| Reporting | Holdings, transactions, income, P&L, corporate actions, risk flags and data-quality labels. |

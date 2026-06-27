# 04 - Platform Implementation, Advisory, and Practitioner Reference

## 1. Platform View of Funds

From a wealth platform perspective, funds are not just simple price x quantity products. They require support for:

- Product hierarchy
- Share classes
- NAV pricing
- Dealing cut-offs
- Pending orders
- Subscriptions and redemptions
- Switches
- Transfers
- Distributions and reinvestment
- Fees and rebates
- Look-through exposure
- Suitability and eligibility
- Valuation freshness
- Illiquid/private fund lifecycle
- Corporate actions such as merger/class conversion/split/liquidation

The platform should model funds as a product family with common structures and subtype-specific extensions.

---

## 2. Recommended Architecture Pattern

```text
Product Master
    ->
Fund Terms / Share Class Terms
    ->
Order Management / Advisory Validation
    ->
Fund Dealing Engine
    ->
Confirmation and Settlement
    ->
Transaction Ledger
    ->
Position Service
    ->
NAV / Valuation Service
    ->
Performance, Risk, Suitability and Reporting
```

Keep order state, transaction state and position state separate.

```text
Order = client instruction and workflow state
Transaction = confirmed economic posting
Position = current holding after transactions
Valuation = market value from NAV/price
Performance = return classification and calculations
```

---

## 3. Common Integration Sources

| Source | Data Provided |
|---|---|
| Fund manager / fund house | Factsheets, prospectus, holdings, distributions, fund terms. |
| Transfer agent | Order confirmations, units, settlement, shareholder register. |
| Custodian | Holdings, cash settlement, distributions, corporate actions. |
| Market data vendor | NAV, exchange prices, reference data, classifications. |
| Exchange | ETF/closed-end fund trading prices. |
| Internal product master | Approved product list, eligibility, risk rating, sales restrictions. |
| Advisory platform | Suitability, recommendation, client consent. |
| Portfolio accounting system | Transactions, positions, cash ledger. |
| Performance engine | NAV returns, TWR/MWR, contribution, attribution. |
| Risk engine | Look-through risk, concentration, liquidity, factor exposure. |

---

## 4. Product Master Controls

A fund should not be tradable or recommendable until key reference data is complete.

Minimum required fields:

| Field | Why Required |
|---|---|
| Share-class identifier | Correct investable instrument. |
| ISIN/ticker/internal ID | Settlement and data matching. |
| Fund type | Determines lifecycle logic. |
| Currency | Valuation and settlement. |
| NAV source | Valuation control. |
| Dealing frequency | Order processing. |
| Cut-off time | Correct NAV assignment. |
| Settlement cycle | Cash planning. |
| Minimum subscription/holding | Order validation. |
| Distribution policy | Income handling. |
| Risk rating | Suitability. |
| Investor eligibility | Sales restrictions. |
| Domicile/registration | Jurisdiction rules. |
| Fee terms | Cost disclosure and transaction modelling. |
| Liquidity terms | Redemption risk and suitability. |
| Complexity flag | Advisory workflow. |

---

## 5. Order Validation Checklist

Before accepting a fund order:

| Check | Example |
|---|---|
| Product active | Fund/share class is open for subscription/redemption. |
| Client eligibility | Retail/accredited/professional restrictions. |
| Country availability | Fund registered or allowed for client jurisdiction. |
| Risk suitability | Fund risk rating fits client profile or exception approved. |
| Product complexity | Complex/alternative fund disclosures completed. |
| Minimum investment | Order meets initial/subsequent minimum. |
| Minimum holding | Redemption does not create invalid residual holding. |
| Cash availability | Sufficient cash for subscription and fees. |
| Units availability | Sufficient available units for redemption. |
| Cut-off | Order submitted before applicable cut-off. |
| Dealing calendar | Fund is dealing on relevant date. |
| Lock-up/notice | Redemption permitted under terms. |
| Concentration | Order does not breach portfolio concentration limits. |
| Currency | Client understands FX exposure. |
| Documentation | Prospectus/factsheet/KID/KIID delivered if required. |

---

## 6. Advisory and Suitability Considerations

Advisors should explain funds using both benefits and risks.

### 6.1 Benefits

| Benefit | Explanation |
|---|---|
| Diversification | One fund may hold many securities. |
| Professional management | Fund manager selects and monitors investments. |
| Access | Provides exposure to markets/strategies not easy to access directly. |
| Operational simplicity | One fund position instead of many underlying securities. |
| Regular investment | Supports monthly/regular savings plan. |
| Liquidity | Many funds offer periodic liquidity. |

### 6.2 Risks

| Risk | Explanation |
|---|---|
| Market risk | NAV can fall. |
| Liquidity risk | Redemption may be delayed, gated or suspended. |
| Credit risk | Bond/credit funds can suffer defaults or spread widening. |
| Interest-rate risk | Bond funds fall when yields rise. |
| Currency risk | Fund/share-class currency may differ from client base currency. |
| Manager risk | Active manager may underperform. |
| Concentration risk | Thematic or sector funds can be concentrated. |
| Fee drag | Fees reduce long-term return. |
| Tracking error | Index funds/ETFs may not perfectly track benchmark. |
| Premium/discount risk | ETFs/closed-end funds can trade away from NAV. |
| Valuation lag | Hedge/private fund NAV may be stale or estimated. |
| Complexity risk | Alternatives, derivatives and leverage require deeper understanding. |

---

## 7. How to Explain Funds to Clients

Plain explanation:

```text
A fund pools money from many investors and invests it into a portfolio managed according to a stated objective. You own units or shares of the fund, not each underlying asset directly. Your return comes from changes in the fund's NAV and any distributions paid by the fund. The fund can help with diversification and access, but you still take market, liquidity, currency, manager and fee risks.
```

For unit trust:

```text
A unit trust is an open-ended fund where you buy or redeem units based on the fund's NAV. Orders are usually forward-priced, so the exact NAV and units are confirmed after the dealing date. The fund may pay distributions or accumulate income into NAV depending on the share class.
```

For ETF:

```text
An ETF is a fund traded on an exchange like a stock. You can buy or sell it intraday at market price, but the market price can differ from the fund's NAV. ETFs are often used for index exposure, but they still carry market, liquidity, tracking and currency risks.
```

For private fund:

```text
A private fund usually involves a capital commitment, capital calls over time, illiquid holdings, delayed NAVs and long investment horizon. It may provide access to private equity, private credit or real assets, but liquidity is limited and valuation is less frequent than public funds.
```

---

## 8. Practitioner Explanation

You can say:

```text
Funds are pooled investment vehicles where clients own units or shares of a fund, while the fund owns the underlying portfolio. In a wealth platform, the accounting position should be the fund share class, because NAV, currency, fees, distribution policy, hedging and eligibility are share-class specific. Fund transactions include subscriptions, redemptions, switches, transfers, distributions, reinvestments, corporate actions and, for private funds, commitments, capital calls and distributions. Valuation is usually units times NAV for open-ended funds, exchange price for ETFs and listed closed-end funds, and administrator-reported NAV for hedge or private funds. The platform should separate order state, transaction posting, position holding, NAV valuation and look-through analytics. Advisory and suitability checks must consider risk rating, liquidity, complexity, fees, currency, investor eligibility, concentration and whether the product is retail, accredited or professional-investor only.
```

---

## 9. Comparison with Bonds and Notes

| Topic | Bond | Note | Fund |
|---|---|---|---|
| Client owns | Debt security | Debt/structured note | Fund units/shares |
| Return source | Coupon + price + redemption | Coupon/payoff linked to terms | NAV movement + distributions |
| Valuation | Price/yield/DCF | Market/issuer/model price | NAV or exchange price |
| Maturity | Usually fixed | Usually fixed | Usually no maturity unless target/closed/private term fund |
| Issuer risk | Bond issuer | Note issuer | Fund holdings and fund structure; not usually issuer debt exposure unless ETN-like |
| Underlying exposure | Direct issuer/security | Linked underlying | Fund portfolio holdings |
| Lifecycle | Coupon, maturity, call | Coupon, barrier, autocall, maturity | Subscription, redemption, NAV, distribution, switch |
| Position unit | Nominal/quantity | Nominal/quantity | Units/shares or commitment |
| Complexity | Low to high | Often high | Low to very high depending on fund type |

---

## 10. Fund-Specific Platform Edge Cases

| Edge Case | Required Handling |
|---|---|
| Order submitted after cut-off | Move to next dealing date. |
| NAV not available | Keep order pending; do not confirm units. |
| NAV restated | Correct transaction/valuation/performance under governance. |
| Redemption creates tiny residual | Apply full-redemption or minimum holding rules. |
| Distribution reinvestment | Book income and reinvestment correctly. |
| Accumulating class | Do not create fake cash income unless officially reported. |
| Fund suspended | Block transactions and mark valuation stale/suspended. |
| Redemption gate | Partially redeem and keep deferred balance. |
| Side pocket | Create restricted side-pocket position. |
| Share-class conversion | Close old class and open new class with linked transaction group. |
| Fund merger | Convert old fund to new fund with no false performance impact. |
| ETF premium/discount | Use exchange price for market value and NAV for analytics. |
| Private fund capital call | Reduce cash and increase paid-in capital/unfunded tracking. |
| Private fund NAV lag | Use report date and valuation date separately. |
| Fund-of-funds | Avoid double counting exposure in look-through. |
| Hedged share class | Use share-class NAV, not base-fund NAV. |
| Multi-currency fund | Separate fund base, share-class, pricing, settlement and client base currency. |

---

## 11. QA and Test Scenarios

### 11.1 Subscription Tests

| Scenario | Expected Result |
|---|---|
| Amount subscription before cut-off | Order assigned current dealing date. |
| Amount subscription after cut-off | Order assigned next dealing date. |
| Sales charge applied | Units calculated from net investment amount. |
| Minimum subscription breach | Order rejected or requires override. |
| NAV delayed | Order remains pending. |
| NAV restated after confirmation | Correction process triggered. |

### 11.2 Redemption Tests

| Scenario | Expected Result |
|---|---|
| Unit redemption | Units reduced, cash proceeds booked. |
| Amount redemption | Units calculated using NAV and rounding rules. |
| Full redemption | All units removed including dust. |
| Redemption below minimum holding | Reject or convert to full redemption. |
| Redemption gate | Partial redemption booked, deferred balance retained. |
| Fund suspended | Redemption blocked or pending under manual process. |

### 11.3 Distribution Tests

| Scenario | Expected Result |
|---|---|
| Cash distribution | Cash income booked. |
| Distribution with tax | Gross income and tax withholding booked. |
| Reinvested distribution | Income and reinvestment units booked. |
| Return of capital | Cost basis or classification adjusted by policy. |
| Accumulating class | No cash distribution unless reported. |

### 11.4 Corporate Action Tests

| Scenario | Expected Result |
|---|---|
| Unit split | Units change, market value unchanged. |
| Fund merger | Old fund closed, new fund opened at equivalent value. |
| Class conversion | Old class out, new class in. |
| Liquidation | Units closed, cash received. |
| Side pocket | Restricted side-pocket position created. |

### 11.5 Valuation Tests

| Scenario | Expected Result |
|---|---|
| Latest NAV loaded | Position market value updated. |
| Stale NAV | Valuation status flagged. |
| ETF price available | Exchange price used for market value. |
| ETF NAV available | Premium/discount analytics calculated. |
| Private fund quarterly NAV | Valuation date and report date stored separately. |
| NAV currency mismatch | Reject or quarantine price. |

---

## 12. Portfolio Analytics Treatment

### 12.1 Holdings View

Show:

- Fund name
- Share class
- Units
- NAV/price
- Market value
- Cost
- Unrealized P&L
- Distribution yield if available
- Fund category
- Risk rating
- Liquidity frequency
- Valuation date/status

### 12.2 Allocation View

Two possible approaches:

| Approach | Use Case |
|---|---|
| Fund classification | Simple reporting where look-through unavailable. |
| Look-through holdings | More accurate asset allocation, concentration and risk. |

Recommended approach:

```text
Use direct classification as default.
Use look-through only when source, date, completeness and methodology are reliable.
Clearly disclose when look-through is estimated or partial.
```

### 12.3 Performance View

Performance should include:

- NAV return
- Distributions
- Reinvestments
- FX impact
- Fees and taxes based on reporting policy
- Subscription/redemption timing
- Corporate action adjustments
- NAV corrections

---

## 13. Controls for Advisory and Reporting

| Control | Purpose |
|---|---|
| Product approved list | Prevent sale of unapproved funds. |
| Client eligibility check | Ensure fund is available to client type/jurisdiction. |
| Risk profile check | Match client risk appetite. |
| Liquidity horizon check | Avoid illiquid products for short-term liquidity needs. |
| Concentration check | Avoid excessive exposure to one fund/manager/strategy/sector. |
| Currency mismatch warning | Highlight FX risk. |
| Complexity disclosure | Required for alternatives/derivatives/leverage. |
| Fee disclosure | Show sales charges, ongoing charges and platform fees. |
| NAV freshness warning | Prevent misleading valuation. |
| Look-through data date warning | Prevent stale exposure reporting. |
| Redemption restriction warning | Highlight lock-ups, gates, notice periods. |

---

## 14. Implementation Anti-Patterns

Avoid these mistakes:

| Anti-Pattern | Why It Is Bad |
|---|---|
| Treat all funds like equities | Ignores NAV dealing, cut-off and settlement rules. |
| Treat all funds like daily NAV products | Fails for hedge/private funds. |
| Use umbrella fund as instrument | Wrong currency/NAV/share-class mapping. |
| Create client positions for every fund holding | Incorrect accounting ownership. |
| Use stale look-through without warning | Misleading risk and allocation. |
| Double count TER as explicit fee | Overstates client cost. |
| Ignore distribution components | Misclassifies income vs return of capital. |
| Confirm order before NAV | Creates inaccurate units/cash. |
| Ignore pending orders in availability | Allows over-redemption. |
| Treat reinvested distribution as external cashflow | Distorts performance. |
| Ignore NAV corrections | Produces wrong historical performance. |
| Use fund base currency instead of share-class currency | Wrong valuation/FX. |

---

## 15. Glossary

| Term | Meaning |
|---|---|
| Fund | Pooled investment vehicle. |
| Unit trust | Fund structure commonly used in Singapore and other markets. |
| Mutual fund | Open-ended pooled fund, common term in the US. |
| ETF | Exchange-traded fund. |
| Closed-end fund | Fund with fixed shares trading on secondary market. |
| NAV | Net asset value per unit/share. |
| Share class | Specific investable class of a fund with its own currency, fee, distribution and eligibility terms. |
| Accumulating class | Income retained in NAV. |
| Distributing class | Income paid to investor. |
| Hedged class | Share class with currency hedging overlay. |
| TER | Total expense ratio, fund-level expenses as percentage of assets. |
| Sales charge/load | Fee charged on subscription or redemption. |
| Cut-off time | Deadline for order to receive a particular dealing date. |
| Dealing date | Date fund processes order. |
| Settlement date | Date cash/units settle. |
| Forward pricing | Order priced at future NAV, not known at order entry. |
| Swing pricing | NAV adjustment to protect existing investors from transaction costs. |
| Redemption gate | Limit on redemption amount. |
| Side pocket | Restricted class holding illiquid/distressed assets. |
| Look-through | Analysis of underlying fund holdings. |
| Premium/discount | Difference between market price and NAV for ETF/closed-end fund. |
| Capital call | Private fund request for committed capital. |
| Unfunded commitment | Remaining commitment not yet called. |
| Vintage year | Year private fund began investing. |
| J-curve | Early negative returns followed by potential later gains in private funds. |

---

## 16. Final Mental Model

```text
Funds are pooled investment products.
The client owns fund units/shares or a private fund interest.
The fund owns the underlying portfolio.
Open-ended funds are NAV/dealing-cycle products.
ETFs and closed-end funds trade like listed securities but remain fund products.
Hedge and private funds add liquidity, valuation and commitment complexity.
A robust wealth platform must model fund hierarchy, share classes, orders, NAV, transactions, positions, distributions, fees, look-through exposure, suitability and valuation controls separately.
```

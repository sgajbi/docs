# 06 - Equity Glossary, Control Checklists and Test Scenarios

## 1. Glossary

| Term | Meaning |
|---|---|
| ADR | American Depositary Receipt representing foreign shares through a depositary bank. |
| Ask price | Price at which sellers are willing to sell. |
| Bid price | Price at which buyers are willing to buy. |
| Board lot | Standard exchange trading unit. |
| Bonus issue | Additional shares issued to existing shareholders, often similar to stock dividend/split. |
| Book cost | Cost basis used for portfolio reporting. |
| Cash dividend | Cash distribution to shareholders. |
| Common share | Ordinary ownership share in a company. |
| Corporate action | Issuer event affecting shareholders. |
| Cost basis | Cost assigned to a holding or lot. |
| CSD | Central securities depository. |
| Cum-dividend | Security trades with entitlement to upcoming dividend. |
| Delisting | Removal of security from exchange listing. |
| Depositary receipt | Receipt representing underlying foreign shares. |
| Dividend yield | Dividend per share divided by share price. |
| DRIP | Dividend reinvestment plan. |
| Ex-date | Date on or after which buyer is not entitled to next distribution. |
| GDR | Global Depositary Receipt. |
| Gross dividend | Dividend before tax and fees. |
| Liquidity | Ability to buy/sell without large price impact. |
| Lot | Specific acquisition block of shares with own cost basis. |
| Market cap | Share price multiplied by shares outstanding. |
| Preferred share | Equity-like security with priority over common shares, often income-oriented. |
| Proxy voting | Shareholder voting through proxy process. |
| Record date | Date issuer/custodian checks eligible holders. |
| REIT | Real Estate Investment Trust. |
| Rights issue | Entitlement to buy new shares at specified terms. |
| Scrip dividend | Dividend paid in shares instead of cash. |
| Settlement date | Date securities and cash exchange. |
| Short sale | Sale of borrowed shares. |
| Spin-off | Distribution of shares in a subsidiary/new company. |
| Stock split | Increase in share count with proportional price decrease. |
| Tender offer | Offer to buy shares from holders. |
| Tick size | Minimum price movement. |
| Trade date | Date trade is executed. |
| Unsettled position | Position from executed but not yet settled trades. |
| Warrant | Security giving right to buy/sell underlying at specified terms. |
| Withholding tax | Tax deducted at source from dividend or distribution. |

## 2. Transaction type quick reference

| Transaction type | Security impact | Cash impact | P&L/income impact |
|---|---:|---:|---|
| EQUITY_BUY | Quantity increases | Cash decreases | New cost basis. |
| EQUITY_SELL | Quantity decreases | Cash increases | Realized P&L. |
| EQUITY_TRANSFER_IN | Quantity increases | Usually none | External transfer or internal movement. |
| EQUITY_TRANSFER_OUT | Quantity decreases | Usually none | External transfer or internal movement. |
| EQUITY_SHORT_SELL | Short quantity created | Cash/margin effect | Short exposure. |
| EQUITY_BUY_TO_COVER | Short quantity reduced | Cash decreases | Realized P&L on short. |
| EQUITY_DIVIDEND_CASH | No change | Cash increases | Income. |
| EQUITY_WITHHOLDING_TAX | No change | Cash decreases | Tax expense. |
| EQUITY_DIVIDEND_STOCK | Quantity increases | No cash | Cost/income treatment policy-driven. |
| EQUITY_DIVIDEND_REINVESTMENT | Quantity increases | Dividend cash used | Income plus reinvestment. |
| EQUITY_STOCK_SPLIT | Quantity changes | No cash | No gain/loss. |
| EQUITY_REVERSE_SPLIT | Quantity changes | Optional cash-in-lieu | No gain/loss except fractional treatment. |
| EQUITY_RIGHTS_ENTITLEMENT | Rights quantity increases | No cash | Entitlement recognition. |
| EQUITY_RIGHTS_SUBSCRIPTION | New shares increase | Cash decreases | New cost. |
| EQUITY_RIGHTS_SALE | Rights decrease | Cash increases | Realized gain/loss. |
| EQUITY_RIGHTS_EXPIRY | Rights removed | No cash | Loss/write-off if rights had cost. |
| EQUITY_SPINOFF_IN | New security increases | No cash | Cost allocation. |
| EQUITY_MERGER_OUT | Old security decreases | Depends | Disposal/conversion. |
| EQUITY_MERGER_IN | New security increases | No cash | Carryover/allocated cost. |
| EQUITY_TENDER_OUT | Shares decrease | Cash later/same | Disposal. |
| EQUITY_TENDER_CASH | No security | Cash increases | Proceeds/P&L. |
| EQUITY_RETURN_OF_CAPITAL | No/quantity change | Cash increases | Cost reduction or income. |
| EQUITY_LIQUIDATION_PAYMENT | Security may reduce | Cash increases | Recovery/proceeds. |
| EQUITY_WRITE_OFF | Quantity/value removed | No cash | Realized loss/write-off. |
| EQUITY_ADR_FEE | No security | Cash decreases | Fee expense. |
| EQUITY_CORRECTION_REVERSAL | Reverses prior | Reverses prior | Audit correction. |

## 3. Corporate action mapping checklist

| Corporate action | Event needed | Entitlement needed | Election needed | Transaction posting |
|---|---|---|---|---|
| Cash dividend | Yes | Yes | No | Dividend + tax. |
| Stock dividend | Yes | Yes | No | New shares + cost treatment. |
| Scrip/optional dividend | Yes | Yes | Yes | Cash or shares based on election. |
| Split | Yes | Yes | No | Quantity/cost adjustment. |
| Reverse split | Yes | Yes | No | Quantity adjustment + cash-in-lieu. |
| Rights issue | Yes | Yes | Yes | Rights, exercise/sale/expiry. |
| Spin-off | Yes | Yes | Usually no | New security + cost allocation. |
| Merger | Yes | Yes | Sometimes | Old out, cash/new security in. |
| Tender offer | Yes | Yes | Yes | Block, tender out, cash, release unaccepted. |
| Buyback | Sometimes | Sometimes | Sometimes | Depends on open-market vs tender. |
| Capital return | Yes | Yes | No | Cash + cost reduction/income. |
| Liquidation | Yes | Yes | No/possible | Payment/write-off. |
| ADR fee | Yes or fee schedule | Holder quantity | No | Cash fee. |
| Proxy vote | Yes | Yes | Yes | Usually no transaction. |

## 4. Product master checklist

| Check | Required? |
|---|---|
| Instrument type populated | Yes. |
| Issuer linked | Yes. |
| Primary identifier valid | Yes. |
| Listing and exchange populated | Yes for listed equities. |
| Currency populated | Yes. |
| Sector and country populated | Required for risk/reporting. |
| Trading status populated | Yes. |
| Settlement cycle configured | Yes. |
| Board lot/tick size configured | Useful for trading. |
| ADR/GDR underlying mapping | Required for DRs. |
| Preferred share terms | Required for preferreds. |
| Rights/warrant expiry and ratio | Required for temporary/equity-linked instruments. |
| Margin eligibility/haircut | Required for lending/buying power. |
| Restricted-list status | Required for advisory/compliance. |

## 5. Transaction processing checklist

| Check | Purpose |
|---|---|
| Idempotency key present | Avoid duplicate trade bookings. |
| Trade date <= settlement date | Basic validation, allowing market exceptions. |
| Quantity sign correct | Buy positive, sell negative. |
| Price non-negative | Prevent bad data. |
| Fees/taxes separated | Clear reporting and P&L. |
| FX rate available | Base-currency valuation. |
| Lot selection valid | Realized P&L. |
| Settlement status tracked | Available quantity and cash. |
| Reversal link preserved | Audit. |
| Source reference stored | Reconciliation. |

## 6. Valuation checklist

| Check | Purpose |
|---|---|
| Price source priority applied | Governance. |
| Price date valid | Stale price control. |
| Market calendar considered | Avoid false stale flags. |
| FX rate available | Base reporting. |
| Suspended/delisted status considered | Valuation policy. |
| Corporate-action adjustment checked | Prevent artificial P&L. |
| Manual price approved | Governance. |
| Valuation lineage stored | Audit/report evidence. |

## 7. Performance checklist

| Check | Purpose |
|---|---|
| Dividends classified as income | Correct return decomposition. |
| Withholding tax classified | Gross/net return clarity. |
| Splits do not create return | Avoid artificial performance. |
| Spin-offs treated as internal transformation | Avoid false external flow. |
| Mergers classified correctly | Cash disposal vs stock conversion. |
| External cashflows separated | TWR accuracy. |
| FX return separated | Multi-currency attribution. |
| Corporate-action corrections rerun performance | Historical accuracy. |

## 8. QA test scenarios

### 8.1 Basic trade and position

| Scenario | Expected result |
|---|---|
| Buy 100 shares at 10, no fee | Quantity 100, cost 1,000. |
| Buy 100 shares at 10 with fee 5 | Quantity 100, cost 1,005 if fees capitalized. |
| Sell 40 shares under FIFO | Quantity 60, realized P&L based on first lot. |
| Sell more than available | Reject or short-sale flow only if allowed. |
| Buy and sell before settlement | Correct unsettled quantities and cash. |

### 8.2 Dividend scenarios

| Scenario | Expected result |
|---|---|
| Hold before ex-date | Dividend entitlement created. |
| Buy on ex-date | No entitlement for next dividend. |
| Sell on/after ex-date | Seller retains entitlement under normal rules. |
| Dividend with withholding tax | Gross income, tax expense, net cash. |
| ADR dividend with fee | Gross dividend, tax, ADR fee, net cash. |
| Optional dividend cash election | Cash dividend posted. |
| Optional dividend stock election | Stock/scrip posted. |

### 8.3 Split and bonus scenarios

| Scenario | Expected result |
|---|---|
| 2-for-1 split | Quantity doubles, unit cost halves, total cost unchanged. |
| 1-for-10 reverse split | Quantity divided by 10, unit cost multiplied by 10. |
| Reverse split fractional cash-in-lieu | Fractional quantity converted to cash. |
| Missing split price adjustment | Price outlier alert. |

### 8.4 Rights scenarios

| Scenario | Expected result |
|---|---|
| Rights received | Rights entitlement created. |
| Rights fully exercised | Cash paid, new shares received. |
| Rights partially exercised | Some new shares, remaining rights sold/expired. |
| Rights sold | Rights position reduced, cash proceeds, P&L. |
| Rights expired | Rights removed, loss/write-off if cost exists. |
| Late election | Rejected or flagged for manual handling. |

### 8.5 Spin-off and merger scenarios

| Scenario | Expected result |
|---|---|
| Spin-off with simple ratio | New shares created. |
| Spin-off with cost allocation | Parent and child cost basis allocated. |
| Cash merger | Old shares removed, cash proceeds, realized P&L. |
| Stock merger | Old shares removed, new shares created, cost carried/allocated. |
| Cash-and-stock merger | Multiple legs posted and reconciled. |
| Merger with fractional shares | Cash-in-lieu posted. |

### 8.6 Data and reconciliation scenarios

| Scenario | Expected result |
|---|---|
| Price missing | Valuation uses fallback or flags exception. |
| FX missing | Base valuation blocked or fallback flagged. |
| Custodian quantity mismatch | Position reconciliation break. |
| Dividend expected but not received | Income reconciliation break. |
| Corporate action terms revised | Event versioning and correction postings. |
| Duplicate custodian feed | Idempotency prevents duplicate transaction. |

## 9. Common defect patterns

| Defect | Root cause | Prevention |
|---|---|---|
| Double dividend | Duplicate feed message. | Idempotency key and entitlement ID. |
| Wrong dividend entitlement | Ex-date logic missing. | Entitlement snapshot by event. |
| Artificial P&L on split | Quantity adjusted but cost/price not adjusted. | Split processing and price control. |
| Wrong ADR dividend | Local share event applied directly to ADR. | DR mapping and depositary event rules. |
| Incorrect realized P&L | Missing lot selection. | Lot engine. |
| Wrong base-currency value | Missing/wrong FX rate. | FX hierarchy and validation. |
| Corporate action missed | Feed coverage gap. | Multi-source CA comparison. |
| Rights expired unexpectedly | Missing election workflow. | Deadline alerts and default handling. |
| Delisted stock valued at stale price forever | No stale/delisting control. | Valuation policy by status. |

## 10. Final quick mental model

```text
Equity position = quantity of shares + cost basis + market price + FX
Equity lifecycle = trade + settlement + dividend + corporate actions + tax + valuation
Equity platform difficulty = corporate actions + data quality + cost basis + reconciliation
```

For implementation:

```text
Instrument master tells what the security is.
Transaction ledger tells what happened.
Position tells what is held now.
Corporate-action event tells why non-trade changes happened.
Valuation tells what it is worth.
Performance tells how return was earned.
Risk tells what the portfolio is exposed to.
```

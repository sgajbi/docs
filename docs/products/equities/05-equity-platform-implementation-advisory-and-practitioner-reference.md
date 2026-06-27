# 05 - Equity Platform Implementation, Advisory and Practitioner Reference

## 1. Platform capabilities required for equities

A wealth-management platform supporting equities should provide the following capabilities.

| Capability | Required behaviour |
|---|---|
| Product master | Maintain issuer, instrument, listing, identifiers, share class, sector, country, currency and risk attributes. |
| Order and trade capture | Capture order, execution, allocation, fees, taxes, settlement and status. |
| Position keeping | Maintain trade-date, settled, unsettled, available and blocked quantities. |
| Lot accounting | Maintain cost lots for P&L, tax, corporate actions and reporting. |
| Market data | Consume official close, intraday price, FX, volume and corporate-action-adjusted prices. |
| Corporate actions | Process dividends, splits, rights, spin-offs, mergers, tender offers, delisting and ADR events. |
| Cash ledger | Book trade cash, dividends, tax, fees and corporate-action proceeds. |
| Performance | Calculate TWR/MWR, contribution, attribution, income and FX effects. |
| Risk analytics | Concentration, volatility, beta, VaR, drawdown, liquidity and factor exposure. |
| Suitability | Product fit, portfolio fit, concentration, complexity and risk-profile checks. |
| Reporting | Holdings, transactions, income, P&L, corporate actions, performance and risk reports. |
| Reconciliation | Broker/custodian/security/cash/CA/price reconciliation. |
| Audit | Preserve source, version, corrections, approvals and calculation lineage. |

## 2. Enterprise architecture view

Recommended architecture layers:

```text
API / UI / Reporting
        ->
Application services
        ->
Domain model and policies
        ->
Ports/interfaces
        ->
Infrastructure adapters: market data, custodian, broker, CA feeds, product master, FX, tax, risk
```

Keep domain rules separate from infrastructure. For example:

- dividend entitlement logic is domain/application logic;
- exchange price feed is infrastructure;
- cost-basis policy is domain/configuration;
- custodian reconciliation is application workflow;
- UI report formatting is presentation logic.

## 3. APIs and services

| API/service | Purpose |
|---|---|
| Instrument API | Search equity instruments, listings, identifiers and product attributes. |
| Price API | Provide current/historical prices, source and stale flags. |
| Trade API | Capture and query equity trades. |
| Position API | Provide current and historical positions. |
| Lot API | Query tax/book lots and cost basis. |
| Corporate Action API | Query events, entitlements, elections and postings. |
| Income API | Dividends, tax and income summary. |
| Performance API | Return, contribution, attribution and periods. |
| Risk API | Concentration, volatility, beta, VaR and liquidity. |
| Suitability API | Check product and portfolio fit. |
| Reporting API | Client holdings, activity and portfolio review. |

## 4. Event-driven design

Equity platforms benefit from event-driven processing.

| Event | Consumer action |
|---|---|
| EquityTradeBooked | Update positions, lots, cash, audit. |
| EquityTradeSettled | Update settled quantity and cash status. |
| EquityTradeCancelled | Reverse/cancel postings. |
| EquityPriceUpdated | Revalue positions and risk. |
| FxRateUpdated | Update base-currency valuations. |
| CorporateActionAnnounced | Create/refresh CA event. |
| CorporateActionElectionReceived | Record election and block quantity if needed. |
| CorporateActionPaid | Post cash/security transactions. |
| DividendPosted | Update income and performance. |
| PositionReconciled | Clear or raise breaks. |

Use idempotency keys so duplicate feed messages do not double-book trades or corporate actions.

## 5. Reconciliation model

| Reconciliation | Compare |
|---|---|
| Trade reconciliation | OMS/broker trade versus internal booking. |
| Settlement reconciliation | Expected settled trades versus custodian settlement. |
| Position reconciliation | Internal quantity versus custodian quantity. |
| Cash reconciliation | Internal cash ledger versus custodian cash. |
| Dividend reconciliation | Expected dividend versus custodian dividend. |
| Corporate action reconciliation | Expected entitlement versus actual cash/security posting. |
| Price reconciliation | Internal valuation price versus approved source/custodian. |
| P&L reconciliation | Internal P&L versus accounting/custodian where applicable. |

Break management should capture:

- break type;
- severity;
- business date;
- owner;
- root cause;
- expected value;
- actual value;
- resolution action;
- audit trail.

## 6. Advisory and suitability checks

For equity recommendations, advisory platforms should check:

| Check | Example question |
|---|---|
| Client risk profile | Can the client tolerate equity volatility and loss? |
| Investment objective | Growth, income, speculation, hedging or diversification? |
| Time horizon | Is the holding period suitable for equity risk? |
| Liquidity need | Can the client handle market drawdown and settlement timing? |
| Product complexity | Ordinary share versus warrant, preferred, ADR, restricted stock. |
| Concentration | Does the trade create overexposure to issuer/sector/country/currency? |
| Existing portfolio fit | Does it improve or worsen diversification? |
| Currency exposure | Is the client comfortable with foreign currency risk? |
| Restricted list | Is the security allowed for the client/advisor? |
| Insider/employee stock | Any dealing restrictions? |
| Market risk | Volatility, beta, drawdown and liquidity. |
| Tax implications | Dividend withholding, capital-gains and stamp duty awareness. |

## 7. DPM and portfolio-construction use cases

Equities in DPM are managed at portfolio strategy level.

| DPM need | Platform implication |
|---|---|
| Model portfolio | Security weights and constraints. |
| Rebalancing | Generate orders to align with target weights. |
| Cash management | Buy/sell around cashflows. |
| Corporate actions | Adjust model and client positions after events. |
| Benchmarking | Compare to equity benchmark/index. |
| Risk budgeting | Track beta, volatility, sector/country deviation. |
| Restrictions | Exclude securities based on client mandate. |
| Drift monitoring | Identify positions outside tolerance. |
| Performance attribution | Allocation/selection/currency/trading effects. |

## 8. Equity reporting requirements

Client reporting should show:

| Report section | Content |
|---|---|
| Holdings | Quantity, price, market value, cost, unrealized P&L, currency, weight. |
| Transactions | Buys, sells, dividends, tax, fees, corporate actions. |
| Income | Gross dividend, tax, net dividend, yield. |
| Performance | Price return, income return, FX return, total return. |
| Risk | Concentration, sector/country/currency exposure, volatility. |
| Corporate actions | Important events and client choices. |
| Tax | Withholding tax and reclaimable amounts, where available. |
| Suitability | Portfolio fit and concentration alerts, if advisory report. |

## 9. Integration with portfolio analytics

Equity data feeds into:

- valuation;
- cost and P&L;
- TWR and MWR;
- contribution and attribution;
- risk analytics;
- concentration risk;
- suitability and health checks;
- investment proposal and simulation;
- portfolio review and client reporting;
- collateral/buying-power calculations.

For your type of platform, one important principle is:

```text
The transaction engine should preserve economic truth.
The analytics engine should consume clean, classified, reconciled positions and cashflows.
```

## 10. Buying power / collateral considerations

Equities may be used as collateral in Lombard or margin lending, but with haircuts.

| Attribute | Use |
|---|---|
| Market value | Base value before haircut. |
| LTV/haircut | Determines collateral value. |
| Eligible flag | Some equities may be ineligible. |
| Concentration haircut | Additional haircut for concentrated holdings. |
| Liquidity haircut | Higher haircut for illiquid stocks. |
| Currency haircut | FX risk adjustment. |
| Price staleness | Stale prices may reduce or block collateral value. |
| Corporate action pending | May affect availability and value. |

Example:

```text
Collateral Value = Market Value x LTV
```

If market value is SGD 1,000,000 and LTV is 60%, collateral value is SGD 600,000.

## 11. Controls and governance

| Control | Why it matters |
|---|---|
| Maker-checker for manual trades | Prevent unauthorized bookings. |
| Price override approval | Prevent manipulated valuations. |
| Corporate-action event versioning | Handle revised terms. |
| Idempotent feed processing | Prevent duplicate bookings. |
| Full audit trail | Regulatory and client evidence. |
| Reconciliation workflow | Detect breaks early. |
| Suitability evidence | Prove why trade was appropriate. |
| Concentration alerts | Prevent unintended portfolio risk. |
| Restricted-list integration | Avoid prohibited securities. |
| Model governance | For risk/performance calculations. |
| Data lineage | Trace report values to source data. |

## 12. Common platform edge cases

| Edge case | Required handling |
|---|---|
| Buy before ex-date, settle after record date | Entitlement depends on market rules, ex-date and settlement mechanics. |
| Sell after ex-date | Seller may retain dividend entitlement. |
| Stock split missing in price feed | Detect abnormal price movement and missing CA. |
| Corporate action revised after posting | Reverse/repost with versioned event. |
| Rights election missed | Rights may expire; client impact and audit needed. |
| Fractional shares not supported | Cash-in-lieu transaction required. |
| ADR fee deducted from dividend | Separate fee from tax and income. |
| Suspended stock | Prevent trading but continue controlled valuation. |
| Delisted stock | Update status; value may not be zero immediately. |
| Multi-listing price mismatch | Use configured primary valuation listing. |
| FX missing | Block base-currency valuation or use approved fallback. |

## 13. Practitioner Questions and Answers

### How is an equity different from a bond?

An equity is ownership in a company. A bond is a debt claim. Bondholders expect contractual coupon and principal repayment, subject to credit risk. Equity holders have no maturity or guaranteed repayment, but participate in upside and dividends. In liquidation, equity ranks below debt.

### Why are equities complex in wealth platforms?

The initial trade is simple, but lifecycle processing is complex. Corporate actions can change quantity, cash, cost basis, tax, instrument identity and entitlement. Platforms also need accurate price/FX data, settlement status, lot-level P&L, dividend treatment, ADR mappings, concentration analytics and reconciliation.

### How should an equity position be modelled?

I would model the position by portfolio, instrument and listing, with total quantity, settled and unsettled quantity, available quantity, blocked quantity, cost basis, market value, unrealized P&L, income and status. I would maintain lots separately for tax and P&L, and derive positions from immutable transactions rather than manually overwriting positions.

### How should a stock split be processed?

A stock split should be processed as a corporate-action event. It changes quantity and cost per share but should not create economic gain or loss by itself. The platform should adjust position quantity, update lot quantities and unit costs, validate price adjustment and ensure performance does not show artificial return.

### How should a cash dividend be processed?

First determine entitlement using ex-date/record-date and eligible quantity. On pay date, book gross dividend income, withholding tax and net cash. Quantity does not change. For performance, dividend is investment income, not an external cashflow unless the cash is withdrawn from the portfolio.

### What is the difference between an ADR and a local share?

An ADR is a depositary receipt representing foreign shares held by a custodian/depositary structure. It trades separately, often in USD, and may have a receipt ratio, ADR fees, different dividend currency, and different corporate-action pass-through treatment. It should be a separate instrument linked to the underlying issuer/local share for look-through risk.

### How should equities feed performance attribution?

For each stock, the engine needs beginning weight, local return, income return, FX return, transaction effects and classification such as sector/country/currency. Contribution can be calculated at security level and aggregated. Attribution can compare portfolio sector weights and returns against benchmark weights and returns, separating allocation, selection, currency and residual effects.

## 14. Senior-platform explanation

> Equities appear straightforward because the core position is just a share quantity, but enterprise wealth platforms need a complete lifecycle model. I would separate instrument master, listings, transactions, position lots, corporate actions, valuations and risk exposures. The accounting transaction layer should capture buys, sells, dividends, tax, fees and corporate-action postings. The corporate-action layer should manage event dates, entitlement snapshots, elections, posting and reconciliation. Analytics should consume clean positions, prices, FX and cashflows to produce valuation, P&L, TWR/MWR, contribution, attribution, risk and concentration. This separation keeps the platform auditable, scalable and suitable for client reporting and advisory governance.

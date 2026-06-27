# Direct Equities And Market Operations Knowledge Map

This page connects direct-equity product knowledge to reusable private-banking platform decisions.

## Current Pack

| Product Area | Pack | Main Use |
|---|---|---|
| Equities | [`equities/`](equities/README.md) | Direct equity product taxonomy, trade lifecycle, settlement, corporate actions, dividends, cost basis, valuation, P&L, performance, risk, advisory, DPM, reporting, controls, and market-operations edge cases. |

## Core Equity Platform Distinctions

| Concept | Project Implication |
|---|---|
| Legal position | The client usually owns a quantity of shares in a listed or unlisted equity instrument. |
| Listing identity | Issuer, instrument, exchange listing, ticker, ISIN, currency, and depositary receipt identity must not be collapsed into one weak identifier. |
| Trade lifecycle | Order, execution, allocation, trade date, settlement date, custody posting, cash movement, and reconciliation can differ. |
| Position lots | Lot-level cost basis, tax lots, realized P&L, and corporate-action adjustments need a stable model. |
| Corporate actions | Dividends, splits, rights, mergers, spin-offs, tender offers, delistings, ADR events, and elections require event, entitlement, election, posting, and reversal controls. |
| Market data | Price, FX, trading status, volume, listing, stale flags, and corporate-action adjusted history need source-quality governance. |
| Risk and analytics | Equity concentration, issuer/sector/country/currency exposure, factor exposure, volatility, beta, drawdown, and contribution/attribution should be derived from clean positions and market data. |

## Questions To Ask Before Building An Equity Feature

1. Is this about order management, transaction booking, settlement, custody, position, lot, market data, corporate action, valuation, risk, performance, or reporting?
2. Is the security identity issuer-level, instrument-level, listing-level, or depositary-receipt-level?
3. Which date drives the behavior: order date, trade date, settlement date, ex-date, record date, pay date, effective date, or booking date?
4. Is the position settled, unsettled, blocked, pledged, lent, short, suspended, delisted, restricted, or partially available?
5. Does the feature require lot-level cost basis or only aggregate position value?
6. Does a corporate action change quantity, cash, cost basis, instrument identity, tax, entitlement, or all of them?
7. Is the UI showing accounting position, market exposure, risk exposure, voting entitlement, or performance contribution?
8. What should happen when price, FX, listing status, corporate-action feed, or custodian posting is missing or stale?

## API And UI Implications

Equity APIs and UI should make these states explicit:

1. instrument and listing identity,
2. settled and unsettled quantity,
3. available and blocked quantity,
4. lot and cost-basis posture,
5. market data source and stale status,
6. corporate-action event status,
7. entitlement and election status,
8. cash, tax, and fee legs,
9. realized and unrealized P&L,
10. risk and concentration exposure,
11. restricted-list or trading-status posture,
12. reconciliation exceptions and operator actions.

## QA Scenarios To Preserve

1. buy and sell with settlement lag,
2. partial sale across multiple lots,
3. cash dividend with withholding tax,
4. stock split and reverse split,
5. stock dividend or bonus issue,
6. rights issue with exercise, sale, lapse, and missed election,
7. spin-off with cost-basis allocation,
8. merger with cash, stock, and mixed consideration,
9. ADR dividend with fee and FX conversion,
10. delisted or suspended security valuation,
11. missing corporate action in price series,
12. multi-listing price-source conflict,
13. restricted-list or watchlist block,
14. short position and borrow-cost treatment where supported,
15. migration of historical positions without complete lot history.

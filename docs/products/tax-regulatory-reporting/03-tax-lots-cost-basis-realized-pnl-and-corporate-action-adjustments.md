# 03 - Tax Lots, Cost Basis, Realised P&L and Corporate Action Adjustments

## 1. What is a tax lot?

A tax lot is a specific acquisition record for a position.

Example:

| Lot | Buy date | Quantity | Price | Cost |
|---|---|---:|---:|---:|
| Lot 1 | 2026-01-10 | 100 | 50 | 5,000 |
| Lot 2 | 2026-03-20 | 200 | 60 | 12,000 |

If the client sells 150 shares, the platform must know which lot or lots were sold to calculate cost basis and realised gain/loss.

## 2. Why tax lots matter beyond tax

Tax lots are useful for:

- realised P&L reporting;
- client statements;
- advisory review;
- turnover analytics;
- performance explanation;
- migration reconciliation;
- backdated transaction correction;
- corporate-action processing;
- tax reporting and year-end packs.

Even where a jurisdiction does not generally tax capital gains, private banks often still provide realised gain/loss reporting for client transparency.

## 3. Cost basis methods

| Method | Description | Common use |
|---|---|---|
| FIFO | First acquired lot sold first | Common default |
| LIFO | Last acquired lot sold first | Some tax/reporting regimes |
| Average cost | Average cost across units | Funds/unit trusts in some contexts |
| Specific identification | Client/advisor chooses lots | Tax optimisation / HNW reporting |
| Highest cost first | Sell lots with highest basis first | Tax-loss management in some jurisdictions |
| Lowest cost first | Sell lots with lowest basis first | Gain realisation strategy |

The platform should support configurable lot-relief methods by jurisdiction, account, product and reporting purpose.

## 4. Core tax-lot fields

| Field | Meaning |
|---|---|
| lot_id | Unique lot identifier |
| account_id | Account/portfolio |
| instrument_id | Security/product |
| acquisition_date | Tax lot date |
| settlement_date | Settlement date |
| original_quantity | Original acquired quantity |
| open_quantity | Remaining quantity |
| local_cost_amount | Cost in security/settlement currency |
| base_cost_amount | Cost in portfolio base currency |
| transaction_id | Originating buy/subscription/transfer |
| cost_basis_method | FIFO/average/specific ID/etc. |
| holding_period_start | May differ from trade date in some rules |
| source_system | Custodian/core banking/migration |
| adjusted_cost_flag | Whether corporate action adjusted basis |

## 5. Realised gain/loss calculation

Basic formula:

```text
Realised P&L = Net Sale Proceeds - Allocated Cost Basis
```

More detailed:

```text
Gross Proceeds
- Selling Fees
- Transaction Taxes
- Allocated Cost Basis
= Realised Gain/Loss
```

For multi-currency portfolios, split realised P&L into:

- local-currency price P&L;
- FX P&L;
- fees/tax effect;
- total base-currency realised P&L.

## 6. Realised P&L example

Client buys:

| Lot | Quantity | Price | Cost |
|---|---:|---:|---:|
| Lot 1 | 100 | 50 | 5,000 |
| Lot 2 | 100 | 70 | 7,000 |

Client sells 150 at 80, fee 30.

Using FIFO:

| Sold from | Quantity | Cost basis |
|---|---:|---:|
| Lot 1 | 100 | 5,000 |
| Lot 2 | 50 | 3,500 |
| Total | 150 | 8,500 |

Sale proceeds:

```text
150 × 80 = 12,000
Net proceeds = 12,000 - 30 = 11,970
Realised gain = 11,970 - 8,500 = 3,470
```

## 7. Corporate actions and cost basis

Corporate actions often change tax lots without a simple cash/security trade.

| Corporate action | Quantity impact | Cost basis impact |
|---|---|---|
| Stock split | Quantity increases/decreases | Total basis usually unchanged; per-share basis changes |
| Bonus issue | Quantity increases | Basis allocated across more shares |
| Rights issue | New rights/security | Basis may be allocated or rights may have zero basis depending policy |
| Spin-off | New security created | Basis allocated between parent and spun entity |
| Merger stock-for-stock | Old security replaced | Carryover or new basis depending rules |
| Cash merger | Position disposed | Realised gain/loss event |
| Return of capital | Cash received | Reduces cost basis, excess may be gain depending rules |
| Liquidation distribution | Cash/security received | Disposal/return of capital treatment |
| DRIP/reinvestment | New units/shares acquired | Creates new lots at reinvestment value |

## 8. Cost basis adjustment examples

### Stock split

Before split:

| Quantity | Cost | Cost per share |
|---:|---:|---:|
| 100 | 5,000 | 50 |

2-for-1 split:

| Quantity | Cost | Cost per share |
|---:|---:|---:|
| 200 | 5,000 | 25 |

No income should be created merely because of the split.

### Spin-off

Parent company spins off new company. Basis allocation is based on official ratio or fair market values.

| Security | Allocation | Basis |
|---|---:|---:|
| Parent after spin | 80% | 8,000 |
| New spin entity | 20% | 2,000 |
| Total | 100% | 10,000 |

The platform must preserve the relationship between original lot and new lots.

### Return of capital

Client receives cash that is classified as return of capital.

| Item | Amount |
|---|---:|
| Original basis | 10,000 |
| Return of capital | 1,000 |
| New basis | 9,000 |

Do not always treat return of capital as ordinary income.

## 9. Transfers and migrated lots

Transferred securities should carry historical lots where available.

| Scenario | Treatment |
|---|---|
| Transfer in with full lot history | Preserve acquisition date and cost |
| Transfer in without cost | Mark cost basis unknown and create exception |
| Migration from legacy platform | Load historical lots with source/version metadata |
| Custodian cannot provide lot detail | Use reporting policy fallback and disclose limitation |

Never silently set missing cost to zero unless that is an explicit rule. Zero-cost basis can materially overstate realised gain.

## 10. Backdated and corrected transactions

Backdated trades can change:

- open lots;
- average cost;
- realised gain/loss;
- holding-period classification;
- corporate action entitlement;
- performance restatement;
- client statement history.

The tax-lot engine should support replay/recalculation by effective date.

Recommended design:

```text
Transaction timeline -> lot engine replay -> realised P&L restatement -> statement correction
```

## 11. Product-specific lot handling

| Product | Lot complexity |
|---|---|
| Equity | High because of corporate actions |
| ETF | Similar to equity, plus distributions |
| Fund/unit trust | Subscriptions/redemptions and average cost may matter |
| Bond | Lots by nominal and clean price, accrued interest separate |
| Note | Lots by nominal, redemption/conversion may create new security lots |
| Option | Premium lots, exercise/assignment links to underlying lots |
| Private fund | Commitment/funded lots less standard; capital account reporting |
| Precious metal | Units/ounces/grams, allocated account details |
| FX | Lot matching for currency disposals may be required in some regimes |

## 12. Lot engine outputs

| Output | Purpose |
|---|---|
| Open lot report | Remaining holdings and cost |
| Realised gain/loss report | Sale proceeds, cost, gain/loss |
| Unrealised gain/loss report | Market value vs cost |
| Holding period report | Short/long term where relevant |
| Corporate action adjustment report | Basis changes from events |
| Unknown cost exception report | Missing or invalid basis |
| Tax optimisation view | Candidate lots for disposal |

## 13. Control checks

| Check | Why it matters |
|---|---|
| Sum of open lot quantity equals position quantity | Prevents holdings mismatch |
| Sum of lot cost equals position cost | Prevents P&L error |
| No negative open lot unless shorts supported | Prevents lifecycle defects |
| Corporate action ratio applied to all eligible lots | Prevents partial adjustment |
| Realised P&L ties to transaction proceeds | Ensures statement accuracy |
| FX rate captured at event date | Ensures reproducible reporting |
| Lot method recorded | Ensures auditability |
| Migration lots reconciled | Prevents opening-position errors |


# 02 - Precious Metals, Bullion Accounts, Custody and Collateral

## 1. Precious metals overview

Precious metals are the most common commodity exposures in private banking. They include:

| Metal | Symbol | Typical role | Key drivers |
|---|---|---|---|
| Gold | XAU | Store of value, reserve asset, crisis hedge, portfolio diversifier | Real yields, USD, inflation expectations, central-bank buying, risk sentiment |
| Silver | XAG | Hybrid precious/industrial metal | Gold sentiment, industrial demand, solar/electronics demand, USD |
| Platinum | XPT | Industrial/precious metal | Auto catalysts, jewellery, hydrogen economy, supply concentration |
| Palladium | XPD | Industrial/precious metal | Auto catalysts, substitution, supply concentration |

Gold is usually the central wealth-management metal because it is widely recognized, liquid, held by central banks and available through multiple retail/private-banking wrappers.

## 2. Physical bullion

Physical bullion means the investor has exposure to actual metal, usually in the form of bars or coins.

Examples:

| Form | Typical client use | Notes |
|---|---|---|
| Large bars | Institutional / vault custody | Operationally efficient but unsuitable for small allocation |
| Kilobars | Asian private banking / high-value retail | Common in Asian bullion markets |
| Small bars | Retail/private client | Higher premium/spread |
| Coins | Retail collection / small-size investment | Premium includes minting and collector effects |
| Jewellery | Consumption/ornamental | Not pure investment-grade exposure |

## 3. Allocated versus unallocated metal

This is one of the most important concepts.

| Account type | Client claim | Platform treatment | Main risk |
|---|---|---|---|
| Allocated metal | Specific bars/coins or identified metal is allocated to client | Custody asset / metal position | Custody and operational risk; generally lower provider credit risk |
| Unallocated metal | Client has a claim on provider for an amount of metal, not specific bars | Metal account / credit claim | Provider credit risk |
| Pooled allocated / fractional allocated | Client has beneficial interest in segregated pool | Metal position with pool reference | Legal structure and custody rules matter |
| Synthetic metal account | Economic exposure, no direct physical ownership | Derivative or structured account | Counterparty and model risk |

Allocated metal is closer to ownership of specific metal. Unallocated metal is closer to a claim against a bullion bank/provider. This distinction affects credit risk, reporting, insolvency treatment, collateral eligibility and advisory disclosure.

## 4. Key bullion data attributes

For physical/allocated bullion, instrument static data is not enough. The platform may need custody-level data.

| Attribute | Example |
|---|---|
| Metal | Gold |
| Unit of measure | Troy ounce / gram / kilogram |
| Purity/fineness | 99.99% / 99.5% |
| Bar serial number | ABC12345 |
| Refiner/mint | LBMA-accredited refiner |
| Bar weight | 400 oz / 1 kg |
| Storage location | London / Zurich / Singapore / Hong Kong |
| Custodian/vault | Approved vault provider |
| Account type | Allocated / unallocated / pooled |
| Ownership structure | Direct / beneficial / creditor claim |
| Insurance coverage | Included / separate / not reported |
| Storage fee rate | bps or flat fee |
| Valuation source | LBMA benchmark / bank quote / vendor price |
| Delivery eligibility | Deliverable / non-deliverable / book-entry only |

## 5. Precious metal cash-style symbols

Platforms often model precious metal balances using ISO-like metal symbols:

| Symbol | Meaning |
|---|---|
| XAU | Gold |
| XAG | Silver |
| XPT | Platinum |
| XPD | Palladium |

A metal account may behave operationally like a cash balance but economically like a commodity. Do not treat XAU exactly like USD cash. It has price risk, bid/ask spread, custody fees and no deposit-insurance characteristics.

## 6. Transaction types for precious metals

| Transaction type | Use | Position impact | Cash impact |
|---|---|---|---|
| METAL_BUY | Purchase metal | Increase metal quantity | Cash out |
| METAL_SELL | Sell metal | Decrease metal quantity | Cash in |
| METAL_TRANSFER_IN | Transfer metal from external custodian | Increase metal | No cash unless fee |
| METAL_TRANSFER_OUT | Transfer metal out | Decrease metal | No cash unless fee |
| METAL_ACCOUNT_CREDIT | Credit metal balance | Increase metal account | Usually no cash |
| METAL_ACCOUNT_DEBIT | Debit metal balance | Decrease metal account | Usually no cash |
| METAL_STORAGE_FEE | Custody/storage fee | No metal change unless paid in metal | Cash or metal out |
| METAL_CONVERSION | Convert between metal units or forms | Reclassifies quantity/form | Possible fee/spread |
| METAL_DELIVERY_REQUEST | Request physical delivery | Lifecycle event | Fee/cash impact later |
| METAL_PHYSICAL_DELIVERY_OUT | Physical delivered to client | Reduce custody position | Delivery fee/tax possible |
| METAL_REFINING_ASSAY_FEE | Assay/refining charge | No investment exposure change | Fee out |
| METAL_COLLATERAL_PLEDGE | Pledge metal for credit | Encumbrance only | No cash at pledge |
| METAL_COLLATERAL_RELEASE | Release pledge | Remove encumbrance | No cash |

## 7. Position modelling

### Allocated physical metal position

| Field | Example |
|---|---|
| account_id | Client portfolio |
| instrument_id | XAU_ALLOCATED_BAR |
| metal | XAU |
| quantity | 100.000 troy oz |
| purity | 99.99% |
| storage_location | Zurich |
| custodian_id | Vault provider |
| allocated_flag | true |
| bar_list_id | Bar inventory reference |
| market_price | USD/oz |
| market_value | quantity × price × purity adjustment |
| accrued_income | normally zero |
| storage_fee_accrued | optional |
| pledged_quantity | portion pledged as collateral |
| available_quantity | quantity - pledged - pending delivery |

### Unallocated metal account position

| Field | Example |
|---|---|
| account_id | Client portfolio |
| instrument_id | XAU_UNALLOCATED_BANK_X |
| metal | XAU |
| quantity | 100.000 oz |
| provider_id | Bank X |
| claim_type | Unallocated creditor claim |
| market_value | quantity × price |
| counterparty_exposure | Bank X |
| storage_fee | often lower/none depending account |
| available_quantity | after pledges/holds |

## 8. Valuation

Simplified valuation:

```text
Metal Market Value = Metal Quantity × Metal Price × FX Rate - Storage/Exit Adjustments
```

For allocated physical metal, valuation may use:

- LBMA benchmark price;
- bank bid/ask quote;
- exchange price;
- custodian valuation;
- independent data vendor;
- local market price for coins/small bars.

For client reporting, it is useful to show:

| Measure | Meaning |
|---|---|
| Spot valuation | Quantity × spot price |
| Bid valuation | Estimated sale value after bid spread |
| Custody-adjusted value | Spot less known storage/exit cost |
| Reporting value | Bank-approved valuation price |
| Pledged value | Collateral value after haircut |

## 9. Collateral treatment

Gold can be eligible collateral in some lending frameworks, but eligibility depends on bank policy.

Important fields:

| Field | Meaning |
|---|---|
| eligible_for_collateral | Whether the metal can support credit |
| collateral_value | Market value after haircut |
| haircut_pct | Risk haircut |
| ltv_pct | Lending value percentage |
| pledge_scope | Account / portfolio / credit line |
| custody_control | Whether bank controls the asset |
| liquidity_score | How quickly asset can be monetized |
| valuation_frequency | Daily/intraday/monthly |
| stress_haircut | Additional market stress haircut |

Example:

| Item | Value |
|---|---:|
| Gold quantity | 100 oz |
| Price | USD 2,300/oz |
| Market value | USD 230,000 |
| LTV | 70% |
| Collateral value | USD 161,000 |

## 10. Reporting treatment

In client reporting, precious metals should be transparent:

| Report item | Recommended display |
|---|---|
| Asset class | Commodities / Precious metals / Real assets |
| Instrument name | Gold allocated account / Gold bar / Gold ETF |
| Quantity | oz/g/kg and equivalent units |
| Price | quoted price per unit |
| Market value | portfolio currency value |
| P&L | price movement + FX effect - fees |
| Income | normally none |
| Fees | storage/custody/transaction fees |
| Risk note | commodity price risk and custody/provider risk |
| Pledge | pledged/available split if collateralized |

## 11. Advisory considerations

Advisors should explain:

- gold and precious metals do not generate earnings or coupon income;
- returns depend on market price and currency movements;
- physical storage and bid/ask spreads can materially affect realized value;
- allocated and unallocated accounts have different legal and credit-risk implications;
- precious metals can diversify a portfolio but should not be sold as guaranteed protection;
- concentration should be controlled, especially when client sentiment is driven by crisis headlines;
- liquidity differs by wrapper, size, location and provider.

## 12. Platform caution

Do not model physical metal as a normal equity-like security without unit, purity, custody and account-type attributes. Also do not model unallocated metal as risk-free cash. It is a commodity exposure plus a provider claim.

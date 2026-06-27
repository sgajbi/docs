# 04 — Swaps, CDS and OTC Derivatives

## 1. What is a swap?

A **swap** is an agreement between two parties to exchange cashflows according to a formula.

The most common pattern is:

```text
Party A pays Leg 1
Party B pays Leg 2
Net cashflow is exchanged on payment dates
```

Swaps are usually OTC contracts governed by legal documentation such as an ISDA master agreement and confirmations. Many standardized swaps may be centrally cleared depending on jurisdiction and product.

## 2. Interest-rate swap, IRS

An IRS exchanges fixed interest payments for floating interest payments in the same currency.

Example:

| Term | Example |
|---|---|
| Notional | USD 10,000,000 |
| Tenor | 5 years |
| Fixed leg | Pay 4.00% annually |
| Floating leg | Receive SOFR + spread quarterly |
| Currency | USD |
| Settlement | Net cash settlement |

Use cases:

| Use case | Example |
|---|---|
| Hedge rate exposure | Convert floating-rate liability to fixed |
| Adjust portfolio duration | Pay fixed to reduce duration / receive fixed to increase duration |
| Relative value | Express curve or rate view |

Risk measures:

| Measure | Meaning |
|---|---|
| DV01 / PV01 | Change in value for 1 bp rate move |
| Curve exposure | Sensitivity by tenor point |
| Carry/roll-down | Expected return from curve shape/time |
| Counterparty exposure | OTC replacement risk |

## 3. Cross-currency swap, CCS

A cross-currency swap exchanges cashflows in two currencies. It may include initial and final principal exchanges.

Example:

| Leg | Details |
|---|---|
| USD leg | Pay USD floating |
| SGD leg | Receive SGD fixed/floating |
| Principal exchange | USD and SGD principal may be exchanged at start/end |

Use cases:

- hedge foreign bond cashflows;
- transform funding currency;
- manage DPM currency exposure;
- support structured products and private-market currency hedging.

Risk measures include FX exposure, rate exposure in both currencies, basis spread risk and counterparty risk.

## 4. Total return swap, TRS

A total return swap transfers the total economic return of an asset or index without direct ownership.

| Leg | Meaning |
|---|---|
| Total return leg | Pays price return + income of reference asset |
| Financing leg | Pays funding rate + spread |

Example: client receives total return of an equity index and pays SOFR + 1%.

Use cases:

| Use case | Comment |
|---|---|
| Synthetic exposure | Gain exposure without buying asset |
| Financing/leverage | Obtain exposure with collateral rather than full cash |
| Market access | Access restricted markets through swap |
| Hedge | Offset existing exposure |

Platform implication: client may have no direct holding in the underlying, but must report look-through exposure.

## 5. Equity swap

An equity swap is a type of TRS linked to a stock, basket or equity index.

Key data fields:

| Field | Meaning |
|---|---|
| reference_asset | Equity/index/basket |
| notional | Reference exposure |
| initial_price | Starting level |
| reset_frequency | Daily/monthly/quarterly |
| financing_leg | Rate + spread |
| dividend_treatment | Included/excluded/gross/net |
| settlement_type | Cash-settled |

Equity swaps can create concentrated exposure without direct custody position. Reporting must show both derivative contract and underlying-equivalent exposure.

## 6. Inflation swap

An inflation swap exchanges fixed payments for payments linked to inflation index movements.

Use cases:

- hedge inflation-linked liabilities;
- express inflation view;
- support institutional or wealth portfolios with inflation-sensitive objectives.

Platform concerns:

- index lag and publication schedule;
- interpolation rules;
- seasonality;
- base CPI level;
- payment lag.

## 7. Swaption

A swaption is an option to enter into a swap.

| Type | Right |
|---|---|
| Payer swaption | Right to pay fixed / receive floating |
| Receiver swaption | Right to receive fixed / pay floating |

Swaptions combine option risk and rate risk:

- underlying swap rate;
- volatility;
- expiry;
- tenor;
- exercise style;
- collateral/discounting convention.

## 8. Credit default swap, CDS

A CDS is a credit derivative where one party buys protection against a credit event of a reference entity.

| Role | Pays/receives |
|---|---|
| Protection buyer | Pays periodic premium; receives protection payment if credit event occurs |
| Protection seller | Receives premium; pays if credit event occurs |

Reference entity is not the same as counterparty.

| Risk | Example |
|---|---|
| Counterparty risk | Dealer may fail |
| Reference credit risk | Company ABC defaults |
| Documentation risk | Credit event determination and settlement process |
| Recovery risk | Final recovery value affects payoff |

## 9. CDS lifecycle

| Event | Object / transaction |
|---|---|
| Trade | CDS_OPEN |
| Premium payment | CDS_PREMIUM_PAYMENT |
| MTM valuation | Valuation record |
| Credit event | CREDIT_EVENT_TRIGGER lifecycle event |
| Auction / settlement determination | CREDIT_EVENT_SETTLEMENT_DETERMINATION |
| Protection payment | CDS_PROTECTION_PAYMENT |
| Recovery delivery/cash settlement | CDS_RECOVERY_SETTLEMENT |
| Termination | CDS_CLOSE / CDS_TERMINATION |

## 10. Recommended swap/CDS transaction types

| Transaction type | Use |
|---|---|
| SWAP_OPEN | Create swap position |
| SWAP_CLOSE | Terminate/close swap |
| SWAP_PARTIAL_TERMINATION | Reduce notional |
| SWAP_RESET | Rate/index/price reset event |
| SWAP_COUPON_PAYMENT | Net or gross swap cashflow |
| SWAP_UPFRONT_FEE | Upfront amount paid/received |
| SWAP_COLLATERAL_POSTED | Collateral posted |
| SWAP_COLLATERAL_RETURNED | Collateral returned |
| SWAP_NOVATION_OUT | Transfer contract to new counterparty |
| SWAP_NOVATION_IN | Receive contract by novation |
| SWAP_COMPRESSION | Compression lifecycle event |
| CDS_OPEN | Open credit derivative position |
| CDS_PREMIUM_PAYMENT | Periodic CDS premium |
| CDS_CREDIT_EVENT_WRITEDOWN | Loss event for seller/protection economics |
| CDS_PROTECTION_PAYMENT | Protection payment on credit event |
| CDS_RECOVERY_PAYMENT | Recovery/cash settlement |
| SWAP_REVERSAL_CORRECTION | Operational correction |

## 11. OTC derivative position model

| Field | Meaning |
|---|---|
| derivative_contract_id | Unique contract |
| product_family | IRS / CCS / TRS / CDS / EQUITY_SWAP etc. |
| account_id | Portfolio/account |
| counterparty_id | Dealer/counterparty |
| legal_agreement_id | ISDA/CSA |
| trade_date | Trade date |
| effective_date | Start date |
| maturity_date | End date |
| notional | Contract notional |
| notional_currency | Currency |
| pay_leg_terms | Formula for pay leg |
| receive_leg_terms | Formula for receive leg |
| reset_schedule | Fixing/reset dates |
| payment_schedule | Payment dates |
| valuation_agent | Dealer/vendor/internal |
| collateral_terms | Threshold, MTA, eligible collateral |
| clearing_status | Bilateral/cleared |
| lifecycle_status | Active/terminated/matured/defaulted |

## 12. Valuation overview

| Product | Simplified valuation approach |
|---|---|
| IRS | PV fixed leg - PV floating leg |
| CCS | PV of both currency legs with FX and basis |
| TRS/equity swap | PV reference asset return leg - financing leg |
| Inflation swap | PV fixed leg - PV inflation-linked leg |
| Swaption | Rate option model |
| CDS | PV premium leg - PV protection leg |

OTC derivative valuation must specify:

- curves used;
- discounting method;
- collateral currency;
- valuation source;
- model version;
- market data timestamp;
- counterparty valuation difference tolerance.

## 13. Advisory and mandate view

Swaps and CDS are often institutional-grade products. For wealth clients they are usually limited to sophisticated or professional clients, DPM mandates or structured product manufacturing.

Advisory questions:

| Question | Why it matters |
|---|---|
| What is the purpose? | Hedge, exposure, leverage or yield? |
| What is the notional? | Market value may understate exposure |
| What is the counterparty? | OTC credit exposure |
| What collateral is required? | Liquidity and margin risk |
| Can it be terminated early? | Liquidity and pricing risk |
| What is worst-case scenario? | Client understanding and suitability |
| How does it affect mandate exposure? | Duration, FX, equity, credit, leverage |

## 14. Reporting expectations

Client/advisor reports should show:

- contract market value;
- notional exposure;
- underlying/reference exposure;
- direction: pay/receive, long/short protection;
- counterparty;
- collateral/margin;
- realized/unrealized P&L;
- DV01/CS01/delta where relevant;
- maturity and next reset/payment date;
- purpose tag: hedge, overlay, income, tactical, leverage.

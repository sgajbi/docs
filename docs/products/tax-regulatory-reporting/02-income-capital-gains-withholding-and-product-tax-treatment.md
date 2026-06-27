# 02 - Income, Capital Gains, Withholding and Product Tax Treatment

## 1. Important disclaimer

Tax treatment is jurisdiction-specific and client-specific. The platform should not hardcode advisory conclusions such as “taxable” or “non-taxable” without considering client residency, product domicile, account wrapper, source country, treaty documentation and local law.

This file explains **platform modelling patterns**, not legal/tax advice.

## 2. Gross, withholding and net reporting

Income reporting should store all three amounts:

```text
Gross Income - Tax Withheld - Fees/Charges = Net Cash Received
```

Example:

| Item | Amount |
|---|---:|
| Gross dividend | USD 1,000 |
| Foreign withholding tax | USD 300 |
| Net dividend received | USD 700 |

The client may only see USD 700 in cash, but reporting must preserve USD 1,000 gross income and USD 300 tax withheld.

## 3. Key income categories

| Income category | Product examples | Common reporting needs |
|---|---|---|
| Interest | Cash deposits, bonds, loans, money market instruments | Accrual, payment date, WHT, source country |
| Coupon | Bonds, notes, structured products | Clean/dirty treatment, accrued interest, conditional coupon |
| Dividend | Equities, ETFs, funds, REITs | Gross/net, WHT, ex-date/pay-date, qualified/non-qualified where relevant |
| Fund distribution | Mutual funds, ETFs, private funds | Component split: income, capital gain, return of capital |
| REIT distribution | REITs, property trusts | Income/capital component, WHT, local rules |
| Derivative settlement | Options, futures, swaps | Premium, P&L, cash settlement, margin |
| Insurance/annuity payment | Annuities, policy benefits | Income vs capital, surrender/maturity/claim distinction |
| Private market distribution | PE/private credit/real assets | Return of capital, income, realised gain, recallable amount |

## 4. Withholding tax concepts

Withholding tax is tax deducted at source before payment reaches the client.

Common drivers:

- source country of income;
- payer/issuer domicile;
- client tax residency;
- beneficial-owner classification;
- treaty rate;
- product wrapper;
- tax documentation status;
- income type.

Typical platform fields:

| Field | Meaning |
|---|---|
| gross_amount | Amount before tax |
| withholding_tax_amount | Tax deducted |
| withholding_tax_rate | Applied WHT rate |
| withholding_tax_currency | Currency of tax deduction |
| source_country | Income source jurisdiction |
| treaty_country | Treaty residence claimed, if any |
| tax_form_id | Documentation supporting rate |
| tax_reclaim_eligible | Whether reclaim may be available |
| tax_reclaim_status | Not requested / pending / received / rejected |
| net_amount | Cash received after tax |

## 5. Capital gains and realised P&L

A capital event is usually created by:

- sale;
- redemption;
- maturity;
- tender;
- merger cash-out;
- fund redemption;
- option exercise/assignment;
- physical settlement;
- default/write-off;
- corporate action with disposal treatment.

Realised gain/loss is generally:

```text
Realised Gain/Loss = Net Disposal Proceeds - Allocated Cost Basis
```

For platform analytics, always separate:

- realised investment P&L;
- accounting gain/loss;
- tax gain/loss;
- performance contribution;
- FX translation effect;
- fees and taxes.

They may not be identical.

## 6. Singapore-aware modelling notes

For Singapore context, the platform should know that Singapore does not generally tax capital gains from sale of shares and financial instruments for individual investment activity, but trading-like activity may be treated differently. IRAS states that gains from the sale of property, shares and financial instruments in Singapore are generally not taxable, while certain trading/profit-seeking activities may be taxable.

Singapore currently does not impose withholding tax on dividend payments. IRAS also provides withholding-tax guidance for payments such as interest, royalties, service fees and other payments to non-residents. These rules should be represented as configurable jurisdiction rules rather than hardcoded into product processing.

## 7. Product treatment matrix

| Product | Income events | Capital/realised events | Special treatment concerns |
|---|---|---|---|
| Cash deposit | Interest | Deposit withdrawal usually not gain/loss | Accrual, WHT, deposit insurance disclosure |
| Money market instrument | Discount/interest | Maturity/redemption | Accretion, clean pricing, short-term yield |
| Bond | Coupon interest | Sale, redemption, maturity, default | Accrued interest, amortisation/accretion, callable events |
| Equity | Dividend | Sale, tender, merger, liquidation | WHT, ex-date/pay-date, corporate-action adjustments |
| Preferred share | Dividend-like coupon | Sale/redemption/call | Hybrid equity/fixed-income classification |
| Fund/unit trust | Distribution | Redemption/switch | Distribution component, reinvestment, equalisation |
| ETF | Dividend/distribution | Exchange sale/redemption | Listed security + fund tax components |
| REIT | Distribution | Sale/redemption | Income/capital components, leverage and property exposure |
| Structured note | Coupon | Autocall, maturity, sale, physical delivery | Conditional coupon, barrier, issuer risk, tax classification |
| Option | Premium | Close, expiry, exercise/assignment | Premium treatment, underlying delivery, short option risk |
| Future | Variation margin/P&L | Close/expiry | Daily mark-to-market, margin cashflows |
| Swap | Periodic net settlement | Termination | Accrual, fair value, collateral |
| FX forward | Forward points/P&L | Settlement | Two-currency cash movements, NDF cash settlement |
| Private equity fund | Distributions | Secondary sale, liquidation | Return of capital, recallable distributions, delayed tax packs |
| Private credit | Interest/distribution | Repayment/default | NAV lag, credit impairment, income vs capital |
| Insurance/annuity | Benefit/annuity payout | Surrender/maturity | Policy value, premium basis, claim event |
| Loan/margin | Interest expense | Repayment | Deductibility depends on tax rules; reporting as expense |

## 8. Tax withheld vs performance return

Tax withholding reduces client cash received, but performance reporting must be explicit about whether returns are:

- gross of tax;
- net of withholding tax;
- net of all taxes and fees;
- net of fees but gross of tax;
- in portfolio base currency;
- in local/security currency.

Example:

| Measure | Treatment |
|---|---|
| Gross income return | Uses gross dividend/coupon |
| Net income return | Uses dividend/coupon after withholding |
| Portfolio performance | Depends on client reporting policy |
| Tax statement | Shows gross, WHT and net |
| Client cash ledger | Shows net cash received and tax debit |

## 9. Reclaimable withholding tax

Some withholding tax may be reclaimable depending on treaty, documentation and local process.

Model separately:

| Field | Meaning |
|---|---|
| tax_withheld_amount | Amount deducted |
| reclaim_eligible_amount | Potential reclaimable amount |
| reclaim_claimed_amount | Amount claimed |
| reclaim_received_amount | Cash actually received |
| reclaim_writeoff_amount | Amount abandoned/rejected |
| reclaim_status | Pending / received / rejected / expired |

Reclaim receipts should be linked back to the original income event. Do not book them as unexplained miscellaneous income.

## 10. Multi-currency tax effects

Tax reporting may involve several currencies:

- security currency;
- income currency;
- tax withholding currency;
- account currency;
- portfolio base currency;
- client tax-reporting currency.

Every tax amount should store:

- original currency amount;
- FX rate used;
- FX source;
- converted reporting amount;
- valuation date/time;
- rounding method.

## 11. Common mistakes

| Mistake | Better approach |
|---|---|
| Store only net income | Store gross, tax, fees and net |
| Treat WHT as investment loss without classification | Classify tax separately and decide performance policy |
| Hardcode tax rules by product only | Use jurisdiction/client/product/source-country rules |
| Ignore tax form validity dates | Apply rate based on valid form at payment date |
| Treat fund distributions as one bucket | Preserve distribution component breakdown |
| Ignore corporate actions in tax lots | Adjust cost basis and lots explicitly |
| Mix accounting P&L and tax P&L | Store separate classifications |
| Use current FX rate for historical tax event | Use event/reporting policy FX rate |


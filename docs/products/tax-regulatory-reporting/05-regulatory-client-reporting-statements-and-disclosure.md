# 05 - Regulatory Client Reporting, Statements and Disclosure

## 1. Difference between client reporting and regulatory reporting

| Reporting type | Audience | Purpose |
|---|---|---|
| Client statement | Client, advisor, family office | Explain portfolio value, activity, income, performance |
| Tax pack/voucher | Client/tax adviser | Support tax filing and tax analysis |
| Regulatory report | Tax authority/regulator | Meet legal reporting obligation |
| Advisor dashboard | RM/IC/PM | Manage client, suitability, risks, exceptions |
| Operations report | Ops/reconciliation teams | Resolve breaks and control issues |

A client statement should be understandable. A regulatory report must be schema-compliant and legally accurate. A tax pack must be traceable and supportable.

## 2. Core client statement sections

| Section | Content |
|---|---|
| Portfolio summary | Opening value, closing value, inflows/outflows, performance |
| Holdings | Asset, quantity/nominal, price/NAV, market value, unrealised P&L |
| Asset allocation | Asset class, currency, geography, sector, risk exposure |
| Income summary | Dividends, coupons, interest, distributions, withholding tax |
| Transactions | Buys, sells, subscriptions, redemptions, maturities, fees, taxes |
| Realised P&L | Disposal proceeds, cost, realised gain/loss |
| Fees and charges | Advisory, custody, platform, transaction charges |
| Tax withheld | Gross income, tax deducted, net income |
| Performance | TWR/MWR, benchmark, contribution, attribution where applicable |
| Risk | Concentration, volatility, drawdown, VaR, leverage, liquidity |
| Disclosures | Product risks, valuation basis, limitations, disclaimers |

## 3. Tax-aware income statement

Recommended columns:

| Column | Description |
|---|---|
| Payment date | Cash date |
| Ex-date/record date | Entitlement reference where relevant |
| Instrument | Security/product |
| Product type | Equity, bond, fund, note, REIT, etc. |
| Income type | Dividend, coupon, interest, distribution |
| Source country | Income source |
| Gross amount | Before tax |
| WHT amount | Tax withheld |
| WHT rate | Applied rate |
| Net amount | Cash received |
| Currency | Income currency |
| Base currency amount | Converted amount |
| Tax form/treaty indicator | Documentation basis if applicable |
| Reclaim status | If tax reclaim process exists |

## 4. Realised gain/loss report

Recommended columns:

| Column | Description |
|---|---|
| Trade date | Sale/redemption date |
| Settlement date | Cash settlement date |
| Instrument | Disposed asset |
| Quantity/nominal sold | Disposal size |
| Gross proceeds | Before fees/taxes |
| Fees/taxes | Transaction costs |
| Net proceeds | After costs |
| Cost basis | Allocated tax/accounting cost |
| Realised gain/loss | Net proceeds minus cost |
| Currency | Security/local currency |
| Base currency gain/loss | Converted amount |
| Lot method | FIFO/average/specific ID |
| Holding period | Short/long term if applicable |
| Corporate action reference | If disposal caused by CA |

## 5. Tax voucher / annual tax statement

A tax voucher should support client and tax adviser review.

Typical sections:

- client/account identity;
- reporting period;
- income by source country;
- income by product category;
- gross income;
- withholding tax;
- net income;
- foreign tax paid;
- tax reclaim amounts;
- realised proceeds;
- realised gain/loss if provided;
- year-end holdings/value;
- notes on reporting basis and limitations.

Do not merge tax voucher logic with monthly statement logic. Monthly statements are portfolio reports; tax vouchers are tax-supporting evidence.

## 6. Product-specific disclosures

| Product | Reporting disclosure emphasis |
|---|---|
| Bonds | Yield, clean/dirty price, accrued interest, credit risk, call risk |
| Notes | Issuer risk, payoff conditions, barrier/autocall, secondary liquidity |
| Equities | Market risk, corporate action effects, dividend withholding |
| Funds | NAV, fund expenses, distribution components, liquidity/gates |
| ETFs/ETNs | Market price vs NAV/indicative value, issuer risk for ETNs |
| Derivatives | Leverage, margin, Greeks, liquidity, potential losses |
| Private markets | NAV lag, unfunded commitment, lock-up, illiquidity, valuation uncertainty |
| REITs | Leverage, property concentration, distribution sustainability |
| Insurance | Surrender charges, guarantee conditions, policy fees |
| Loans/margin | Interest cost, margin calls, forced liquidation risk |
| Trusts/family office | Ownership/reporting scope and beneficiary limitations |

## 7. Regulatory reporting examples

| Report/process | Common purpose |
|---|---|
| CRS | Automatic exchange of financial account information for reportable tax residents |
| FATCA | Reporting of US persons/US-owned entities and related withholding compliance |
| 1042-S style reporting | US-source income paid to foreign persons and withholding reporting |
| 1099-style reporting | US tax information reporting for US persons |
| Local withholding reports | Payments subject to withholding tax |
| Transaction reporting | Market/regulatory transaction reporting, where applicable |
| Product disclosure reports | Complex/SIP/EIP product disclosure and acknowledgements |
| Suitability evidence | Advisor recommendation and client acceptance trail |

## 8. Statement restatement and correction

Corrections are common in wealth reporting due to:

- late corporate-action information;
- revised fund distribution components;
- corrected withholding rates;
- late tax documentation;
- backdated transactions;
- price/NAV corrections;
- custodian restatements;
- migration defects.

Support:

| Capability | Purpose |
|---|---|
| Statement versioning | Preserve issued statements |
| Correction indicators | Show restated records |
| Delta report | Explain what changed |
| Audit trail | Support client and regulator queries |
| Effective-date recalculation | Recompute from correct historical date |
| Client notification workflow | Manage material corrections |

## 9. Advisory reporting vs official tax reporting

Advisory tax analytics may estimate tax impact to support portfolio decisions. Official tax reporting must follow the applicable rules and validated data.

| Advisory analytics | Official reporting |
|---|---|
| Scenario-based | Rule-based |
| May use assumptions | Must use official classifications |
| Supports decision-making | Supports compliance/tax filing |
| Can show estimated tax drag | Must show reported amounts |
| Advisor-facing | Client/regulator-facing |

## 10. Reporting hierarchy for HNW/UHNW clients

Client groups may need multi-level reporting:

```text
Family group
  -> family branch
    -> legal entity/trust/foundation
      -> account/portfolio
        -> mandate
          -> asset/product/position
```

Tax and regulatory reporting may need to happen at the legal-account holder level, while family office reporting may consolidate across multiple entities. The platform must maintain both views without losing legal entity boundaries.

## 11. What a strong client report should explain

For every income or tax number, the report should answer:

- Which product generated it?
- Was it gross or net?
- Was tax withheld?
- Which jurisdiction/source country applied?
- Is it cash income, accrued income or estimated income?
- Is it included in performance?
- Is it part of realised gain/loss?
- Is it a correction from a previous period?
- Is the value final or provisional?


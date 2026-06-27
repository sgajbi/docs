# 05 - Fees, Charges, Tax Withholding and Client Charging Models

## 1. Why fees need first-class modelling

Fees are often treated as small cash movements, but they materially affect client return, suitability, reporting, profitability and trust. A platform must distinguish fee types, fee bases, fee timing and fee treatment in performance.

| Fee dimension | Examples |
|---|---|
| Product fee | Fund TER/OCR, structured-product embedded margin, insurance charges |
| Transaction fee | Brokerage, commission, exchange fee, stamp duty, custody transaction charge |
| Advisory fee | Advisory platform fee, portfolio review fee |
| DPM fee | Management fee, performance fee, mandate fee |
| Financing fee | Loan interest, margin interest, commitment fee, facility fee |
| Custody fee | Safekeeping, account maintenance, statement fee |
| Exit fee | Fund redemption fee, surrender charge, early withdrawal penalty |
| Rebate | Trailer rebate, retrocession rebate, fee waiver |

## 2. Fee transaction model

| Field | Meaning |
|---|---|
| fee_id | Unique fee event |
| account_id | Charged account |
| related_transaction_id | Trade/income/product event if applicable |
| fee_type | Brokerage, custody, advisory, DPM, financing, product, tax, penalty |
| fee_basis | Trade notional, AUM, NAV, commitment, loan balance, income amount |
| fee_rate | Rate if applicable |
| fee_amount_gross | Fee before tax/rebate |
| fee_tax | GST/VAT/withholding/stamp duty if applicable |
| rebate_amount | Rebate or waiver |
| fee_amount_net | Final charged amount |
| fee_currency | Currency charged |
| charge_date | Date charged |
| accrual_period_start/end | Period covered |
| performance_treatment | Expense, excluded, external flow, embedded |
| client_visible_flag | Whether shown on statement/advisory disclosure |

## 3. Gross versus net performance

| Return view | Treatment |
|---|---|
| Gross of fees | Excludes selected management/advisory fees; may still include trading costs depending on policy |
| Net of fees | Includes selected fees and expenses charged to portfolio |
| Gross income | Income before withholding tax |
| Net income | Income after withholding tax |
| Product net NAV | Fund NAV already net of internal expenses |
| Advisor-visible all-in cost | Explicit + embedded + product-level + financing costs where available |

Reporting must say what is included. Do not label performance as "net" unless the fee universe is clear.

## 4. Transaction costs

Transaction costs may be capitalized into cost basis or expensed depending on product, accounting basis and reporting policy.

| Product | Common treatment in platform reporting |
|---|---|
| Equity buy commission | Add to cost basis or show as fee expense depending on policy |
| Equity sell commission | Reduce sale proceeds |
| Bond buy accrued interest | Separate from cost; affects income accrual treatment |
| Fund subscription fee | May be added to cost or treated as charge |
| Structured product upfront fee | Often embedded in issue price; explicit if separately charged |
| Derivative premium | Cost of option, not ordinary fee |
| Futures commission | Expense/transaction cost |
| FX spread | Often embedded; optionally estimated for all-in cost |

## 5. Embedded fees

Some products have embedded economics that are not booked as explicit cash fees.

| Product | Embedded fee examples |
|---|---|
| Fund/ETF | TER/OCR deducted inside NAV |
| Structured note | Issuer margin, structuring cost, hedging cost inside issue price |
| Insurance/ILP | Policy charges, bid-offer spread, mortality charges, fund charges |
| FX | Bid/ask spread, markup |
| Private funds | Management fee, carried interest, fund expenses inside NAV/capital account |

Client reporting may not be able to calculate all embedded fees precisely, but advisory disclosure should avoid presenting them as zero cost.

## 6. Withholding tax and other taxes

Tax treatment is jurisdiction-specific. The platform should model tax as configurable classification rather than hardcoded rules.

| Tax item | Example |
|---|---|
| Dividend withholding | Foreign equity dividend withholding |
| Coupon withholding | Cross-border bond interest withholding |
| Fund distribution withholding | Fund tax event by domicile and investor type |
| Stamp duty | Equity/property/security transfer tax |
| GST/VAT | Fee tax on advisory/custody services |
| Financial transaction tax | Market-specific levy |
| Capital gains tax | Jurisdiction-specific, often tax-lot dependent |

## 7. Tax transaction pattern

For an income event:

| Leg | Meaning |
|---|---|
| Gross income leg | Dividend/coupon/distribution before tax |
| Tax withholding leg | Tax withheld at source |
| Net cash leg | Cash received by client |

Example:

```text
Gross dividend: USD 1,000
Withholding tax: USD -300
Net cash received: USD 700
```

Reporting can show gross income, tax withheld and net income separately.

## 8. Fee allocation in family / multi-account portfolios

Fees may be charged at one level but allocated to many portfolios.

| Scenario | Design requirement |
|---|---|
| Family office consolidated fee | Allocate fee to accounts by AUM, mandate, agreed split or legal invoice |
| DPM model fee | Allocate by mandate sleeve/account |
| External billing | Report fee for transparency but no portfolio cash movement |
| Fee paid from master cash account | Link fee to benefited portfolios |
| Fee waived/rebated | Store waiver/rebate for net cost and audit |

## 9. Advisory and mandate considerations

Advisors and PMs need fee-aware views:

- all-in product cost;
- expected transaction costs of rebalance;
- cost impact of selling illiquid products;
- exit charges and surrender penalties;
- financing cost of leveraged strategies;
- gross/net return comparison;
- mandate constraints on fee levels or share-class eligibility;
- product suitability based on cost versus benefit.

## 10. QA scenarios

| Scenario | Expected control |
|---|---|
| Fee charged twice | Idempotency detects duplicate source fee |
| Fee accrued monthly, charged quarterly | Accrual reverses on payment; no double expense |
| Dividend withholding tax missing | Gross/net income reconciliation flags difference |
| Fund TER shown as explicit fee | Prevent double-counting if NAV already net |
| External billed fee | Performance treatment follows configured policy |
| Fee rebate posted after period close | Restatement or adjustment with lineage |
| Wrong GST/VAT currency | Currency validation flags mismatch |

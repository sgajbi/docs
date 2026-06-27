# 01 - Tax-Aware Reporting Fundamentals and Taxonomy

## 1. What tax-aware reporting means

Tax-aware reporting is the discipline of capturing, classifying, calculating and presenting investment activity in a way that supports tax, regulatory, client and advisor use cases.

It does not mean that the platform gives tax advice. It means the platform preserves the facts required for tax and regulatory reporting:

- what happened;
- when it happened;
- who the legal and beneficial owner was;
- which account or wrapper held the asset;
- what product generated the cashflow;
- which jurisdiction sourced the income;
- what amount was gross, withheld and net;
- what cost basis and realised gain/loss was created;
- what regulatory reportability flags apply;
- what documentation supports reduced withholding or reporting treatment.

## 2. Why tax-aware reporting is difficult in wealth management

Wealth platforms are complex because clients can be multi-jurisdictional and products can be multi-layered.

Example:

A Singapore resident client holds a US-listed ETF through a private-bank account booked in Singapore, inside a family trust, with a foreign tax form on file, receiving a distribution that includes dividend income, capital-gain distribution and possible return of capital.

The platform must know:

- account owner and beneficial owner;
- tax residency and reportable jurisdiction;
- product domicile and listing market;
- source country of income;
- withholding tax applied;
- distribution classification;
- tax form validity;
- whether the payment is reportable under CRS/FATCA or local statements;
- how the cashflow impacts portfolio performance and reporting.

## 3. Core concepts

| Concept | Meaning | Platform implication |
|---|---|---|
| Legal owner | Entity legally holding the asset/account | Account and custody record |
| Beneficial owner | Person/entity who economically benefits | Tax forms, CRS/FATCA, suitability, reporting |
| Tax residence | Jurisdiction where client is tax resident | Reportability and tax rules |
| Source country | Jurisdiction from which income is sourced | Withholding tax and treaty rules |
| Booking centre | Bank branch/entity where account is booked | Regulatory and statement obligations |
| Product domicile | Legal domicile of fund/issuer/entity | WHT, reporting and product classification |
| Listing venue | Exchange where product trades | Market data and sometimes tax indicators |
| Income type | Dividend, interest, coupon, rent, annuity, etc. | Tax classification and reporting bucket |
| Capital event | Sale, redemption, maturity, corporate action | Realised P&L and tax-lot handling |
| Withholding tax | Tax deducted at source before payment | Gross/net cash and tax-credit reporting |
| Tax lot | Specific acquisition lot used for cost basis | Realised gain/loss calculation |
| Reportable account | Account in scope for CRS/FATCA or local reporting | Regulatory files and controls |

## 4. Tax-aware reporting layers

A good platform separates tax-aware reporting into layers.

| Layer | Purpose | Examples |
|---|---|---|
| Product economics | What the investment did | Dividend, coupon, sale, redemption |
| Accounting ledger | What was booked | Cash debit/credit, security movement |
| Tax enrichment | How the event is classified | Interest, qualified dividend, return of capital |
| Withholding | Tax deducted at source | US dividend WHT, treaty rate, reclaimable amount |
| Tax lot | Cost and realised gain/loss | FIFO, average cost, specific ID |
| Regulatory reportability | Whether account/event must be reported | CRS, FATCA, local tax statement |
| Client reporting | What appears to client/advisor | Income schedule, tax voucher, realised P&L |
| Compliance audit | Evidence and lineage | Form validity, source files, calculation versions |

## 5. Important difference: accounting classification vs tax classification

A product cashflow can have different meanings depending on lens.

Example: fund distribution.

| Lens | Possible classification |
|---|---|
| Client cashflow | Distribution received |
| Performance | Income, unless reinvested internally |
| Accounting | Cash income transaction |
| Tax | Dividend, interest, capital gain distribution, return of capital, exempt income, mixed distribution |
| Advisory | Yield/income feature |
| Mandate | Income objective / distribution requirement |
| Reporting | Income schedule, tax statement, holdings report |

Therefore, avoid a single field called `income_type` being used by every process. Use separate classifications:

- product event type;
- accounting transaction type;
- performance classification;
- tax income category;
- regulatory reportability category;
- client-reporting display category.

## 6. Reporting audiences

| Audience | Needs |
|---|---|
| Client | Clear summary of income, gains, tax withheld, charges and portfolio value |
| Advisor/RM | Explainable view of what happened and what needs attention |
| Portfolio manager | Tax-aware implementation, turnover and mandate impact |
| Operations | Reconciliation, exception handling and corrected bookings |
| Tax operations | Tax forms, withholding, reclaim and year-end statements |
| Compliance | Suitability, disclosure, CRS/FATCA and audit evidence |
| Regulator | Required reporting files and controls |
| Platform team | Data lineage, calculation logic and supportability |

## 7. Common reporting outputs

| Output | Purpose |
|---|---|
| Client statement | Holdings, transactions, income, valuations and performance |
| Income statement | Dividends, interest, coupons, distributions, tax withheld |
| Realised gain/loss report | Disposal proceeds, cost basis, gain/loss, tax lots |
| Tax voucher | Income and withholding details for tax filing/support |
| CRS report | Reportable account information under CRS |
| FATCA report | US-reportable account information under FATCA |
| Trust/family office report | Ownership hierarchy and beneficiary-level views |
| DPM mandate report | Objective, restrictions, turnover, realised activity |
| Product disclosure report | Complex product risk and structured payoff details |
| Regulatory exception report | Missing documentation, stale residency, invalid GIIN/TIN, etc. |

## 8. Product taxonomy from a tax/reporting perspective

| Product family | Main tax/reporting concerns |
|---|---|
| Cash/deposits | Interest, withholding, accrued interest, account balances |
| Money market funds | Distributions, NAV, low-risk but not deposits |
| Bonds | Coupon interest, accrued interest, amortisation/accretion, redemption |
| Equities | Dividends, withholding, sale proceeds, corporate actions |
| Funds/ETFs | Distribution components, equalisation, capital-gain distributions, look-through |
| Notes/structured products | Coupons, derivative-linked redemption, physical settlement, issuer risk |
| Derivatives | Premiums, margin, realised P&L, exercise/assignment, settlement |
| Private markets | Capital calls, distributions, return of capital, K-1-like reporting, NAV lag |
| REITs/real assets | Property income, capital return, WHT, leverage disclosures |
| Insurance/annuities | Premiums, surrender value, maturity/claim, annuity income |
| Loans/margin | Interest expense, collateral, borrowing, pledge reporting |
| Trusts/family office | Beneficial ownership, entity hierarchy, fiduciary reporting |

## 9. Key design principle for product platforms

Do not force every product into the same tax treatment. Force every product into a common **event and ledger framework**, then apply configurable tax/reporting interpretation.

Recommended flow:

```text
Product event
  -> transaction booking
  -> cash/security ledger
  -> tax-lot engine
  -> withholding engine
  -> reportability engine
  -> client/regulatory reporting
```

## 10. What good looks like

A mature tax-aware reporting platform should provide:

- gross, tax and net amount for every income event;
- source country, payment country and account booking centre;
- client tax residency and beneficial-owner evidence;
- documented tax classification for each event;
- accurate tax lots and realised gain/loss;
- correction/reversal handling;
- restated statement support;
- CRS/FATCA reportability controls;
- tax voucher generation;
- portfolio performance that does not confuse tax withholding with investment performance;
- explainable lineage from statement number back to transaction and source file.


# 03 — Private Credit, Direct Lending, Distressed, Mezzanine and Special Situations

## 1. What is private credit?

Private credit refers to non-bank, privately negotiated lending or credit investments. It may include direct lending to companies, mezzanine financing, distressed debt, special situations, asset-based lending, real estate debt, infrastructure debt and opportunistic credit.

Private credit has grown because many borrowers seek financing outside traditional bank lending and public bond markets. From a wealth-platform perspective, it combines bond-like income concepts with private-market lifecycle, illiquidity and valuation complexity.

Simple definition:

> Private credit is privately originated or privately held debt exposure where return comes from interest, fees, spread, repayment and sometimes restructuring or equity upside.

## 2. Private credit taxonomy

| Strategy | Description | Risk/return profile |
|---|---|---|
| Senior direct lending | Senior secured loans to private companies | Lower risk within private credit, floating-rate income |
| Unitranche lending | Single blended senior/junior loan | Higher yield, structural complexity |
| Mezzanine debt | Subordinated debt, often with warrants/equity kicker | Higher yield, higher loss risk |
| Distressed debt | Debt of stressed/defaulted companies | Event-driven, legal/restructuring risk |
| Special situations | Complex opportunistic credit | High dispersion and complexity |
| Asset-based lending | Loans backed by receivables, inventory, equipment, etc. | Collateral and monitoring intensity |
| Real estate debt | Senior/mezzanine property loans | Property and refinancing risk |
| Infrastructure debt | Loans to infrastructure projects/assets | Long-dated cashflow and regulatory risk |
| Venture debt | Debt to venture-backed companies | Sponsor/enterprise-value risk, warrants |
| NAV lending | Loans secured by private fund portfolios | Fund NAV, collateral and liquidity risk |
| Fund finance | Subscription lines/capital-call facilities | Investor call/collateral risk |

## 3. Core return components

Private credit returns may include:

| Component | Meaning |
|---|---|
| Base rate | SOFR/SORA/Euribor or other floating benchmark |
| Credit spread | Compensation for borrower credit risk |
| Original issue discount | Upfront discount increasing yield |
| Arrangement/upfront fees | Loan origination economics |
| Commitment fees | Paid on undrawn facility |
| Prepayment fees | Compensation if loan repaid early |
| Payment-in-kind interest | Interest added to principal instead of paid in cash |
| Equity kicker/warrants | Upside participation |
| Restructuring gain/loss | Recovery or loss in distressed situations |

## 4. Private credit versus public bonds

| Dimension | Public bond | Private credit |
|---|---|---|
| Market | Public/security market | Bilateral or club lending/private funds |
| Liquidity | Often tradable | Usually illiquid |
| Pricing | Market/evaluated price | Manager valuation/model/appraisal |
| Coupon | Fixed or floating | Often floating + spread + fees |
| Covenants | Varies; public bonds may be covenant-lite | Often stronger negotiated covenants but not always |
| Default handling | Public bond restructuring | Private workout/restructuring process |
| Transparency | Public filings/market data | Manager reporting and loan-level data may be limited |
| Position model | Nominal + accrued + price | Fund commitment/NAV or direct loan model |

## 5. Direct lending lifecycle

```text
Loan origination -> commitment/allocation -> funding -> interest accrual -> coupon/interest payment -> covenant monitoring -> amendment/refinancing/default -> repayment or recovery
```

For a private credit fund investor, the client usually does not hold each loan directly. The client holds a fund interest. However, analytics may need look-through exposure by borrower, sector, rating, seniority, geography and maturity.

## 6. Platform modelling approaches

### 6.1 Fund-of-loans model

Most wealth clients hold private credit via a fund or feeder.

| Layer | Model |
|---|---|
| Client position | Private credit fund interest |
| Commitment | Capital commitment and unfunded obligation |
| Valuation | Fund NAV, usually monthly/quarterly |
| Income | Distributions classified as income/return of capital/gain |
| Look-through | Loan exposure if available |

### 6.2 Direct loan model

For direct private loans:

| Field | Requirement |
|---|---|
| borrower_id | Company/project/asset borrower |
| facility_id | Loan/facility identifier |
| principal_outstanding | Current loan principal |
| committed_amount | Total facility commitment |
| drawn_amount | Drawn amount |
| undrawn_amount | Undrawn commitment |
| rate_type | Fixed/floating/PIK |
| reference_rate | SOFR/SORA/Euribor/etc. |
| spread | Loan spread |
| floor/cap | Rate floor/cap if any |
| maturity_date | Contractual maturity |
| amortisation_schedule | Repayment profile |
| seniority | Senior secured, second lien, mezzanine |
| collateral_type | Assets pledged |
| covenant_package | Key covenants |
| credit_status | Performing/watchlist/default/restructured |

## 7. Private credit transaction types

| Transaction type | Use |
|---|---|
| ALT_COMMITMENT | Commitment to private credit fund |
| ALT_CAPITAL_CALL | Fund call notice recorded |
| ALT_CONTRIBUTION | Cash paid to fund |
| CREDIT_FACILITY_DRAWDOWN | Direct loan funded |
| CREDIT_INTEREST_ACCRUAL | Interest accrued |
| CREDIT_INTEREST_PAYMENT | Interest received |
| CREDIT_PIK_INTEREST | Interest added to principal |
| CREDIT_PRINCIPAL_REPAYMENT | Principal repaid |
| CREDIT_AMORTISATION | Scheduled principal amortisation |
| CREDIT_PREPAYMENT | Early repayment |
| CREDIT_FEE_INCOME | Upfront/commitment/prepayment fees |
| CREDIT_DEFAULT_EVENT | Default status event |
| CREDIT_WRITEDOWN | Principal/NAV write-down |
| CREDIT_RESTRUCTURE | Terms amended/restructured |
| CREDIT_RECOVERY | Recovery payment after default |
| ALT_DISTRIBUTION_INCOME | Private credit fund income distribution |
| ALT_DISTRIBUTION_ROC | Return of capital distribution |
| ALT_DISTRIBUTION_GAIN | Gain distribution |

## 8. Accrual and income classification

Private credit income is often important in advisory and portfolio review.

| Item | Treatment |
|---|---|
| Cash interest | Investment income when earned/paid based on accounting policy |
| PIK interest | Adds to principal/NAV; may be reported as non-cash income depending on policy |
| Commitment fee | Fee income if client directly holds loan; fund-level if held via fund |
| OID accretion | Yield component; requires amortised-cost/effective-interest treatment if applicable |
| Distribution from fund | Must be classified using manager notice if possible: income, return of capital, gain |
| Withholding tax | Tax transaction reducing net cash received |

## 9. Credit risk dimensions

| Dimension | Example |
|---|---|
| Borrower default risk | Borrower cannot pay interest/principal |
| Seniority | First lien, second lien, mezzanine, unsecured |
| Collateral coverage | Loan-to-value, asset coverage |
| Covenant strength | Maintenance covenants, incurrence covenants |
| Sector risk | Cyclical sector borrower |
| Sponsor risk | PE-backed borrower sponsor quality |
| Refinancing risk | Borrower cannot refinance at maturity |
| Rate risk | Floating-rate burden rises for borrower |
| Liquidity risk | Position cannot be sold easily |
| Valuation risk | Manager estimates may lag deterioration |
| Concentration risk | Exposure to same borrower/sponsor/sector |

## 10. Private credit valuation

Private credit valuation may use:

| Method | Use |
|---|---|
| Amortised cost | Short-term/high-quality loans if appropriate |
| Discounted cashflow | Expected interest/principal discounted using credit spread |
| Market comparables | Public loan/bond spreads or similar trades |
| Broker/third-party valuation | Independent marks where available |
| Recovery model | Distressed/defaulted assets |
| Manager NAV | Fund-level valuation |

Key inputs:

- benchmark rates;
- spreads;
- borrower credit quality;
- seniority;
- collateral value;
- default probability;
- recovery rate;
- time to maturity;
- liquidity discount;
- covenant status;
- restructuring probability.

## 11. Private credit analytics

Useful reporting metrics:

| Metric | Meaning |
|---|---|
| Current yield | Current income / current value |
| Yield to maturity | Expected IRR to repayment/maturity |
| Spread over base rate | Credit premium |
| Weighted average spread | Portfolio yield driver |
| Weighted average maturity | Duration of loan book |
| Senior secured percentage | Quality/seniority indicator |
| Non-accrual percentage | Problem-loan indicator |
| Default rate | Credit deterioration indicator |
| Recovery rate | Loss mitigation indicator |
| LTV | Collateral leverage |
| DSCR / interest coverage | Borrower repayment capacity |
| Covenant breaches | Watchlist signal |
| NAV volatility | Valuation sensitivity |
| DPI/TVPI/IRR | Fund-level performance |

## 12. Advisory treatment

Private credit may be attractive to income-oriented HNW clients, but advisors must clearly explain:

- income is not the same as low risk;
- senior secured does not mean risk-free;
- floating-rate income may rise, but borrower stress may also rise;
- illiquidity is significant;
- valuation can be stale or manager-estimated;
- fund distributions may include return of capital, not only income;
- default and restructuring cycles can materially affect NAV;
- diversification across borrowers, managers, vintage years and strategies matters.

## 13. Platform design warning

Do not treat private credit fund distributions as ordinary coupon by default. A distribution notice may split the cash into:

```text
interest income + fee income + return of capital + realised gain + tax withholding
```

If the notice is unavailable, classify as `ALT_DISTRIBUTION_UNCLASSIFIED` first and allow enrichment/reclassification later. This prevents false income reporting and performance distortion.

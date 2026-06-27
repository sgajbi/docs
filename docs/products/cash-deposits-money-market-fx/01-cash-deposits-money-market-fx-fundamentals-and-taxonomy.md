# 01 — Cash, Deposits, Money Market and FX Fundamentals and Taxonomy

## 1. Why this product family matters

Cash, deposits, money market instruments and FX are foundational products in wealth management. They are not merely operational by-products. They directly affect:

- order funding;
- settlement risk;
- buying power;
- liquidity management;
- portfolio currency exposure;
- yield on idle balances;
- collateral and credit-line usage;
- advisory suitability;
- DPM mandate compliance;
- client reporting;
- performance calculation.

A portfolio can have excellent investment selection but poor cash management. Common issues include avoidable idle cash drag, unexpected settlement shortfalls, unhedged FX exposure, overdraft interest, missed deposit maturity reinvestment and incorrect performance treatment of cashflows.

## 2. Product taxonomy

| Family | Product examples | Main purpose | Platform representation |
|---|---|---|---|
| Cash account balances | Current account, savings account, settlement cash, call cash | Liquidity, settlement, spending, portfolio funding | Account/currency balance position |
| Term deposits | Fixed deposit, time deposit, notice deposit, foreign currency fixed deposit | Predictable yield for agreed tenor | Deposit contract / cash-like product position |
| Certificates of deposit | Negotiable CD, bank CD | Short-term bank funding instrument | Security or deposit instrument depending on market |
| Treasury bills | Government T-bills | Short-term government debt / liquidity | Discount security / bond-like money market instrument |
| Commercial paper | Corporate short-term debt | Short-term corporate funding | Discount security / money market instrument |
| Repos | Repurchase agreements | Secured short-term cash investment or financing | Collateralised money market transaction |
| Money market funds | Government MMF, prime MMF, short-duration liquidity fund | Pooled liquidity product | Fund position with NAV |
| FX spot | Immediate currency conversion with value date | Settlement, investment currency conversion | Two-leg cash transaction |
| FX forward | Future-dated currency exchange | Hedging or future settlement funding | Derivative/forward position + future cash legs |
| FX swap | Spot plus forward exchange | Funding/liquidity/roll of FX exposure | Linked spot/forward transactions |
| NDF | Non-deliverable forward | Hedging restricted/non-deliverable currencies | Cash-settled derivative position |

## 3. Cash versus deposit versus money market fund

These products can all be used for liquidity, but their legal and risk characteristics differ.

| Product | Legal/economic nature | Typical risk | Liquidity | Valuation |
|---|---|---|---|---|
| Cash balance | Bank deposit / client cash ledger | Bank/custodian risk, currency risk | Highest, subject to settlement/restrictions | Par in account currency |
| Term deposit | Deposit placed for fixed tenor | Bank credit risk, early break penalty, currency risk | Lower until maturity | Principal + accrued interest, often par if held to maturity |
| T-bill | Government short-term security | Sovereign/rate/liquidity risk | Often liquid but market-dependent | Discounted security price |
| Commercial paper | Corporate short-term debt | Issuer credit/liquidity risk | Market-dependent | Discounted price / amortised cost where appropriate |
| Money market fund | Fund investing in short-term instruments | Fund liquidity/NAV/credit/rate risk | Daily or near-daily, subject to rules | NAV per unit/share |

## 4. Cash is not always risk-free

Cash is often treated as low risk, but it has several risks:

| Risk | Explanation |
|---|---|
| Bank/custodian credit risk | Client depends on the bank/custodian where cash is held. |
| Deposit insurance limit risk | Protection may be capped and may not cover all currencies/products. |
| Inflation risk | Purchasing power may decline. |
| Reinvestment risk | Deposits mature and may need to be reinvested at lower rates. |
| FX risk | Foreign currency cash can move materially against reporting/base currency. |
| Opportunity cost | Excess idle cash can drag performance. |
| Settlement risk | Available cash may differ from ledger cash because of pending trades. |
| Restriction risk | Cash may be blocked, pledged, reserved, margin-held or subject to account restrictions. |
| Operational risk | Incorrect value date, wrong currency, stale FX rate or missed deposit maturity can cause errors. |

## 5. Key balance concepts

| Balance type | Meaning | Used for |
|---|---|---|
| Ledger balance | Book balance recorded in the cash account | Accounting and reporting |
| Settled balance | Cash available after completed settlements | Cash availability |
| Trade-date balance | Balance including trades booked on trade date | Portfolio analytics and TWR depending on policy |
| Value-date balance | Balance projected by settlement/value date | Settlement forecasting |
| Available balance | Cash that can be used after restrictions, holds and pending commitments | Orders and withdrawals |
| Blocked/reserved balance | Cash unavailable due to pledge, margin, order reservation, fees, tax or regulatory hold | Controls |
| Projected balance | Future cash based on pending settlements, coupons, maturities and withdrawals | Liquidity planning |
| Base-currency cash | Cash translated to reporting currency | Portfolio reporting |

## 6. Deposit product types

| Deposit type | Description | Typical platform-specific requirements |
|---|---|---|
| Call deposit | Cash available on demand, sometimes with tiered interest | Daily interest accrual, rate tiers |
| Savings/current account | Operational cash account | Payments, settlement, fees, account restrictions |
| Fixed deposit / time deposit | Amount placed for fixed tenor and rate | Maturity schedule, interest accrual, break penalty |
| Notice deposit | Withdrawal requires notice period | Notice date, earliest withdrawal date |
| Foreign currency fixed deposit | Deposit in non-base/non-local currency | FX exposure, currency eligibility, deposit-insurance treatment |
| Structured deposit | Deposit wrapper with derivative-linked payoff | Should be modelled as structured product/deposit, not simple cash |
| Sweep deposit | Automatic sweep from idle cash to yield product | Sweep rules, cutoff, eligibility, liquidity impact |

## 7. Money market instruments

Money market instruments are short-term debt or funding instruments. They can be used by portfolios for liquidity management, short-term yield or collateral-style exposure.

| Instrument | Issuer | Typical form | Risk profile |
|---|---|---|---|
| Treasury bill | Government | Discount security | Sovereign/rate/liquidity risk |
| Commercial paper | Corporation | Discount or interest-bearing | Corporate credit/liquidity risk |
| Certificate of deposit | Bank | Negotiable or non-negotiable | Bank credit/liquidity risk |
| Repo | Bank/dealer/financial institution | Collateralised borrowing/lending | Counterparty/collateral/liquidity risk |
| Short-term note | Sovereign/bank/corporate | Interest-bearing security | Issuer/rate/liquidity risk |

## 8. Money market funds

Money market funds are pooled funds that invest in high-quality short-term instruments. They are often used as cash-management products, but they are investment funds, not bank deposits.

Important distinctions:

| Topic | Treatment |
|---|---|
| Position unit | Fund units/shares |
| Valuation | NAV per unit/share |
| Income | Distribution or accumulating NAV depending on share class |
| Liquidity | Usually daily, but subject to fund rules and market conditions |
| Risk | NAV, liquidity, credit, rate and sponsor risk |
| Mandate treatment | Can be treated as liquidity bucket or short-term fixed income depending on rules |

## 9. FX product types

| FX product | Description | Common use |
|---|---|---|
| FX spot | Buy one currency and sell another for near-term value date | Settlement currency conversion |
| Same-day / tomorrow-next FX | Shorter value-date FX trades | Urgent funding / roll liquidity |
| FX forward | Agree today to exchange currencies on future date | Hedge future cashflows or foreign asset exposure |
| FX swap | Spot exchange plus reverse forward exchange | Funding roll / short-term liquidity |
| NDF | Cash-settled forward for restricted/non-deliverable currencies | Hedge emerging-market/restricted currency exposure |
| FX option | Right but not obligation to exchange FX at strike | Optional hedge / structured product building block |

FX options are covered in the derivatives pack. This pack focuses on spot, forward, swap and NDF from cash, settlement and portfolio perspective.

## 10. Wealth-management view

For wealth platforms, this product family should be understood through four lenses:

| Lens | Question |
|---|---|
| Liquidity | How much cash is actually available now and on future value dates? |
| Yield | Is idle cash earning appropriate return for risk and liquidity need? |
| Currency | What FX exposure does the portfolio have versus client base currency? |
| Funding | Can pending orders, fees, withdrawals, capital calls and loan obligations be met? |

## 11. Interview-ready explanation

Cash, deposits, money market products and FX form the liquidity layer of a wealth portfolio. Cash is not just one balance; it must be modelled by account, currency, value date and availability. Deposits add tenor, interest accrual and maturity lifecycle. Money market instruments and funds provide short-term yield but introduce issuer, NAV, liquidity and valuation considerations. FX connects all multi-currency holdings and drives both settlement funding and portfolio currency exposure. From a platform perspective, this product family requires strong balance modelling, transaction classification, value-date projections, interest accrual, FX translation, liquidity analytics, mandate checks and reporting controls.

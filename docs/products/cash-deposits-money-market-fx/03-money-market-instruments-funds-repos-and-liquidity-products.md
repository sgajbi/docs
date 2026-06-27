# 03 - Money Market Instruments, Funds, Repos and Liquidity Products

## 1. What "money market" means

The money market is the short-term funding and liquidity market. Instruments are typically short-dated, high-quality and used for cash management, liquidity reserves, collateral management and short-term yield.

In wealth management, money market products sit between cash and longer-term fixed income:

```text
Cash / call account
  -> term deposit
  -> treasury bill / commercial paper / certificate of deposit
  -> money market fund
  -> short-duration bond fund
  -> longer-duration bonds/funds
```

## 2. Product taxonomy

| Product | Typical issuer/counterparty | Structure | Client position type |
|---|---|---|---|
| Treasury bill | Government | Discount security | Security position |
| Commercial paper | Corporation | Short-term unsecured debt | Security position |
| Certificate of deposit | Bank | Bank short-term debt/deposit instrument | Security or deposit position |
| Repo | Dealer/bank | Collateralised cash lending/borrowing | Money market transaction with collateral |
| Reverse repo | Dealer/bank | Cash investor receives collateral | Secured cash investment |
| Money market fund | Fund manager | Fund investing in short-term instruments | Fund units/shares |
| Liquidity fund | Fund manager | Short-duration liquidity strategy | Fund units/shares |
| Cash sweep | Bank/platform | Automatic sweep into deposit/MMF | Operational rule + product position |

## 3. Treasury bills

A Treasury bill is a short-term government debt instrument usually issued at a discount and redeemed at par.

Example:

| Item | Value |
|---|---:|
| Face value | SGD 100,000 |
| Purchase price | SGD 98,750 |
| Maturity value | SGD 100,000 |
| Gain at maturity | SGD 1,250 |

### Platform treatment

| Aspect | Treatment |
|---|---|
| Position | Face/nominal amount plus units if needed |
| Cost | Purchase consideration |
| Income | Discount accretion or realised gain at maturity depending on accounting policy |
| Valuation | Market price / amortised cost / discounted cashflow depending on source and policy |
| Risk | Sovereign, interest-rate, liquidity, FX if foreign currency |

### Transaction types

| Lifecycle | Transaction type |
|---|---|
| Buy | MM_INSTRUMENT_BUY or SECURITY_BUY |
| Sell | MM_INSTRUMENT_SELL or SECURITY_SELL |
| Accretion | MM_DISCOUNT_ACCRETION, optional |
| Maturity | MM_MATURITY_REDEMPTION |
| Tax | TAX_WITHHOLDING, if applicable |

## 4. Commercial paper

Commercial paper is short-term unsecured corporate debt.

| Feature | Description |
|---|---|
| Issuer | Corporate or financial institution |
| Tenor | Usually short-term |
| Return | Discount or interest-bearing |
| Risk | Issuer credit and liquidity risk |
| Liquidity | Can be less liquid than government bills |

Commercial paper can be used for yield enhancement but should not be treated the same as insured bank cash.

## 5. Certificates of deposit

Certificates of deposit can be non-negotiable deposits or negotiable securities, depending on market and product structure.

| Type | Treatment |
|---|---|
| Non-negotiable CD / fixed deposit | Deposit contract |
| Negotiable CD | Security / money market instrument |
| Foreign currency CD | Currency exposure and bank issuer risk |

## 6. Repo and reverse repo

A repo is economically a collateralised financing transaction.

- In a **repo**, one party sells securities and agrees to repurchase them later.
- In a **reverse repo**, the cash provider lends cash and receives securities as collateral.

From client/advisory perspective, a reverse repo can look like a secured cash investment, but it still has risks:

| Risk | Explanation |
|---|---|
| Counterparty risk | Counterparty may default |
| Collateral risk | Collateral value can fall |
| Haircut risk | Collateral haircut may be insufficient |
| Liquidity risk | Collateral may not liquidate at expected value |
| Operational risk | Collateral substitution/margining errors |
| Legal risk | Enforceability and close-out terms matter |

## 7. Money market funds

Money market funds invest in short-term instruments and are often used as cash alternatives. They may be government, treasury, prime, institutional, retail, distributing or accumulating, depending on jurisdiction and fund structure.

### Common types

| Type | Typical holdings | Typical risk |
|---|---|---|
| Government MMF | Government securities, repos backed by government securities, cash | Lower credit risk, still fund/liquidity risk |
| Treasury MMF | Treasury bills and government instruments | Sovereign/rate/liquidity risk |
| Prime MMF | Bank CDs, commercial paper, repos, short-term corporate instruments | Higher credit/liquidity risk |
| Tax-exempt MMF | Municipal or tax-exempt instruments | Municipal credit/liquidity risk |
| Institutional MMF | Institutional investors, may have floating NAV depending on rules | NAV/liquidity/redemption risk |
| Retail MMF | Retail investors, jurisdiction-specific rules | NAV/liquidity risk |

## 8. Stable NAV versus floating NAV

| NAV type | Meaning | Platform impact |
|---|---|---|
| Stable / constant NAV | Fund seeks to maintain stable unit value, e.g. 1.00 | Position value often looks cash-like, but risk remains |
| Floating NAV | NAV changes with market value | Valuation and P&L behave like fund/security |
| Accumulating NAV | Income retained in NAV | Income appears through price/NAV growth |
| Distributing NAV | Income paid as cash/distribution | Income transaction required |

Do not assume a money market fund is the same as cash. A fund can impose rules, fees, gates or liquidity measures depending on jurisdiction and fund documents.

## 9. MMF lifecycle

| Stage | Event | Transaction type |
|---|---|---|
| Order | Subscription placed | Order only |
| Subscription settlement | Units allocated | FUND_SUBSCRIPTION / MMF_SUBSCRIPTION |
| NAV update | Daily NAV received | Valuation record, no transaction |
| Distribution | Income paid | FUND_DISTRIBUTION / MMF_DISTRIBUTION |
| Reinvestment | Distribution reinvested | FUND_DISTRIBUTION_REINVESTMENT |
| Redemption order | Redemption placed | Order only |
| Redemption settlement | Units reduced, cash received | FUND_REDEMPTION / MMF_REDEMPTION |
| Liquidity fee | Fee applied on redemption | FUND_LIQUIDITY_FEE |
| Gate / suspension | Redemptions delayed | Lifecycle event, no transaction until settled |

## 10. MMF position model

| Field | Description |
|---|---|
| instrument_id | Fund/share-class ID |
| fund_type | Money market / liquidity fund |
| share_class | Accumulating/distributing, currency, hedged/unhedged |
| units | Units/shares held |
| nav | Latest NAV |
| nav_date | NAV valuation date |
| market_value | Units x NAV |
| dealing_frequency | Daily/weekly/etc. |
| settlement_cycle | T+0/T+1/T+2 depending on product |
| cutoff_time | Subscription/redemption cutoff |
| liquidity_bucket | Cash equivalent / liquidity / short-term fixed income |
| distribution_policy | Accumulating/distributing |
| risk_rating | Internal/advisory risk rating |

## 11. Fund versus instrument treatment

Money market funds are often used as cash equivalents, but in a platform they should remain fund positions.

| Topic | Cash balance | Money market fund |
|---|---|---|
| Legal form | Cash/deposit with bank/custodian | Fund units/shares |
| Valuation | Par in currency | NAV x units |
| Income | Interest | Distribution or NAV accretion |
| Settlement | Immediate/settled cash | Fund dealing cycle |
| Liquidity | Subject to cash restrictions | Subject to fund liquidity and redemption terms |
| Risk | Bank/currency/inflation | NAV/credit/liquidity/fund risk |
| Reporting | Cash bucket | Cash equivalent or fund/short-term fixed income bucket depending on policy |

## 12. Sweep products

A cash sweep automatically moves idle cash into a yield product.

Common patterns:

| Sweep type | Example |
|---|---|
| Bank deposit sweep | Idle cash swept into overnight deposit |
| MMF sweep | Idle cash automatically subscribed into MMF |
| Multi-bank sweep | Cash spread across banks to manage deposit exposure |
| Currency-specific sweep | USD cash swept into USD liquidity fund |

Platform requirements:

- sweep eligibility;
- minimum cash retention threshold;
- sweep cutoff time;
- redemption timing for future buys;
- order funding logic;
- reversal/correction handling;
- client reporting clarity;
- performance classification.

## 13. Liquidity buckets

Advisory and mandate systems often classify liquidity into buckets:

| Bucket | Example products | Typical use |
|---|---|---|
| Immediate liquidity | Cash / call account | Payments, near-term withdrawals |
| Near-cash | T+0/T+1 MMF, overnight deposit | Short-term reserve |
| Short-term liquidity | T-bills, short CDs, short-duration funds | Yield enhancement with moderate liquidity |
| Restricted liquidity | Notice deposits, gated funds, pledged cash | Not available for normal use |

## 14. Risks and controls

| Risk | Control |
|---|---|
| MMF treated as cash for settlement | Settlement engine should check redemption cycle before funding trades |
| Stable NAV assumed guaranteed | Risk disclosure and NAV stress scenarios |
| Stale NAV | NAV-date validation and tolerance checks |
| Wrong share class | Share-class mapping control |
| Incorrect income treatment | Distribution/accrual policy controls |
| Liquidity gate ignored | Lifecycle event and order blocking |
| Sweep creates negative cash | Sweep and order reservation integration |
| Commercial paper over-concentration | Issuer and credit concentration limits |
| Repo collateral not modelled | Collateral and haircut data model |

## 15. Reporting treatment

Client reports should avoid simply labelling all liquidity products as "cash". A better approach is:

| Reporting category | Includes |
|---|---|
| Cash | Actual bank/custody cash balances |
| Deposits | Term deposits, notice deposits, CDs where deposit-like |
| Money market instruments | T-bills, CP, negotiable CDs, repos |
| Money market funds | MMF fund positions |
| Cash equivalents | Aggregated analytical bucket with disclosure |
| Restricted liquidity | Pledged, blocked, notice, gated or margin cash/products |

## 16. Interview-ready explanation

Money market products are short-term liquidity and yield instruments. They are often used as cash alternatives, but they should not be blindly treated as cash. T-bills, CDs, commercial paper and repos are short-term instruments with issuer, rate, liquidity and collateral risk. Money market funds are fund positions valued by NAV and subject to fund rules. From a platform perspective, the key is to distinguish accounting position type, liquidity availability, valuation method, income treatment, settlement cycle and reporting classification.

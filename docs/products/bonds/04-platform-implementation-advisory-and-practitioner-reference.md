# 04 — Platform Implementation, Advisory, and Practitioner Reference

## 1. Platform Capability Checklist

A wealth-management platform supporting bonds should cover the full chain from instrument setup to client reporting.

| Capability | Why It Matters |
|---|---|
| Product master | Store issuer, coupon, maturity, day count, seniority, currency, rating, schedules. |
| Price and valuation | Calculate market value, accrued interest, yield, duration, and P&L. |
| Transaction processing | Buy, sell, coupon, redemption, call, put, conversion, default, recovery, fees, taxes. |
| Position keeping | Maintain nominal/current face, cost, accrued, market value, realized/unrealized P&L. |
| Corporate actions | Process calls, maturities, coupons, amortisation, tender offers, exchanges, defaults. |
| Risk analytics | Duration, DV01, spread, rating, issuer, sector, country, currency, liquidity, concentration. |
| Performance | Treat coupons, accruals, sales, redemptions, FX, defaults, and recoveries correctly. |
| Advisory / suitability | Check risk profile, complexity, concentration, tenor, liquidity, currency, credit quality. |
| Reporting | Show income, maturity ladder, yield, duration, ratings, issuer exposure, cashflow projections. |
| Reconciliation | Reconcile positions, transactions, coupon payments, prices, accruals, and corporate actions. |
| Auditability | Track source, timestamp, manual overrides, corrections, and event lineage. |

---

## 2. Minimum Product Master Requirements

For a production-grade bond platform, every bond should have the following minimum data before it is considered operationally usable.

| Area | Required Data |
|---|---|
| Identification | ISIN/CUSIP/local ID, instrument ID, issuer, currency, security type. |
| Contract | issue date, maturity date or perpetual flag, coupon type, coupon rate/formula, frequency. |
| Cashflow rules | day-count convention, business-day convention, payment calendar, ex-coupon rules. |
| Principal | face value, denomination, minimum tradable amount, redemption price. |
| Credit | issuer, guarantor if any, seniority, secured/unsecured, rating, country of risk, sector. |
| Lifecycle | call/put/amortisation/conversion/default flags and schedules if applicable. |
| Valuation | price source, quote type, clean/dirty convention, stale price policy. |
| Tax | withholding/tax status where relevant. |
| Suitability | product complexity, risk classification, target client eligibility, jurisdiction restrictions. |

---

## 3. Product Variant Implementation Notes

| Bond Type | Special Platform Requirement |
|---|---|
| Fixed-rate bond | Coupon schedule, accrued interest, maturity redemption. |
| Floating-rate bond | Rate fixing, benchmark fallback, reset calendar, spread, cap/floor. |
| Zero-coupon bond | Discount accretion, no coupon schedule, maturity pull-to-par. |
| Callable bond | Call schedule, yield-to-call, yield-to-worst, call announcement processing. |
| Putable bond | Put election workflow and deadline. |
| Convertible bond | Conversion ratio, conversion price, equity delivery, cash-in-lieu, equity look-through. |
| Perpetual/hybrid | No maturity, call/extension risk, coupon deferral, regulatory capital treatment. |
| Subordinated / AT1 | Loss absorption, coupon cancellation, write-down/conversion triggers. |
| Inflation-linked | Inflation index, lag, index ratio, adjusted principal/coupon. |
| Amortising bond | Current factor/current face, principal schedule. |
| ABS/MBS | Pool factor, prepayment/default assumptions, tranche, waterfall. |
| High-yield bond | Default/recovery workflow, liquidity controls, credit monitoring. |
| Foreign-currency bond | FX valuation, local/base P&L, withholding tax/currency controls. |
| Green/social/sustainability bond | Use-of-proceeds label, issuer risk still remains. |

---

## 4. Advisory and Suitability View

Before recommending or approving a bond, the advisor/platform should check whether it fits the client.

| Suitability Area | Questions to Ask |
|---|---|
| Risk profile | Can the client accept interest-rate, credit, spread, liquidity, and FX risk? |
| Time horizon | Does the maturity or expected holding period match client needs? |
| Liquidity need | Can the client hold to maturity or tolerate secondary-market bid/ask spreads? |
| Credit quality | Is issuer rating and seniority suitable? |
| Concentration | Is the client overexposed to one issuer, sector, country, currency, or rating bucket? |
| Currency | Is the client comfortable with bond currency and FX movement? |
| Complexity | Does the client understand callable, perpetual, AT1, convertible, ABS/MBS, or high-yield risk? |
| Yield source | Is higher yield coming from credit, duration, currency, subordination, illiquidity, or optionality? |
| Tax | Are coupon withholding, capital gains, or tax-exempt features relevant? |
| Scenario downside | What happens if rates rise, spreads widen, issuer is downgraded, or bond is not called? |
| Portfolio role | Is the bond for income, stability, liquidity, tactical credit, duration, or currency exposure? |

Key advisory sentence:

```text
A bond is not automatically safe because it pays a coupon or has a maturity date.
The quality of the issuer, seniority, liquidity, duration, currency, and embedded features determine the risk.
```

---

## 5. How to Explain Bonds to Clients

Simple explanation:

> A bond is a loan made by investors to an issuer. The issuer promises to pay interest and repay principal at maturity, subject to the issuer's ability to pay and the specific terms of the bond.

More complete explanation:

> A bond can provide regular income and may be less volatile than equities, but its value can still move. If market interest rates rise, the price of existing fixed-rate bonds usually falls. If the issuer's credit quality worsens, the bond price may fall. If the bond is callable, the issuer may redeem it early when it is favourable to the issuer. If the bond is in a foreign currency, the client also takes FX risk.

For higher-yield bonds:

> The higher yield is compensation for taking additional risk. We need to understand whether that risk comes from weaker credit, longer maturity, lower seniority, call/extension risk, illiquidity, currency, or structural complexity.

---

## 6. Common Misconceptions

| Misconception | Correct Understanding |
|---|---|
| “Bonds are always safe.” | Bonds can lose value due to rates, credit, liquidity, FX, default, or structure. |
| “If I hold to maturity, I cannot lose.” | Holding to maturity helps avoid market-price loss only if issuer pays in full and currency/tax risks are acceptable. |
| “Coupon equals return.” | Return depends on purchase price, coupons, maturity/redemption value, reinvestment, FX, taxes, and default risk. |
| “High yield is better.” | High yield usually means higher risk. |
| “Investment grade means no default risk.” | Investment grade means lower risk, not zero risk. |
| “Callable bond yield to maturity is enough.” | Yield to call and yield to worst may be more relevant. |
| “Perpetuals will always be called.” | Issuers may extend instead of calling; extension risk can be material. |
| “Floating-rate bonds have no risk.” | They still have credit, spread, liquidity, reset, cap/floor, and issuer risk. |
| “Listed bond means liquid.” | Listing does not guarantee tight bid/ask or strong market depth. |
| “Green bond means safer.” | Green label does not remove issuer credit risk. |

---

## 7. Bond Portfolio Construction Concepts

### 7.1 Bond ladder

A ladder staggers maturities across time.

Example:

| Maturity Year | Allocation |
|---|---:|
| 2027 | 20% |
| 2028 | 20% |
| 2029 | 20% |
| 2030 | 20% |
| 2031 | 20% |

Benefits:

| Benefit | Explanation |
|---|---|
| Predictable cashflows | Regular maturities provide liquidity. |
| Reinvestment discipline | Maturing bonds can be reinvested. |
| Rate diversification | Avoids putting all capital at one yield point. |

### 7.2 Bullet portfolio

A bullet concentrates maturities around a target date.

Useful when matching a known liability, such as school fees, property payment, or future cash need.

### 7.3 Barbell portfolio

A barbell combines short-term and long-term bonds while holding less in the middle.

Used to balance liquidity and yield/duration exposure.

### 7.4 Credit barbell

A client may combine high-quality defensive bonds with a smaller allocation to high-yield or emerging-market bonds.

Useful only when concentration and downside are controlled.

---

## 8. Reporting Requirements

A strong client report should show more than market value.

| Report Section | Recommended Content |
|---|---|
| Holdings | Bond name, issuer, currency, nominal, price, market value, accrued interest. |
| Income | Coupon rate, next coupon date, accrued interest, income received YTD. |
| Maturity profile | Maturity ladder by year/month. |
| Credit quality | Rating distribution, issuer concentration, sector/country exposure. |
| Duration and yield | Weighted average yield, duration, DV01, yield-to-worst for callable bonds. |
| Currency | Bond currency exposure and base-currency value. |
| Realized/unrealized P&L | Price P&L, FX P&L, income, realized gains/losses. |
| Risk flags | High yield, unrated, subordinated, callable, perpetual, illiquid, stale price. |
| Corporate actions | Upcoming coupon, maturity, call, put, tender, conversion events. |
| Cashflow projection | Expected coupons and principal repayments. |

---

## 9. Risk Analytics Required for Bonds

| Analytic | Purpose |
|---|---|
| Market value | Portfolio exposure. |
| Accrued interest | Earned but unpaid income. |
| Yield to maturity | Expected yield if held to maturity and no default/call. |
| Yield to call | Return if called. |
| Yield to worst | Conservative yield for callable/putable bonds. |
| Duration | Interest-rate sensitivity. |
| DV01/PV01 | Currency amount sensitivity to 1 bp yield move. |
| Convexity | Non-linear price sensitivity. |
| Spread | Compensation over benchmark. |
| Rating distribution | Credit quality profile. |
| Issuer concentration | Single-name exposure. |
| Sector/country exposure | Macro/sector risk. |
| Liquidity score | Exit risk. |
| FX exposure | Currency risk. |
| Stale price flag | Valuation quality. |
| Watchlist/downgrade flag | Credit monitoring. |

---

## 10. Controls and Reconciliation

| Control | Description |
|---|---|
| Position reconciliation | Match nominal/current face with custodian. |
| Cash reconciliation | Match coupon/redemption cash with cash ledger. |
| Accrued interest reconciliation | Validate day-count and coupon schedule. |
| Price reconciliation | Compare market/vendor/custodian prices. |
| Corporate action reconciliation | Ensure calls, maturities, tenders, conversions, defaults are processed. |
| Rating feed reconciliation | Confirm latest ratings and effective dates. |
| Stale price control | Flag or escalate old prices. |
| Outlier control | Investigate abnormal price/yield movement. |
| Negative yield support | Allow valid negative yields. |
| Default status control | Stop or adjust accrual depending policy. |
| Manual override audit | Record user, reason, timestamp, approval. |
| Tax withholding validation | Match coupon gross/net/tax treatment. |
| FX rate control | Validate local-to-base valuation. |

---

## 11. QA and Regression Test Scenarios

### Core trade and position tests

| Scenario | Expected Result |
|---|---|
| Buy bond at par | Nominal increases, cash decreases, cost set to par. |
| Buy at discount | Cost below par, YTM above coupon. |
| Buy at premium | Cost above par, YTM below coupon. |
| Buy with accrued interest | Cash includes clean cost + accrued interest + fees. |
| Sell with accrued interest | Nominal decreases; accrued interest received; realized P&L correct. |
| Partial sale | Tax lot/cost basis consumed correctly. |
| Transfer in | Position created without market trade. |
| Transfer out | Position reduced without sale P&L unless policy requires. |

### Coupon and accrual tests

| Scenario | Expected Result |
|---|---|
| Fixed coupon paid | Income booked, cash increases, accrued resets. |
| Floating coupon fixing | Coupon rate determined from reference rate + spread. |
| Coupon with tax withholding | Gross income, tax, and net cash booked correctly. |
| Ex-coupon trade | Entitlement handled according to market convention. |
| Defaulted coupon | Accrual/payment suppressed or adjusted by policy. |

### Lifecycle tests

| Scenario | Expected Result |
|---|---|
| Maturity redemption | Nominal closes, principal cash posted. |
| Callable bond called at 102 | Position closes, cash = nominal × 102%. |
| Putable bond exercised | Position closes/reduces, cash posted at put price. |
| Amortising bond principal payment | Current face reduces, cash posted. |
| Convertible bond conversion | Bond closes, equity position created. |
| Tender accepted | Bond reduced and cash posted at tender price. |
| Exchange offer | Old bond removed, new bond added, cash adjustment if any. |
| Default write-down | Position value/cost adjusted, loss booked. |
| Recovery payment | Recovery cash or new security booked. |

### Valuation and risk tests

| Scenario | Expected Result |
|---|---|
| Clean price changes | Clean market value and unrealized P&L update. |
| Accrued interest grows daily | Dirty value increases from accrual if policy uses dirty MV. |
| Interest rates rise | Fixed-rate bond price falls in sensitivity model. |
| Duration higher for longer maturity | Risk analytics reflect higher rate sensitivity. |
| Callable bond has lower yield-to-worst | YTW uses earliest/worst call scenario. |
| Stale price | Stale flag set and valuation quality downgraded. |
| FX moves | Base value changes while local value may be unchanged. |
| Rating downgrade | Risk classification and alerts update. |

---

## 12. Implementation Anti-Patterns

| Anti-Pattern | Problem | Better Approach |
|---|---|---|
| Store only market value | Cannot process coupons, redemptions, or P&L correctly. | Store nominal/current face, price, accrued, cost. |
| Treat accrued interest as normal price P&L | Misstates income and performance. | Separate clean price, accrued interest, and income. |
| Ignore schedules | Miss coupons, calls, maturities, amortisation. | Maintain cashflow and lifecycle schedules. |
| Use one latest rating field only | No history or audit. | Store rating history with effective dates. |
| Assume all bonds mature at par | Callable, defaulted, amortising, convertibles, and hybrids differ. | Use redemption/call/conversion terms. |
| Treat all bonds as low risk | High-yield, AT1, perpetuals, subordinated, FX bonds can be risky. | Classify complexity and risk flags. |
| Use stale prices silently | Client reports become misleading. | Stale price flags and source quality controls. |
| Ignore tax | Net income and client reporting wrong. | Store gross coupon, withholding, net cash. |
| Mix base and local currency | P&L attribution becomes wrong. | Store local and base amounts separately. |

---

## 13. Glossary

| Term | Meaning |
|---|---|
| Accrued interest | Interest earned since last coupon date but not yet paid. |
| Ask price | Price at which dealer sells bond to investor. |
| Bid price | Price at which dealer buys bond from investor. |
| Callable bond | Bond issuer can redeem before maturity. |
| Clean price | Bond price excluding accrued interest. |
| Convexity | Curvature of price/yield relationship. |
| Coupon | Interest payment from issuer. |
| Credit spread | Extra yield over benchmark for credit/liquidity risk. |
| Current face | Remaining principal after amortisation/factor reductions. |
| Current yield | Annual coupon divided by current price. |
| Dirty price | Clean price plus accrued interest. |
| Duration | Bond price sensitivity to yield movement. |
| DV01 / PV01 | Value change for 1 bp yield move. |
| Face value / par | Principal amount used for coupon and redemption. |
| Floating-rate bond | Coupon resets based on reference rate plus spread. |
| High-yield bond | Lower-rated bond with higher credit risk. |
| Investment-grade bond | Higher-rated bond, generally lower credit risk. |
| Maturity | Date principal is due to be repaid. |
| Nominal amount | Face value held by investor. |
| OAS | Option-adjusted spread. |
| Perpetual bond | Bond with no fixed maturity date. |
| Putable bond | Investor can require issuer to redeem on specified terms. |
| Seniority | Priority of claim in issuer default. |
| Spread risk | Risk that credit spreads widen. |
| Yield to call | Yield assuming bond is called. |
| Yield to maturity | Discount rate matching price to maturity cashflows. |
| Yield to worst | Lowest yield across possible redemption outcomes. |
| Zero-coupon bond | Bond with no periodic coupon, issued/traded at discount. |

---

## 14. Practitioner Questions and Answers

### Q1. What is a bond?

A bond is a debt security where the investor lends money to an issuer. The issuer promises to pay coupons and repay principal at maturity, subject to the issuer's ability to pay and the bond's contractual terms.

### Q2. What is the difference between coupon and yield?

Coupon is the contractual interest rate paid on face value. Yield is the market-implied return based on price, coupon, maturity, redemption terms, and expected cashflows. A bond bought at a discount usually has yield above coupon; a bond bought at a premium usually has yield below coupon.

### Q3. Why do bond prices fall when interest rates rise?

Existing fixed coupons become less attractive when new bonds offer higher yields. To make the old bond competitive, its price falls. The reverse usually happens when market yields fall.

### Q4. How should bonds be modelled in a platform?

A bond should be modelled as a security position primarily by nominal/current face, with clean price, accrued interest, dirty market value, cost, yield, duration, rating, and lifecycle status. Coupon schedules, call/put schedules, amortisation, ratings, lifecycle events, and valuations should be separate data structures linked to the instrument.

### Q5. What are the main bond transaction types?

The main transaction types are subscription, secondary buy, secondary sell, coupon payment, accrued interest paid/received, maturity redemption, call redemption, put redemption, amortisation redemption, conversion, tender/exchange, default write-down, recovery payment, fees, tax withholding, transfer, and correction reversal.

### Q6. What is clean versus dirty price?

Clean price excludes accrued interest. Dirty price includes accrued interest. Bond trades are often quoted on clean price, but settlement cash equals clean consideration plus accrued interest, fees, and taxes.

### Q7. What is duration?

Duration measures the sensitivity of bond price to yield movements. A modified duration of 5 means the bond price will move roughly 5% in the opposite direction for a 1% yield move, ignoring convexity.

### Q8. What is yield to worst?

Yield to worst is the lowest yield across possible redemption scenarios, such as maturity and call dates. It is important for callable bonds because yield to maturity may overstate the return if the issuer can redeem early.

### Q9. What makes a high-yield bond risky?

High-yield bonds are issued by lower-credit-quality issuers. They offer higher yield to compensate for higher default risk, spread volatility, liquidity risk, downgrade risk, and potentially lower recovery in default.

### Q10. How are bonds different from structured notes?

A plain bond mainly depends on issuer credit, coupon, maturity, rates, spread, and seniority. A structured note is also an issuer obligation, but its payoff may depend on derivatives linked to equities, indices, FX, rates, credit events, barriers, or autocall features. Structured notes usually require more payoff and lifecycle modelling.

---

## 15. Senior Architect Summary

For enterprise wealth platforms, bonds should be treated as first-class fixed-income securities with strong instrument master data, clean lifecycle processing, precise transaction modelling, and robust valuation controls. The accounting position should be based on nominal/current face, while market value should be derived from clean price plus accrued interest, then converted into portfolio base currency. The platform should separate contract schedules, lifecycle events, and transactions. It must support simple bonds as well as callable, floating, zero-coupon, inflation-linked, amortising, convertible, perpetual, subordinated, and asset-backed instruments. Good bond support requires price source hierarchy, stale price control, rating history, accrual accuracy, corporate-action processing, yield/duration/spread analytics, concentration risk, and performance treatment that distinguishes income, price return, FX return, fees, taxes, defaults, and recoveries.

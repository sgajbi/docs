# 01 — Notes Fundamentals and Product Taxonomy

## 1. What Is a Note?

A **note** is a debt instrument. In simple terms, it is an IOU issued by a bank, company, government, or financial institution. The investor lends money to the issuer, and the issuer promises to repay according to agreed terms.

At the simplest level, a note can behave like a bond.

Example:

| Feature | Example |
|---|---:|
| Investment amount | USD 100,000 |
| Issuer | Bank X |
| Tenor | 2 years |
| Coupon | 5% p.a. |
| Redemption | 100% of principal at maturity |

Cashflow:

| Time | Investor Cashflow |
|---|---:|
| Initial purchase | -100,000 |
| End of year 1 | +5,000 coupon |
| End of year 2 | +5,000 coupon + 100,000 principal |

This is a **plain vanilla note**.

In private banking, however, when people say **note**, they often mean **structured note**.

---

## 2. Bond vs Note vs Deposit vs Structured Deposit

| Product | Simplified Meaning | Typical Client Perception | Key Difference |
|---|---|---|---|
| Bond | Tradable debt security, often longer-term | Fixed-income investment | Usually has a broader public bond-market context. |
| Note | Debt instrument, often medium-term or issued under a note programme | Fixed-income or structured product | In wealth management, often means structured note. |
| Deposit | Money placed with a bank | Cash/deposit product | May have deposit protection depending on jurisdiction and eligibility. |
| Structured deposit | Deposit-like product with derivative-linked return | Enhanced deposit | May have different regulatory treatment from notes. |
| Structured note | Debt product with derivative-linked payoff | Yield / market-linked product | Investor takes issuer risk and underlying/payoff risk. |
| ETN | Exchange-traded note | Listed product tracking an index/strategy | Exchange traded, but still unsecured issuer debt. |

The key point:

```text
A note is normally an issuer obligation.
A structured note is an issuer obligation whose payoff depends on a formula.
The formula references markets, assets, rates, currencies, or credit events.
```

---

## 3. What a Structured Note Is Combined With

A structured note usually combines several economic components.

| Component | Purpose | Client-Facing Result |
|---|---|---|
| Debt component | Provides issuer obligation and maturity structure | The security called the note |
| Derivative component | Creates market-linked payoff | Coupon, upside, downside, barrier, autocall, conversion, credit event, FX outcome |
| Issuer credit exposure | Investor depends on issuer solvency | Issuer risk |
| Lifecycle rules | Define what happens and when | Observation, coupon, call, maturity, settlement |

The derivative may be economically similar to:

| Embedded Exposure | Common Product Example |
|---|---|
| Long call option | Principal-protected upside note |
| Short put option | Equity-linked note / reverse convertible |
| Barrier option | Knock-in / knock-out note |
| Basket option | Worst-of note |
| Autocall option | Autocallable note / Phoenix note |
| FX option | Dual currency note |
| Credit default swap-like exposure | Credit-linked note |
| Rate option / swap exposure | Range accrual / CMS / rate-linked note |

The client normally does **not** own these embedded derivatives as separate accounting positions. They are part of the note contract.

---

## 4. Why Clients Buy Notes

Clients typically buy notes for one or more of these objectives:

| Objective | How Notes Help |
|---|---|
| Enhanced yield | Coupon may be higher than deposits or plain bonds. |
| Income generation | Notes may pay monthly, quarterly, or periodic coupons. |
| Market view expression | Client can express a view on equity, index, FX, rates, credit, or volatility. |
| Conditional protection | Some notes protect principal unless a barrier or event occurs. |
| Custom payoff | Payoff can be tailored to client view and risk appetite. |
| Access | Gives exposure to indices, baskets, markets, or credit views not easily held directly. |
| Portfolio construction | Can be used for yield enhancement, tactical exposure, or downside-buffer structures. |

Important mental model:

```text
The higher coupon is not free.
The client is usually being paid to accept a specific risk.
```

Examples of risks that fund the coupon:

| Source of Enhanced Coupon | Meaning |
|---|---|
| Equity option premium | Client accepts downside equity exposure. |
| FX option premium | Client may be repaid in another currency. |
| Credit spread | Client accepts issuer or reference-entity credit risk. |
| Volatility | Higher volatility increases option value and possible coupon. |
| Barrier risk | Client loses protection if barrier condition is triggered. |
| Worst-of risk | Weakest asset in basket drives payoff. |
| Liquidity risk | Early exit may be difficult or expensive. |
| Tenor | Longer lock-in may increase coupon. |

---

## 5. Key Risks

| Risk | Explanation |
|---|---|
| Issuer credit risk | Investor depends on issuer's ability to pay. Even if the underlying performs well, issuer default can cause loss. |
| Underlying market risk | Equity, index, FX, rate, commodity, or credit reference may move adversely. |
| Principal loss risk | Many notes are not principal protected. Others are only conditionally protected. |
| Early exit / liquidity risk | Private-bank structured notes are often unlisted or illiquid. Secondary sale may depend on issuer bid. |
| Complexity risk | Payoff can be hard to understand, especially barriers, memory coupons, autocall, worst-of baskets. |
| Valuation risk | Mark-to-market may depend on issuer quote or model assumptions. |
| Concentration risk | Client may accumulate too much exposure to one issuer, underlying, currency, sector, or product type. |
| Correlation risk | Basket and worst-of products depend heavily on correlation between underlyings. |
| Volatility risk | Option value changes with volatility. |
| Interest-rate risk | Rate changes affect discounting and bond component value. |
| Dividend risk | Equity-linked note pricing depends on expected dividends. |
| FX risk | Currency-linked notes or foreign-currency notes expose client to currency movement. |
| Tax risk | Coupon, discount, redemption, conversion, and withholding treatment can vary by jurisdiction. |
| Operational risk | Termsheet errors, lifecycle event misses, stale prices, incorrect settlement handling. |

---

## 6. Common Product Variants

### 6.1 Plain Vanilla Note / Medium-Term Note

A simple debt note with fixed or floating coupon and principal redemption at maturity.

| Feature | Typical Value |
|---|---|
| Coupon | Fixed or floating |
| Principal protection | Contractual repayment subject to issuer credit |
| Underlying | None |
| Complexity | Low |
| Main risk | Issuer default, rates, liquidity |

Used when the investor wants fixed-income-like exposure to an issuer.

---

### 6.2 Principal-Protected Note

A principal-protected note aims to return some or all principal at maturity while offering upside linked to an asset or index.

| Feature | Example |
|---|---|
| Principal protection | 100% at maturity |
| Underlying | S&P 500 |
| Participation | 70% of upside |
| Coupon | Often none |
| Main risk | Issuer risk, opportunity cost, early exit value |

Example payoff:

| S&P 500 Performance | Participation | Client Return Before Issuer Risk |
|---:|---:|---:|
| +40% | 70% | +28% |
| 0% | 70% | 0% |
| -30% | Principal protected | 0%, principal returned |

Important: principal protection is usually at maturity only and depends on issuer solvency.

---

### 6.3 Equity-Linked Note / Reverse Convertible

An equity-linked note links coupon and/or principal repayment to an equity or equity index. A reverse convertible usually pays a high coupon but exposes the client to downside in the underlying.

| Feature | Example |
|---|---|
| Underlying | Apple share |
| Coupon | 12% p.a. |
| Barrier | 70% of initial price |
| Settlement | Cash or physical shares |
| Main risk | Equity downside, physical delivery, issuer risk |

Example:

| Term | Value |
|---|---:|
| Notional | USD 100,000 |
| Initial Apple price | USD 200 |
| Barrier | USD 140 |
| Coupon | 12% p.a. |

Possible maturity outcomes:

| Final Apple Price | Outcome |
|---:|---|
| 220 | Full principal + coupon |
| 180 | Full principal + coupon, depending on terms |
| 150 | Full principal + coupon, if barrier not triggered or final barrier safe |
| 120 | Loss or physical delivery of Apple shares, depending on terms |

The client is effectively being paid a coupon for accepting downside equity risk.

---

### 6.4 Fixed Coupon Note

A fixed coupon note pays a fixed periodic coupon, often linked to one or more equities or indices. In many private-banking contexts, FCNs are economically similar to reverse convertibles.

| Feature | Description |
|---|---|
| Coupon | Fixed and often unconditional, but terms vary |
| Underlying | Single stock, basket, index, or worst-of basket |
| Downside | Conditional barrier or direct downside exposure |
| Settlement | Cash or physical |
| Common use | Yield enhancement |

Advisory warning:

```text
The product may feel like fixed income because it pays coupons,
but the risk can behave like equity risk.
```

---

### 6.5 Worst-Of Note

A worst-of note has multiple underlyings, but the payoff is driven by the worst-performing one.

Example basket:

| Underlying | Initial Price | Final Price | Performance |
|---|---:|---:|---:|
| Apple | 200 | 210 | +5% |
| Microsoft | 400 | 380 | -5% |
| Nvidia | 1,000 | 600 | -40% |

Worst performer is Nvidia. The note payoff is driven by Nvidia even though the other names performed better.

Worst-of notes can offer higher coupons because the client accepts:

- Downside risk on multiple names
- Correlation risk
- Weakest-name risk
- Concentration risk if repeated across portfolios

---

### 6.6 Autocallable Note / Phoenix Note

An autocallable note can redeem early if the underlying meets a condition on an observation date.

Typical features:

| Feature | Example |
|---|---|
| Tenor | 3 years |
| Observation | Quarterly |
| Autocall level | 100% of initial index |
| Coupon | 8% p.a. |
| Coupon barrier | 70% |
| Knock-in barrier | 60% |
| Memory coupon | Yes/no |

Possible outcomes:

| Market Scenario | Outcome |
|---|---|
| Market rises early | Note autocalled; client receives principal + coupon. |
| Market moves sideways | Note may continue paying coupon. |
| Market falls moderately | Coupon may stop or accrue as memory; no autocall. |
| Market falls heavily | Knock-in/downside loss may apply at maturity. |

Autocallables are path-dependent because observation dates matter.

---

### 6.7 Dual Currency Note / Dual Currency Investment

A dual currency note gives enhanced yield, but the investor may be repaid in another currency.

| Feature | Example |
|---|---|
| Investment currency | USD |
| Alternate currency | SGD |
| Tenor | 1 month |
| Conversion rate | Pre-agreed USD/SGD level |
| Coupon | Higher than normal deposit |

At maturity:

| FX Outcome | Investor Receives |
|---|---|
| FX remains favorable | Principal + interest in original currency |
| FX crosses conversion level | Principal + interest converted into alternate currency |

The client is economically selling an FX option.

---

### 6.8 Credit-Linked Note

A credit-linked note pays enhanced coupon in exchange for taking credit risk on a reference entity.

| Feature | Example |
|---|---|
| Issuer | Bank X |
| Reference entity | Company ABC |
| Coupon | 9% p.a. |
| Credit event | Bankruptcy, failure to pay, restructuring |
| Recovery | Cash or physical settlement based on terms |

Two different credit risks must be separated:

| Risk | Meaning |
|---|---|
| Issuer credit risk | Bank X may default on the note. |
| Reference credit risk | Company ABC may suffer a credit event. |

A CLN is economically similar to the investor selling credit protection.

---

### 6.9 Interest-Rate-Linked Note

A rate-linked note pays coupon based on interest-rate behaviour.

| Type | Example Exposure |
|---|---|
| Floating-rate note | SOFR + spread |
| Capped/floored note | Floating coupon with cap/floor |
| CMS note | Constant maturity swap rate |
| Range accrual note | Coupon accrues only when rate stays within range |

Valuation depends on yield curves, forward rates, volatility, caps/floors, and issuer credit.

---

### 6.10 Exchange-Traded Note

An exchange-traded note is a listed unsecured debt obligation whose return tracks an index, benchmark, commodity, volatility strategy, or other reference asset.

| Feature | ETN |
|---|---|
| Listed | Usually yes |
| Trading | Exchange traded |
| Issuer risk | Yes |
| Underlying ownership | Usually no actual ownership of reference assets |
| Valuation | Exchange price and indicative value |

ETNs trade more like listed securities, but legally they remain issuer debt.

---

## 7. Are Notes Listed?

There are three practical cases.

| Case | Listed? | Liquidity | Valuation Source |
|---|---|---|---|
| Private-bank structured note | Usually unlisted / OTC | Usually issuer bid or arranged secondary market | Issuer quote, valuation vendor, internal model |
| Listed structured note/certificate | Listed | May still be thinly traded | Exchange price or issuer market-making quote |
| ETN | Exchange listed | Usually more tradable, but not risk-free | Exchange price + indicative value |

Important point:

```text
Listing does not automatically mean deep liquidity.
Unlisted does not mean impossible to value, but valuation depends on issuer/vendor/model inputs.
```

---

## 8. Advisory and Suitability View

Before recommending or approving a note, the advisor and platform should consider:

| Area | Key Question |
|---|---|
| Client risk profile | Can the client accept market, issuer, liquidity, and complexity risk? |
| Product knowledge | Does the client understand the payoff and worst-case scenario? |
| Liquidity need | Can the client hold until maturity? |
| Concentration | Is exposure too high to issuer, underlying, sector, currency, or product type? |
| Currency | Is the investment and settlement currency suitable? |
| Time horizon | Does tenor match client objective? |
| Downside scenario | Can client tolerate knock-in, conversion, write-down, or principal loss? |
| Issuer risk | Is client comfortable with issuer credit quality? |
| Portfolio fit | Does it improve or worsen diversification? |
| Disclosure | Are terms, risks, fees, and valuation treatment transparent? |

Suitability is not only about expected return. It is about whether the risk, complexity, liquidity, tenor, currency, and downside scenario fit the client.

---

## 9. Interview-Ready Explanation

> Notes are debt instruments issued by banks, companies, or financial institutions. In private banking, the term often refers to structured notes, where coupon or principal repayment depends on an underlying such as equities, indices, FX, rates, credit, or commodities. A structured note combines a debt component with embedded derivatives. The client is exposed to both issuer credit risk and the risk of the underlying payoff formula. Notes are often used for enhanced yield or market-view expression, but higher coupon usually means the client is accepting risks such as barrier risk, equity downside, FX conversion, credit event risk, liquidity risk, or worst-of basket risk. From a platform perspective, the client should hold one note position, while the embedded derivative features should be modelled as product terms, lifecycle rules, valuation inputs, and look-through risk exposures.

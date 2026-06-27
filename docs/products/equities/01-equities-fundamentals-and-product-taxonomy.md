# 01 - Equities Fundamentals and Product Taxonomy

## 1. What is an equity?

An equity represents ownership interest in a company or equity-like entity. When a client buys ordinary shares, the client becomes a shareholder. The shareholder participates in the company's economic performance through price appreciation, dividends and corporate actions.

In simple terms:

```text
Equity = ownership interest in a company
Bond = debt claim against an issuer
Fund = units in a pooled investment vehicle
Note = issuer debt instrument, often with embedded derivative payoff
Derivative = contract whose value depends on an underlying
```

A shareholder is different from a creditor. A bondholder expects contractual coupon and principal repayment. A shareholder usually has no fixed maturity, no guaranteed return and no guaranteed repayment of principal. The shareholder is a residual claimant: the upside can be high, but the downside can be the full investment.

## 2. Capital-structure ranking

Equity sits at the bottom of the issuer's capital structure. This matters for risk, valuation and suitability.

| Rank in claim hierarchy | Example instrument | Typical implication |
|---|---|---|
| Senior secured debt | Secured bonds / loans | Highest claim on specified collateral. |
| Senior unsecured debt | Senior bonds | Paid before subordinated debt and equity. |
| Subordinated debt | Tier 2 bonds, junior debt | Higher risk than senior debt. |
| Preferred equity / AT1-like hybrid capital | Preferred shares, preference shares, bank capital securities | Above common equity, below debt; income may be discretionary or deferrable. |
| Common equity | Ordinary shares | Residual claim; highest upside but highest loss absorption. |

The further down the structure, the more the investor participates in growth, but also the more the investor absorbs losses.

## 3. Core characteristics of equities

| Characteristic | Meaning |
|---|---|
| Ownership | Investor owns a share of the issuer or equity vehicle. |
| No contractual maturity | Ordinary shares usually do not mature. |
| Variable return | Return depends on market price movement, dividends and FX. |
| Voting rights | Common shareholders may vote on board appointments and major decisions, subject to share class rules. |
| Dividend uncertainty | Common dividends are discretionary and may be reduced, suspended or increased. |
| Residual claim | In insolvency, common shareholders rank below creditors and preferred shareholders. |
| Exchange trading | Listed equities trade on stock exchanges; some shares trade OTC or are privately held. |
| Corporate actions | Events can change quantity, cost basis, cash, tax, instrument identity and entitlements. |
| Market risk | Share price can be volatile and can fall to zero. |
| Liquidity risk | Large-cap shares are usually liquid, but small-cap, suspended, restricted or delisted shares may be hard to sell. |

## 4. Equity return components

Total equity return normally has three components:

```text
Total Return = Price Return + Dividend Income + FX Return
```

| Component | Explanation |
|---|---|
| Price return | Gain or loss from the movement in share price. |
| Dividend income | Cash or stock distribution from company profits, reserves or capital. |
| FX return | Gain or loss when security currency differs from portfolio base currency. |

Example:

| Item | Value |
|---|---:|
| Purchase price | USD 100 |
| Final price | USD 115 |
| Dividend received | USD 3 |
| Local-currency total return | 18% |

```text
Price return = (115 - 100) / 100 = 15%
Dividend return = 3 / 100 = 3%
Total return = 18%
```

If the portfolio base currency is SGD and the equity is priced in USD, the final portfolio return also depends on USD/SGD movement.

## 5. Equities versus other products

| Product | Client owns | Main return source | Main risk | Maturity? | Platform complexity |
|---|---|---|---|---|---|
| Equity | Shares in an issuer | Price appreciation and dividends | Company, market, liquidity and FX risk | Usually no | Corporate actions, cost basis, market data, tax, voting, settlement. |
| Bond | Debt claim | Coupon and principal repayment | Credit, duration, spread and liquidity risk | Yes, except perpetuals | Accrued interest, yield, amortisation, calls, maturity. |
| Fund | Units in pooled vehicle | NAV movement and distributions | Portfolio, liquidity, manager and fee risk | Usually open-ended or fund-life based | NAV, subscription/redemption cycles, share classes, fees, gates. |
| Structured note | Issuer debt instrument | Payoff formula and coupon | Issuer plus underlying payoff risk | Yes | Lifecycle events, barriers, observations, valuation model. |
| Option | Derivative contract | Option value movement | Leverage, volatility and time decay | Yes | Exercise, expiry, Greeks, margin. |

## 6. Main equity product types

### 6.1 Ordinary / common shares

Ordinary shares are the most common direct equity product. They generally provide ownership rights, potential voting rights and potential dividends.

| Feature | Typical treatment |
|---|---|
| Voting rights | Usually yes, but one-share-one-vote is not universal. |
| Dividend | Discretionary; not guaranteed. |
| Capital upside | Unlimited in theory. |
| Downside | Can lose entire investment. |
| Ranking | Below debt and preferred equity. |
| Platform position | Quantity of shares. |
| Price quote | Exchange price per share. |
| Income | Cash dividend, stock dividend, capital return or other distribution. |

Common shares are usually the default equity type for wealth-platform holdings.

### 6.2 Multiple share classes

Some companies issue multiple classes of common stock. Classes may differ by voting rights, economic rights, transfer restrictions or listing venue.

| Share class feature | Example implication |
|---|---|
| Different voting rights | Class A may have one vote, Class B may have ten votes, or vice versa. |
| Different dividend entitlement | One class may have a different dividend preference. |
| Different listing | Same issuer may have multiple share classes listed on different exchanges. |
| Convertibility | One class may convert into another. |
| Restricted transferability | Some shares may be subject to holding restrictions. |

Platform implication: share class must be treated as a separate instrument, not just as the same issuer. Concentration analytics may aggregate by issuer, but positions, prices, trading and corporate actions must use the correct class.

### 6.3 Preferred shares / preference shares

Preferred shares sit between common equity and debt. They often pay a fixed or floating dividend and generally rank above common shares but below debt.

| Feature | Possible treatment |
|---|---|
| Dividend | Fixed, floating, cumulative, non-cumulative, discretionary or deferrable depending on terms. |
| Voting rights | Often limited or none. |
| Ranking | Senior to common equity, junior to debt. |
| Callability | Issuer may redeem/call at specified dates/prices. |
| Convertibility | Some preferred shares can convert into common shares. |
| Perpetual nature | Many preferred shares have no maturity. |
| Valuation drivers | Credit quality, interest rates, dividend rate, call features, liquidity and issuer outlook. |

Preferred shares can behave like equity legally but like fixed income economically. A platform should not blindly process them as ordinary shares. They need extra terms similar to bonds or hybrids:

- dividend rate;
- day-count and frequency, if applicable;
- cumulative flag;
- callable flag;
- reset dates;
- conversion ratio;
- ranking;
- issuer optionality;
- regulatory capital classification, where relevant.

### 6.4 REITs

A Real Estate Investment Trust is an exchange-traded or listed vehicle that owns income-producing real estate or real-estate-related assets.

| Feature | Meaning |
|---|---|
| Security form | Usually listed equity-like unit. |
| Income | Regular distributions from rental income or asset sales. |
| Risk | Property market, interest rate, leverage, tenant, sector and liquidity risk. |
| Platform treatment | Often equity-like position, but may use fund/REIT classification for sector and income reporting. |
| Tax | Distribution components may have different tax treatment. |

REITs are important in Singapore and Asian wealth portfolios. They may appear as equities from trading and custody perspective but require careful asset-class mapping.

### 6.5 Depositary receipts: ADR and GDR

A depositary receipt represents shares of a foreign company held through a depositary bank. It allows investors to trade exposure to a foreign company in another market and often in another currency.

| Type | Meaning |
|---|---|
| ADR | American Depositary Receipt, commonly traded in the U.S. |
| GDR | Global Depositary Receipt, often listed in international markets such as London or Luxembourg. |
| Sponsored DR | Issuer participates in the depositary programme. |
| Unsponsored DR | Depositary bank creates the programme without full issuer sponsorship. |

Typical structure:

```text
Investor owns ADR/GDR
Depositary bank issues receipt
Custodian holds underlying local shares
Receipt ratio maps DR to underlying shares
```

Example:

| Item | Example |
|---|---|
| Underlying local share | Company ABC ordinary share in Hong Kong |
| ADR currency | USD |
| ADR ratio | 1 ADR = 2 ordinary shares |
| Dividend source | Local dividend converted into USD, less tax/fees |
| Risk | Company risk + local market risk + FX risk + depositary/custody mechanics |

Platform implications:

- ADR/GDR is a separate instrument from the local ordinary share.
- Maintain receipt ratio and underlying security mapping.
- Price, currency, market, corporate actions and dividends may differ from local shares.
- ADR fees may be charged through dividend deductions or separate cash fees.
- Voting and corporate action participation can be constrained by depositary rules.
- Look-through exposure should usually map to the underlying issuer and country.

### 6.6 Rights

A right gives existing shareholders the ability to buy additional shares, usually at a specified subscription price and within a limited period.

| Feature | Meaning |
|---|---|
| Origin | Created by a rights issue. |
| Holder choice | Subscribe, sell rights, let expire, or oversubscribe if allowed. |
| Life | Short-term. |
| Valuation | Depends on share price, subscription price and ratio. |
| Platform treatment | Separate temporary security/entitlement. |

Rights are equity-linked instruments, but they behave like short-lived options. They require corporate-action election processing and expiry controls.

### 6.7 Warrants

A warrant gives the holder the right to buy or sell an underlying security at a specified price before or at expiry. Warrants may be issuer warrants, covered warrants or company-issued equity warrants.

| Feature | Meaning |
|---|---|
| Underlying | Usually equity or index. |
| Direction | Call warrant or put warrant. |
| Leverage | Small price movement in underlying can create large warrant movement. |
| Expiry | Defined expiry date. |
| Platform treatment | Often separate security type, not ordinary equity. |
| Risk | Can expire worthless. |

Warrants should not be treated as simple shares. They need derivative-like attributes: strike, expiry, conversion ratio, exercise style, underlying, issuer and settlement method.

### 6.8 Convertible securities

Convertible securities can convert into shares. They are often closer to bonds or hybrids, but they connect to equities.

| Product | Description |
|---|---|
| Convertible bond | Debt instrument convertible into equity. |
| Mandatory convertible | Must convert at maturity or trigger. |
| Convertible preferred | Preferred share convertible into common equity. |

Platform treatment depends on legal form. The holding may be bond, preferred share or hybrid until conversion. After conversion, equity position is created and original instrument is reduced/closed.

### 6.9 Private equity shares

Private company shares are equity interests not listed on a public exchange.

| Feature | Meaning |
|---|---|
| Liquidity | Usually very limited. |
| Valuation | Appraisal, funding-round price, NAV, independent valuation or model. |
| Transfer | May require issuer approval. |
| Corporate actions | Manual or legal-document-driven. |
| Platform treatment | Private asset / unlisted equity with valuation frequency and lock-up terms. |

This pack focuses mostly on listed equities, but the modelling principles can be extended to private shares with additional restrictions and valuation controls.

## 7. Listed versus unlisted equities

| Attribute | Listed equity | Unlisted/private equity |
|---|---|---|
| Trading venue | Stock exchange or regulated market | Private transfer, OTC or company-administered process |
| Price source | Exchange close, real-time quote, vendor price | Appraisal, funding round, NAV, internal valuation |
| Liquidity | Often higher, but varies by market cap | Usually low |
| Corporate actions | Standard market/custodian feeds | Manual legal notices |
| Settlement | Market settlement cycle | Contractual/private settlement |
| Reporting | Daily valuation possible | Periodic valuation common |

## 8. Trading and settlement concepts

| Concept | Meaning |
|---|---|
| Trade date | Date the order is executed. |
| Settlement date | Date securities and cash legally exchange. |
| Booking date | Date transaction is recorded in the system. |
| Value date | Date cash/security movement becomes effective. |
| Ex-date | Date on or after which a buyer is not entitled to a declared distribution. |
| Record date | Date issuer checks shareholder records for entitlement. |
| Pay date | Date cash or securities are distributed. |
| Lot size | Minimum board lot or exchange trading unit. |
| Tick size | Minimum price movement. |
| Market identifier | Exchange or venue code, often MIC. |
| CSD/custodian | Infrastructure or custodian holding securities. |

Settlement cycles differ by market. For example, many U.S. securities moved to T+1 for applicable trades from May 28, 2024, while Singapore ready-market securities settle on T+2 under SGX rules. A platform should configure settlement by market, security type and trade venue rather than hard-code a universal cycle.

## 9. Price quotation and market data

Equities are usually quoted as price per share in the listing currency.

| Field | Example |
|---|---|
| Last traded price | 105.25 USD |
| Bid / ask | 105.20 / 105.30 |
| Official close | 104.90 |
| Previous close | 103.50 |
| Volume | 5,000,000 shares |
| Market cap | Shares outstanding x price |
| Adjusted close | Historical price adjusted for splits/dividends, often used for analytics. |

For portfolio valuation, use a clearly governed price hierarchy:

1. official exchange close;
2. vendor evaluated close;
3. custodian price;
4. last traded price;
5. stale price with control flag;
6. manual price with approval.

## 10. Key equity risks

| Risk | Explanation |
|---|---|
| Market risk | Broad market movements affect share prices. |
| Issuer/company risk | Business performance, management, profitability and balance sheet quality. |
| Sector risk | Exposure to technology, financials, real estate, energy, healthcare, etc. |
| Country risk | Political, regulatory, economic and capital-control risk. |
| Currency risk | Security or issuer revenue may be in a different currency. |
| Liquidity risk | Ability to sell without meaningful price impact. |
| Concentration risk | Overexposure to one stock, issuer, sector, country or currency. |
| Corporate-action risk | Misprocessing of events can cause incorrect positions, cash or tax. |
| Tax risk | Withholding tax, capital gains tax, stamp duty and jurisdiction-specific rules. |
| Settlement risk | Trade fails or delayed settlement. |
| Custody risk | Issues in holding chain, nominee accounts, depositary receipts or market restrictions. |
| Short-sale risk | Potentially unlimited loss, borrow recall, margin and regulatory risk. |

## 11. Advisory use cases

Equities are used for:

- long-term capital appreciation;
- dividend income;
- tactical market exposure;
- thematic investing;
- concentrated founder/executive holdings;
- employee stock awards;
- hedging via options or structured products;
- DPM portfolio construction;
- collateral portfolios in lending/buying-power engines;
- client reporting and performance attribution.

For advisory and DPM, the key question is not just "is this a good stock?" but:

```text
Does this equity exposure fit the client's risk profile, objective, time horizon, liquidity need, concentration limit, currency exposure and portfolio strategy?
```

## 12. Practitioner explanation

A strong explanation:

> An equity is an ownership interest in an issuer. Unlike a bond, it has no contractual maturity or guaranteed principal repayment. The investor participates in price appreciation, dividends and corporate actions, but also bears company-specific, market, liquidity and currency risk. From a wealth-platform perspective, simple buy/sell processing is only one part of equities. The hard parts are corporate actions, ex-date and record-date entitlement, cost-basis adjustments, multi-listing and ADR mapping, tax and fee treatment, settlement cycles, price sourcing, FX translation, risk concentration and performance classification.

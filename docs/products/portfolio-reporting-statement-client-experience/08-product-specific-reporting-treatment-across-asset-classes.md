# 08 - Product-Specific Reporting Treatment Across Asset Classes

## 1. Why product-specific reporting matters

A common report format is useful, but not every product behaves like a listed equity. Product-specific reporting avoids misleading clients and helps advisors explain risk and performance correctly.

## 2. Cash and deposits

Report:

- cash balance by currency;
- available, blocked, pledged and unsettled cash;
- deposit principal;
- deposit maturity;
- interest rate;
- accrued interest;
- renewal instruction;
- deposit insurance caveat where relevant;
- FX translation into report currency.

Common pitfalls:

- treating pledged cash as freely available;
- ignoring unsettled trades;
- mixing accrued and paid interest;
- hiding negative cash from unsettled activity.

## 3. Bonds and fixed income

Report:

- issuer;
- coupon;
- maturity;
- nominal amount;
- clean price and/or dirty price policy;
- accrued interest;
- yield to maturity/call where supported;
- rating;
- duration/DV01;
- realized/unrealized P&L;
- amortization/accretion where applicable;
- callability and next call date.

Explain:

- price/yield inverse relationship;
- duration sensitivity;
- credit spread movement;
- accrued interest versus market price;
- default and downgrade risk.

## 4. Equities

Report:

- shares/quantity;
- average cost;
- market price;
- dividends;
- corporate actions;
- realized/unrealized P&L;
- sector/country/currency;
- concentration;
- voting/rights events where relevant.

Special handling:

- stock splits;
- spin-offs;
- mergers;
- rights issues;
- tender offers;
- delisting;
- ADR ratio changes.

## 5. Funds and ETFs

Report:

- units;
- NAV/price;
- NAV date;
- share class;
- accumulation/distribution treatment;
- fund currency;
- TER/ongoing charges where available;
- redemption frequency;
- gates/suspensions;
- look-through exposure where available;
- trailing/performance return.

For ETFs, also show exchange price and possibly NAV premium/discount where available.

## 6. Structured notes and structured products

Report:

- issuer;
- product subtype;
- underlyings;
- coupon terms;
- maturity;
- barrier/strike/autocall level;
- next observation date;
- current underlying level versus barrier/strike;
- valuation source;
- issuer credit exposure;
- payoff scenario summary;
- coupon received and expected/unpaid if applicable.

Avoid presenting high coupon as fixed-income-like safety. Show downside conditions and issuer risk.

## 7. Derivatives

Report:

- contract type;
- notional;
- expiry;
- strike/forward rate/swap terms;
- market value;
- unrealized P&L;
- margin/collateral;
- Greeks where applicable;
- underlying exposure;
- settlement type;
- counterparty/clearing status.

Performance and exposure should use notional/sensitivity-aware measures, not only market value weight.

## 8. Private markets and alternatives

Report:

- commitment;
- funded amount;
- unfunded amount;
- remaining commitment;
- latest NAV/capital account date;
- capital calls;
- distributions;
- recallable distributions;
- IRR/TVPI/DPI/RVPI where available;
- vintage year;
- strategy;
- lock-up/liquidity terms;
- manager/entity.

Explain NAV lag clearly.

## 9. Real estate, REITs and infrastructure

For listed REITs:

- units/shares;
- market price;
- distribution yield;
- sector/property type;
- leverage/gearing where available;
- distribution history.

For private real estate/infrastructure funds:

- NAV date;
- commitment/funded/unfunded;
- distributions;
- appraisal basis;
- liquidity/lock-up;
- leverage at fund/vehicle level.

## 10. Commodities and precious metals

Report:

- quantity and unit: ounces, grams, barrels, contracts, shares;
- spot price or product price;
- custody model for physical/allocated/unallocated metals;
- storage/custody fees;
- futures roll exposure for futures-based products;
- collateral/margin for derivatives;
- FX exposure;
- inflation-hedge narrative where appropriate.

## 11. Loans, Lombard, margin and collateral

Report:

- outstanding loan principal;
- interest rate;
- accrued interest;
- next payment date;
- credit line limit;
- used exposure;
- collateral value;
- eligible collateral value;
- LTV;
- available headroom;
- margin call status;
- pledged asset list;
- cross-pledge relationships.

For advisory, show whether leverage is amplifying portfolio risk.

## 12. Insurance and annuities

Report:

- policy number;
- policy owner;
- insured life;
- beneficiary summary where permitted;
- sum assured;
- cash/account value;
- surrender value;
- premium status;
- rider benefits;
- loan against policy;
- annuity income schedule;
- valuation date.

Do not treat surrender value, death benefit and investment account value as interchangeable.

## 13. Trusts and family office structures

Report:

- legal owner;
- beneficial owner/beneficiary where permitted;
- account/entity hierarchy;
- trust/foundation/company structure;
- reporting group;
- access entitlement;
- mandate and authority rules;
- consolidated family view;
- entity-level and beneficiary-level reporting distinction.

## 14. Tax-aware reporting

Report:

- income classification;
- withholding tax;
- realized gains/losses;
- tax lots;
- corporate-action adjustments;
- tax document status;
- CRS/FATCA indicators where permitted;
- jurisdictional disclaimers.

Tax reporting should be configurable and should not hardcode a single tax regime into product logic.

## 15. Product reporting design matrix

| Product | Quantity basis | Valuation basis | Key warning |
|---|---|---|---|
| Equity | Shares | Market price | Corporate actions affect quantity/cost. |
| Bond | Nominal | Clean/dirty price + accrued | Price/yield and credit spread risk. |
| Fund | Units | NAV | NAV lag, gates, share-class terms. |
| Note | Nominal/units | Issuer/vendor/model price | Payoff and issuer risk. |
| Derivative | Contracts/notional | MTM/model price | Notional exposure and leverage. |
| Private fund | Commitment/capital account | Manager NAV | NAV lag and illiquidity. |
| Loan | Principal | Outstanding plus accrued | Collateral/LTV risk. |
| Insurance | Policy/account value | Insurer statement/model | Surrender and benefit values differ. |
| Cash | Currency balance | Par plus FX conversion | Available vs blocked/pledged cash. |

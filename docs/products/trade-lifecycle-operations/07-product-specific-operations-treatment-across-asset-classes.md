# 07 - Product-Specific Operations Treatment Across Asset Classes

## 1. Purpose

This file maps the trade-lifecycle operating model to each product family covered in the broader knowledge base.

The goal is to avoid generic transaction handling that breaks for non-equity products.

## 2. Equities, ETFs and listed REITs

### Lifecycle

```text
Order -> execution/fill -> trade capture -> allocation -> settlement -> custody position -> dividends/corporate actions -> reporting
```

### Key operational features

- exchange order types
- partial fills
- board lots/odd lots
- settlement cycle by market
- dividends
- splits
- rights issues
- mergers/spin-offs
- short-sale and restricted-stock controls
- custodian/CDP/nominee account handling

### Transaction types

- EQUITY_BUY
- EQUITY_SELL
- CASH_DIVIDEND
- STOCK_DIVIDEND
- STOCK_SPLIT
- REVERSE_SPLIT
- RIGHTS_ENTITLEMENT
- RIGHTS_EXERCISE
- TENDER_ACCEPTANCE
- MERGER_EXCHANGE_OUT/IN
- SPINOFF_IN
- CASH_IN_LIEU

### Position basis

- trade-date exposure for analytics
- settlement-date holding for custody
- available quantity for order validation
- pledged/blocked quantity for collateral

## 3. Bonds and fixed income

### Lifecycle

```text
RFQ/order -> execution -> trade capture with clean price/accrued -> confirmation -> settlement -> coupon/accrual -> corporate action/redemption -> reporting
```

### Key operational features

- nominal rather than share units
- clean vs dirty price
- accrued interest
- minimum denominations
- settlement currency
- coupon schedule
- callable/putable/maturity events
- amortisation/paydown
- default/restructuring
- ex-coupon rules

### Transaction types

- BOND_BUY
- BOND_SELL
- COUPON_PAYMENT
- ACCRUED_INTEREST_PAID/RECEIVED
- BOND_MATURITY_REDEMPTION
- PARTIAL_REDEMPTION
- CALL_REDEMPTION
- DEFAULT_WRITEDOWN
- RECOVERY_PAYMENT

### Position basis

- nominal amount
- amortised/current face
- accrued interest
- market value clean/dirty policy
- yield/duration analytics

## 4. Funds, unit trusts and mutual funds

### Lifecycle

```text
Subscription/redemption/switch order -> cut-off -> dealing date -> NAV date -> allocation/units -> settlement -> distribution/reinvestment -> reporting
```

### Key operational features

- forward pricing
- NAV date may differ from order date
- dealing cut-off
- settlement cycle by share class
- subscription by amount, redemption by units/amount/all
- swing pricing, dilution levy, entry/exit fee
- distribution versus accumulation class
- fund holidays and gates
- transfer agent confirmation

### Transaction types

- FUND_SUBSCRIPTION
- FUND_REDEMPTION
- FUND_SWITCH_OUT
- FUND_SWITCH_IN
- FUND_DISTRIBUTION
- FUND_REINVESTMENT
- FUND_FEE
- FUND_EQUALISATION
- FUND_TRANSFER_IN/OUT

### Position basis

- units
- NAV price
- NAV date
- pending units/cash
- distribution entitlement

## 5. Structured notes and structured products

### Lifecycle

```text
Subscription -> allocation/issue -> holding valuation -> observation events -> coupon/autocall/barrier/fixing -> redemption/physical delivery -> reporting
```

### Key operational features

- issuer allocation
- termsheet capture
- observation schedule
- barriers/autocall events
- conditional coupons
- physical settlement into underlying
- issuer valuation/secondary bid
- maturity/redemption event

### Transaction types

- NOTE_SUBSCRIPTION
- NOTE_SECONDARY_BUY/SELL
- NOTE_COUPON_PAYMENT
- NOTE_AUTOCALL_REDEMPTION
- NOTE_MATURITY_CASH_REDEMPTION
- NOTE_PHYSICAL_REDEMPTION_OUT
- NOTE_PHYSICAL_DELIVERY_IN
- NOTE_CREDIT_EVENT_WRITEDOWN

### Position basis

- note nominal/units
- issuer exposure
- underlying look-through exposure
- lifecycle status
- valuation source

## 6. Derivatives

### Options

Lifecycle:

```text
Order -> premium trade -> margin/control -> valuation/Greeks -> expiry/exercise/assignment/close-out -> settlement
```

Transactions:

- OPTION_BUY_TO_OPEN
- OPTION_SELL_TO_OPEN
- OPTION_BUY_TO_CLOSE
- OPTION_SELL_TO_CLOSE
- OPTION_PREMIUM
- OPTION_EXERCISE
- OPTION_ASSIGNMENT
- OPTION_EXPIRY
- MARGIN_POSTING
- MARGIN_RELEASE

### Futures

Lifecycle:

```text
Order -> execution -> clearing -> initial margin -> daily variation margin -> expiry/roll/delivery/cash settlement
```

Transactions:

- FUTURE_OPEN
- FUTURE_CLOSE
- VARIATION_MARGIN
- INITIAL_MARGIN_POSTING
- FUTURE_EXPIRY_SETTLEMENT
- FUTURE_ROLL

### Swaps/CDS

Lifecycle:

```text
Trade -> confirmation -> collateral/margin -> periodic resets/payments -> valuation -> termination/maturity/credit event
```

Transactions:

- SWAP_TRADE
- SWAP_INTEREST_PAYMENT
- SWAP_RESET
- COLLATERAL_POSTING
- COLLATERAL_RETURN
- SWAP_TERMINATION
- CDS_PREMIUM
- CDS_CREDIT_EVENT_SETTLEMENT

## 7. FX spot, forwards, swaps and NDFs

### Lifecycle

```text
FX order -> trade -> confirmation -> settlement instruction -> PVP/bilateral settlement -> cash booking -> revaluation/reporting
```

### Key operational features

- currency pair and direction
- near/far legs for swaps
- value date
- settlement calendars for both currencies
- PVP/CLS eligibility where applicable
- NDF fixing and cash settlement
- funding and overdraft controls

### Transaction types

- FX_SPOT_BUY_SELL
- FX_FORWARD_OPEN
- FX_FORWARD_SETTLEMENT
- FX_SWAP_NEAR_LEG
- FX_SWAP_FAR_LEG
- NDF_FIXING
- NDF_CASH_SETTLEMENT
- FX_REALIZED_GAIN_LOSS

## 8. Cash, deposits and money-market instruments

### Lifecycle

```text
Cash movement/deposit placement -> interest accrual -> maturity/rollover/withdrawal -> reporting
```

### Key operational features

- value date and ledger cash
- blocked/available cash
- interest accrual
- deposit maturity
- rollover instruction
- money-market instrument discount/yield
- cash sweep and MMF subscription/redemption

### Transaction types

- CASH_DEPOSIT
- CASH_WITHDRAWAL
- CASH_TRANSFER
- TERM_DEPOSIT_PLACEMENT
- TERM_DEPOSIT_INTEREST
- TERM_DEPOSIT_MATURITY
- MM_INSTRUMENT_BUY/SELL/MATURITY
- CASH_SWEEP_IN/OUT

## 9. Loans, Lombard, margin and collateral

### Lifecycle

```text
Credit line setup -> collateral pledge -> drawdown -> interest accrual -> repayment -> margin/LTV monitoring -> call/liquidation/release
```

### Key operational features

- pledged assets
- collateral eligibility
- LTV/haircut
- exposure and availability
- cross-pledging
- in-flight orders
- interest accrual
- margin call/liquidation

### Transaction types

- CREDIT_LINE_SETUP
- COLLATERAL_PLEDGE
- COLLATERAL_RELEASE
- LOAN_DRAWDOWN
- LOAN_REPAYMENT
- LOAN_INTEREST_ACCRUAL
- LOAN_INTEREST_PAYMENT
- MARGIN_CALL
- COLLATERAL_LIQUIDATION

## 10. Private markets and alternatives

### Lifecycle

```text
Subscription/commitment -> capital calls -> funded position/NAV updates -> distributions -> recallable capital -> transfer/secondary -> wind-down/liquidation
```

### Key operational features

- committed capital
- unfunded commitment
- capital call notices
- NAV lag
- distributions
- recallable distributions
- equalisation
- secondary transfer
- IRR/TVPI/DPI/RVPI

### Transaction types

- PRIVATE_MARKET_COMMITMENT
- CAPITAL_CALL
- FUNDED_CAPITAL
- DISTRIBUTION_INCOME
- DISTRIBUTION_RETURN_OF_CAPITAL
- RECALLABLE_DISTRIBUTION
- NAV_UPDATE
- SECONDARY_TRANSFER
- FINAL_LIQUIDATION

## 11. Insurance and annuities

### Lifecycle

```text
Application -> underwriting -> policy issue -> premium payments -> policy value updates -> benefits/claims/surrender/annuity payments -> reporting
```

### Key operational features

- policy status
- premium schedule
- cash/surrender value
- policy loan
- riders
- beneficiary
- ILP fund units
- annuity income
- claims processing

### Transaction types

- POLICY_PREMIUM
- POLICY_FEE
- ILP_FUND_ALLOCATION
- POLICY_VALUE_UPDATE
- POLICY_LOAN_DRAWDOWN
- POLICY_LOAN_REPAYMENT
- SURRENDER_PAYMENT
- CLAIM_PAYMENT
- ANNUITY_PAYMENT

## 12. Real estate, REITs and infrastructure

Listed REITs follow equity-like lifecycle. Private real estate/infrastructure funds follow private-market lifecycle.

Special concerns:

- distribution sustainability
- leverage metrics
- NAV/appraisal lag
- capital calls
- property-level look-through
- liquidity gates
- tax classification of distributions

## 13. Cross-product operating summary

| Product | Position unit | Price/value basis | Key lifecycle challenge |
|---|---|---|---|
| Equity | Shares | Market price | Corporate actions |
| Bond | Nominal | Clean/dirty price, yield | Accrued interest/redemption/default |
| Fund | Units | NAV | Dealing date/NAV lag/distributions |
| Note | Nominal/units | Issuer/model price | Observations/barriers/redemption |
| Option | Contracts | Premium/MTM | Exercise/assignment/expiry |
| Future | Contracts | Daily settlement | Margin and variation cashflows |
| FX | Currency notionals | FX rate/forward points | Value date and settlement risk |
| Private market | Commitment/funded NAV | Manager NAV | Capital calls/NAV lag |
| Loan | Outstanding principal | Amortised balance | LTV/collateral/margin calls |
| Insurance | Policy/account value | Surrender/account value | Premiums/claims/beneficiaries |

## 14. Design rule

Use common lifecycle objects but product-specific extensions. Do not force all products into an equity-style buy/sell/dividend model.

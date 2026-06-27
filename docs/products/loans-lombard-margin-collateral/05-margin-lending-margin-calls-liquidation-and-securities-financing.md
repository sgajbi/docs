# 05 — Margin Lending, Margin Calls, Liquidation and Securities Financing

## 1. Margin lending overview

Margin lending allows a client to borrow against securities in an account, often to buy additional securities. It increases purchasing power but also magnifies losses and may require the client to provide more collateral or repay debt when asset values fall.

Core variables:

| Variable | Meaning |
|---|---|
| Account equity | Market value of assets minus debit balance |
| Debit balance | Amount borrowed from broker/bank |
| Initial margin | Minimum equity required to open a position |
| Maintenance margin | Minimum equity required to keep position open |
| Excess liquidity | Buffer above maintenance requirement |
| Margin call | Demand to add funds/collateral or reduce positions |
| Liquidation | Forced sale/closeout if requirements are not met |

## 2. Margin account formula

```text
Account Equity = Market Value of Securities - Debit Balance
Equity Ratio = Account Equity / Market Value of Securities
```

Example:

| Item | Amount |
|---|---:|
| Securities market value | USD 200,000 |
| Debit balance | USD 80,000 |
| Account equity | USD 120,000 |
| Equity ratio | 60% |

If maintenance margin requirement is 30%, account is above requirement.

## 3. Price fall example

Initial position:

| Item | Amount |
|---|---:|
| Securities market value | 200,000 |
| Debit balance | 80,000 |
| Equity | 120,000 |
| Equity ratio | 60% |

If securities fall to USD 110,000:

| Item | Amount |
|---|---:|
| Securities market value | 110,000 |
| Debit balance | 80,000 |
| Equity | 30,000 |
| Equity ratio | 27.27% |

If maintenance requirement is 30%, this triggers a margin shortfall.

## 4. Margin call amount

One simple method:

```text
Required Equity = Maintenance Margin % × Securities Market Value
Margin Shortfall = Required Equity - Actual Equity
```

Using the example:

```text
Required Equity = 30% × 110,000 = 33,000
Actual Equity = 30,000
Shortfall = 3,000
```

The actual deposit required may be higher depending on house rules, concentration, settlement state and liquidation buffers.

## 5. Margin call lifecycle

```text
Market value falls / requirement rises
  → margin engine detects shortfall
  → margin call event generated
  → client/advisor notified
  → cure period begins if allowed
  → client cures by cash deposit, securities pledge, repayment or sale
  → if not cured, liquidation may occur
  → account returns to compliant state or escalates
```

## 6. Margin call event model

| Field | Meaning |
|---|---|
| margin_call_id | Unique call ID |
| account_id | Margin account |
| credit_line_id | Facility if linked |
| trigger_datetime | When breach detected |
| shortfall_amount | Amount needed |
| currency | Shortfall currency |
| margin_requirement | Required collateral/equity |
| actual_collateral/equity | Actual collateral/equity |
| cure_deadline | Deadline to cure |
| status | Open, cured, waived, escalated, liquidated |
| trigger_reason | Market fall, LTV change, rating downgrade, concentration change |
| notification_status | Sent/acknowledged |

## 7. Cure methods

| Cure method | Transaction / event |
|---|---|
| Deposit cash | CASH_DEPOSIT or TRANSFER_IN |
| Pledge additional asset | COLLATERAL_PLEDGE |
| Repay loan | LOAN_PRINCIPAL_REPAYMENT |
| Sell asset voluntarily | SECURITY_SELL + repayment/reserve release |
| Transfer collateral from another account | TRANSFER_IN + COLLATERAL_PLEDGE |
| Reduce derivative exposure | DERIVATIVE_CLOSEOUT |
| Bank waives/extends | MARGIN_CALL_WAIVER / EXTENSION event |

## 8. Forced liquidation

Forced liquidation means the bank/broker sells assets or closes positions to reduce exposure or meet margin requirements.

Important platform points:

- liquidation is a real trade transaction;
- it may happen without client instruction if agreement permits;
- sale proceeds reduce loan/debit balance or restore margin;
- realized P&L must be calculated;
- tax and fees may apply;
- reporting should clearly identify forced sale reason.

Transaction group example:

| Transaction | Effect |
|---|---|
| EQUITY_SELL_FOR_MARGIN_CALL | Sell pledged equity |
| CASH_PROCEEDS | Cash received |
| LOAN_PRINCIPAL_REPAYMENT | Cash used to reduce debit |
| FEE_PAYMENT | Liquidation fee if any |
| MARGIN_CALL_CURED | Lifecycle event |

## 9. Margin requirement types

| Requirement type | Description |
|---|---|
| Rules-based margin | Fixed percentages by product/position |
| Risk-based margin | Portfolio stress/scenario model |
| Portfolio margin | Requirement based on net portfolio risk |
| House margin | Broker/bank overlay above regulatory minimum |
| Concentration margin | Higher requirement for concentrated holdings |
| Volatility margin | Higher requirement for volatile securities |
| Event margin | Higher requirement around earnings/corporate events |
| Intraday margin | Requirements updated during day |

## 10. Eligible and non-eligible assets

Margin eligibility varies by policy.

Potentially marginable:

- listed equities
- ETFs
- government bonds
- investment-grade bonds
- certain mutual funds after holding period
- listed options/futures under specific rules

Often restricted or non-marginable:

- penny stocks
- suspended stocks
- private placements
- illiquid funds
- complex structured notes
- concentrated single-name positions
- restricted securities
- assets with stale or missing price

## 11. Short selling and securities borrowing

Short selling creates a different margin profile.

```text
Client borrows security
Client sells borrowed security
Client must later buy back and return security
```

Risks:

- unlimited theoretical loss if price rises
- borrow recall
- short borrow fees
- dividend compensation payments
- buy-in risk
- margin requirement changes

Position modelling:

| Object | Meaning |
|---|---|
| Short security position | Negative quantity of security |
| Cash proceeds | Cash from short sale, often restricted |
| Borrow liability | Obligation to return shares |
| Margin requirement | Collateral required to support short |
| Borrow fee accrual | Cost of securities borrowing |
| Manufactured dividend | Payment to lender if dividend occurs |

## 12. Securities lending from client portfolio

A client may lend securities and receive collateral/fee income.

Lifecycle:

```text
Security lent
  → collateral received
  → lending fee accrues
  → corporate action/dividend manufactured payment handled
  → security recalled/returned
  → collateral returned
```

Position modelling should distinguish:

- beneficial ownership retained by client;
- security is on loan and may be unavailable for sale/vote;
- collateral received may or may not be investable;
- lending income is income, not price return;
- counterparty exposure exists.

## 13. Repo and reverse repo

A repo is economically a collateralized loan.

| Transaction | Economic meaning |
|---|---|
| Repo | Client sells security and agrees to repurchase; client borrows cash |
| Reverse repo | Client buys security and agrees to resell; client lends cash |

For wealth platforms, repos are usually institutional/private-bank product rather than standard retail holdings, but the modelling concepts are important.

## 14. OTC derivatives margin and collateral

OTC derivatives may create collateral requirements based on mark-to-market and potential future exposure.

Concepts:

| Term | Meaning |
|---|---|
| Initial margin | Collateral against future exposure |
| Variation margin | Collateral against current mark-to-market |
| Threshold | Exposure allowed before collateral is posted |
| Minimum transfer amount | Avoids tiny collateral calls |
| Independent amount | Extra collateral buffer |
| Netting set | Group of trades netted under agreement |

## 15. Liquidation priority

A bank may define which assets to liquidate first.

Potential criteria:

- most liquid assets first
- assets with lowest tax impact first
- highest LTV assets first
- pledged cash first
- assets in same currency as loan
- avoid restricted/mandate assets
- avoid client-excluded securities if possible
- sell concentrated risky assets first

A platform should support liquidation simulation, but actual liquidation is governed by legal agreement and operations process.

## 16. Reporting margin risk

Client/advisor reports should show:

| Metric | Meaning |
|---|---|
| Debit balance | Amount borrowed |
| Account equity | Net account value after debit |
| Maintenance requirement | Minimum equity/collateral needed |
| Excess liquidity | Buffer above requirement |
| Margin call amount | Shortfall if any |
| Liquidation buffer | Distance to forced sale threshold |
| Leverage ratio | Gross exposure / net equity |
| Financing cost | Interest and fees |
| Stressed margin buffer | Buffer after market shock |

## 17. Platform controls

Mandatory controls:

- no stale price for marginable collateral unless policy allows fallback
- immediate block on non-marginable purchases if insufficient cash
- order reservation before execution
- real-time or frequent recalculation after price changes
- margin-call workflow with audit trail
- forced liquidation reason code
- client/advisor notification status
- override approval tracking
- independent reconciliation against custodian/broker statements

## 18. Key difference: Lombard availability versus margin requirement

| Concept | Lombard / collateral lending | Margin trading |
|---|---|---|
| Main question | How much can client borrow? | Is account sufficiently collateralized? |
| Key metric | Lending value minus exposure | Equity versus margin requirement |
| Trigger | LTV/collateral shortfall | Margin requirement breach |
| Use | Liquidity/loan facility | Trading leverage |
| Overlap | Both use collateral, haircuts and liquidation rights |

In practice, private-bank systems often need both approaches because a Lombard facility may also support trading/buying power.

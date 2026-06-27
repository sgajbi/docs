# 01 — Loans, Lombard Lending, Margin, Collateral and Credit Lines: Fundamentals and Taxonomy

## 1. What is a loan in wealth management?

A loan is a borrowing arrangement where the client receives money or credit availability and agrees to repay principal, interest and fees according to contractual terms.

In private banking, lending is often connected to investment portfolios. The client may pledge securities, funds, deposits, insurance policies, property or other eligible assets as collateral. The bank lends against the collateral value rather than only against salary or unsecured credit profile.

## 2. Why lending matters in private banking

Private-banking lending is important because it allows clients to:

- access liquidity without selling long-term investments
- fund investment opportunities
- bridge settlement timing gaps
- diversify or rebalance without liquidating existing assets immediately
- manage tax or estate-planning timing constraints
- borrow in one currency while holding assets in another currency
- support business, property or family-office liquidity needs
- implement leveraged portfolio strategies where suitable

For the bank, lending creates:

- interest income
- deeper client relationship
- higher product penetration
- secured exposure backed by collateral
- platform complexity around collateral, exposure, limits and monitoring

## 3. Main lending product types

| Product | Description | Typical private-bank relevance |
|---|---|---|
| Overdraft | Flexible borrowing linked to a bank account | Short-term liquidity, settlement buffer |
| Revolving credit line | Facility that can be drawn, repaid and redrawn | Lombard / portfolio lending |
| Term loan | Fixed loan amount over defined tenor | Property, investment or liquidity financing |
| Lombard loan | Loan secured by marketable investment assets | Core HNW/UHNW product |
| Margin loan | Broker lending to buy securities using account assets as collateral | Trading leverage and buying power |
| Portfolio loan | Loan secured by diversified investment portfolio | Similar to securities-based lending |
| Bridge loan | Temporary loan before expected cash inflow | Settlement, sale proceeds, liquidity bridge |
| Mortgage-backed wealth loan | Credit secured by real estate | Wealth lending / liquidity |
| Structured lending | Facility with complex collateral, covenant or purpose rules | UHNW, family office, concentrated assets |

## 4. Lombard lending / securities-based lending

A Lombard loan is a secured loan backed by financial assets, usually marketable securities and funds.

The client keeps the portfolio invested, but the assets are pledged to the bank. The bank assigns lending value to eligible collateral using LTV or haircut rules.

Example:

| Item | Amount |
|---|---:|
| Market value of eligible portfolio | USD 1,000,000 |
| Weighted average LTV | 60% |
| Lending value | USD 600,000 |
| Existing loan utilization | USD 350,000 |
| Available credit | USD 250,000 |

The client has not sold the portfolio. Instead, the portfolio supports borrowing capacity.

## 5. Margin lending

Margin lending is borrowing from a broker or financial institution to purchase or hold securities. The securities in the margin account act as collateral. Margin increases purchasing power but also magnifies losses.

A margin account normally has:

- initial margin requirement
- maintenance margin requirement
- marginable securities list
- collateral value / equity value
- debit balance / loan balance
- margin call logic
- liquidation rights if collateral falls below required level

Margin lending and Lombard lending are related, but not always the same.

| Dimension | Lombard lending | Margin lending |
|---|---|---|
| Main use | Liquidity or investment financing | Trading leverage |
| Facility style | Often credit-line based | Often account-based |
| Collateral | Broad eligible portfolio | Marginable securities in account |
| Risk monitoring | LTV / collateral shortfall | Margin requirement / equity ratio |
| Client segment | Private banking, HNW/UHNW | Brokerage / trading |
| Product control | Credit facility + collateral rules | Trading account + margin rules |

## 6. Credit line versus loan drawdown

A credit line is permission to borrow up to an approved limit. A drawdown is actual borrowing.

Example:

| Concept | Amount |
|---|---:|
| Approved credit limit | USD 1,000,000 |
| Collateral-based lending value | USD 750,000 |
| Drawn loan amount | USD 400,000 |
| Undrawn availability | USD 350,000 |

Availability is normally the lower of:

```text
Approved limit minus current exposure
Collateral lending value minus current exposure
```

In platform design, never confuse **limit** with **availability**.

## 7. Exposure types

Exposure is broader than outstanding loan principal.

| Exposure type | Meaning |
|---|---|
| Cash loan principal | Drawn loan outstanding |
| Overdraft balance | Negative cash balance |
| Accrued interest | Interest earned by bank but not yet paid |
| FX forward exposure | Potential settlement/mark-to-market exposure |
| OTC derivative exposure | Positive exposure to bank after netting/collateral |
| Securities purchase exposure | Pending settlement or in-flight order exposure |
| Guarantee exposure | Bank’s contingent exposure |
| Credit card / facility exposure | If linked to same collateral pool |
| Reserved exposure | Exposure reserved for approved but pending transactions |

A buying-power engine must decide which exposure types reduce availability immediately and which reduce it only after booking or settlement.

## 8. Collateral types

| Collateral type | Examples | Notes |
|---|---|---|
| Cash | Deposits, money market balances | High quality, currency mismatch still matters |
| Bonds | Government, investment-grade, high-yield | LTV depends on rating, duration, liquidity |
| Equities | Listed shares, ETFs | LTV depends on liquidity, concentration, volatility |
| Funds | Mutual funds, ETFs, hedge funds | LTV depends on fund liquidity/look-through |
| Structured products | Notes, certificates | Often low or zero LTV unless approved |
| Private markets | PE/private credit/real estate funds | Usually low LTV due to illiquidity and NAV lag |
| Insurance policies | Surrender value | Operational/legal complexity |
| Real estate | Residential/commercial property | Requires valuation, legal charge, jurisdiction controls |
| Guarantees | Third-party guarantees | Legal enforceability and credit quality matter |

## 9. LTV versus haircut

Two equivalent ways to express lending value:

```text
Lending value = Market value × LTV
```

or

```text
Lending value = Market value × (1 - Haircut)
```

Example:

| Market value | LTV | Haircut | Lending value |
|---:|---:|---:|---:|
| 100,000 | 70% | 30% | 70,000 |

A 70% LTV is equivalent to a 30% haircut.

## 10. Main risk types

| Risk | Description |
|---|---|
| Market risk | Collateral value falls |
| Credit risk | Client fails to repay |
| Liquidity risk | Collateral cannot be sold quickly enough |
| Concentration risk | Collateral depends on few issuers/assets |
| FX risk | Loan and collateral currencies differ |
| Interest-rate risk | Borrowing cost rises |
| Gap risk | Collateral price jumps down before bank can act |
| Wrong-way risk | Borrower credit quality worsens when collateral value falls |
| Legal risk | Pledge/enforcement not valid or delayed |
| Operational risk | Incorrect LTV, stale prices, missing pledge, late margin call |
| Suitability risk | Leverage unsuitable for client profile |

## 11. Wealth-platform lens

A wealth platform must treat loans as both:

1. **client liability positions** for portfolio reporting, net worth, cashflow and performance; and
2. **credit-control objects** for buying power, trading eligibility, collateral monitoring and mandate compliance.

This means the system needs a clean separation of:

- product master data
- facility / credit line
- drawdown / loan position
- collateral pool
- pledge relationship
- exposure calculation
- availability calculation
- lifecycle events
- accounting transactions
- risk analytics
- advisory and mandate controls

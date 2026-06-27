# 05 — Lifecycle, Transactions, Position, Cashflow and Event Modelling

## 1. Why lifecycle modelling is hard

Real estate and infrastructure products use multiple operational patterns:

- listed security trading;
- fund subscription/redemption;
- private fund commitment/capital calls;
- direct asset valuation and rental income;
- corporate actions;
- property-level events;
- loan/debt and leverage events;
- income distributions with multiple components.

A robust model should not create one transaction list per product. It should use a common transaction language with product-specific attributes.

## 2. Wrapper-driven lifecycle pattern

| Wrapper | Lifecycle pattern |
|---|---|
| Listed REIT/property stock | Order, trade, settlement, distributions, corporate actions |
| REIT ETF/fund | Subscription/buy, NAV/market valuation, distribution, redemption/sell |
| Private real estate fund | Commitment, calls, NAV, distributions, transfer, liquidation |
| Infrastructure fund | Commitment/calls or fund subscriptions, NAV, distributions, exits |
| Direct property | Purchase, valuation updates, rent, expenses, loan, sale |
| Real estate debt | Drawdown, interest accrual, repayment, default/recovery |

## 3. Common transaction model

| Field | Meaning |
|---|---|
| transaction_id | Unique transaction ID |
| transaction_group_id | Groups related legs |
| account_id | Client/account/portfolio |
| instrument_id | REIT/fund/property/debt/security |
| transaction_type | Standard business transaction type |
| trade_date | Execution date |
| effective_date | Economic date |
| value_date | Cash settlement date |
| booking_date | System accounting date |
| quantity_delta | Units/shares change |
| nominal_delta | Commitment/loan/nominal change |
| cash_amount | Cash movement |
| cash_currency | Currency |
| price | Market price/NAV/unit price |
| nav_date | NAV date if fund/private asset |
| income_component | Ordinary income/return of capital/capital gain |
| tax_amount | Withholding or other tax |
| fee_amount | Brokerage/management/custody/admin fee |
| realised_pnl | Disposal or redemption gain/loss |
| source_system | Custodian/fund admin/manager/manual |
| lifecycle_event_id | Link to corporate action/capital call/appraisal etc. |
| performance_classification | External cashflow/income/internal transfer/fee |

## 4. Transaction type catalogue

### 4.1 Listed REIT / property security transactions

| Transaction type | Description |
|---|---|
| REIT_BUY | Buy listed REIT units |
| REIT_SELL | Sell listed REIT units |
| REIT_DISTRIBUTION_INCOME | Income distribution |
| REIT_RETURN_OF_CAPITAL | Return of capital component |
| REIT_CAPITAL_GAIN_DISTRIBUTION | Capital gain distribution component |
| REIT_DISTRIBUTION_REINVESTMENT | Distribution reinvestment |
| REIT_RIGHTS_ENTITLEMENT | Rights credited |
| REIT_RIGHTS_EXERCISE | Rights exercised |
| REIT_RIGHTS_SALE | Rights sold |
| REIT_UNIT_SPLIT_CONSOLIDATION | Quantity adjustment |
| REIT_MERGER_EXCHANGE | Merger/stock consideration |
| REIT_MERGER_CASH | Merger/tender cash consideration |
| REIT_DELISTING | Delisting event/status change |
| REIT_TRANSFER_IN/OUT | Custody transfer |

### 4.2 Private fund transactions

| Transaction type | Description |
|---|---|
| REAL_ASSET_COMMITMENT | Client commitment accepted |
| REAL_ASSET_CAPITAL_CALL | Fund calls capital |
| REAL_ASSET_CONTRIBUTION | Cash contributed to fund |
| REAL_ASSET_NAV_UPDATE | NAV valuation update |
| REAL_ASSET_DISTRIBUTION_INCOME | Income distribution |
| REAL_ASSET_DISTRIBUTION_REALISED_GAIN | Realised gain distribution |
| REAL_ASSET_RETURN_OF_CAPITAL | Return of capital |
| REAL_ASSET_RECALLABLE_DISTRIBUTION | Distribution classified as recallable |
| REAL_ASSET_MANAGEMENT_FEE | Management/advisory fee |
| REAL_ASSET_CARRY_PERFORMANCE_FEE | Carry/incentive allocation |
| REAL_ASSET_SECONDARY_SALE | Sale of fund interest |
| REAL_ASSET_TRANSFER_IN/OUT | Transfer of interest |
| REAL_ASSET_FINAL_LIQUIDATION | Final distribution and close |

### 4.3 Direct property transactions

| Transaction type | Description |
|---|---|
| PROPERTY_PURCHASE | Buy property/direct asset |
| PROPERTY_SALE | Sell property/direct asset |
| PROPERTY_VALUATION_UPDATE | Appraisal/valuation update |
| PROPERTY_RENTAL_INCOME | Rent received |
| PROPERTY_EXPENSE | Tax/maintenance/insurance/management cost |
| PROPERTY_CAPEX | Capital improvement expenditure |
| PROPERTY_DEPRECIATION | Optional accounting depreciation |
| PROPERTY_MORTGAGE_DRAW | Loan drawdown |
| PROPERTY_MORTGAGE_REPAYMENT | Principal repayment |
| PROPERTY_MORTGAGE_INTEREST | Interest expense |
| PROPERTY_OWNERSHIP_ADJUSTMENT | Ownership share changes |

### 4.4 Infrastructure-specific events

| Event/transaction | Description |
|---|---|
| INFRA_CAPITAL_CALL | Capital call for project/fund |
| INFRA_PROJECT_MILESTONE | Construction/completion milestone |
| INFRA_AVAILABILITY_PAYMENT | Availability-based income |
| INFRA_CONCESSION_PAYMENT | Concession/user-payment income |
| INFRA_REFINANCING_DISTRIBUTION | Distribution from refinancing |
| INFRA_ASSET_SALE_DISTRIBUTION | Sale proceeds distributed |
| INFRA_IMPAIRMENT | Write-down of project/fund NAV |

## 5. Position modelling

### 5.1 Listed REIT position

| Field | Meaning |
|---|---|
| units | Number of REIT units |
| average_cost | Average cost per unit |
| cost_amount | Total cost |
| market_price | Exchange price |
| market_value | Units × market price |
| accrued_income | Usually not accrued unless policy supports declared income |
| unrealised_pnl | Market value - cost |
| realised_pnl | From sales/corporate actions |
| income_ytd | Distributions received |
| tax_withheld_ytd | Tax withheld |
| property_sector | Retail/office/logistics etc. |
| geography | Country/region exposure |
| collateral_eligibility | Eligible/not eligible, haircut |

### 5.2 Private real asset fund position

| Field | Meaning |
|---|---|
| commitment_amount | Total committed capital |
| called_capital | Cumulative calls |
| unfunded_commitment | Remaining obligation |
| contributed_capital | Paid-in capital |
| distributions_received | Cumulative distributions |
| recallable_amount | Amount distributable but recallable |
| current_nav | Latest reported NAV |
| valuation_date | NAV date |
| valuation_received_date | Date received |
| nav_lag_days | Reporting lag |
| strategy | Core/value-add/infrastructure etc. |
| vintage_year | Fund vintage |
| fund_life_stage | Investment/harvest/extension/liquidation |

### 5.3 Direct property position

| Field | Meaning |
|---|---|
| property_value | Latest appraisal/valuation |
| ownership_pct | Client ownership share |
| gross_value_client_share | Property value × ownership % |
| linked_debt | Mortgage/loan amount |
| net_equity_value | Gross share minus linked debt |
| rental_income_ytd | Rent received |
| expenses_ytd | Property expenses |
| valuation_method | Appraisal/model/manual |
| valuation_confidence | High/medium/low |
| collateral_haircut | Lending haircut if collateralised |

## 6. Lifecycle events versus transactions

Not every event is a transaction.

| Event | Transaction? | Explanation |
|---|---|---|
| REIT distribution declared | No | Entitlement event only |
| REIT distribution paid | Yes | Cash movement |
| Rights announced | No | Terms recorded |
| Rights credited | Yes/position event | Rights position may be created |
| Capital call notice received | No initially | Obligation created |
| Capital call paid | Yes | Cash contribution |
| NAV report received | Valuation update | No cash movement |
| Property appraisal received | Valuation update | No cash movement |
| Rent invoice issued | Optional accrual | Cash transaction when paid |
| Infrastructure project completed | Lifecycle event | No cash unless payment triggered |
| Fund liquidation notice | No | Final payment later |

## 7. Performance classification

| Transaction/event | TWR treatment |
|---|---|
| Buy funded from existing portfolio cash | Internal investment transaction |
| External cash added to buy property/fund | External inflow |
| REIT distribution retained in portfolio | Investment income |
| REIT distribution withdrawn by client | Income then external outflow |
| Return of capital | Investment cashflow reducing cost/capital classification |
| Capital call funded by existing cash | Internal investment contribution to private fund |
| Capital call funded by new money | External inflow then investment |
| NAV update | Market/appraisal return |
| Direct property appraisal gain | Unrealised valuation return |
| Property rental income | Investment income |
| Mortgage interest | Expense/financing cost |
| Linked loan drawdown | Liability cashflow, not investment return unless policy says otherwise |

## 8. Event status model

| Status | Meaning |
|---|---|
| ANNOUNCED | Event announced but not actionable |
| PENDING_CLIENT_ACTION | Requires decision: rights, tender, capital call funding |
| INSTRUCTED | Instruction captured |
| CONFIRMED | Custodian/manager confirmed |
| BOOKED | Transaction booked |
| REVERSED | Cancelled/reversed |
| RECONCILED | Matched against cash/position/manager statement |

## 9. Design principle

Use one transaction ledger with product-specific attributes, but keep lifecycle events separate from economic postings.

```text
Lifecycle event decides what should happen.
Transaction records what actually happened.
Position is the current state derived from transactions + valuations.
Analytics add look-through and risk interpretation.
```

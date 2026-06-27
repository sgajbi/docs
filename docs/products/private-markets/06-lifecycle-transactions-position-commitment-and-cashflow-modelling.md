# 06 — Lifecycle, Transactions, Position, Commitment and Cashflow Modelling

## 1. Why private-market modelling needs a separate lifecycle model

For public securities, a trade usually creates a funded position immediately. For private markets, the client may legally commit capital long before the position is fully funded.

Therefore, use three linked concepts:

```text
Commitment = legal obligation
Cashflow = actual money movement
Position/NAV = current estimated value of funded interest
```

A correct model must answer all of these:

- How much has the client committed?
- How much has been called?
- How much has been paid?
- How much remains unfunded?
- How much is recallable?
- What is the latest NAV?
- How much has been distributed?
- How much of distributions is income, return of capital or gain?
- What is realised versus unrealised value?
- What is the remaining liquidity/lock-up profile?

## 2. Generic private fund lifecycle

| Stage | Business event | System object | Transaction? |
|---|---|---|---|
| Product setup | Fund onboarded | Instrument/fund master | No |
| Offering | Fund open to eligible investors | Offering record | No |
| Subscription | Client signs commitment | Commitment record | Usually yes: ALT_COMMITMENT, but no cash |
| Closing | Investor admitted to fund | Closing event | Possibly equalisation true-up |
| Capital call notice | GP requests capital | Lifecycle notice/event | No cash yet |
| Capital contribution | Client pays call | Cash/security transaction | Yes |
| Investment period | Fund invests capital | Fund-level info | Usually no client transaction |
| NAV reporting | Manager reports NAV | Valuation record | Usually no cash transaction |
| Distribution notice | Fund returns cash/securities | Distribution event | Yes when settled |
| Recallable distribution | Returned capital can be called again | Commitment adjustment | Yes + recallable tracking |
| Recycling | Capital reused | Commitment/unfunded logic | Depends on notice |
| Secondary sale | Client sells interest | Transfer/sale transaction | Yes |
| Fund extension | Fund life extended | Lifecycle event | No |
| Liquidation | Final distribution | Final close transaction | Yes |

## 3. Transaction type catalogue

### 3.1 Commitment and closing transactions

| Transaction type | Purpose | Cash impact | Position impact |
|---|---|---:|---|
| ALT_COMMITMENT | Record legal commitment | 0 | Commitment increases |
| ALT_COMMITMENT_INCREASE | Increase commitment | 0 | Commitment increases |
| ALT_COMMITMENT_REDUCTION | Reduce/cancel commitment | 0 | Commitment decreases |
| ALT_COMMITMENT_TRANSFER_IN | Transfer commitment in | 0 | Commitment/unfunded increases |
| ALT_COMMITMENT_TRANSFER_OUT | Transfer commitment out | 0 | Commitment/unfunded decreases |
| ALT_EQUALIZATION_PAYMENT | True-up for later closing investor | Cash out/in | Cost/NAV/economics adjusted |
| ALT_ORGANIZATIONAL_EXPENSE | Investor share of setup costs | Cash out or NAV impact | Cost/expense |

### 3.2 Capital call and contribution transactions

| Transaction type | Purpose | Cash impact | Position impact |
|---|---|---:|---|
| ALT_CAPITAL_CALL_NOTICE | Record call notice | 0 | Payable created |
| ALT_CONTRIBUTION | Cash paid for capital call | Cash out | Paid-in capital increases |
| ALT_MANAGEMENT_FEE_CALL | Fee portion of call | Cash out | Fee/expense classification |
| ALT_EXPENSE_CALL | Expense portion of call | Cash out | Expense/cost classification |
| ALT_RECALLABLE_CALL | Call against recallable distribution | Cash out | Recallable balance decreases |
| ALT_CALL_REVERSAL | Correct/cancel call | Reverse | Recalculate paid-in/unfunded |

### 3.3 Distribution transactions

| Transaction type | Purpose | Cash impact | Position impact |
|---|---|---:|---|
| ALT_DISTRIBUTION_INCOME | Income distribution | Cash in | Income recognised |
| ALT_DISTRIBUTION_ROC | Return of capital | Cash in | Cost/paid-in basis reduced or distribution tracked |
| ALT_DISTRIBUTION_GAIN | Realised gain distribution | Cash in | Realised gain recognised |
| ALT_DISTRIBUTION_UNCLASSIFIED | Distribution when split unknown | Cash in | Temporary classification |
| ALT_RECALLABLE_DISTRIBUTION | Distribution that can be recalled | Cash in | Recallable balance increases |
| ALT_IN_KIND_DISTRIBUTION | Securities distributed | Security in | New position created |
| ALT_WITHHOLDING_TAX | Tax deducted | Cash out/reduced | Tax recognised |
| ALT_DISTRIBUTION_REVERSAL | Correct distribution | Reverse | Recalculate metrics |

### 3.4 Valuation and NAV transactions/events

| Type | Purpose | Cash impact | Position impact |
|---|---|---:|---|
| ALT_NAV_UPDATE | Store reported NAV | 0 | Market value changes |
| ALT_NAV_RESTATEMENT | Correct prior NAV | 0 | Historical valuation restated |
| ALT_VALUATION_ADJUSTMENT | Internal/evaluated adjustment | 0 | Market value changes |
| ALT_WRITEDOWN | Reduce NAV/principal due to loss | 0 | Unrealised/realised loss |
| ALT_WRITEOFF | Full loss/zero value | 0 | Position reduced/closed |

### 3.5 Liquidity and exit transactions

| Transaction type | Purpose | Cash impact | Position impact |
|---|---|---:|---|
| ALT_SECONDARY_SELL | Sell private fund interest | Cash in | Position/commitment reduced |
| ALT_SECONDARY_BUY | Buy existing fund interest | Cash out | Position/commitment assumed |
| ALT_REDEMPTION_REQUEST | Request redemption | 0 | Pending redemption status |
| ALT_REDEMPTION_SETTLEMENT | Redemption settled | Cash in | Units/NAV reduced |
| ALT_GATE_APPLIED | Redemption gated | 0 | Pending amount adjusted |
| ALT_TRANSFER_IN | Transfer position in | 0 | Position increases |
| ALT_TRANSFER_OUT | Transfer position out | 0 | Position decreases |
| ALT_FINAL_LIQUIDATION | Final distribution/close | Cash/security in | Position closed |
| ALT_ESCROW_RELEASE | Release held-back amount | Cash in | Residual value reduced |

## 4. Position model

A private-market position should include both commitment and NAV fields.

| Field | Meaning |
|---|---|
| account_id | Client account/portfolio |
| instrument_id | Fund/SPV/security |
| commitment_amount | Total legal commitment |
| commitment_currency | Currency of commitment |
| commitment_date | Date commitment signed/admitted |
| original_commitment | Original amount before transfers/reductions |
| funded_amount / paid_in_capital | Total contributed capital |
| unfunded_commitment | Commitment not yet called, adjusted for recallable/recycling rules |
| recallable_amount | Distributions that may be called again |
| outstanding_call_payable | Capital called but not yet paid |
| nav_amount | Latest reported residual value |
| nav_currency | NAV currency |
| nav_date | Valuation date |
| nav_received_date | When NAV was received |
| nav_status | Estimated/final/restated/stale |
| distributions_total | Total distributions received |
| distributions_income | Income component |
| distributions_roc | Return of capital component |
| distributions_gain | Realised gain component |
| cost_basis | Platform accounting basis |
| realised_gain_loss | Realised P&L |
| unrealised_gain_loss | NAV less remaining basis |
| lifecycle_status | Active/investment period/harvest/liquidating/closed |
| vintage_year | Vintage classification |
| strategy | PE, VC, private credit, real estate, infra, hedge fund |
| manager_id | GP/manager |
| liquidity_terms | Lock-up/redemption/gate/transfer restrictions |
| expected_final_maturity | Expected fund termination |
| legal_entity / share_class | Fund legal structure/share class |

## 5. Commitment mathematics

Basic formulas:

```text
Paid-in capital = sum(contributions)
Unfunded commitment = commitment amount - called capital + recallable amount +/- adjustments
Total value = NAV + distributions received
TVPI = (NAV + distributions) / paid-in capital
DPI = distributions / paid-in capital
RVPI = NAV / paid-in capital
PIC = paid-in capital / commitment amount
```

Important: Some funds may allow recycling, recallable distributions, commitment reductions, transfers or fee calls that complicate these formulas. The system must store source amounts rather than relying only on derived balances.

## 6. Cashflow modelling for performance

For private-market IRR:

| Cashflow | Sign |
|---|---:|
| Capital contribution | Negative |
| Fee call paid by client | Negative, unless included in contribution policy |
| Distribution | Positive |
| Recallable distribution | Positive now, but may increase future unfunded obligation |
| Ending NAV | Positive terminal value |
| Secondary sale proceeds | Positive |
| Secondary purchase price | Negative |

For portfolio TWR:

| Scenario | Treatment |
|---|---|
| Client adds new cash to fund capital call | External inflow into portfolio, then contribution to fund |
| Capital call paid from existing portfolio cash | Internal investment transaction |
| Distribution retained in portfolio cash | Internal investment result |
| Distribution withdrawn by client | External outflow after investment result |
| NAV update | Market value movement, not cashflow |
| Commitment recorded with no cash | Exposure/obligation event, not performance cashflow |

## 7. Capital call modelling

A capital call notice should be a lifecycle event before payment.

| Field | Meaning |
|---|---|
| call_id | Unique call notice |
| fund_id | Fund |
| investor_account_id | Account |
| notice_date | Date notice received |
| due_date | Payment due date |
| call_amount | Total amount due |
| currency | Call currency |
| purpose | Investment / management fee / expense / recallable / other |
| percent_of_commitment | Call as % of commitment |
| bank_instructions | Payment details |
| status | Pending / paid / overdue / cancelled |
| linked_transaction_id | Contribution transaction when paid |

## 8. Distribution notice modelling

A distribution notice should preserve classification.

| Field | Meaning |
|---|---|
| distribution_id | Unique notice |
| notice_date | Date notice received |
| payment_date | Date cash expected |
| gross_amount | Gross distribution |
| tax_withheld | Tax |
| net_amount | Cash received |
| income_component | Income portion |
| return_of_capital_component | ROC portion |
| realised_gain_component | Gain portion |
| recallable_component | Amount that can be called again |
| in_kind_component | Securities delivered |
| currency | Distribution currency |
| classification_status | Final / estimated / unclassified |

## 9. NAV update modelling

NAV updates should be valuations, not transactions.

| Field | Meaning |
|---|---|
| valuation_id | Unique valuation |
| fund_id | Fund |
| account_id | Investor account |
| nav_date | Valuation date |
| nav_received_date | Date loaded |
| nav_amount | Investor NAV |
| nav_per_unit | If unitised |
| units | If unitised |
| capital_account_balance | If partnership accounting |
| status | Estimated/final/restated/stale |
| source | Manager/admin/custodian/internal |
| confidence_level | High/medium/low |
| restates_valuation_id | Link to corrected valuation |

## 10. Secondary transfer modelling

Secondaries are operationally complex because economic effective date and legal closing date may differ.

Key fields:

- effective date;
- closing date;
- reported NAV at reference date;
- purchase price;
- discount/premium to NAV;
- interim calls and distributions;
- transferred unfunded commitment;
- GP consent status;
- transfer fees;
- buyer/seller allocations.

## 11. In-kind distribution modelling

If a private fund distributes listed shares after IPO:

| Transaction | Effect |
|---|---|
| ALT_IN_KIND_DISTRIBUTION | Distribution event from private fund |
| EQUITY_DELIVERY_IN | Listed shares created |
| COST_BASIS_ALLOCATION | Allocate cost or fair value depending on policy |
| LOCKUP_STATUS_UPDATE | Mark lock-up if shares cannot be sold |

Do not treat this as a normal cash distribution only.

## 12. Common modelling anti-patterns

| Anti-pattern | Problem |
|---|---|
| Treat commitment as market value | Overstates current assets |
| Ignore unfunded commitment | Understates future obligation and liquidity risk |
| Treat all distributions as income | Overstates income and misreports ROC/gain |
| Update NAV as cash transaction | Distorts cashflow/performance |
| Ignore NAV date | Hides stale valuation risk |
| Use TWR only | Misses private-market cashflow economics |
| Ignore recallable distributions | Understates future calls |
| Treat secondary price as same as NAV | Misstates cost and performance |
| Close position at zero before final liquidation | Loses tail distributions/escrows |

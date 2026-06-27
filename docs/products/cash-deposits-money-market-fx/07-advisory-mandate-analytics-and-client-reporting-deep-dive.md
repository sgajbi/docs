# 07 — Advisory, Mandate, Analytics and Client Reporting Deep Dive

## 1. Advisory relevance

Cash, deposits, money market and FX decisions are advisory decisions, not merely operational instructions.

Advisors should understand:

- client liquidity needs;
- cash-reserve requirements;
- upcoming withdrawals or capital calls;
- base currency and spending currency;
- acceptable currency risk;
- deposit tenor and early-break constraints;
- money market fund liquidity and NAV risk;
- concentration with banks/issuers;
- mandate rules and suitability;
- tax and reporting preferences;
- cost of FX spreads and hedging.

## 2. Client objectives

| Client objective | Product response |
|---|---|
| Immediate liquidity | Cash/call account |
| Slightly higher yield with known maturity | Fixed deposit / T-bill |
| Daily liquidity with fund wrapper | Money market fund |
| Short-term reserve | T-bill ladder / deposit ladder / liquidity fund |
| Future foreign currency payment | FX spot/forward or foreign currency cash |
| Reduce currency volatility | FX hedge / base-currency share class |
| Diversify bank exposure | Multi-bank deposits / T-bills / MMFs |
| Preserve capital | High-quality cash/deposit/government instruments, subject to risk disclosure |
| Support DPM mandate | Mandate-defined liquidity bucket and cash ranges |

## 3. Advisory suitability checklist

| Question | Why it matters |
|---|---|
| What is the client’s base currency? | Determines FX risk and reporting return |
| What currencies does the client spend in? | Foreign cash may be natural hedge or speculation |
| When is cash needed? | Determines tenor and liquidity product suitability |
| Can client tolerate early-break penalty? | Fixed deposits may not be suitable for uncertain needs |
| Is deposit insurance relevant and sufficient? | Large balances may exceed protected amount |
| Is the client comfortable with fund/NAV risk? | MMFs are not deposits |
| Is there issuer/bank concentration? | Cash/deposit/CD exposure may be concentrated |
| Are there upcoming capital calls? | Need liquidity reserve |
| Are there loan/collateral obligations? | Cash may be pledged or reserved |
| Is FX hedge suitable? | Forwards create obligations and may require collateral |
| Is client allowed to use derivatives? | Mandate/suitability may restrict FX forwards/options |

## 4. Mandate treatment

Mandates should define exactly how this product family is classified.

| Product | Possible mandate bucket |
|---|---|
| Cash | Cash/liquidity |
| Fixed deposit <= 3 months | Cash equivalent / liquidity |
| Fixed deposit > 12 months | Deposit / short-term fixed income / restricted liquidity |
| T-bill | Cash equivalent or fixed income depending tenor/rules |
| Commercial paper | Short-term credit / money market |
| Money market fund | Cash equivalent / liquidity fund / fund |
| Foreign cash | Cash plus currency exposure |
| FX forward hedge | Overlay / derivative / hedge exposure |
| Negative cash | Liability / leverage / overdraft |

## 5. DPM and discretionary mandate controls

DPM mandates often need rules such as:

| Rule | Example |
|---|---|
| Minimum cash buffer | Maintain at least 2% available cash |
| Maximum idle cash | Cash above 10% requires explanation or sweep |
| Currency exposure cap | Non-base currency exposure max 30% |
| Deposit tenor cap | Max deposit tenor 12 months |
| Bank exposure cap | Max 20% with one bank issuer |
| MMF eligibility | Only approved liquidity funds |
| Derivative permission | FX forwards allowed only for hedging |
| Negative cash restriction | No overdraft unless approved |
| Capital-call reserve | Maintain cash/liquidity for expected capital calls |
| Restricted cash exclusion | Pledged cash excluded from available liquidity |

## 6. Advisory conversation examples

### Cash reserve

Advisor explanation:

> Your portfolio currently holds 18% in cash. Part of this is intentional because you have upcoming commitments, but part appears idle. We can keep your near-term spending reserve in cash and place excess liquidity into a ladder of short-term deposits, T-bills or approved money market funds, subject to your liquidity needs and risk appetite.

### Foreign currency deposit

Advisor explanation:

> The USD deposit rate is higher than the SGD rate, but your reporting and spending currency is SGD. The final SGD return depends not only on the USD interest rate but also on USD/SGD movement. If USD weakens, the SGD return can be lower or even negative.

### Money market fund

Advisor explanation:

> The money market fund may provide better yield than idle cash and daily liquidity under normal conditions, but it is an investment fund, not a bank deposit. Its NAV, liquidity and redemption terms depend on the fund portfolio and rules.

## 7. Analytics for advisory and portfolio review

| Analytics | Purpose |
|---|---|
| Cash allocation by currency | Shows liquidity and FX exposure |
| Cash equivalent allocation | Shows liquidity products beyond cash |
| Maturity ladder | Shows deposit/T-bill/CD maturities |
| Projected liquidity | Shows future cash shortfalls/surpluses |
| Cash drag | Quantifies opportunity cost of excess cash |
| Yield on liquidity | Measures income from cash/deposits/MMFs |
| Bank/issuer concentration | Controls cash/deposit/CD risk |
| FX exposure by currency | Shows gross and net currency risk |
| Hedge ratio | Measures currency hedge coverage |
| Negative cash/overdraft report | Identifies funding/leverage issue |
| Restricted cash report | Separates usable and unusable liquidity |

## 8. Maturity ladder reporting

Example:

| Maturity bucket | Deposits | T-bills/CDs | Expected cash inflow |
|---|---:|---:|---:|
| 0-7 days | 100,000 | 50,000 | 150,000 |
| 8-30 days | 250,000 | 100,000 | 350,000 |
| 31-90 days | 500,000 | 300,000 | 800,000 |
| 91-180 days | 200,000 | 0 | 200,000 |
| >180 days | 100,000 | 150,000 | 250,000 |

## 9. FX exposure reporting

Example:

| Currency | Cash | Securities | Funds | FX forwards | Net exposure | % of portfolio |
|---|---:|---:|---:|---:|---:|---:|
| SGD | 500,000 | 1,000,000 | 200,000 | 0 | 1,700,000 | 34% |
| USD | 200,000 | 2,000,000 | 500,000 | -1,000,000 | 1,700,000 | 34% |
| EUR | 50,000 | 800,000 | 0 | -400,000 | 450,000 | 9% |
| CHF | -100,000 | 0 | 0 | 0 | -100,000 | -2% |

Negative CHF should be shown clearly. It is not simply offset by other positive cash without explanation.

## 10. Income reporting

Cash/deposit/MMF income should be broken down:

| Income source | Example |
|---|---:|
| Cash interest | 1,200 |
| Deposit interest | 8,500 |
| T-bill accretion/gain | 3,000 |
| MMF distribution | 2,100 |
| Overdraft interest | -500 |
| Net liquidity income | 14,300 |

## 11. Performance reporting

Performance reports should separate:

- cash return;
- cash drag;
- FX translation impact;
- income contribution;
- liquidity product contribution;
- overdraft/financing cost;
- hedging gain/loss.

Example contribution table:

| Component | Contribution |
|---|---:|
| Cash interest | +0.05% |
| Deposit income | +0.12% |
| MMF return | +0.04% |
| FX translation on cash | -0.08% |
| FX forward hedge | +0.03% |
| Overdraft cost | -0.01% |
| Total liquidity/FX contribution | +0.15% |

## 12. Mandate compliance examples

| Rule | Portfolio state | Result |
|---|---|---|
| Cash min 2% | Available cash 1.2% | Breach |
| Cash max 10% | Cash + cash equivalents 18% | Warning or breach depending rule |
| Non-base currency max 30% | USD net exposure 42% | Breach |
| Deposit tenor max 12M | 18M deposit | Breach |
| One bank exposure max 20% | Bank A deposits 28% | Breach |
| Derivatives hedge-only | FX forward exceeds underlying exposure | Breach/warning |
| No negative cash | EUR cash -25,000 | Breach |

## 13. Reporting hierarchy

Recommended report structure:

1. total cash and cash equivalents;
2. cash by currency;
3. available versus restricted cash;
4. negative cash/overdrafts;
5. deposits by tenor/maturity;
6. money market funds and instruments;
7. liquidity maturity ladder;
8. FX exposure and hedges;
9. income/yield from liquidity products;
10. cash drag and portfolio impact;
11. upcoming cash events;
12. mandate exceptions.

## 14. Client-facing wording

### Cash and deposits

> Cash provides liquidity and stability, but may earn lower returns than invested assets and can lose purchasing power after inflation. Term deposits may offer higher rates than call cash but reduce flexibility and may involve penalties if broken early.

### Money market funds

> Money market funds invest in short-term instruments and can be used for liquidity management, but they are investment funds and are not the same as bank deposits. Their liquidity and NAV depend on fund rules and market conditions.

### FX exposure

> Foreign currency holdings can increase or reduce portfolio return when translated into your reporting currency. Higher foreign-currency yield does not guarantee higher base-currency return.

## 15. Wealth-platform practitioner answer

A strong answer:

> Cash and FX are central to wealth platforms because they drive settlement, buying power, liquidity, performance and reporting. I would not model cash as a single portfolio number. I would model cash by account, currency, value date and balance type, with a clear distinction between ledger, settled, available, blocked and projected cash. Deposits should be modelled as term contracts with interest, maturity and early-break lifecycle. Money market funds remain fund positions valued by NAV, even if classified as cash equivalents for analytics. FX should be modelled as multi-leg transactions and open contracts for forwards/NDFs, with separate treatment for settlement cashflows, MTM, realised P&L and currency translation. Advisory and mandate systems should use these models to control liquidity, currency exposure, issuer concentration, cash drag and suitability.

# 03 - Annuities, Retirement Income and Longevity Products

## 1. What an annuity is

An annuity is an insurance contract that converts premium or accumulated value into one or more income payments. It is mainly used to manage retirement income and longevity risk.

The core question an annuity answers is:

> How can capital be converted into predictable income, possibly for life?

## 2. Annuity phases

| Phase | Meaning | Platform focus |
|---|---|---|
| Accumulation phase | Client pays premiums or invests capital | Premiums, account value, surrender value, fees |
| Deferral phase | Value grows before income starts | Valuation, charges, guarantees, liquidity |
| Annuitization / payout phase | Income payments begin | Payment schedule, remaining guarantee, survival status |
| Claim / death phase | Death benefit or survivor benefit assessed | Beneficiary, joint-life, guaranteed period rules |

## 3. Main annuity types

| Type | Description | Key risks |
|---|---|---|
| Immediate annuity | Income starts soon after premium | Illiquidity, inflation, insurer credit |
| Deferred annuity | Income starts later | Surrender, market/rate risk, insurer credit |
| Fixed annuity | Insurer credits fixed or declared rate | Inflation, insurer credit, liquidity |
| Variable annuity | Account value linked to sub-accounts/funds | Market risk, fees, surrender, complexity |
| Fixed indexed annuity | Return linked partly to index formula with limits | Cap/spread/participation complexity, liquidity |
| Lifetime annuity | Income for life | Loss of liquidity, longevity pooling |
| Joint-life annuity | Income while one or both lives survive | Survivor benefit design |
| Term-certain annuity | Payments for fixed period | No lifetime protection after term |
| Guaranteed-period annuity | Lifetime plus minimum payment period | Estate/payout trade-off |
| Inflation-linked annuity | Payments adjusted by inflation formula | Lower initial income, index basis risk |

## 4. Product combinations

| Annuity design | Combined with |
|---|---|
| Fixed annuity | Insurance contract + general account guarantee |
| Variable annuity | Insurance contract + investment sub-accounts + optional riders |
| Indexed annuity | Insurance contract + index-crediting formula + caps/floors/spreads |
| Lifetime annuity | Longevity risk pooling + insurer guarantee |
| Guaranteed lifetime withdrawal benefit | Account value + rider-based income guarantee |
| Joint-life annuity | Lifetime annuity + survivor benefit option |

## 5. Payout design

| Payout option | Meaning | Advisory trade-off |
|---|---|---|
| Life only | Highest lifetime income, stops at death | No residual value for heirs |
| Life with guaranteed period | Pays for life, but at least N years | Lower income, better estate protection |
| Joint and survivor | Continues to spouse/second life | Lower income, family protection |
| Period certain | Pays for fixed years | Predictable but no longevity protection |
| Lump sum plus income | Partial liquidity plus annuity | Lower recurring income |
| Escalating income | Income grows over time | Lower starting amount, inflation protection |

## 6. Annuity lifecycle and transactions

| Event | Transaction / event type | Effect |
|---|---|---|
| Application | POLICY_APPLICATION | Pending underwriting/acceptance |
| Premium / purchase payment | ANNUITY_PREMIUM_PAYMENT | Cash out; annuity value created |
| Premium allocation | ANNUITY_ACCOUNT_ALLOCATION | Sub-account units/value created if variable |
| Bonus credit | ANNUITY_BONUS_CREDIT | Increase account value, may be subject to conditions |
| Charge deduction | ANNUITY_CHARGE | Reduce account value |
| Free withdrawal | ANNUITY_WITHDRAWAL | Cash paid, value reduced |
| Surrender | ANNUITY_SURRENDER | Contract closed, surrender value paid |
| Annuitization | ANNUITY_ANNUITIZATION | Accumulation value converted to income stream |
| Periodic payment | ANNUITY_INCOME_PAYMENT | Cash in to client/account |
| Death benefit | ANNUITY_DEATH_BENEFIT | Pay beneficiary based on contract |
| Rider activation | ANNUITY_RIDER_ACTIVATION | Income/benefit guarantee begins |

## 7. Position and valuation treatment

Before annuitization:

| Value | Meaning |
|---|---|
| Account value | Accumulated contract value before surrender effects |
| Surrender value | Amount accessible after surrender charges/adjustments |
| Guaranteed minimum value | Contractual minimum under stated conditions |
| Death benefit base | May differ from account value |
| Income benefit base | Used to calculate guaranteed withdrawals; may not be cashable |

After annuitization:

| Value | Meaning |
|---|---|
| Income payment amount | Contractual periodic income |
| Remaining guaranteed payments | If guaranteed-period or refund option applies |
| Present value estimate | Optional analytics estimate, not necessarily surrenderable |
| Death/survivor benefit | Based on payout option |

Important: an income benefit base is often **not the same as account value** and may not be withdrawable as a lump sum.

## 8. Performance treatment

Annuities create difficult performance questions.

| Situation | Treatment |
|---|---|
| Client pays premium from outside portfolio | External outflow from client, policy asset created if tracked |
| Premium paid from portfolio cash | Internal reallocation from cash to policy asset |
| Account value grows | Investment/crediting result |
| Charges deducted | Product expense reducing value |
| Income payment received into portfolio | Income/cash inflow; classify carefully |
| Lifetime income paid after annuitization | Could be income, return of capital, or mixed depending accounting policy |
| Surrender | Disposal of policy asset; realized gain/loss versus basis |
| Death benefit | Insurance proceeds; often non-market gain |

## 9. Advisory considerations

Annuities are suitable only when the client understands the trade-offs:

- income certainty versus liquidity;
- longevity protection versus estate residual value;
- fixed income versus inflation risk;
- insurer credit quality;
- surrender charges and withdrawal restrictions;
- product fees and riders;
- taxation and jurisdiction rules;
- whether client already has pension/CPF/government retirement income;
- spouse/survivor needs;
- beneficiary and estate objectives.

## 10. Common mistakes

| Mistake | Correct understanding |
|---|---|
| "Guaranteed income means no risk" | Insurer credit, inflation and liquidity risks remain |
| "Benefit base is my account value" | Benefit base is often only a calculation base |
| "I can exit anytime at full value" | Surrender charges and market adjustments may apply |
| "Variable annuity is just a fund" | It is an insurance contract with sub-accounts, fees and guarantees |
| "Highest payout is best" | Highest payout may leave no residual benefit to spouse/heirs |

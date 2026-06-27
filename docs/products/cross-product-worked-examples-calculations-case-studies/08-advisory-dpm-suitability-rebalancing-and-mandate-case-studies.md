# 08 - Advisory, DPM, Suitability, Rebalancing and Mandate Case Studies

## 1. Advisory proposal case study

### Scenario

Client risk profile: balanced. Current portfolio has 85% equities, 10% bonds, 5% cash. Target allocation is 60% equities, 35% bonds, 5% cash.

### Drift

| Asset class | Current | Target | Drift |
|---|---:|---:|---:|
| Equity | 85% | 60% | +25% |
| Bonds | 10% | 35% | -25% |
| Cash | 5% | 5% | 0% |

### Advisory recommendation

- reduce equity exposure
- increase bond exposure
- maintain cash
- check tax/cost implications
- check product suitability
- generate proposal
- obtain client approval before trade

### QA assertions

- Proposed products are in APU.
- Product risk rating fits client profile.
- Concentration and liquidity checks pass.
- Client approval is stored.

## 2. DPM mandate breach case study

### Scenario

DPM mandate allows equity allocation 40%-70%. Portfolio equity is 75%.

### Expected workflow

```text
Breach detected
-> breach severity calculated
-> portfolio manager notified
-> reason captured
-> rebalance proposal generated
-> approval obtained
-> trades executed
-> breach closed with evidence
```

### QA assertions

- Passive market movement breach is distinguished from active breach if policy requires.
- Breach age is tracked.
- Waiver/override requires approval.
- Client report can explain action if required.

## 3. Rebalancing with minimum trade size

### Scenario

Portfolio value USD 1,000,000. Equity target 60%, current 62%. Minimum trade size USD 25,000.

### Drift amount

```text
Current equity value = 620,000
Target equity value = 600,000
Trade required = sell 20,000
```

Since trade required is below minimum trade size, system may recommend no trade.

### QA assertions

- Drift threshold and trade threshold are both considered.
- No-trade decision is explainable.
- Mandate remains within allowed range if range permits.

## 4. Suitability failure case study

### Scenario

Advisor proposes high-risk structured note for conservative client.

### Expected outcome

- suitability check fails
- recommendation blocked or escalated
- reason recorded
- alternative product suggested if allowed
- client cannot approve blocked recommendation without required override policy

### QA assertions

- Product risk rating compared to client profile.
- Complex product knowledge/eligibility checked.
- Disclosure required.
- Override workflow controlled.

## 5. Liquidity constraint case study

### Scenario

Client needs USD 200,000 liquidity within 30 days. Portfolio has USD 50,000 cash and USD 500,000 private equity NAV with lock-up.

### Expected advisory output

- private equity is not reliable short-term liquidity
- identify liquid assets
- avoid recommending illiquid products
- document liquidity need

### QA assertions

- Liquidity need influences suitability.
- Illiquid holdings are not treated as immediate cash source.
- Report shows liquidity profile.

## 6. AI-assisted advisory case study

### Scenario

AI suggests "increase high-yield bonds" because income is low.

### Required controls

- AI must show data basis
- product suitability engine must check client profile
- concentration and credit risk checks required
- advisor must approve
- recommendation must cite approved product/investment view if used

### QA assertions

- AI suggestion is not directly executable.
- Human approval required.
- Suitability controls cannot be bypassed.
- Prompt/output stored for audit.

## 7. Key takeaway

Advisory and DPM examples must combine investment logic, client context, rules, workflow and evidence.

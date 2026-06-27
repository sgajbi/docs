# 08 - Platform Implementation, Controls, Reconciliation, Test Scenarios and Glossary

## 1. Platform capability checklist

A wealth platform supporting commodities should support:

| Capability | Required behavior |
|---|---|
| Instrument master | Commodity, wrapper, unit, exchange, issuer, custodian, complexity |
| Commodity master | Family, benchmark, unit, grade, price source |
| Position engine | Quantity by unit, notional, cost, market value, pledged/available |
| Transaction engine | Spot/metal, ETP, futures, options, swaps, notes, fees, margin |
| Valuation engine | Spot, benchmark, exchange, NAV, issuer, model prices |
| Performance engine | Price, FX, roll, income, fee, derivative P&L classification |
| Risk engine | Volatility, VaR, exposure, roll, counterparty, liquidity, concentration |
| Corporate/lifecycle engine | ETP split, futures expiry/roll, structured product observation, delivery |
| Collateral engine | Haircuts, pledged value, LTV, availability |
| Advisory controls | Suitability, complexity, mandate eligibility, concentration |
| Reporting engine | Holdings, exposure, P&L, income, liquidity, risk notes |
| Reconciliation | Custodian, exchange, issuer, fund admin, margin broker, vault |

## 2. Data quality controls

| Control | Why it matters |
|---|---|
| Unit-of-measure validation | Prevent oz/gram/barrel/tonne errors |
| Price unit validation | Prevent USD/oz versus USD/gram errors |
| FX conversion control | Commodity usually USD-denominated; portfolios may not be |
| Wrapper classification | ETF versus ETN versus physical affects risk |
| Issuer/provider mapping | Needed for ETN/unallocated/certificate risk |
| Custody location mapping | Needed for physical metal reporting |
| Futures contract size validation | Errors create huge notional misstatement |
| Expiry calendar control | Prevent accidental physical delivery or stale contracts |
| Roll schedule control | Futures-based products require roll monitoring |
| Collateral haircut validation | Buying-power/LTV depends on correct haircuts |
| Complexity flag control | Required for advisory and retail safeguards |

## 3. Reconciliation points

| Reconciliation | Source A | Source B |
|---|---|---|
| Physical metal quantity | Vault/custodian | Position system |
| Bar list | Custodian inventory | Client allocated holdings |
| Metal account balance | Provider statement | Portfolio system |
| ETP units | Custodian/broker | Position system |
| Futures contracts | FCM/broker | Derivatives position system |
| Variation margin | Broker statement | Cash ledger |
| Initial margin | Broker/collateral system | Collateral ledger |
| Structured product valuation | Issuer/vendor | Portfolio valuation system |
| Collateral pledge | Credit system | Position encumbrance system |
| Commodity exposure | Look-through engine | Risk/reporting engine |

## 4. Test scenarios

### Physical gold buy and sale

- Buy 100 oz gold from USD cash.
- Validate quantity, price, cash debit, cost basis, market value.
- Revalue at new gold price.
- Sell 40 oz.
- Validate realized P&L and remaining cost basis.

### Allocated versus unallocated account

- Book 50 oz allocated gold and 50 oz unallocated gold.
- Ensure both show gold exposure.
- Ensure unallocated account shows provider credit exposure.
- Ensure allocated holding shows custody/vault location.

### Storage fee

- Post quarterly storage fee.
- Validate no quantity change unless fee paid in metal.
- Validate performance expense classification.

### Futures daily variation margin

- Open 2 gold futures contracts.
- Post daily settlement change.
- Validate variation margin cash movement and futures P&L.
- Validate notional exposure remains separate from margin cash.

### Futures roll

- Close near-month crude future.
- Open next-month future.
- Validate realized P&L, new contract, roll return and exposure continuity.

### Futures expiry prevention

- Position reaches configured expiry threshold.
- Generate alert/workflow.
- Validate system prevents unintended physical delivery for non-delivery mandate.

### Commodity ETN issuer risk

- Book commodity ETN.
- Validate commodity look-through exposure and issuer credit exposure.
- Downgrade issuer.
- Validate risk report flags issuer concentration.

### Commodity-linked note

- Subscribe to gold-linked note.
- Process observation event.
- Pay coupon.
- Redeem at maturity.
- Validate note position closes and gold exposure disappears.

### Pledged gold collateral

- Pledge gold account to credit line.
- Calculate collateral value using haircut.
- Process gold price shock.
- Validate availability and shortfall/margin call logic.

### Leveraged commodity ETP suitability

- Attempt purchase in conservative mandate.
- Validate suitability/mandate block.
- Record override path only if governance allows.

## 5. Production incidents to expect

| Incident | Likely cause | Fix/control |
|---|---|---|
| Gold market value 31x wrong | Troy oz vs gram conversion error | Unit validation and price-unit metadata |
| Futures exposure missing | Position model only captured margin | Separate notional and margin fields |
| Commodity ETF performance differs from spot | Roll yield/fees ignored | Add return decomposition/disclosure |
| ETN shown as fund | Wrong wrapper classification | Security master enrichment |
| Unallocated gold shown as cash | Product taxonomy gap | Classify as commodity plus provider claim |
| Physical metal available despite pledge | Encumbrance not linked to position | Pledged/available quantity controls |
| Stale futures contract held after expiry | Expiry workflow missing | Contract lifecycle alerts |
| Structured product commodity exposure persists after redemption | Look-through not closed | Lifecycle-driven exposure closeout |

## 6. Glossary

| Term | Meaning |
|---|---|
| Allocated gold | Gold held in a way that specific bars/metal are allocated to holder |
| Unallocated gold | Claim on provider for metal amount, not specific bars |
| Bullion | Investment-grade precious metal bars/coins |
| Troy ounce | Standard precious-metal weight unit |
| Fineness | Purity of metal |
| Spot price | Price for near-term commodity delivery |
| Futures price | Price for delivery/settlement at future date |
| Forward curve | Set of prices across maturities |
| Contango | Futures prices above spot/near prices |
| Backwardation | Futures prices below spot/near prices |
| Roll yield | Return from rolling futures exposure |
| Basis | Difference between spot/cash and futures price |
| Storage cost | Cost of storing physical commodity |
| Convenience yield | Benefit of holding physical inventory |
| Variation margin | Daily futures mark-to-market cash settlement |
| Initial margin | Collateral required to open futures position |
| ETC | Exchange-traded commodity/security/certificate depending market |
| ETN | Exchange-traded note; unsecured issuer debt linked to index |
| iNAV | Intraday indicative NAV for ETPs |
| Good Delivery | Market standard for acceptable bullion bars/refiners |
| Warehouse receipt | Document/electronic title or claim over stored commodity |
| Look-through exposure | Economic exposure derived from wrapper holdings/payoff |
| Delta-adjusted exposure | Option exposure adjusted by sensitivity to underlying |
| Collateral value | Market value after haircut for lending |
| Haircut | Percentage reduction applied for collateral/risk purposes |

## 7. Practitioner review guide

A strong answer:

“Commodity products must be modelled by wrapper. Physical gold, unallocated gold, a gold ETF, a futures-based oil ETP, a commodity ETN, a commodity future and a commodity-linked note all create commodity exposure, but the legal position, valuation source, transaction lifecycle and risk are different. The platform should separate legal holding from look-through exposure. It should handle unit-of-measure, custody, storage fees, futures margin, roll yield, issuer/counterparty risk, collateral haircuts, suitability and mandate controls. For reporting, it should show market value, notional exposure, P&L, fees, liquidity, issuer/custody risk and commodity-family exposure.”

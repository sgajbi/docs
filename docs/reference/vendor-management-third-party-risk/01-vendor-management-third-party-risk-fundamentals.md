# 01 - Vendor Management and Third-Party Risk Fundamentals

## 1. Why this matters in wealth platforms

Modern wealth platforms depend on many third parties:

- cloud providers
- market data vendors
- pricing vendors
- KYC / AML providers
- document generation providers
- PDF rendering engines
- CRM / workflow systems
- portfolio analytics engines
- AI / LLM providers
- custody and execution venues
- reference data sources
- tax and regulatory reporting providers
- SaaS observability, ticketing and security tooling

In a bank, every third-party dependency creates risk. The risk is not only whether the vendor can deliver a feature. It includes whether the vendor can protect data, maintain resilience, survive commercially, support audits, meet regulatory expectations and exit cleanly if required.

## 2. Vendor management versus outsourcing

Not every vendor relationship is outsourcing.

| Term | Meaning |
|---|---|
| Vendor / supplier | Provides product or service to the bank |
| Third party | Any external party supporting bank activity |
| Outsourcing | Third party performs an activity that would otherwise be performed by the institution itself |
| Critical outsourcing | Outsourcing that can materially affect operations, clients, compliance or financial stability |
| ICT third-party service | Technology service provider, including cloud, SaaS, data, hosting, security or platform services |
| Intragroup outsourcing | Service provided by another entity within the same group |
| Fourth party | Vendor's subcontractor or downstream provider |

## 3. Common risk categories

| Risk | Example |
|---|---|
| Information security risk | Vendor has access to client data but weak controls |
| Privacy risk | Data processed in a jurisdiction not approved for that client or booking centre |
| Operational risk | Vendor outage blocks portfolio reporting or order capture |
| Cyber risk | Vendor compromise exposes credentials, tokens or client metadata |
| Regulatory risk | Outsourcing arrangement lacks required audit and access rights |
| Concentration risk | Too much dependency on one cloud, market data or pricing provider |
| Financial risk | Vendor becomes insolvent or cannot continue support |
| Strategic risk | Vendor roadmap diverges from bank needs |
| Legal risk | Contract lacks liability, data, audit or exit protections |
| Reputational risk | Vendor failure affects client trust |
| AI/model risk | AI vendor produces unsupported, biased or hallucinated output |
| Data quality risk | Vendor data becomes stale, incomplete or inconsistent |

## 4. Risk-based approach

A bank should not assess all vendors equally. A low-risk office-supply vendor is different from an AI platform processing client portfolios.

Risk tiering should consider:

- criticality of service
- client impact
- regulatory impact
- data sensitivity
- access to production systems
- integration depth
- substitutability
- concentration
- financial and operational dependency
- cross-border data movement
- subcontractor chain
- use of cloud / AI / automation

## 5. Vendor lifecycle

The standard lifecycle is:

```text
Need identified
-> sourcing / RFP
-> initial risk assessment
-> due diligence
-> approval
-> contracting
-> onboarding
-> implementation
-> production use
-> ongoing monitoring
-> periodic review
-> issue / incident management
-> renewal or exit
-> offboarding and data deletion
```

## 6. Bank-buyable software mindset

A bank-buyable software product should be able to answer:

- What problem does it solve?
- Which data does it process?
- Where is data stored and processed?
- Who can access it?
- How is it secured?
- How is it monitored?
- How is it recovered?
- How is it tested?
- How is it audited?
- How can the bank exit?
- How are incidents handled?
- How are regulatory obligations supported?
- How is AI governed, if AI is used?
- How are changes, releases and vulnerabilities managed?

## 7. Wealth management-specific vendor risk

Wealth platforms have specific sensitivities:

| Area | Why sensitive |
|---|---|
| Client and portfolio data | Highly confidential and relationship-sensitive |
| Advisory recommendations | May trigger suitability, audit and conduct obligations |
| DPM mandates | Incorrect logic can breach mandate or policy |
| Product data | Wrong risk rating, restriction or eligibility can cause mis-selling |
| Pricing and valuation | Incorrect valuations affect statements and client decisions |
| Performance reporting | Misstated returns can damage trust and trigger complaints |
| AI copilots | Unsupported output can be interpreted as advice |
| Cross-border servicing | Data and product distribution rules differ by jurisdiction |
| Outsourced operations | Breaks can block reporting, settlement or client service |

## 8. Key takeaway

Vendor management is not procurement paperwork. It is an operating control that protects clients, bank resilience and regulatory accountability.

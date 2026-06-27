<!--
Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems Reference Pack
Generated: 2026-06-27
Purpose: Professional study, architecture, platform design, project delivery, advisory/reporting/analytics integration, and future reference.
Disclaimer: Educational and architecture reference only. Not cloud, security, regulatory, legal, or investment advice.
-->

# 04 - Networking, Ingress, Service Mesh, Private Connectivity and API Gateway

## 1. Why networking is critical in wealth platforms

Wealth platforms integrate with many systems:

- client master
- product master
- market data
- pricing
- transactions
- positions
- collateral/exposure
- OMS/trading
- document archive
- reporting/PDF engines
- AI model gateways
- external data vendors

Most production failures are not pure code failures. They often involve network routing, firewall rules, DNS, certificates, proxy behavior, endpoint policies, dependency timeouts or cross-zone connectivity.

## 2. Kubernetes network model

Key assumptions:

- each pod gets an IP
- pods communicate over the cluster network
- services provide stable virtual endpoints
- DNS resolves service names
- ingress/gateway routes external traffic
- network policy controls allowed traffic where supported/enforced

## 3. Ingress and API gateway distinction

| Layer | Purpose |
|---|---|
| Ingress controller / Gateway API | Routes traffic into Kubernetes services |
| API gateway | API governance, authentication, rate limits, routing, request policy, developer portal |
| Service mesh | East-west traffic, mTLS, retries, traffic splitting, observability |
| Load balancer | L4/L7 traffic distribution |
| WAF | Web application protection |
| Private endpoint | Private connectivity to managed cloud service |
| Egress gateway/proxy | Controlled outbound access |

In enterprise wealth architecture, API gateway and Kubernetes ingress are not the same thing.

## 4. North-south and east-west traffic

| Traffic type | Description | Example |
|---|---|---|
| North-south | User/system traffic entering or leaving cluster | Advisor UI -> portfolio API |
| East-west | Service-to-service traffic inside platform | holdings API -> valuation service |
| Egress | Cluster workload calling external system | product service -> market-data API |
| Private ingress | Internal network-only entry | CAWB -> CRDS APIs |
| Public ingress | Internet-facing entry | client portal/mobile app |

## 5. Private connectivity

For bank systems, prefer private connectivity:

- private clusters
- private endpoints
- private link
- VNET peering
- VPN / ExpressRoute / direct connect
- firewall allowlists
- DNS private zones
- controlled outbound proxy
- no public database endpoints
- no direct internet egress from sensitive workloads

## 6. Network segmentation

Segment by:

- environment
- application domain
- data sensitivity
- internet-facing versus internal
- production versus non-production
- country/booking centre when needed
- AI model/data boundary
- platform shared services

Example:

```text
advisor-ui -> api-gateway -> portfolio-api -> portfolio-db
                              -> performance-api -> analytics-store
                              -> reporting-api -> renderer -> archive
```

Network policies should permit only required flows.

## 7. Service mesh

A service mesh can provide:

- mTLS service-to-service encryption
- traffic policy
- retries/timeouts
- circuit breaking
- traffic shadowing
- canary routing
- observability
- service identity
- policy enforcement

But it also adds complexity. Use it when there is clear operational value, not because it is fashionable.

## 8. API gateway design

For wealth APIs, gateway controls should include:

- authentication and token validation
- entitlement propagation
- request throttling
- per-client rate limits
- request/response size limits
- correlation ID injection
- audit logging
- version routing
- policy enforcement
- API product catalog
- traffic analytics
- IP allowlists where appropriate

## 9. Timeout and retry patterns

Bad retry design can overload downstream systems.

| Control | Good practice |
|---|---|
| Timeout | Set explicit client/server timeouts |
| Retry | Retry only safe operations or idempotent writes |
| Backoff | Use exponential backoff with jitter |
| Circuit breaker | Stop calling failing dependency temporarily |
| Bulkhead | Isolate resource pools |
| Fallback | Degraded but honest output |
| Idempotency | Required for write retries |
| Trace | Every retry should be visible in traces/logs |

## 10. DNS and certificate controls

Common production issues:

- wrong DNS zone
- expired certificate
- missing SAN
- trust-store mismatch
- mTLS policy mismatch
- environment endpoint confusion
- private DNS not linked
- stale service discovery
- certificate rotation not tested

Controls:

- certificate inventory
- automated renewal
- mTLS expiry monitoring
- endpoint registry
- environment endpoint validation
- synthetic connectivity checks

## 11. Wealth-specific network examples

| Scenario | Network requirement |
|---|---|
| Portfolio API used by internal UI | Private ingress, entitlement propagation |
| Client portal statement download | Public WAF + API gateway + document entitlement |
| Market data feed ingestion | Controlled vendor connectivity and file/API allowlist |
| AI copilot using model gateway | No direct client PII to unapproved model endpoint |
| Buying power calling credit systems | Low-latency private connectivity and timeout discipline |
| Reporting PDF renderer | Internal service connectivity + archive write path |
| Reconciliation jobs | Access to source/custodian feeds and break store |

## 12. Practitioner summary

A strong answer:

> In enterprise Kubernetes platforms, networking must be designed as part of the domain architecture. Ingress, API gateways, service mesh, private endpoints, DNS, certificates and egress controls must be explicit. For wealth systems, the most important controls are private connectivity, entitlement propagation, dependency timeouts, traceability, network segmentation, and safe degradation when source systems are unavailable.

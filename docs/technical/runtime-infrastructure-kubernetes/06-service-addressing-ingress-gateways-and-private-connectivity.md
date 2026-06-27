# Service Addressing, Ingress, Gateways, and Private Connectivity

## Purpose

This file explains how services are addressed and exposed in enterprise runtime platforms.

Networking design determines how users, services, gateways, private systems, and operations tools reach workloads safely.

---

## Addressing Layers

| Layer | Example |
|---|---|
| Local process | `http://127.0.0.1:8000` |
| Local canonical host | `http://service.dev.local` |
| Cluster service | `http://service-name.namespace.svc` |
| Internal gateway | `https://internal-api.example` |
| External gateway | `https://api.example` |
| Private endpoint | Cloud-private service endpoint |
| Service mesh name | Mesh-managed identity/route |

---

## Service Discovery

Inside Kubernetes, services provide stable names for pods.

Example:

```text
http://portfolio-api:8080
```

Do not call pod IPs directly.

---

## Ingress

Ingress controls HTTP routing into the cluster.

Ingress should define:

- hostname
- path routing
- TLS
- authentication integration where applicable
- rate limits where applicable
- backend service
- timeout settings
- body size limits
- security headers
- logging

---

## API Gateway

An API gateway may provide:

- authentication
- authorization integration
- rate limiting
- request validation
- routing
- response transformation
- observability
- traffic policies
- API publication
- developer portal integration

Do not confuse API gateway with domain ownership.

---

## Private Connectivity

Regulated platforms often require private connectivity to:

- databases
- event hubs
- storage
- identity services
- source systems
- market-data systems
- internal APIs

Private connectivity considerations:

- DNS
- firewall rules
- subnet routes
- private endpoints
- service endpoints
- network security groups
- outbound restrictions
- certificate trust
- connection monitoring

---

## Network Policy

Network policy restricts traffic.

Good network policy answers:

- which pods can call this service?
- which namespaces are allowed?
- which external endpoints are allowed?
- which ports are open?
- is egress restricted?

---

## Addressing Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Hardcoded localhost in container | Fails in cluster. |
| Pod IP used directly | Breaks on restart. |
| UI calls internal service directly | Bypasses gateway and security. |
| Public endpoint for private dependency | Security exposure. |
| No egress control | Data exfiltration risk. |
| Inconsistent local hostnames | Developer and CI drift. |
| Gateway recomputes domain truth | Boundary violation. |

---

## Review Checklist

- Are service names documented?
- Is ingress required?
- Is hostname stable?
- Is TLS handled?
- Are timeouts configured?
- Is private connectivity required?
- Is DNS documented?
- Are network policies defined?
- Is egress restricted?
- Are local and cluster addresses separate?
- Are gateway and domain responsibilities clear?

---

## Summary

Service addressing is part of architecture.

A service should be reachable by the right callers, through the right path, with the right security and observability controls.

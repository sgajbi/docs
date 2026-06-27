# Runtime, Infrastructure, Cloud, Container, and DevOps Migration

## Purpose

This file explains migration from one runtime or infrastructure platform to another.

Replatforming changes how services are built, deployed, configured, monitored, secured, and supported.

---

## Runtime Migration Scope

Scope may include:

- containerization
- Kubernetes/OpenShift/AKS deployment
- CI/CD workflows
- artifact repository
- secrets manager
- identity/workload identity
- networking
- ingress
- DNS
- certificates
- logging
- metrics
- tracing
- dashboards
- alerts
- backup/restore
- DR

---

## Containerization

Validate:

- Dockerfile
- image build
- startup command
- health/readiness
- environment variables
- secrets
- non-root execution
- image scanning
- Docker smoke

---

## Platform Migration

Consider:

- namespace/project structure
- resource requests/limits
- network policies
- service accounts
- ConfigMaps/Secrets
- ingress/route
- autoscaling
- persistent storage
- platform-specific constraints

---

## DevOps Migration

CI/CD migration includes:

- build pipeline
- test gates
- security scans
- deployment workflows
- environment promotion
- release evidence
- rollback automation
- runner permissions
- artifact retention

---

## Runtime Anti-Patterns

| Anti-Pattern | Impact |
|---|---|
| App containerized without readiness | Traffic routing risk. |
| Secrets copied manually | Security risk. |
| CI/CD not migrated with app | Release gap. |
| Dashboards recreated after cutover | Support blind. |
| Resource limits guessed | Stability/cost issue. |
| Network connectivity tested late | Cutover blocker. |
| DR not included | Resilience gap. |

---

## Review Checklist

- Is runtime scope complete?
- Is container build tested?
- Are configs/secrets migrated?
- Are health/readiness meaningful?
- Are network paths tested?
- Are CI/CD workflows ready?
- Are dashboards/alerts ready?
- Are rollback workflows ready?
- Are platform constraints documented?
- Is runtime evidence captured?

---

## Summary

Replatforming changes the operating model.

Treat runtime migration as engineering transformation, not only hosting change.

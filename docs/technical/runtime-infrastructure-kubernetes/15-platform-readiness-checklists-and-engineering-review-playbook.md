# Platform Readiness Checklists and Engineering Review Playbook

## Purpose

This file provides practical checklists for reviewing runtime, infrastructure, containers, Kubernetes/OpenShift/AKS, deployment, security, observability, resilience, and production readiness.

Use it for architecture reviews, PR reviews, release readiness, platform onboarding, and engineering leadership assessments.

---

## Runtime Contract Checklist

- [ ] Service name is defined.
- [ ] Service type is defined.
- [ ] Container image is defined.
- [ ] Startup command is documented.
- [ ] Ports are documented.
- [ ] Health endpoint exists.
- [ ] Liveness endpoint exists where applicable.
- [ ] Readiness endpoint exists and is meaningful.
- [ ] Metrics endpoint exists.
- [ ] Required configuration is documented.
- [ ] Secrets are externalized.
- [ ] Dependencies are documented.
- [ ] Runbook exists.

---

## Container Checklist

- [ ] Dockerfile exists.
- [ ] Base image is approved.
- [ ] Image is minimal.
- [ ] `.dockerignore` excludes local artifacts.
- [ ] Secrets are not baked in.
- [ ] Container runs as non-root where possible.
- [ ] Startup command works.
- [ ] Image builds in CI.
- [ ] Image scan runs.
- [ ] SBOM/provenance generated where required.

---

## Local Runtime Checklist

- [ ] Local process run command documented.
- [ ] Docker run or Compose available where useful.
- [ ] Ports and hostnames documented.
- [ ] Dependencies documented.
- [ ] Seed data is synthetic.
- [ ] Migration command documented.
- [ ] Cleanup command safe.
- [ ] Local limitations documented.
- [ ] New developer can start service.

---

## Kubernetes Workload Checklist

- [ ] Workload type is correct.
- [ ] Replicas defined.
- [ ] Resources defined.
- [ ] Probes defined.
- [ ] ConfigMaps/Secrets defined.
- [ ] Service account defined.
- [ ] Network policy considered.
- [ ] Graceful shutdown handled.
- [ ] Autoscaling considered.
- [ ] Rollout strategy defined.

---

## Ingress and Connectivity Checklist

- [ ] Service DNS documented.
- [ ] Ingress required and defined.
- [ ] TLS handled.
- [ ] API gateway rules documented.
- [ ] Private endpoints considered.
- [ ] Egress restrictions considered.
- [ ] Timeouts configured.
- [ ] Local and cluster identities separated.
- [ ] Connectivity troubleshooting documented.

---

## Configuration and Secrets Checklist

- [ ] Configuration typed.
- [ ] Required variables documented.
- [ ] Defaults safe.
- [ ] Profiles documented.
- [ ] Secrets externalized.
- [ ] Secrets redacted.
- [ ] Diagnostic config safe.
- [ ] Feature flags owned.
- [ ] Missing config fails fast where appropriate.

---

## Probe and Diagnostics Checklist

- [ ] Health endpoint lightweight.
- [ ] Liveness does not depend on transient dependencies.
- [ ] Readiness checks required dependencies.
- [ ] Startup probe considered.
- [ ] Diagnostics protected.
- [ ] Diagnostics source-safe.
- [ ] Probe thresholds reasonable.
- [ ] Readiness failure monitored.

---

## Resource and Scaling Checklist

- [ ] CPU request defined.
- [ ] Memory request defined.
- [ ] CPU limit defined where policy requires.
- [ ] Memory limit defined.
- [ ] Autoscaling rule defined if needed.
- [ ] Dependency capacity considered.
- [ ] Connection pool sized.
- [ ] Batch windows considered.
- [ ] Saturation metrics monitored.
- [ ] Known limits documented.

---

## Stateful Dependency Checklist

- [ ] Database connectivity documented.
- [ ] Migration plan defined.
- [ ] Backup/restore defined.
- [ ] Cache TTL/invalidation defined.
- [ ] Queue retry/dead-letter defined.
- [ ] Stream consumer lag monitored.
- [ ] Object storage access controlled.
- [ ] Dependency failures represented in readiness/supportability.
- [ ] Runbook covers dependency failure.

---

## Deployment and Rollback Checklist

- [ ] Deployment strategy defined.
- [ ] Backward compatibility assessed.
- [ ] Migration compatibility assessed.
- [ ] Feature flags used where needed.
- [ ] Rollback plan documented.
- [ ] Post-deploy checks defined.
- [ ] Monitoring updated.
- [ ] Support handoff complete.
- [ ] Release activation audited.

---

## Runtime Security Checklist

- [ ] Least-privilege service account.
- [ ] RBAC scoped.
- [ ] Network policy defined.
- [ ] Workload identity used where possible.
- [ ] Secrets safe.
- [ ] Trusted image registry.
- [ ] Non-root runtime where possible.
- [ ] No privilege escalation.
- [ ] Egress controlled.
- [ ] Runtime security events observable.

---

## Observability Checklist

- [ ] Structured logs.
- [ ] Metrics exposed.
- [ ] Trace propagation.
- [ ] Pod/container metrics.
- [ ] Restart monitoring.
- [ ] Readiness failure dashboard.
- [ ] Dependency health dashboard.
- [ ] Alerts actionable.
- [ ] Runbooks linked.
- [ ] Diagnostics safe.

---

## Resilience and DR Checklist

- [ ] Availability requirement defined.
- [ ] RPO/RTO defined where stateful.
- [ ] Backup configured.
- [ ] Restore tested.
- [ ] DR plan documented.
- [ ] Capacity headroom understood.
- [ ] Retry/backoff safe.
- [ ] Cost controls reviewed.
- [ ] Retention policy defined.
- [ ] Operational automation considered.

---

## Engineering Review Questions

- Can this service run outside a developer laptop?
- What makes readiness true?
- What happens if the database is down?
- What happens if an optional dependency is stale?
- How does the service shut down?
- How is it scaled?
- What is the rollback plan?
- What secrets does it need?
- What metrics show saturation?
- What does support do during failure?
- What evidence proves the runtime works?

---

## Summary

Platform readiness is not a one-time deployment step.

It is the discipline of making services reproducible, secure, observable, scalable, recoverable, and supportable.

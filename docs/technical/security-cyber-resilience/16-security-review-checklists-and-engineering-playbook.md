# Security Review Checklists and Engineering Playbook

## Purpose

This file provides practical checklists for security design reviews, PR reviews, production-readiness reviews, and operational maturity assessments.

Use these checklists to make security review repeatable and evidence-based.

---

## Secure Design Checklist

- [ ] Capability and data flow are documented.
- [ ] Trust boundaries are clear.
- [ ] Sensitive data is classified.
- [ ] Authentication model is defined.
- [ ] Authorization model is defined.
- [ ] Caller context is typed.
- [ ] Entitlement scope is defined.
- [ ] Secrets are externalized.
- [ ] Logging boundaries are defined.
- [ ] Threat model or abuse cases exist.
- [ ] Residual risk documented.

---

## API Security Checklist

- [ ] Server-side authorization enforced.
- [ ] Caller context validated.
- [ ] Permission-blocked response safe.
- [ ] Errors source-safe.
- [ ] Idempotency required for write actions.
- [ ] Pagination bounded.
- [ ] Input validation complete.
- [ ] Output filtering enforced.
- [ ] Audit events for sensitive actions.
- [ ] API security tests exist.

---

## Secrets and Configuration Checklist

- [ ] No secrets in source.
- [ ] No secrets in docs.
- [ ] No secrets in tests.
- [ ] No secrets in images.
- [ ] No secrets in logs/artifacts.
- [ ] Config is typed.
- [ ] Required config validated.
- [ ] Defaults safe.
- [ ] Redaction implemented.
- [ ] Rotation process defined.

---

## Sensitive Data Checklist

- [ ] Data classified.
- [ ] Test data synthetic.
- [ ] Logs safe.
- [ ] Metrics safe.
- [ ] Traces safe.
- [ ] Diagnostics safe.
- [ ] Screenshots safe.
- [ ] Evidence filtered.
- [ ] AI prompts/responses governed.
- [ ] Retention defined.

---

## DevSecOps Checklist

- [ ] SAST or static security scan runs.
- [ ] Dependency scan runs.
- [ ] Secrets scan runs.
- [ ] Container scan runs.
- [ ] SBOM generated where required.
- [ ] Provenance captured where required.
- [ ] Workflow permissions least privilege.
- [ ] Actions pinned.
- [ ] Vulnerability triage process exists.
- [ ] Exceptions time-bound.

---

## Runtime Security Checklist

- [ ] Workload service account scoped.
- [ ] RBAC least privilege.
- [ ] Network policy defined.
- [ ] Workload identity used where possible.
- [ ] Secrets mounted only where needed.
- [ ] Container non-root where possible.
- [ ] Image trusted and scanned.
- [ ] Egress controlled.
- [ ] Runtime security events monitored.
- [ ] Incident runbook exists.

---

## Observability Security Checklist

- [ ] Logs structured and source-safe.
- [ ] Metrics labels bounded.
- [ ] Trace attributes safe.
- [ ] Diagnostics protected.
- [ ] Dashboards filtered.
- [ ] Alerts source-safe.
- [ ] Runbook examples synthetic.
- [ ] CI artifacts checked.
- [ ] No-sensitive-content gate exists.
- [ ] Evidence packs filtered.

---

## AI Security Checklist

- [ ] Approved knowledge sources.
- [ ] Entitlement-aware retrieval.
- [ ] Prompt injection considered.
- [ ] Prompts source-safe.
- [ ] Outputs grounded.
- [ ] Tool calls authorized.
- [ ] Human review required for sensitive actions.
- [ ] Model-risk evidence captured.
- [ ] Prompts/responses not logged raw.
- [ ] Unsupported use blocked.

---

## Audit and Evidence Checklist

- [ ] Risks mapped to controls.
- [ ] Controls mapped to implementation.
- [ ] Tests/gates prove controls.
- [ ] Evidence current.
- [ ] Evidence safe for audience.
- [ ] Owners defined.
- [ ] Review cadence defined.
- [ ] Exceptions documented.
- [ ] Residual risk recorded.
- [ ] Procurement summary accurate.

---

## Production Security Readiness Checklist

- [ ] Threat model complete.
- [ ] Security tests pass.
- [ ] Dependency findings triaged.
- [ ] Container findings triaged.
- [ ] Secrets verified.
- [ ] Runtime permissions reviewed.
- [ ] Logs/metrics/traces safe.
- [ ] Incident runbook available.
- [ ] BCP/DR posture known.
- [ ] Residual risk accepted or remediated.

---

## Engineering Playbook

### Before Build

1. Classify data.
2. Identify trust boundaries.
3. Define identity and authorization.
4. Define secrets and config.
5. Write abuse cases.
6. Define evidence.

### During Build

1. Enforce server-side access.
2. Keep logs/metrics/traces safe.
3. Add security tests.
4. Use typed configuration.
5. Avoid secrets in artifacts.
6. Add audit for sensitive actions.

### Before Merge

1. Run security gates.
2. Review workflow permissions.
3. Check sensitive content.
4. Update docs and runbooks.
5. Record residual risk.

### Before Release

1. Review vulnerability posture.
2. Confirm runtime security.
3. Confirm incident runbook.
4. Confirm evidence.
5. Confirm rollback/recovery.

---

## Summary

Security review should be practical, repeatable, and evidence-backed.

The goal is not to create paperwork. The goal is to make secure engineering the normal way of building.

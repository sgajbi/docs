# Runtime Security, RBAC, Network Policy, and Workload Identity

## Purpose

This file explains runtime security controls for containerized enterprise services.

Runtime security protects workloads after code is built and deployed.

---

## Runtime Security Areas

- service accounts
- RBAC
- workload identity
- secrets
- network policy
- pod security
- image security
- filesystem restrictions
- egress controls
- ingress controls
- audit logging

---

## Service Accounts and RBAC

Workloads should run with the least privilege needed.

Avoid:

- default service account with broad permissions
- cluster-admin bindings
- shared credentials across workloads
- write permissions for read-only services
- unused permissions

---

## Workload Identity

Workload identity allows service-to-cloud-resource access without static secrets.

Benefits:

- fewer long-lived secrets
- scoped permissions
- better rotation
- stronger audit

Use where platform supports it.

---

## Network Policy

Network policy should restrict:

- which services can call this workload
- which dependencies this workload can call
- which namespaces are allowed
- which ports are open
- outbound internet access

---

## Pod Security

Controls may include:

- non-root user
- read-only root filesystem
- no privilege escalation
- restricted capabilities
- seccomp/apparmor profiles
- image pull policy
- resource limits
- trusted registry

---

## Runtime Security Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Runs as root unnecessarily | Higher compromise impact. |
| Broad RBAC | Lateral movement. |
| No network policy | Unrestricted service access. |
| Secrets as plain env in logs | Leakage. |
| Public endpoint for internal service | Exposure. |
| Untrusted image registry | Supply-chain risk. |
| Shell/debug tools in production image | Attack surface. |

---

## Review Checklist

- Does workload use least-privilege service account?
- Are RBAC permissions scoped?
- Is workload identity used where possible?
- Are secrets externalized?
- Is network policy defined?
- Is egress controlled?
- Is image from trusted registry?
- Does container run as non-root?
- Are privileged settings avoided?
- Are runtime security events observable?

---

## Summary

Runtime security assumes that deployed workloads need guardrails.

Secure code is not enough if runtime permissions, networks, secrets, and images are weak.

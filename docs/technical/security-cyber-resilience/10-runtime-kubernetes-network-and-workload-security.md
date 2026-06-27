# Runtime, Kubernetes, Network, and Workload Security

## Purpose

This file explains security controls for containerized runtime platforms such as Kubernetes, OpenShift, and AKS.

Runtime security protects services after deployment.

---

## Runtime Security Areas

- service accounts
- RBAC
- workload identity
- secrets
- network policy
- ingress policy
- egress control
- pod security
- image trust
- namespace isolation
- runtime monitoring

---

## Least Privilege

Workloads should have only the permissions they need.

Avoid:

- default broad service account
- cluster-admin bindings
- shared service identities
- write permissions for read-only workloads
- unnecessary secret access
- broad network access

---

## Network Policy

Network policy should restrict:

- who can call the service
- what the service can call
- which ports are open
- which namespaces are allowed
- whether internet egress is allowed
- access to sensitive dependencies

---

## Workload Identity

Workload identity reduces static secrets.

Benefits:

- short-lived credentials
- scoped cloud permissions
- improved rotation
- better audit
- fewer secrets in environment

---

## Pod Security

Controls:

- run as non-root
- no privilege escalation
- drop capabilities
- read-only root filesystem where possible
- seccomp/apparmor where available
- trusted images
- resource limits
- restricted volume mounts

---

## Runtime Anti-Patterns

| Anti-Pattern | Risk |
|---|---|
| Runs as root unnecessarily | Higher compromise impact. |
| No network policy | Lateral movement. |
| Broad RBAC | Privilege escalation. |
| Secrets mounted into all pods | Exposure. |
| Public ingress for internal service | Attack surface. |
| Egress unrestricted | Data exfiltration. |
| Debug tools in production image | Attack surface. |

---

## Review Checklist

- Is service account scoped?
- Are RBAC permissions minimal?
- Is workload identity used where possible?
- Are secrets restricted?
- Is ingress controlled?
- Is egress controlled?
- Is network policy defined?
- Is pod security hardened?
- Are images trusted and scanned?
- Are runtime security events monitored?

---

## Summary

Runtime security assumes compromise is possible and limits blast radius through identity, network, privilege, and policy controls.

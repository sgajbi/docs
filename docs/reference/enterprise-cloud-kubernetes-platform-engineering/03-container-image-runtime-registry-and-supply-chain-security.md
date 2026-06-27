<!--
Enterprise Cloud, Kubernetes, Resilience and Platform Engineering for Wealth Systems Reference Pack
Generated: 2026-06-27
Purpose: Professional study, architecture, platform design, project delivery, advisory/reporting/analytics integration, and future reference.
Disclaimer: Educational and architecture reference only. Not cloud, security, regulatory, legal, or investment advice.
-->

# 03 - Container Images, Runtime, Registry and Supply-Chain Security

## 1. Why container supply chain matters

In a bank, a container image is not just a deployment artifact. It is a controlled software package containing:

- application code
- runtime dependencies
- operating-system packages
- certificates and trust stores
- startup commands
- library versions
- sometimes model artifacts or static assets

If the image is compromised, the platform can be compromised.

## 2. Container image lifecycle

```text
Source code
  -> dependency resolution
  -> build
  -> unit/security tests
  -> image creation
  -> vulnerability scan
  -> SBOM generation
  -> signing/attestation
  -> registry push
  -> deployment approval
  -> runtime policy admission
  -> runtime monitoring
```

## 3. Image design principles

| Principle | Good practice |
|---|---|
| Minimal base | Use small, approved base images |
| Reproducible build | Pin dependencies and versions |
| Non-root runtime | Run as non-root user |
| Immutable image | No mutation after build |
| No secrets | Never bake secrets into image |
| Separate build/runtime | Multi-stage builds |
| Metadata | Include labels: version, commit, build time |
| Scanning | Scan OS and application dependencies |
| Signing | Sign images and verify in admission |
| SBOM | Generate software bill of materials |

## 4. Dockerfile guidance

Good Dockerfile pattern:

```dockerfile
FROM python:3.12-slim AS runtime

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

RUN groupadd -r app && useradd -r -g app app

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY src/ ./src/
USER app

EXPOSE 8080
CMD ["python", "-m", "src.main"]
```

Avoid:

- `latest` tags
- root user
- installing debug tools in production image
- copying local `.env` files
- embedding certificates or keys
- package managers left in final image when unnecessary
- unpinned dependencies

## 5. Registry controls

| Control | Purpose |
|---|---|
| Private registry | Prevent unauthorized image distribution |
| Immutable tags | Prevent tag overwrite |
| Vulnerability scanning | Detect known CVEs |
| Image signing | Verify provenance |
| Promotion flow | dev -> test -> uat -> prod |
| Retention policy | Avoid uncontrolled artifact sprawl |
| Access logs | Audit image push/pull |
| Allowlist | Deploy only approved registries/images |

## 6. SBOM and provenance

An SBOM should answer:

- what packages are in the image?
- what versions are used?
- which source commit created it?
- which pipeline built it?
- which vulnerabilities are known?
- who approved promotion?
- which environments are running it?

For regulated wealth platforms, SBOM and provenance matter for audit, incident response and vulnerability remediation.

## 7. Runtime security

| Area | Control |
|---|---|
| User | Run as non-root |
| Filesystem | Read-only root filesystem when possible |
| Capabilities | Drop unnecessary Linux capabilities |
| Privilege | Disallow privileged containers |
| Host access | Avoid hostPath, hostNetwork, hostPID |
| Seccomp/AppArmor | Use restrictive profiles |
| Network | Apply network policies |
| Secrets | Mount only required secrets |
| Egress | Restrict outbound traffic |
| Admission | Enforce policies before deployment |
| Runtime detection | Monitor suspicious process/network behavior |

## 8. NIST container security categories

NIST SP 800-190 frames container security risk across layers such as images, registries, orchestrators, containers and host operating systems. For wealth platforms, map these to controls:

| Layer | Wealth control |
|---|---|
| Image | approved base, scan, sign, SBOM |
| Registry | private registry, immutable promotion |
| Orchestrator | RBAC, policy, admission, audit |
| Container | non-root, restricted capabilities |
| Host | hardened node image, patching |
| Application | auth, validation, secrets, observability |

## 9. Wealth-specific examples

| Workload | Supply-chain concern |
|---|---|
| Pricing service | wrong library version can change calculations |
| Performance engine | numeric dependency change can alter returns |
| Report renderer | PDF library vulnerability can expose data |
| RAG service | model/embedding package version affects retrieval |
| Buying power service | rule-engine artifact must be controlled |
| Market data consumer | parser dependency can create ingestion failures |

## 10. Controls for AI images

AI workloads add additional artifact controls:

- model version
- embedding model version
- prompt templates
- RAG index version
- evaluation dataset version
- guardrail configuration
- model gateway configuration
- tool/action allowlist
- prompt-injection filters
- client-data isolation policy

## 11. Release gate checklist

Before production deployment:

- image built by approved pipeline
- image scanned with no critical unresolved vulnerabilities
- SBOM generated and stored
- image signed
- image provenance linked to commit and PR
- dependency license check completed
- secrets not present in image
- runtime user non-root
- Kubernetes manifests pass policy checks
- rollback image exists
- deployment ticket references image digest, not mutable tag

## 12. Practitioner summary

A strong answer:

> Container security is a supply-chain problem, not just a runtime problem. In a bank-grade Kubernetes platform, images should be built from approved bases, scanned, signed, linked to SBOM/provenance, promoted through controlled environments and admitted only if they pass policy. Runtime controls should enforce non-root, least privilege, restricted networking, secret isolation, observability and audit.

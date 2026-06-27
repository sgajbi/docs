# Data Protection, Privacy, Encryption, Tokenization and Key Management

## 1. Data protection in wealth platforms

Wealth data is highly sensitive because it reveals identity, assets, family structures, risk appetite, tax status, product holdings, trading behavior and advisory decisions.

Protected data includes:

- client identity and contact data
- KYC, AML and tax residency information
- beneficial ownership and household relationships
- account balances and holdings
- transactions and cashflows
- performance, risk, suitability and mandate data
- client reports and statements
- insurance, trust and estate-planning details
- digital consent and interaction history
- AI prompts, retrieved context and generated outputs
- operations evidence and incident records

## 2. Data classification

| Classification | Example | Controls |
|---|---|---|
| Public | Public product brochure | Integrity and brand control |
| Internal | Technical runbook without client data | Access limited to employees/contractors |
| Confidential | Product governance decisions, pricing models | Need-to-know access |
| Restricted | Client holdings, tax status, account statements | Strong encryption, monitoring, DLP |
| Highly restricted | Credentials, secrets, private keys, privileged logs | Vault, HSM/KMS, no human visibility |

## 3. Data lifecycle controls

| Stage | Controls |
|---|---|
| Collection | Purpose limitation, consent/legal basis, minimization |
| Ingestion | Schema validation, source authentication, malware scanning for files |
| Storage | Encryption, access controls, retention policy |
| Processing | Data masking, purpose scoping, lineage |
| Transmission | TLS/mTLS, integrity checks, secure file transfer |
| Reporting | Watermarking, access logging, distribution controls |
| AI retrieval | RAG authorization, redaction, prompt logging policy |
| Archival | Retention classification and legal hold |
| Disposal | Secure deletion and evidence of destruction |

## 4. Encryption controls

| Layer | Recommended treatment |
|---|---|
| Data in transit | TLS 1.2+ / TLS 1.3; mTLS for service-to-service where appropriate |
| Data at rest | Database, object storage, volume and backup encryption |
| Field-level sensitive data | Encrypt or tokenize where access is rare or highly sensitive |
| Reports/documents | Encryption at rest, controlled download, secure delivery |
| Logs | Avoid secrets/PII; encrypt log stores; restricted access |
| Backups | Encrypt, isolate and periodically restore-test |
| Secrets | Never stored as normal config; use vault/KMS/secret manager |

## 5. Key management

Key management is often more important than encryption itself.

Controls:

- keys generated and stored in approved KMS/HSM/secrets manager
- key owners and custodians defined
- separation between key administration and data administration
- key rotation policy
- emergency revocation process
- audit logs for key use and administrative actions
- key material not exposed to applications unnecessarily
- backup and recovery for keys
- crypto-agility plan for algorithm changes and future quantum-safe transition

## 6. Tokenization and masking

| Technique | Use |
|---|---|
| Static masking | Lower environments and test data |
| Dynamic masking | Support users see partial values only |
| Tokenization | Replace sensitive value with reversible token under controlled access |
| Hashing | Non-reversible comparison, e.g. deduplication |
| Pseudonymization | Analytics without direct identifiers |
| Redaction | Remove sensitive values from reports, logs or AI context |

Example:

```text
Client name: Sandeep Gajbi -> Client name: S****** G****
Account number: 1234567890 -> Account number: ******7890
Tax ID: sensitive -> token: tok_9f8a...
```

## 7. Logging and sensitive data

Security logs must be useful without becoming a data-leak source.

Do log:

- user ID, role, action, resource ID, decision, timestamp
- policy version and correlation ID
- approval status
- error category
- source system and request metadata

Do not log:

- passwords, tokens, API keys
- full account numbers where not needed
- full tax identifiers
- private keys or secrets
- raw prompts containing excessive client data
- full statements or report payloads

## 8. Data protection for AI

AI systems introduce new data paths:

```text
source document -> embedding -> vector store -> retrieval -> prompt/context -> model output -> tool call -> audit log
```

Controls:

- classify documents before indexing
- preserve document-level entitlement metadata in vector store
- retrieve only documents the user is allowed to access
- redact client identifiers where not necessary
- separate public product knowledge from restricted client data
- log prompt/output metadata without leaking sensitive content
- prevent model from training on confidential prompts unless explicitly approved
- validate generated outputs before client/advisor use

## 9. Privacy-by-design checklist

- What data is strictly required?
- What is the purpose?
- Who can view it?
- Can it be masked or tokenized?
- How long is it retained?
- Can the client/access subject request correction or deletion where applicable?
- Does the data cross border?
- Does any AI component process it?
- Is the data visible in logs, reports, prompts or analytics exports?
- Is there an audit trail for access and disclosure?

## 10. Common data protection failures

| Failure | Consequence |
|---|---|
| Production data copied into dev/test | Privacy and breach risk |
| Client reports stored in uncontrolled folder | Unauthorized access |
| Full PII in application logs | Log platform becomes sensitive data store |
| Embeddings built without entitlement metadata | RAG leaks restricted documents |
| Long-lived encryption keys with no rotation | Increased blast radius |
| Secrets in Git repository | Credential compromise |
| Data exports not watermarked | Poor traceability after leakage |


# Integration Frameworks for Ellucian Cloud Migration

> Standardized, repeatable templates for integrating campus systems with Banner in **Ellucian Cloud** using:
> 1) **Ellucian Ethos**, 2) **Direct APIs via MuleSoft/iPaaS**, and 3) **AWS SFTP/Flat‑File** for vendors without APIs.

---

## Table of Contents

- [Purpose](#purpose)
- [Scope & Audience](#scope--audience)
- [Decision Tree](#decision-tree)
- [Framework 1 — Ellucian Ethos (Low Complexity)](#framework-1--ellucian-ethos-low-complexity)
- [Framework 2 — API via MuleSoft (Medium Complexity)](#framework-2--api-via-mulesoft-medium-complexity)
- [Framework 3 — AWS SFTP / Flat-File (High Complexity)](#framework-3--aws-sftp--flat-file-high-complexity)
- [Universal Integration Checklist](#universal-integration-checklist)
- [Security & Secrets](#security--secrets)
- [Operational Runbooks](#operational-runbooks)
- [Roadmap & Ownership](#roadmap--ownership)
- [License](#license)

---

## Purpose

Provide a **repeatable integration playbook** that clarifies *what our team owns* vs. *what Ellucian provides*, and accelerates delivery with three prebuilt frameworks:
- **Ethos** for Ellucian‑partnered solutions (low friction).
- **API via MuleSoft** for vendors that expose REST/SOAP.
- **AWS SFTP** for legacy/no‑API vendors.

> **Key Principle:** Ellucian hosts and maintains Banner in the cloud; **we own the integrations** (design, build, data mapping, credentials, and operations).

---

## Scope & Audience

- **Primary:** Integration engineers, MuleSoft developers, data engineers, and platform/SRE.
- **Secondary:** Product owners, project managers, vendor liaisons, InfoSec, IAM.

---

## Decision Tree

Use the following to select your framework quickly:

```
Is the app an Ellucian Partner and/or uses Ethos data models?
 ├─ Yes → Use Framework 1 (Ethos)
 └─ No
     ├─ Vendor has REST/SOAP APIs? → Use Framework 2 (API via MuleSoft)
     └─ No APIs → Use Framework 3 (AWS SFTP / Flat‑File)
```

**Complexity Heuristic**
- **LOW:** Ellucian partner + Ethos
- **MEDIUM:** Non‑partner but has APIs
- **HIGH:** No API; batch file/SFTP only

---

## Framework 1 — Ellucian Ethos (Low Complexity)

**When to use:** Ellucian partner solutions or apps mapped to **Ethos** resources.

### Steps

1. **Intake & Assessment**
   - Confirm partner status and needed Ethos data models (e.g., Person, Course, Section).
   - Define data direction (Inbound to Banner, Outbound, or bidirectional).

2. **Ethos Connectivity**
   - Register application in **Ethos Integration Layer**.
   - Generate API key and assign least‑privilege scopes.
   - Whitelist domains/IPs if required.

3. **Data Mapping**
   - Create mapping for **Vendor → Ethos** resources.
   - Define transformations (codes, IDs, dates, enumerations).

4. **Connectivity Tests**
   - Validate auth and basic reads/writes via Ethos tools.
   - Capture and document error payloads.

5. **End‑to‑End (E2E)**
   - Test create/update/query scenarios.
   - Verify downstream Banner behaviors (jobs, audit tables, events).

6. **Cutover**
   - Rotate to **production** keys.
   - Retire legacy feeds; update runbooks and on‑call alerts.

**Deliverables**
- `ethos-config.json` (app registration + scopes)
- `mapping-ethos.xlsx` (field maps & transforms)
- `test-scenarios.md` (E2E use cases & expected results)
- `runbook-ethos.md` (support, dashboards, alerts, rollbacks)

---

## Framework 2 — API via MuleSoft (Medium Complexity)

**When to use:** Vendor exposes **REST/SOAP** endpoints (e.g., Salesforce, Qualtrics, ApplicantStack, Common App, Handshake, OCLC, etc.).

### Steps

1. **Requirements**
   - Collect API docs.
   - Identify authentication (OAuth2, API Key, JWT, Basic), rate limits, pagination, versioning.
   - Confirm sandbox vs production endpoints.

2. **Credentials**
   - Request sandbox + prod credentials.
   - Store in secrets manager (e.g., Azure Key Vault / AWS Secrets Manager).
   - Define rotation policy (automated if possible).

3. **Architecture**
   - Choose integration platform (**MuleSoft** recommended).
   - Decide polling vs webhooks.
   - Establish canonical logging, tracing (correlation IDs), and alerting.

4. **Data Mapping**
   - Create **Source ↔ Target** mapping and transformation rules.
   - Document required/optional fields, enums, and validation constraints.

5. **Flow Design**
   - Implement **GET/POST/PATCH/PUT** as needed.
   - Add retry/backoff for 429/5xx, idempotency keys for creates, and dead‑letter handling.

6. **Testing**
   - Unit tests (mocks).
   - Sandbox E2E (success + error injection).
   - Performance tests within vendor limits.

7. **Deploy & Monitor**
   - Promote to prod; swap credentials.
   - Enable dashboards, SLIs/SLOs (latency, error %, throughput).
   - Create incident procedures and playbooks.

**Deliverables**
- `/mulesoft/` project with CI/CD pipeline
- `mapping-api.xlsx` (field maps)
- `postman_collection.json` (endpoint tests)
- `runbook-api.md` (on‑call procedures)

---

## Framework 3 — AWS SFTP / Flat‑File (High Complexity)

**When to use:** Vendor has **no API**; expects **SFTP** batch files (CSV, TSV, fixed‑width, XML).

### Steps

1. **AWS Transfer Family**
   - Provision SFTP endpoint.
   - Back with **S3** landing buckets (e.g., `incoming/`, `processed/`, `error/`, `archive/`).
   - Configure IAM roles, SCP/IP restrictions, and server‑side encryption.

2. **Vendor Coordination**
   - Exchange SSH keys or credentials.
   - Define **file naming**, schedule (cron), and **format specs**.
   - Agree on success/failure receipts (ACK/NACK file, email, or API callback if available).

3. **File Spec & Validation**
   - Author a strict **parsing spec** (columns, data types, max lengths, enums).
   - Define validation rules, quarantining logic, and schema drift detection.

4. **ETL / Orchestration**
   - Pipeline: `Vendor → SFTP → S3 → MuleSoft/ETL → Banner`.
   - Optional: PGP encryption/decryption.
   - Implement error routing (to `error/`), auto‑alerts, and reprocessing hooks.

5. **Testing**
   - Validate samples and malformed edge cases.
   - E2E test in non‑prod Banner.
   - Confirm backfills and partial loads.

6. **Cutover**
   - Switch vendor to prod endpoint.
   - 30‑day hyper‑care with heightened alerting and daily health checks.

**Deliverables**
- `aws-transfer.tf` (IaC for SFTP, S3, IAM) or CloudFormation
- `file-spec-vendor.xlsx` (schema & validation rules)
- `mulesoft-etl/` project
- `runbook-sftp.md` (paths, alerts, retry, restore)

---

## Universal Integration Checklist

- **Path Selected:** Ethos ☐ | API ☐ | SFTP ☐  
- **Docs:** API spec or file format received; sample payload/files collected  
- **Security:** Credentials in secrets manager; TLS/SFTP enforced; scopes least‑privilege; rotation plan set  
- **Mapping:** Source ↔ Target table complete; transformations documented; required/optional flagged  
- **Build:** MuleSoft flows; Ethos config; AWS SFTP endpoint & S3 buckets; IaC committed  
- **Testing:** Unit; E2E; error injection; performance where feasible  
- **Go‑Live:** Prod creds; dashboards/alerts; runbook finalized; vendor “ready” confirmation  
- **Day‑2 Ops:** 30‑day hyper‑care; weekly vendor sync; backlog of enhancements captured

---

## Security & Secrets

- Store **all** credentials in the approved secrets manager (no `.env` in repo).  
- Enforce least‑privilege scopes on Ethos/API keys; short TTL tokens preferred.  
- Enable transport encryption (HTTPS/TLS, SFTP with key‑based auth).  
- Define rotation policy (automation via CI/CD + secrets manager).

---

## Operational Runbooks

Each integration must include a runbook:

- **Where to look:** logs, dashboards, traces
- **Common failures:** auth expiry, schema drift, rate limit, malformed files
- **Recovery:** replay strategy, dead‑letter reprocessing, vendor resubmission
- **Contacts:** vendor support, internal SMEs, escalation path
- **SLOs:** availability, latency, error budgets

---

## Roadmap & Ownership

- **Ownership:** Our team builds and operates integrations; Ellucian operates Banner SaaS.  
- **Outsourcing:** Contract medium/high‑complexity work as needed (API builds, AWS SFTP pipelines).  
- **Quarterly Review:** Reassess API availability for legacy SFTP vendors; retire batch feeds when APIs appear.  
- **Tech Debt:** Track manual transforms and schema quirks for future cleanup.

---

## License

This playbook is internal documentation. If sharing externally, add a permissive license (e.g., MIT) or mark as “All Rights Reserved” per institutional policy.


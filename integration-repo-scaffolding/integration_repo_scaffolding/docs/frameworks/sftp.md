# AWS SFTP / Flat‑File Framework (High Complexity)

## Steps
1. AWS Transfer Setup (SFTP endpoint → S3 landing)
2. Vendor Coordination (SSH keys, naming, schedule, format)
3. File Spec & Validation (schema, rules, drift detection)
4. ETL / Orchestration (Vendor → SFTP → S3 → MuleSoft → Banner)
5. Testing (samples, malformed cases, full ingest)
6. Production Cutover (prod endpoint, 30‑day hyper‑care)

## Deliverables
- `aws-transfer.tf` or CloudFormation
- `file-spec-vendor.xlsx`
- `mulesoft-etl/` project
- `runbook-sftp.md`

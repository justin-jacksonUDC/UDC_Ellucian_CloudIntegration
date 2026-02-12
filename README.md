# UDC Ellucian Cloud Integration Repository
**Centralized documentation for integration patterns, app-specific configuration, cloud migration work, and operational support.**

This repository houses everything the team needs to design, build, test, deploy, and maintain integrations as we transition to **Ellucian Cloud**. It standardizes three primary paths:

- **Ellucian Ethos** (partner/ethos-aligned)  
- **Direct APIs via MuleSoft/iPaaS**  
- **AWS SFTP/Flat‑File** for vendors without APIs

---

## 📌 Start Here

If you’re new to this repo, begin with the **Frameworks** section to select the right approach for your integration, then review **Mapping Templates**, **Samples**, and the relevant **Runbook** for Day‑2 operations.

---

## 🧭 Decision Tree

```text
Is the app an Ellucian Partner and/or uses Ethos data models?
 ├─ Yes → Use Ethos Framework
 └─ No
     ├─ Vendor has REST/SOAP APIs? → Use API Framework (MuleSoft)
     └─ No APIs → Use AWS SFTP / Flat‑File Framework

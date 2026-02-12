# UDC Ellucian Cloud Integration Repository
Centralized documentation for all integration work related to UDC’s migration to **Ellucian Cloud / Banner SaaS**.

---

## 🚀 New to this repository?
Start with the onboarding guide:

👉 **[START-HERE.md](./_START-HERE.md)**  

This guide explains:
- How to navigate this repo  
- Integration frameworks and when to use them  
- Where app information lives  
- How to begin a new integration  
- How to work with MuleSoft, APIs, and SFTP pipelines  

---

# 📘 Purpose

This repository exists to:

- Document **all integrations** required for Ellucian Cloud  
- Provide **standard frameworks** for Ethos, API, and SFTP integrations  
- Centralize **architecture diagrams**, **mappings**, **runbooks**, and **samples**  
- Act as a **single source of truth** for application integration information  
- Support **team onboarding and continuity** in case of staffing changes  

---

# 🧭 Navigation (Click to Open Folders)

Below are direct links to every folder currently in the repo, mapped to your structure:

### **📁 Frameworks**  
Integration patterns for **Ethos**, **API (MuleSoft)**, and **AWS SFTP**  
👉 [`/frameworks`](./frameworks)

---

### **📁 Architecture Diagrams**  
High-level and system-specific integration diagrams  
👉 [`/architecture`](./architecture)

---

### **📁 Mapping Templates**  
Use these to define **source → target** fields and transformations  
👉 [`/mapping-templates`](./mapping-templates)

---

### **📁 Runbooks (Day‑2 Operational Support)**  
On‑call procedures and recovery steps for integrations  
👉 [`/runbooks`](./runbooks)

---

### **📁 Samples**  
Real API payloads and SFTP files for reference  
👉 [`/samples`](./samples)  
Contains:  
- `/sample-json/` — example API payloads  

---

### **📁 Security Standards**  
Credential management, secrets handling, OAuth guidance  
👉 [`/security`](./security) 

---

### **📄 Application Integration Inventory Spreadsheet**  
The master file listing all applications, integration methods, API links, and owner notes  
👉 >**View the full documentation website:**
> https://liveudc.sharepoint.com/:x:/r/sites/OITDocumentRepository-OITApplications/_layouts/15/Doc.aspx?sourcedoc=%7B568AD5AF-F543-46A6-88E3-F4AAE06BF21A%7D&file=Application%20Integration%20Spreadsheet.xlsx&action=default&mobileredirect=true

This is essential for:
- Understanding existing integrations  
- Planning migration scope  
- Determining whether an application uses Ethos, API, or SFTP  

---

# 🧰 Integration Frameworks Overview

Integration work is based on **three primary patterns**:

### **1. Ethos Framework (Low Complexity)**  
For Ellucian partners or apps aligned with Ethos data models.  
👉 See: `/frameworks/ethos.md`

### **2. API Integration Framework (Medium Complexity)**  
For systems with REST/SOAP APIs, typically built via MuleSoft.  
👉 See: `/frameworks/api.md`

### **3. SFTP / Flat‑File Framework (High Complexity)**  
For vendors with no APIs; migrated to **AWS Transfer Family** for cloud readiness.  
👉 See: `/frameworks/sftp.md`

---

# 📑 Universal Integration Checklist

Every integration should include:

- [ ] Confirm integration method (Ethos / API / SFTP)  
- [ ] Fill out mapping template  
- [ ] Obtain and store vendor credentials securely  
- [ ] Review architecture diagrams  
- [ ] Build and test flows (API or SFTP pipeline)  
- [ ] Perform E2E and negative testing  
- [ ] Document runbook + escalation contacts  
- [ ] Update Application Inventory spreadsheet  

---

# 🔐 Security Principles

- Store **all credentials** in a secure vault (no local `.env`)  
- Use **least privilege** access (API scopes, S3 permissions)  
- Enforce **TLS** or **SFTP** for all data in transit  
- Rotate keys regularly  
- Log all access patterns  

See:  
👉 `/security/`

---

# 🧩 Responsibilities Summary

### **Ellucian Cloud Provides:**
- Hosting & infrastructure (AWS)  
- Upgrades, patches, and platform security  
- Ethos integration services  

### **Our Team Provides:**
- All integration pipelines  
- API calls and MuleSoft flows  
- SFTP → S3 migrations (AWS Transfer Family)  
- Data mapping and transformation  
- Monitoring, runbooks, and break/fix support  

---

# 📄 License  
See: [`LICENSE`](./LICENSE)

---

# 📬 Feedback or Questions?

Update:
- **START-HERE.md** for onboarding improvements  
- **README.md** if folder structure changes  
- **Application Integration Spreadsheet.xlsx** for system updates

Happy integrating! 🎉

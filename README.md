# FHIR-standard-in-Healthcare-Systems

# FHIR Easy Project – Interoperability Demo

Minimal FHIR (R4) demo for the course *Interoperability and Information Integration*:
- `Patient`
- Two lab `Observation`s:
  - Hemoglobin — **LOINC 718-7**
  - Glucose — **LOINC 2345-7**
- `DiagnosticReport` referencing both observations

**Backend:** HAPI FHIR JPA Server (Docker)  
**Testing:** Postman collection

---

## 🎯 What this demonstrates
- **Standardized data model** with FHIR resources (`Patient`, `Observation`, `DiagnosticReport`)
- **Semantic interoperability** via **LOINC** codes (no custom/local naming)
- **Standard REST and search**: portable queries like `patient`, `code`, `date`
- Clear **CRUD** lifecycle and resource **versioning** (`meta.versionId`)

---

## 🚀 Quickstart

### Prereqs
- Docker Desktop / Engine
- Postman (or curl)

### 1) Start FHIR server
```bash
docker compose up -d
# Verify:
# open http://localhost:8080/fhir/metadata

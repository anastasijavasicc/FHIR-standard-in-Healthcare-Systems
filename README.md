# FHIR-standard-in-Healthcare-Systems

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

---

### 2) Import the Postman collection
- Import `postman_collection.json` into Postman.
- In the collection **Variables**, ensure:
  - `baseUrl = http://localhost:8080/fhir` (already default)
  - `patientId` can be left empty; a test script will set it automatically after creating a Patient.
  - (For `DiagnosticReport`, set `obsHemoglobinId` and `obsGlucoseId` after creating the Observations.)

### 3) Run the flow (click–by–click)
1. **Create Patient** → server returns `201 Created` with `Location: Patient/{id}`  
   - A Postman **test script** automatically extracts `{id}` and sets **`patientId`** in collection variables.
2. **Create Observation – Hemoglobin** (LOINC **718-7**)
3. **Create Observation – Glucose** (LOINC **2345-7**)
4. **Search Observations – All for Patient** or **Hemoglobin only**  
   - Example: `GET /Observation?patient=Patient/{{patientId}}&code=loinc|718-7`
5. **Create DiagnosticReport (links Hemo+Glucose)**  
   - Uses variables `patientId`, `obsHemoglobinId`, `obsGlucoseId`.
6. **Search DiagnosticReports – by Patient**


## 🧩 Project contents



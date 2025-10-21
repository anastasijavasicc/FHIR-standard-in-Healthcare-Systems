
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
```
The `/fhir/metadata` endpoint returns the server **CapabilityStatement**.

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

---

## 🧩 Project contents

```
.
├─ docker-compose.yml          # HAPI FHIR server (R4) via Docker
├─ patient.json                # Sample Patient
├─ observation_hemoglobin.json # Observation (LOINC 718-7)
├─ observation_glucose.json    # Observation (LOINC 2345-7)
├─ diagnostic_report.json      # DiagnosticReport referencing both Observations
├─ postman_collection.json     # Ready-to-run Postman requests (+ auto patientId)
└─ README.md                   # This file
```

---

## 🧪 Sample curl commands

**Create Patient**
```bash
curl -i -X POST "http://localhost:8080/fhir/Patient"   -H "Content-Type: application/fhir+json"   --data-binary "@patient.json"
```

**Search Patient by MRN**
```bash
curl -s "http://localhost:8080/fhir/Patient?identifier=http://hospital.rs/mrn|MRN-000123"
```

**Search Hemoglobin Observations for the Patient**
```bash
curl -s "http://localhost:8080/fhir/Observation?patient=Patient/<ID>&code=loinc|718-7"
```

---

## 🩺 Domain notes 

- **MRN (Medical Record Number):** local hospital identifier stored in `Patient.identifier`.
- **Observation:** a single measurement/finding (e.g., a lab value).  
  Key fields: `code` (what was measured; **LOINC**), `valueQuantity` (the value + unit), `subject` (patient), `effectiveDateTime`, `referenceRange`.
- **DiagnosticReport:** a higher-level clinical report that groups one or more Observations under a conclusion (e.g., a lab report).
- **CapabilityStatement:** available at `/fhir/metadata`; machine-readable contract describing what the server supports.

---

## 🛡️ Interoperability highlights

- **Standardized resources** → no custom JSON, widely reusable.
- **Terminology binding** → LOINC codes give universal meaning.
- **Consistent API** → queries like `patient`, `code` work on any FHIR server.
- **Versioning & validation** → built into the spec.

---

## 🔧 Troubleshooting

- **Can’t reach `/fhir/metadata`** → container not running → `docker compose up -d`.
- **415 Unsupported Media Type** → check header → must be `Content-Type: application/fhir+json`.
- **404 Not Found** → remember the `/fhir` base path.
- **Search returns `total: 0`** → ensure `patientId` is set and Observations reference `Patient/{id}`.

---


---

## 🙌 Acknowledgments

- [HAPI FHIR Project](https://hapifhir.io)  
- [HL7® FHIR® Specification (R4)](https://hl7.org/FHIR/R4/)  

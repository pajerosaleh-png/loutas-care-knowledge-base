# ADR-012-Endoscopy-Module.md

**Document ID:** ADR-012  
**Document Classification:** Architecture Decision Record  
**Owner:** Chief Software Architect  
**Status:** Proposed — four product decisions approved (2026-11-06); the rest awaits Product Owner approval  
**Version:** 0.1  
**Created:** 2026-11-06  
**Last Updated:** 2026-11-06  
**Next Review:** at Gate E1 approval  
**Approval Authority:** Product Owner

---

# ADR-012 — Endoscopy Specialty Module

---

# Status

**Proposed.** Nothing in this ADR is implemented. Endoscopy is today a registry entry only (`endoscopy`, `المناظير`, Core EMR only — ADR-011). This ADR records how the Endoscopy module is built on ADR-011, the decisions already approved, and the gates. No gate starts before the Product Owner approves this ADR.

| Part | Gate | State |
|---|---|---|
| Decisions and architecture (this ADR) | E0 | **In review** |
| Endoscopy shell: manifest, overview, summary card | E1 | Planned |
| Endoscopy case and pre-procedure assessment | E2 | Planned |
| Procedure record: time-out, times, team, findings, complications | E3 | Planned |
| Specimens → pathology (lab orders), pending results, acknowledgement | E4 | Planned |
| Endoscopy report: draft, sign, signed amendment, print | E5 | Planned |
| Recovery and discharge | E6 | Planned |
| Billing (explicit Add to invoice) and follow-up | E7 | Planned |
| Security and audit matrix; imaging links | E8 | Planned |
| Clinical review of vocabulary, forms and workflow | After E8 | Planned — needs a practising gastroenterologist / endoscopy lead |

---

# Context

ADR-011 (Gates 1–9, implemented 2026-11-06) gives every specialty the same frame: the Specialty Registry, clinic specialties, the module resolver (`lib/emr/resolveEmrTabs`), code-defined manifests, module routes mounted through `src/modules/index.ts`, and the Dental module as the worked example. A review of `main` on 2026-11-06 (backend `31d85ca`, frontend `159333b`) found that most of what the 2026-10-18 Endoscopy plan listed as blockers now exists:

| Need | Exists today | Source |
|---|---|---|
| Endoscopy tabs only on an endoscopy visit of a clinic that enabled it | Module resolver | ADR-011 Gate 3 |
| Consent and checklists stored on the server, versioned, signed, amendable | Core Forms (`FormTemplate` draft → published with versions; `FormResponse` signed, never edited, corrected by an amendment) | `src/routes/forms.ts` |
| Forms that belong to a specialty | Form templates assigned to a specialty (Dental forms, Gate 8) | ADR-011 Gate 8 |
| Results that arrive after the visit closes | `POST /visits/:visitId/orders/:orderId/result` accepts a completed visit, with `labs:results` | `src/routes/visits.ts` |
| Reopen a completed visit | Doctor-owned Reopen clears `closedAt` | `src/routes/appointments.ts` |
| Charges added explicitly, listed for the billing desk | Dental decision 7; module billables (`GET /billing/invoices/:id/module-items`) | `src/modules/billables.ts` |
| Audit rows scoped to clinic, patient, visit | Gate 9 | `src/lib/auditScope.ts` |
| Procedures, operations and files locked with their visit | Backend PR #24 (in review) | `recordEntryVisitGuard` |

Not available: a signed **report** document with versions (Forms cover forms, not a structured report), an acknowledgement on a result, recovery observations beyond the current `Vitals` fields, and image storage (ADR-011 D4: bytes wait for storage in Egypt).

---

# Decision

## 1. Approved by the Product Owner on 2026-11-06

| # | Question | Decision |
|---|---|---|
| E-D1 | Correcting a signed report after the visit is closed | **Signed amendment without reopening**: a new signed version with a reason; the original is kept and shown as superseded. Same rule as signed Forms and CLN-006. Reopen is not required for a report correction. |
| E-D2 | Sending biopsy specimens to pathology | **Core lab order** for a pathology catalog item. The lab bench enters the report after discharge with the existing *Enter lab results* permission (`labs:results`). No new order type or permission. |
| E-D3 | Consent, pre-procedure, time-out and discharge checklists | **Core Forms assigned to the Endoscopy specialty.** Starter forms are created as drafts; the clinic's clinical lead edits and publishes them. No checklist is fixed in code. |
| E-D4 | Bronchoscopy | **Not in V1.** V1 covers GI endoscopy. Bronchoscopy is reconsidered later. |

## 2. Proposed defaults (Product Owner to confirm with this ADR)

| # | Topic | Proposed default |
|---|---|---|
| E-P1 | V1 procedure types | Gastroscopy (upper GI endoscopy), colonoscopy, flexible sigmoidoscopy, ERCP, EUS, capsule endoscopy, other endoscopy procedure. System reference data, inserted by the server; a clinic chooses which it offers. Capsule endoscopy records the reading, not a sedation / specimen workflow. |
| E-P2 | When the endoscopy case starts | At **Start Visit** on a visit whose specialty is Endoscopy. Pre-assessment, procedure, report, recovery and discharge are recorded in that visit. Booking and check-in stay in Appointments, unchanged. |
| E-P3 | Findings vocabulary | Defined once in code (English + Arabic), like the Dental finding types; may change after clinical review without changing the architecture. No published terminology (for example the World Endoscopy Organization's MST) is copied into the repository until its licence is confirmed. |
| E-P4 | Rooms, staff and endoscopes | Recorded on the case as text (room, assisting staff names, endoscope ID). No room or device scheduling in V1. |
| E-P5 | Permissions | Reuse: `emr:read` (read), `emr:write` ⚕ (assessment, findings, recovery), `procedures:manage` ⚕ (procedure record, sign report), `invoices:manage` (Add to invoice), `labs:results` (pathology report). No `endoscopy:*` keys unless a gate shows a need. |
| E-P6 | Images | Links only (as Dental Gate 8): an endoscopy image is a Core patient file or radiology item linked to the case and an anatomical site. No image bytes until storage in Egypt exists (ADR-011 D4). No device or video integration in V1. |
| E-P7 | Clinical review | Not a blocker for E1–E7. A practising gastroenterologist / endoscopy lead reviews vocabulary, forms and workflow after E8; corrections are made then (as ADR-011 decision 8). |
| E-P8 | Language | Every label in English and Arabic, using the approved terminology list (*LOUTAS Care — Endoscopy EMR Labels*, 2026-10-20). |

## 3. Architecture

- **A module of the shared EMR**, exactly as Dental: frontend `app/patients/endoscopy/manifest.ts` registered in `lib/emr/modules.ts`; backend `src/modules/endoscopy/` mounted from `src/modules/index.ts`. The Core never names Endoscopy. One patient, one record; no endoscopy patient file.
- **Every route** answers `409 MODULE_NOT_ENABLED` when the clinic has Endoscopy off; reads apply the record branch walls (`lib/recordWall.ts`); writes need an **open** endoscopy visit in a branch the member works in (the Dental `writableDentalVisit` rule); every write and its audit row (`clinicId`, `patientId`, `visitId`) are one transaction.
- **Reuse, not copies**: allergies, medications and history are read from the Core record; consent and checklists are Core Forms; specimens are Core lab orders; performed procedures are Core `Procedure` rows; charges go through `invoiceService.addInvoiceCharge`; recovery observations are Core `Vitals`.
- **The case state** (proposed): `PREP → CLEARED → IN_PROCEDURE → PROCEDURE_DONE | INCOMPLETE → RECOVERY → DISCHARGED`, plus `NOT_CLEARED` (reason required) → cancel or reschedule through Appointments. The visit can be completed only after `DISCHARGED` or a cancellation.
- **The report** is the one new document type: `DRAFT → SIGNED`; a correction is a new `SIGNED` version that amends the previous one (E-D1), never an edit.

## 4. Data model (implementation level — table names and fields are proposed at each gate after schema review)

| Entity | Purpose | Gate |
|---|---|---|
| `EndoscopyProcedureType` | Reference data (E-P1), server-inserted, never edited by users | E1 |
| `EndoscopyCase` | One per endoscopy visit: type, state, endoscopist, team / room / endoscope text, sedation, start / end, outcome, reasons, discharge time and destination | E2–E3, E6 |
| `EndoscopyFinding` | Site, finding, size, description, intervention; never edited — voided with a reason | E3 |
| `EndoscopySpecimen` | Label, site, link to the Core lab `ServiceOrder` | E4 |
| `EndoscopyReport` | Versions: draft, signed, signed amendment with reason | E5 |
| `ServiceOrder.reviewedById / reviewedAt` | Core: a clinician acknowledges a result (nullable columns) | E4 |
| `Vitals.respiratoryRate / consciousness / painScore` | Core: recovery observations (nullable columns) | E6 |

All changes are additive and nullable. **Schema prerequisite:** the 2026-11-06 review found that `prisma/migrations` does not rebuild the schema on an empty database (it stops at `20261025_schema_batch_a`, `ClinicalHistoryItem` was created by `db push`). The proposed reviewed-migration policy and its baseline task (ADR-011) must be approved and done **before E2**, the first Endoscopy schema change.

## 5. Gates

Each gate follows the ADR-011 flow: `main` → gate branch → implementation → tests → database installation by the Product Owner → verification → review → approval → merge. Work stops after each gate.

| Gate | Scope | Exit criteria |
|---|---|---|
| E1 | Manifest, Endoscopy overview tab, summary card, `GET /patients/:id/endoscopy/summary`, procedure types | Tabs appear only on an endoscopy visit of a clinic that enabled Endoscopy; `MODULE_NOT_ENABLED` otherwise |
| E2 | Case at Start Visit; pre-assessment reading Core allergies / medications; anticoagulant review; consent and pre-procedure forms (drafts); cleared / not cleared | Clearance refused without the required published forms; reason required for not cleared |
| E3 | Time-out form, server-stamped times, team, endoscope ID, sedation, findings by site, complications, incomplete | Start refused before the time-out form is signed; incomplete needs a reason |
| E4 | Specimens create Core lab orders (pathology items); pending pathology list; result acknowledgement | Every specimen has one order; an unacknowledged result stays on the endoscopist's list |
| E5 | Report assembled from the case; sign; signed amendment; print view | A signed version cannot be changed by any route; amendment keeps the original |
| E6 | Recovery observations; discharge form; discharge time, destination; visit completion guard | Visit cannot complete while the case is in recovery |
| E7 | Procedure and sedation charges through Add to invoice; consumables through inventory dispense; module billables for the desk; follow-up booking | No automatic charge; one charge per item |
| E8 | Security matrix of every Endoscopy route (401 / other clinic / 403 / branch wall / closed visit); image links; audit card | Matrix passes on PostgreSQL in CI |

## 6. Out of scope for V1

Bronchoscopy (E-D4); device or video-processor integration; image upload and viewing (until storage in Egypt); endoscope reprocessing traceability; room, staff and equipment scheduling; automatic charges; sending preparation instructions by WhatsApp.

---

# Consequences

- Endoscopy reuses the ADR-011 frame and the Dental patterns; the new code is the case, findings, specimens link, report versions and their screens.
- Two Core additions benefit every specialty: result acknowledgement and recovery observations.
- The signed-amendment rule (E-D1) applies to the endoscopy report; the project rule that a closed visit is not changed still holds for every other entry of the visit.
- The schema baseline becomes a blocking prerequisite for E2.

---

# Related documents

- `ADR/ADR-011-Multi-Specialty-Architecture.md`
- `04-Clinical/CLN-006-Clinical-Documents-Architecture.md`
- *LOUTAS Care — Endoscopy Module Plan (Proposed)*, 2026-10-18
- *LOUTAS Care — Endoscopy EMR Labels*, 2026-10-20
- Backend PR #24, frontend PR #23 (visit lock for procedures, operations and files)

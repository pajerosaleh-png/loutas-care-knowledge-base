# ADR-012-Endoscopy-Module.md

**Document ID:** ADR-012  
**Document Classification:** Architecture Decision Record  
**Owner:** Chief Software Architect  
**Status:** Accepted as the Endoscopy baseline (2026-11-06) — no gate approved yet  
**Version:** 1.5  
**Created:** 2026-11-06  
**Last Updated:** 2026-11-07  
**Next Review:** before Gate E1 and at CR-1  
**Approval Authority:** Product Owner (product decisions) · Clinical Reviewer (clinical terminology, fields and workflow)

---

# ADR-012 — Endoscopy Specialty Module

---

# Status

**Accepted as the Endoscopy baseline** by the Product Owner on 2026-11-06 (Knowledge Base PR #5 merged). Nothing in this ADR is implemented. Endoscopy is today a registry entry only (`endoscopy`, `المناظير`, Core EMR only — ADR-011).

Accepting this ADR **does not** approve any gate, nor the merge of backend #24 or frontend #23. Each gate, and each merge, needs its own explicit approval. The approvals in sections 1 and 2 remain **in principle**; the clinical content of 2.1 and 2.3 is confirmed at CR-1.

| Part | Gate | State |
|---|---|---|
| Decisions and architecture (this ADR) | E0 | **Accepted** 2026-11-06 |
| Endoscopy shell: manifest, overview, summary card | E1 | **Implemented** 2026-11-07 — backend #25, frontend #24 merged; CI green |
| **Early Clinical Review** (stop point) | CR-1 | **Preliminarily Approved — Pending Clinical Validation** (Product Owner, 2026-11-07, technical planning only). Designated reviewer: Dr. Mohammed Saleh — **not yet reviewed**. See 2.8 and `ADR-012-CR1-Decision-Matrix.md` |
| Endoscopy case and pre-procedure assessment | E2 | **Technical Groundwork — Approved with Conditions** (2026-11-07) · **Clinical Activation — Blocked Pending Clinical Validation** · **Database Migration — applied on Railway 2026-11-07** (backup first; status up to date; no schema difference). Groundwork **merged** 2026-11-07: backend #28, frontend #25 (acceptance test 22/22 on a local copy). Plan: `ADR-012-E2-Implementation-Plan.md`; decisions: `ADR-012-Technical-Decision-Register.md` |
| Procedure record: time-out, times, team, findings, complications | E3 | **Technical Groundwork — Approved with Conditions** (Product Owner, 2026-11-07, same terms as E2) · **Clinical Activation — Blocked Pending Clinical Validation** · **Database Migration — applied on Railway 2026-11-07** (backend #29, backup first, no schema difference) · groundwork **merged** 2026-11-07: backend #30, frontend #28 (local acceptance 14/14; checked on the Product Owner's system). Time-out-before-start rule built but OFF (`settings.endoscopy.requireTimeoutBeforeStart`) |
| Specimens → pathology request (lab order), pending results, acknowledgement | E4 | **Technical Groundwork — in progress** (Product Owner, 2026-11-07, same terms as E2/E3) · **Database Migration — Pending Separate Review and Approval** (backend #31: `EndoscopySpecimen`, Core `ResultAcknowledgement`) |
| Endoscopy report: draft, sign, signed amendment, print | E5 | Planned — not approved |
| Recovery and discharge | E6 | Planned — not approved |
| Billing (explicit Add to invoice) and follow-up | E7 | Planned — not approved |
| Security and audit matrix; image links | E8 | Planned — not approved |
| **Final Clinical Review** | CR-2 | Planned — after E8 |

**Approved on 2026-11-06:** E-P8 language of the saved report (2.7) · the reviewed-migration policy, with the schema baseline approved for preparation (section 4).  
**Done on 2026-11-07:** schema baseline merged (backend #26) and recorded on Railway with the Product Owner's approval (backup taken first; status up to date; no schema difference).  
**Still open:** the details of E-P1, E-P2, E-P3 and E-P5 (confirmed at CR-1 / the gate that builds them).

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
| Procedures, operations and files locked with their visit | Backend PR #24 (in review, not merged) | `recordEntryVisitGuard` |

Not available: a signed **report** document with versions (Forms cover forms, not a structured report), an acknowledgement on a result, recovery observations beyond the current `Vitals` fields, and image storage (ADR-011 D4: bytes wait for storage in Egypt).

---

# Decision

## 1. Product decisions — approved in principle on 2026-11-06

| # | Question | Decision |
|---|---|---|
| E-D1 | Correcting a signed report after the visit is closed | **Signed amendment without reopening**: a new signed version with a reason; the original is kept and shown as superseded. Same rule as signed Forms and CLN-006. |
| E-D2 | Sending biopsy specimens to pathology | **Core lab order** for a pathology catalog item. The lab bench enters the result after discharge with the existing *Enter lab results* permission (`labs:results`). No new order type or permission. |
| E-D3 | Consent, pre-procedure, time-out and discharge checklists | **Core Forms assigned to the Endoscopy specialty.** Starter forms are created as drafts; the clinic's clinical lead edits and publishes them. No checklist is fixed in code. |
| E-D4 | Bronchoscopy | **Not in V1.** V1 covers GI endoscopy. Bronchoscopy is reconsidered later. |

## 2. Proposed defaults — approved in principle, with the conditions below

| # | Topic | Proposal | Condition attached to the approval in principle |
|---|---|---|---|
| E-P1 | V1 procedure types | Gastroscopy (upper GI endoscopy), colonoscopy, flexible sigmoidoscopy, ERCP, EUS, capsule endoscopy, other endoscopy procedure — system reference data; a clinic chooses which it offers | State the **common fields**, the **fields specific to each type**, and how **Other** is handled — section 2.1 |
| E-P2 | When the case starts | The case opens at **Start Visit** on a visit whose specialty is Endoscopy | Keep **Check-in**, **Start Visit**, **Procedure started** and **Procedure ended** as separate recorded events — section 2.2 |
| E-P3 | Findings vocabulary | Defined in code (English + Arabic), like the Dental finding types; no published terminology (for example the WEO MST) copied before its licence is confirmed | **Stable identifiers**, English and Arabic labels, and **version management** — section 2.3 |
| E-P4 | Room, endoscope, team | Recorded as text on the case in V1; no room or device scheduling | The **values as recorded on each case are kept** (history), never re-derived from a list that changes later |
| E-P5 | Permissions | Reuse existing keys; no `endoscopy:*` keys unless a gate shows a need | Conditional on an explicit **permission matrix** with **branch scope** and **audit** — section 2.4 |
| E-P6 | Images | Link **existing** files and radiology items to the case and a site; no new upload; no bytes until storage in Egypt (ADR-011 D4) | **Access to the original file is checked** when linking and when showing the link — section 2.5 |
| E-P7 | Clinical review | Two stages: **Early Clinical Review** and **Final Clinical Review** | Section 2.6 |
| E-P8 | Language | Every label in English and Arabic (approved list *Endoscopy EMR Labels*, 2026-10-20) | Language of the saved report — **approved 2026-11-06**, section 2.7 |

### 2.1 Procedure types: common fields, type-specific fields, Other (E-P1)

The field lists below are **structure for the Early Clinical Review**, not approved clinical content. The clinical reviewer confirms, adds or removes fields before E2 / E3.

| Group | Applies to | Proposed fields (to be confirmed at CR-1) |
|---|---|---|
| Common | Every type | Indication; sedation / anaesthesia; procedure started / ended; endoscopist; assisting staff; endoscope ID; findings by anatomical site; interventions; specimens; complications and immediate action; outcome (completed / incomplete with reason) |
| Lower GI | Colonoscopy, flexible sigmoidoscopy | Bowel preparation quality; extent reached |
| Pancreatico-biliary | ERCP, EUS | Site list for the biliary / pancreatic anatomy (the common "site" field takes a different list) |
| Capsule | Capsule endoscopy | Capsule identifier; ingestion time; reading completed by and when; completeness of the study. No sedation, specimens or interventions |
| Other | Other endoscopy procedure | A **procedure name is required**; only the common fields apply; it is shown as "Other — <name>" in lists and the report; it is never priced automatically — a charge is added by hand in Billing |

### 2.2 Separate events (E-P2)

| Event | Where it is recorded | Who |
|---|---|---|
| Check-in | Appointments (unchanged) — patient in the waiting area | Reception |
| Start Visit | The visit opens; the Endoscopy case is created in state `PREP` | Clinician (`visit:start`) |
| Procedure started | Case timestamp, stamped by the server after the time-out form is signed | Endoscopist / nurse |
| Procedure ended | Case timestamp, stamped by the server; not before *Procedure started* | Endoscopist / nurse |

Each event has its own time and actor; none is inferred from another (for example, *Start Visit* is not *Procedure started*).

### 2.3 Vocabulary identifiers and versions (E-P3)

- Each term has a **stable code** that is never changed or reused (as `DentalFindingType.code`).
- Each term has an English and an Arabic label; a label may be corrected without changing the code.
- The vocabulary carries a **version**; each recorded finding stores the code **and** the vocabulary version it was recorded with.
- A term that is withdrawn is marked inactive, never deleted; findings already recorded with it keep showing it.

### 2.4 Permission matrix (E-P5)

Every row also needs: Endoscopy enabled for the clinic (else `409 MODULE_NOT_ENABLED`), the member's branch (writes: `403 BRANCH_FORBIDDEN`; reads: the record branch walls), and one audit row with `clinicId`, `patientId`, `visitId` in the same transaction.

| Action | Permission | Visit state required | Notes |
|---|---|---|---|
| Read the Endoscopy record | `emr:read` | any | Walled by branch unless the record is shared |
| Pre-assessment, findings, recovery observations | `emr:write` ⚕ | open | |
| Procedure started / ended, interventions, specimens | `procedures:manage` ⚕ | open | |
| Sign the report | `procedures:manage` ⚕ | open, or completed for a signed amendment (E-D1) | Only the responsible endoscopist |
| Enter the pathology result | `labs:results` (or `emr:write`) | any (results arrive later) | Existing Core rule |
| Acknowledge a pathology result | `emr:write` ⚕ | any | Ordering / responsible clinician |
| Add a charge to the invoice | `invoices:manage` | any | Explicit only; never automatic |
| Read the Endoscopy audit trail | existing audit permission | any | Gate 9 viewer |

The matrix is tested route by route in E8 (as the Dental security matrix).

### 2.5 Image links (E-P6)

- Only a file or radiology item **already in this patient's record** can be linked.
- When linking: the item must belong to the same clinic and patient, and the member must be able to read it under the record branch walls (`recordWall` / `sees`). Otherwise `404`, as for any record the member cannot see.
- When showing the case: a linked item the reader cannot read is not shown.
- No upload, preview or bytes in V1.

### 2.6 Clinical review in two stages (E-P7)

1. **Early Clinical Review (CR-1)** — before E2 and E3, i.e. before the parts that shape clinical decisions are built: the pre-procedure and time-out forms, the findings vocabulary and sites, the type-specific fields (2.1), and the procedure documentation flow. The reviewer's comments and the decisions taken on them are recorded in this ADR (or a linked review note). **E2 and E3 do not start before those comments are recorded and addressed.** E2 technical scaffolding with no clinical rule activated may proceed on the Product Owner's approval of the E2 scope (2.8; `ADR-012-E2-Implementation-Plan.md`).
2. **Final Clinical Review (CR-2)** — after E8: the whole chain — assessment, findings, report, specimens, pathology request, pathology result and the clinician's acknowledgement — is reviewed together, to confirm that what was built matches the approved clinical decisions. Corrections found here are made before the module is called production-ready.

### 2.7 Language of the saved report (E-P8) — approved 2026-11-06

Approved by the Product Owner:

- Labels, headings and coded terms are shown in the **reader's** language (English or Arabic), from the stable codes.
- Free text (indication details, descriptions, impression, recommendations) is saved **as written** and never machine-translated.
- The signed version stores the language it was signed in; the print view prints the labels in the chosen language and the free text as written.

### 2.8 CR-1 — preliminary Product Owner approval (2026-11-07)

**Status: Preliminarily Approved — Pending Clinical Validation.** The Product Owner approved the CR-1 recommendations as the design baseline **for technical planning and preparation only**. This is **not** a clinical approval and is not attributed to the clinical reviewer. Dr. Mohammed Saleh, the designated gastroenterologist, has not yet reviewed them.

Preliminary scope: the seven procedure types; the common and type-specific fields; the workflow (check-in, Start Visit, pre-procedure assessment, clearance, time-out, sedation, procedure and scope times, recovery, discharge, report signing) as separate timestamped events, none inferred from another; the consent, pre-procedure, time-out and discharge form structures; the findings, interventions, complications and incomplete-procedure catalogs (not claimed to be clinically complete); five distinct linked records — Endoscopy Case, Endoscopy Report, Pathology Order, Pathology Result, Doctor Acknowledgement; signed amendments; bilingual labels, permissions, branch / clinic scope and audit.

**E2 groundwork (2026-11-07):** the Product Owner approved the E2 technical groundwork with conditions and decided TD-01 to TD-05 (`ADR-012-Technical-Decision-Register.md`): the case is created only at the explicit Start Visit; each step is its own timestamped event; Cleared / Not cleared requires `procedures:manage`; starter forms are per-clinic drafts published only by the designated clinical lead; current medications are active prescription items only. Technical requirements also kept in the plan: separate sedation-start and procedure-start, scope insertion and withdrawal events; several assisting staff with roles; a unique Specimen ID; INR and platelets shown when present. These are technical planning requirements, not clinical approvals; the Boston scale, ASA, caecal intubation criteria and the other clinical choices remain pending.

Restrictions: no unvalidated form is published, and no unvalidated clinical field, threshold or rule is activated. INR, platelets and other labs may be shown when available, with no automatic eligibility rule. ASA classification is configurable per clinic policy. Each item and its validation status is tracked in `ADR-012-CR1-Decision-Matrix.md`; rows are updated only after the gastroenterologist's own review.

## 3. Architecture

- **A module of the shared EMR**, exactly as Dental: frontend `app/patients/endoscopy/manifest.ts` registered in `lib/emr/modules.ts`; backend `src/modules/endoscopy/` mounted from `src/modules/index.ts`. The Core never names Endoscopy. One patient, one record; no endoscopy patient file.
- **Every route** follows the matrix in 2.4: module enabled, branch, visit state, one audited transaction per write.
- **Reuse, not copies**: allergies, medications and history are read from the Core record; consent and checklists are Core Forms; the pathology request is a Core lab order; performed procedures are Core `Procedure` rows; charges go through `invoiceService.addInvoiceCharge`; recovery observations are Core `Vitals`.
- **Three separate, linked records** — they are never merged into one:
  1. the **endoscopy report** (the endoscopist's document: findings, specimens taken, impression, recommendations — signed, amended by new versions);
  2. the **pathology request** (a Core lab `ServiceOrder` per specimen, linked from the specimen);
  3. the **pathology result** (a Core `ServiceResult` on that order, entered by the lab, then acknowledged by the clinician).
  The report shows the request and, once available, the result as links; signing the report does not wait for the result, and a late result never changes the signed report — a clinician who wants it reflected adds a signed amendment.
- **Case state** (proposed): `PREP → CLEARED → IN_PROCEDURE → PROCEDURE_DONE | INCOMPLETE → RECOVERY → DISCHARGED`, plus `NOT_CLEARED` (reason required) → cancel or reschedule through Appointments. The visit can be completed only after `DISCHARGED` or a cancellation.
- **Report**: `DRAFT → SIGNED`; a correction is a new `SIGNED` version that amends the previous one (E-D1), never an edit.

## 4. Data model (implementation level — names and fields are proposed at each gate after schema review)

| Entity | Purpose | Gate |
|---|---|---|
| `EndoscopyProcedureType` | Reference data (E-P1), stable codes. **E1: code-defined** (`src/modules/endoscopy/procedureTypes.ts`, frozen, versioned) — no table, so E1 needed no schema change before the baseline; whether a table is needed is decided at E2 | E1 |
| `EndoscopyCase` | One per endoscopy visit: type, state, the event times of 2.2, endoscopist, team / room / endoscope text as recorded (E-P4), sedation, outcome, reasons, discharge time and destination | E2–E3, E6 |
| `EndoscopyFinding` | Site, finding code **and vocabulary version**, size, description, intervention; never edited — voided with a reason | E3 |
| `EndoscopySpecimen` | Label, site, link to the Core lab `ServiceOrder` (the pathology request) | E4 |
| `EndoscopyReport` | Versions: draft, signed, signed amendment with reason; signed language | E5 |
| ~~`ServiceOrder.reviewedById / reviewedAt`~~ → **`ResultAcknowledgement`** (Core) | A clinician acknowledges one result **version** as its own record (TD-11, Product Owner 2026-11-07: separate record, matching CR-1 F1; additive; history kept; works for every lab result) | E4 |
| `Vitals.respiratoryRate / consciousness / painScore` | Core: recovery observations (nullable columns) | E6 |

All changes are additive and nullable.

**Schema prerequisite:** the 2026-11-06 review found that `prisma/migrations` does not rebuild the schema on an empty database (it stops at `20261025_schema_batch_a`; `ClinicalHistoryItem` was created by `db push`). The reviewed-migration policy (ADR-011) was **approved on 2026-11-06** and the schema baseline is **approved for preparation**. Applying the baseline on any database needs a separate approval. The baseline must be done **before E2**, the first Endoscopy schema change. **Done 2026-11-07:** baseline `20261107000000_baseline` merged (backend #26, CI proves it rebuilds an empty database and is a no-op on Railway) and marked applied on Railway after a backup; Railway matches the code.

## 5. Gates

Each gate follows the ADR-011 flow: `main` → gate branch → implementation → tests → database installation by the Product Owner → verification → review → approval → merge. Work stops after each gate. **The scope of every gate below is still to be reviewed and approved; nothing here authorises a gate.**

| Gate | Scope | Exit criteria |
|---|---|---|
| E1 | Manifest, Endoscopy overview tab, summary card, `GET /patients/:id/endoscopy/summary`, procedure types | Tabs appear only on an endoscopy visit of a clinic that enabled Endoscopy; `MODULE_NOT_ENABLED` otherwise |
| **CR-1** | Early Clinical Review (2.6) | Reviewer's comments recorded and addressed; fields, vocabulary and forms for E2 / E3 approved |
| E2 | Case at Start Visit; pre-assessment reading Core allergies / medications; anticoagulant review; consent and pre-procedure forms (drafts); cleared / not cleared | Clearance refused without the required published forms; reason required for not cleared |
| E3 | Time-out form; procedure started / ended (server-stamped); team, endoscope ID, sedation; findings by site; complications; incomplete | Start refused before the time-out form is signed; incomplete needs a reason |
| E4 | Specimens create pathology requests (Core lab orders); pending pathology list; result acknowledgement | Every specimen has one request; an unacknowledged result stays on the clinician's list; report, request and result stay three linked records |
| E5 | Report assembled from the case; sign; signed amendment; print view in the chosen language | A signed version cannot be changed by any route; an amendment keeps the original |
| E6 | Recovery observations; discharge form; discharge time and destination; visit completion guard | Visit cannot complete while the case is in recovery |
| E7 | Procedure and sedation charges through Add to invoice; consumables through inventory dispense; module billables for the desk; follow-up booking | No automatic charge; one charge per item |
| E8 | Permission matrix (2.4) tested on every route on PostgreSQL; image links with access checks (2.5); audit card | Matrix passes in CI |
| **CR-2** | Final Clinical Review (2.6) | End-to-end chain confirmed; corrections made |

## 6. Out of scope for V1

Bronchoscopy (E-D4); device or video-processor integration; image upload and viewing (until storage in Egypt); endoscope reprocessing traceability; room, staff and equipment scheduling; automatic charges; sending preparation instructions by WhatsApp.

## 7. Roles

| Role | Responsibility |
|---|---|
| Product Owner | Product decisions; final approval of this ADR, of each gate and of every merge |
| Clinical Reviewer | **Dr. Mohammed Saleh** (named by the Product Owner, 2026-11-07). Approves terminology, fields and clinical flow (CR-1, CR-2) |
| Architecture / security review | Reviews each update for architectural and security consistency; sets the stop points before each gate |
| Claude | Updates this ADR and records decisions; implements only an approved gate; never merges without approval |

---

# Consequences

- Endoscopy reuses the ADR-011 frame and the Dental patterns; the new code is the case, findings, the specimen link, report versions and their screens.
- Two Core additions benefit every specialty: result acknowledgement and recovery observations.
- The signed-amendment rule (E-D1) applies to the endoscopy report; the project rule that a closed visit is not changed still holds for every other entry of the visit.
- Two clinical stop points (CR-1, CR-2) are added; E2 and E3 wait for CR-1.
- The schema baseline is a blocking prerequisite for E2.

---

# Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-11-06 | First draft: four product decisions, proposed defaults, architecture, gates E1–E8 |
| 0.2 | 2026-11-06 | All approvals marked *in principle*; conditions added to E-P1–E-P8 (sections 2.1–2.7); clinical review in two stages (CR-1 before E2/E3, CR-2 after E8); report, pathology request and pathology result stated as three separate linked records; permission matrix; roles; open items listed |
| 1.1 | 2026-11-07 | Gate E1 implemented (read-only shell; procedure types code-defined) |
| 1.2 | 2026-11-07 | Clinical reviewer named (Dr. Mohammed Saleh); CR-1 review sheet prepared; schema baseline merged and recorded on Railway |
| 1.5 | 2026-11-07 | E3 technical groundwork approved with conditions (all patients on the shared database are test data); E3 migration submitted for review (backend #29) |
| 1.4 | 2026-11-07 | E2 technical groundwork approved with conditions; TD-01 to TD-05 decided (case created at Start Visit only); technical decision register; E2 migration submitted for review (backend PR #27) |
| 1.3 | 2026-11-07 | CR-1 preliminarily approved by the Product Owner for technical planning — pending clinical validation (2.8); CR-1 decision matrix and E2 technical plan added |
| 1.0 | 2026-11-06 | Accepted as the Endoscopy baseline (KB PR #5); E-P8 (report language) approved; reviewed-migration policy approved and schema baseline approved for preparation. No gate approved |

---

# Related documents

- `ADR/ADR-012-CR1-Decision-Matrix.md`
- `ADR/ADR-012-E2-Implementation-Plan.md`
- `ADR/ADR-012-Technical-Decision-Register.md`
- `ADR/ADR-011-Multi-Specialty-Architecture.md`
- `04-Clinical/CLN-006-Clinical-Documents-Architecture.md`
- *LOUTAS Care — Endoscopy Module Plan (Proposed)*, 2026-10-18
- *LOUTAS Care — Endoscopy EMR Labels*, 2026-10-20
- Backend PR #24, frontend PR #23 (visit lock for procedures, operations and files) — not merged at acceptance

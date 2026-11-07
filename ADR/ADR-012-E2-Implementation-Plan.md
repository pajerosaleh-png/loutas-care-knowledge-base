# ADR-012 — Gate E2 Technical Implementation Plan (Endoscopy case and pre-procedure assessment)

**Document ID:** ADR-012-E2  
**Linked to:** `ADR/ADR-012-Endoscopy-Module.md`, `ADR/ADR-012-CR1-Decision-Matrix.md`  
**Status:**
- **E2 Technical Groundwork — Approved with Conditions** (Product Owner, 2026-11-07)
- **Clinical Activation — Blocked Pending Clinical Validation**
- **Database Migration — Pending Separate Review and Approval** (backend PR #27)

**Date:** 2026-11-07 · **Decisions:** `ADR-012-Technical-Decision-Register.md`

---

## 1. Readiness status

| Area | Status |
|---|---|
| Schema baseline (prerequisite) | **Done** — backend #26 merged; recorded on Railway 2026-11-07; CI migration guard active |
| Gate E1 (shell, procedure types) | **Done** — merged |
| CR-1 clinical validation | **Blocked** — Dr. Mohammed Saleh has not reviewed the sheet yet |
| E2 technical plan | **Ready** — this document |
| E2 technical groundwork (section 3) | **Approved with conditions** 2026-11-07 — **implemented, pending approval:** backend #28, frontend #25 (CI green, not merged) |
| E2 clinical activation (section 4) | **Blocked pending clinical validation** (CR-1 rows C1–C6, D1–D2, A2, A19) |
| E2 database migration | Backend #27 **merged** 2026-11-07; **not applied on Railway** — needs the Product Owner's separate approval (backup, `prisma migrate deploy`) before #28 can be used |

**Blockers:** (1) gastroenterologist validation of CR-1 (clinical activation only); (2) Product Owner review and approval of the E2 migration, PR #27 (before the implementation PR can be merged).

## 2. Clinical vs technical separation

| Technical (can proceed with PO approval) | Clinical (waits for Dr. Mohammed Saleh) |
|---|---|
| Case record, state machine, event log, timestamps | Which events are mandatory for which procedure type |
| Permission checks, branch / clinic scope, audit rows | Which forms are required before clearance |
| Read-only display of existing allergies and medications | Whether the anticoagulant review is required for every patient |
| Anticoagulant review *field structure* (drug, last dose, plan, note) | The plan options' clinical wording; any rule on them |
| Clearance mechanics (cleared / not cleared + reason, audit) | Clearance criteria — **none are encoded** |
| Draft form *structures* stored as drafts | Form content, wording, publication |
| Terminology storage with codes, versions, EN/AR, retirement | The terms themselves and their Arabic wording |
| Lab panel showing existing results | Which labs (INR, platelets…), time window — **no thresholds** |
| ASA as a clinic-configurable option, off by default | Whether ASA is required |

## 3. E2 technical scaffolding (no clinical rule activated)

### 3.1 Data model (proposed — through a reviewed migration, never `db push`)

| Model | Purpose | Key fields |
|---|---|---|
| `EndoscopyCase` | One per endoscopy visit (shared patient record, existing `Visit`); **opened only by the explicit Start Visit** (TD-01) | `id`, `clinicId`, `branchId`, `patientId`, `visitId` (unique), `procedureTypeCode`, `procedureTypesVersion`, `otherProcedureName`, `otherDescription`, `state` (`PREP` · `CLEARED` · `NOT_CLEARED` · later gates add more), `endoscopistId`, `createdById`, `revision`, timestamps, `shared` |
| `EndoscopyEvent` | Append-only timeline; one row per event, own server time and actor; never derived from another event | `id`, `caseId`, `clinicId`, `branchId`, `patientId`, `visitId`, `code`, `at` (server), `actorId`, `actorName`, `note` |
| `EndoscopyAssessment` | Anticoagulant / antiplatelet review, versioned (each save a new row; history kept) | `caseId`, `revision` (unique per case), `anticoagulants` (JSON: drug, last dose, plan, note), `note`, `recordedById`, `recordedAt` |

The clearance decision is the case `state` plus a `cleared` / `not_cleared` event (reason in the event `note`), so every decision and change of decision stays in the timeline. Exact SQL: backend PR #27, migration `20261107120000_endoscopy_e2_case`.

Event codes are a frozen, versioned code list (as the procedure types). E2 writes only `start_visit`, `assessment_saved`, `cleared`, `not_cleared`; check-in is read from the existing Appointment. Later gates add, each as its own event with its own time: `timeout_signed`, `sedation_started`, `procedure_started`, `scope_inserted`, `scope_withdrawn`, `procedure_ended` (E3), `recovery_started`, `discharged` (E6), `report_signed` (E5). Sedation start and procedure start, and scope insertion and withdrawal, are always separate events (TD-02).

**Planned for later gates (no E2 table):** assisting staff — several members, each with a role (E3, `EndoscopyCaseStaff`); specimens — each with a unique Specimen ID linked to its pathology lab order (E4, `EndoscopySpecimen`).

**Not created:** a separate patient record or EMR; new permission keys; any table for forms (Core `FormTemplate` / `FormResponse` are reused).

### 3.2 Backend APIs (module `src/modules/endoscopy/`, mounted through `src/modules/index.ts`)

| Method and path | Does | Permission | Visit |
|---|---|---|---|
| `GET /visits/:visitId/endoscopy/case` | Case, events, assessment, required-forms status | `emr:read` + record walls | any |
| *(no route)* | The case is opened by a Core hook inside the Start Visit transaction (`onVisitStarted`, registered by the module): when the visit's specialty is `endoscopy` and the clinic has Endoscopy enabled, it creates the case and its `start_visit` event in the same transaction as `VISIT_STARTED`. Idempotent (unique `visitId`): a reopen adds no second case. A visit started before E2 shows "No endoscopy case — this visit was not started as an endoscopy visit with the module on"; opening a tab never creates one (TD-01) | the Start Visit permission (`visit:start` + Core rules) | — |
| `PUT /visits/:visitId/endoscopy/case/type` | Procedure type; Other requires a name | `emr:write` ⚕ | open |
| `PUT /visits/:visitId/endoscopy/assessment` | Anticoagulant review (structure only), revision-checked (`409 STALE`) | `emr:write` ⚕ | open |
| `POST /visits/:visitId/endoscopy/clearance` | Cleared / not cleared; reason required for not cleared; refused while required published forms are unsigned | **`procedures:manage`** ⚕ (TD-03) | open |
| `GET /patients/:id/endoscopy/pre-assessment` | Read-only: active allergies (`PatientFlag`, `status = active`); **current medications** — `PrescriptionItem` with `status = active` only, each with its status, prescribing visit, date and prescriber (TD-05); INR and platelet results when present on the record (`ServiceResult`), with their date — no threshold | `emr:read` + walls | any |
| `GET /endoscopy/event-codes`, `GET /endoscopy/terminology` | Frozen code lists, marked `provisional: true` | authenticated | — |

Every write: `409 MODULE_NOT_ENABLED` if the clinic has not enabled Endoscopy; `403 BRANCH_FORBIDDEN` outside the member's branch; existing visit-lock rules (`visitWritable`) — a closed visit is never modified; `404` for records behind a branch wall.

### 3.3 Frontend (`app/patients/endoscopy/`)

| Component | Content |
|---|---|
| `EndoscopyCaseTab` | New manifest tab on endoscopy visits; procedure type picker; case state; event timeline |
| `PreAssessmentPanel` | Allergies, medications, available labs — **read-only**, links to the main record to change them |
| `AnticoagulantReview` | Structured entry; no warnings or thresholds |
| `ClearanceControl` | Cleared / Not cleared; reason dialog; shows which required forms are unsigned |
| `RequiredFormsList` | Endoscopy-scoped Core Forms for the visit and their signed state; opens the Core form filler |
| `ProvisionalBanner` | "Clinical content pending validation" shown on every Endoscopy clinical screen until CR-1 is validated |

All labels in `lib/i18n/ar.ts`; free text kept as written (E-P8).

### 3.4 Draft forms

The four form structures (consent, pre-procedure checklist, time-out, discharge) are delivered as **draft** `FormTemplate`s with `scopeType: 'specialty'`, `scopeRef: 'endoscopy'`, through an explicit, per-clinic "add starter drafts" action — never seeded automatically, never published by the system. Only the clinic's clinical lead publishes them (existing Forms permissions).

### 3.5 As built (backend #28, frontend #25)

- Module hook `src/modules/visitHooks.ts` called from both Core Start Visit transactions (queue "In room" and `POST /appointments/:id/open-visit`); skipped when the desk moves a visit without reading it.
- Routes as in 3.2; an extra timeline event `type_set` records each change of procedure type.
- **Required clearance forms are clinic configuration:** `Clinic.settings.endoscopy.clearanceFormIds` — empty by default, so no form is required until the clinic configures it; only *published* forms in that list are checked. (Setting screen: later, with the clinical lead.)
- Frontend tab **Endoscopy case** (default on an endoscopy visit) with the "Clinical content pending validation" banner; "Add starter forms (drafts)" on the same tab.

## 4. Clinical activation (blocked until validation)

Until the related CR-1 rows are validated: no form is published; no clearance criterion is encoded; no mandatory field beyond the technical ones (Other name, not-cleared reason); no lab threshold; ASA off; terminology flagged provisional. After validation, the matrix rows are updated, the drafts are corrected, and the Product Owner approves activation.

## 5. Schema and release plan

1. Gate branch `gate-e2` from `main` (backend and frontend).
2. Schema change in `schema.prisma` **with** its migration (`prisma migrate diff` from the current schema), reviewed in the PR; the CI migration guard proves it applies on an empty and a Railway-shaped database.
3. **No migration is run on Railway** by Claude. After merge, the Product Owner approves and runs `prisma migrate deploy` (backup first), as for the baseline.
4. Nothing merged without the Product Owner's approval.

## 6. Security and permissions

| Action | Permission | Scope | Audit action |
|---|---|---|---|
| Read case / pre-assessment | `emr:read` (or `visit:start`) | clinic + record branch walls | — |
| Open case (inside Start Visit) | the Start Visit permission | member's branch | `ENDOSCOPY_CASE_OPENED` (same transaction as `VISIT_STARTED`) |
| Set procedure type | `emr:write` ⚕ | branch; open visit | `ENDOSCOPY_TYPE_SET` |
| Save anticoagulant review | `emr:write` ⚕ | branch; open visit | `ENDOSCOPY_ASSESSMENT_SAVED` (fields changed, revision) |
| Clear / not clear | `procedures:manage` ⚕ (held by the doctor role by default; nurses do not hold it) | branch; open visit | `ENDOSCOPY_CLEARED` / `ENDOSCOPY_NOT_CLEARED` (reason) |
| Add starter draft forms | `forms:build` | clinic | existing `FORM_*` audit; drafts only — publication by the clinic's designated clinical lead (TD-04) |

Each audit row carries `clinicId`, `patientId`, `visitId` and is written in the same transaction as the change. ⚕ = clinical permission (`CLINICAL_PERMS`). No new permission keys (E-P5).

## 7. Validation rules (technical)

- One case per visit; the visit's specialty must be `endoscopy`.
- Procedure type must be one of the frozen codes; `other` requires a name (1–120 characters).
- Not cleared requires a reason; cleared requires every **published** required form to be signed for this visit (rule inactive while none are published).
- Event times are set by the server; the client never sends a time.
- Revision check on assessment saves (`409 STALE`).
- A closed visit refuses every write (existing lock).

## 8. Test plan

| Level | Scenarios |
|---|---|
| Unit | Event-code and terminology lists frozen, versioned, EN + AR; procedure-type validation; clearance input validation |
| Route (mocked) | Permission matrix per route; `409 MODULE_NOT_ENABLED`; `403 BRANCH_FORBIDDEN`; closed visit refused |
| PostgreSQL (CI) | Start Visit of an endoscopy visit opens exactly one case and one `start_visit` event in the same transaction; a reopen adds none; a non-endoscopy visit or a clinic without Endoscopy gets none; opening the tab creates nothing; a nurse (no `procedures:manage`) cannot clear; a discontinued or cancelled prescription never appears as a current medication; audit row in the same transaction; branch wall hides another branch's case; not cleared without reason refused; cleared refused while a published required form is unsigned; drafts never published by the system; stale revision refused |
| Migration | CI guard: the E2 migration applies on empty and Railway-shaped databases |
| Frontend | Tab shown only on endoscopy visits of a clinic with Endoscopy enabled; pre-assessment read-only; provisional banner; Arabic labels |
| Manual acceptance | Script for the Product Owner, run before merge |

## 9. Technical decisions

Recorded in `ADR-012-Technical-Decision-Register.md` (TD-01 to TD-10).

## 10. Dependencies on Core

Start Visit seam (`getOrCreateOpenVisit` and the two Start Visit transactions in `src/routes/appointments.ts` — a module hook is added there, the Core still never names a specialty); visit lifecycle and visit lock (`visitWritable`, `recordEntryVisitGuard`); module resolver and `moduleEnabled` (ADR-011); record walls (`recordWall` / `sees`); `PatientFlag` (allergies); `Prescription` / `PrescriptionItem` (`status` active / discontinued); `ServiceOrder` / `ServiceResult` (labs, later pathology); Core Forms (`FormTemplate`, `FormResponse`, `formAppliesTo` with `scopeType: 'specialty'`); `AuditLog`; `lib/i18n/ar.ts`.

## 11. Unresolved clinical decisions (for Dr. Mohammed Saleh)

1. Procedure types: confirm the seven; Arabic names.
2. Indication list.
3. Sedation types, and the staff roles involved.
4. Bowel preparation scale (named scale or simple grades).
5. Whether to record caecal intubation and withdrawal time; whether either becomes required.
6. Whether ASA classification is required.
7. Lifecycle: whether any event is missing; which are mandatory per procedure type.
8. Who signs the time-out.
9. Anticoagulant review: required for every patient or only when an anticoagulant is listed; the plan options.
10. Which labs to show (INR, platelets, others) and how recent.
11. Which forms are required before clearance.
12. Content and Arabic wording of the four forms.
13. Escort requirement after sedation; consent by guardian.
14. Sites, findings, interventions, complications and incomplete reasons: completeness and Arabic wording.
15. Capsule reading fields.
16. Discharge criteria and destinations.
17. Who acknowledges a pathology result.

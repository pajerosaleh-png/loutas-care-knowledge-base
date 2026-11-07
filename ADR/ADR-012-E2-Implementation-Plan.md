# ADR-012 — Gate E2 Technical Implementation Plan (Endoscopy case and pre-procedure assessment)

**Document ID:** ADR-012-E2  
**Linked to:** `ADR/ADR-012-Endoscopy-Module.md`, `ADR/ADR-012-CR1-Decision-Matrix.md`  
**Status:** Plan only — Gate E2 **not approved**; clinical content **Preliminarily Approved — Pending Clinical Validation**  
**Date:** 2026-11-07

---

## 1. Readiness status

| Area | Status |
|---|---|
| Schema baseline (prerequisite) | **Done** — backend #26 merged; recorded on Railway 2026-11-07; CI migration guard active |
| Gate E1 (shell, procedure types) | **Done** — merged |
| CR-1 clinical validation | **Blocked** — Dr. Mohammed Saleh has not reviewed the sheet yet |
| E2 technical plan | **Ready** — this document |
| E2 technical scaffolding (section 3) | **Ready to start** once the Product Owner approves the E2 scaffolding scope |
| E2 clinical activation (section 4) | **Blocked** until CR-1 rows C1–C6, D1–D2, A2, A19 are validated |

**Blockers:** (1) gastroenterologist validation of CR-1; (2) Product Owner approval of the E2 gate scope and of the E2 schema migration (section 5); (3) the open technical decisions in section 9.

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
| `EndoscopyCase` | One per endoscopy visit (shared patient record, existing `Visit`) | `id`, `clinicId`, `branchId`, `patientId`, `visitId` (unique), `procedureTypeCode`, `procedureTypesVersion`, `otherProcedureName`, `otherDescription`, `state` (`PREP` · `CLEARED` · `NOT_CLEARED` · later gates add more), `endoscopistId`, `createdById`, `revision`, timestamps, `shared` |
| `EndoscopyEvent` | Append-only timeline; one row per event, own server time and actor; never derived from another event | `id`, `caseId`, `clinicId`, `branchId`, `patientId`, `visitId`, `code`, `at` (server), `actorId`, `actorName`, `note` |
| `EndoscopyAssessment` | Anticoagulant / antiplatelet review and clearance decision, versioned (a change is a new revision; history kept) | `caseId`, `anticoagulants` (JSON: drug, last dose, plan, note), `decision`, `notClearedReason`, `decidedById`, `decidedAt`, `revision` |

Event codes are a frozen, versioned code list (as the procedure types). E2 writes only `start_visit`, `assessment_saved`, `cleared`, `not_cleared`; check-in is read from the existing Appointment. The other nine events are added by E3–E6.

**Not created:** a separate patient record or EMR; new permission keys; any table for forms (Core `FormTemplate` / `FormResponse` are reused).

### 3.2 Backend APIs (module `src/modules/endoscopy/`, mounted through `src/modules/index.ts`)

| Method and path | Does | Permission | Visit |
|---|---|---|---|
| `GET /visits/:visitId/endoscopy/case` | Case, events, assessment, required-forms status | `emr:read` + record walls | any |
| `POST /visits/:visitId/endoscopy/case` | Idempotent open of the case (advisory lock, unique `visitId`); records `start_visit` with the visit's own start time | `visit:start` or `emr:write` | open |
| `PUT /visits/:visitId/endoscopy/case/type` | Procedure type; Other requires a name | `emr:write` ⚕ | open |
| `PUT /visits/:visitId/endoscopy/assessment` | Anticoagulant review (structure only), revision-checked (`409 STALE`) | `emr:write` ⚕ | open |
| `POST /visits/:visitId/endoscopy/clearance` | Cleared / not cleared; reason required for not cleared; refused while required published forms are unsigned | `emr:write` ⚕ (decision T-3) | open |
| `GET /patients/:id/endoscopy/pre-assessment` | Read-only allergies (`PatientFlag`), active medications (prescriptions), latest lab results present on the record | `emr:read` + walls | any |
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
| Open case | `visit:start` / `emr:write` | member's branch; open visit | `ENDOSCOPY_CASE_OPENED` |
| Set procedure type | `emr:write` ⚕ | branch; open visit | `ENDOSCOPY_TYPE_SET` |
| Save anticoagulant review | `emr:write` ⚕ | branch; open visit | `ENDOSCOPY_ASSESSMENT_SAVED` (fields changed, revision) |
| Clear / not clear | `emr:write` ⚕ | branch; open visit | `ENDOSCOPY_CLEARED` / `ENDOSCOPY_NOT_CLEARED` (reason) |
| Add starter draft forms | existing Forms manage permission | clinic | existing `FORM_*` audit |

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
| PostgreSQL (CI) | Case open is idempotent under concurrency; audit row in the same transaction; branch wall hides another branch's case; not cleared without reason refused; cleared refused while a published required form is unsigned; drafts never published by the system; stale revision refused |
| Migration | CI guard: the E2 migration applies on empty and Railway-shaped databases |
| Frontend | Tab shown only on endoscopy visits of a clinic with Endoscopy enabled; pre-assessment read-only; provisional banner; Arabic labels |
| Manual acceptance | Script for the Product Owner, run before merge |

## 9. Open technical decisions (Product Owner)

| # | Decision | Proposal |
|---|---|---|
| T-1 | When the case is created | Lazily and idempotently when the Endoscopy tab is first opened on an open endoscopy visit (as the Dental examination); the `start_visit` event takes the visit's own start time — the visit's real event, not inferred |
| T-2 | Event storage | Append-only `EndoscopyEvent` table rather than timestamp columns, so E3–E6 add events without schema changes |
| T-3 | Who may clear | `emr:write` ⚕ (clinicians); a nurse cannot clear unless the clinic grants that permission |
| T-4 | Starter drafts | Per-clinic action, never automatic |
| T-5 | Medications source | Active prescriptions on the record; no separate medication list |

## 10. Dependencies on Core

Visit lifecycle and visit lock (`visitWritable`, `recordEntryVisitGuard`); module resolver and `moduleEnabled` (ADR-011); record walls (`recordWall` / `sees`); `PatientFlag` (allergies); `Prescription` (medications); `ServiceOrder` / `ServiceResult` (labs, later pathology); Core Forms (`FormTemplate`, `FormResponse`, `formAppliesTo` with `scopeType: 'specialty'`); `AuditLog`; `lib/i18n/ar.ts`.

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

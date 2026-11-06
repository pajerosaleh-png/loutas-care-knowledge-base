# ADR-011-Multi-Specialty-Architecture.md

**Document ID:** ADR-011  
**Document Classification:** Architecture Decision Record  
**Owner:** Chief Software Architect  
**Status:** Approved  
**Version:** 1.3  
**Created:** 2026-10-09  
**Last Updated:** 2026-11-06  
**Next Review:** 2027-10-09  
**Approval Authority:** Product Owner (decisions of 2026-10-08 and 2026-10-09)

---

# ADR-011 — Multi-Specialty Architecture: Specialty Registry, Clinic Specialties and the Configurable EMR

---

# Status

**Approved** — architecture and the eight project decisions below.

**Implementation status**

| Part | Gate | State |
|---|---|---|
| Specialty Registry, Clinic Specialty, Primary Specialty, three registry APIs, audit scope columns | Gate 1 | **Implemented** on `main` of `loutascare-backend` (commit `6b13ac2`); applied to the development database and verified on 2026-10-11 |
| Clinic Setup specialty screen; booking uses the registry | Gate 2 | **Implemented** on `main` (backend `f055da0`, frontend `31a0e16`, 2026-11-05), on the ported live-site routes: booking, edit and walk-in validate against the clinic's enabled specialties (stored as the key; blank → primary); `GET /clinic/specialties` answers from the registry |
| Configurable-by-specialty EMR / module resolver | Gate 3 | **Implemented** on `main` (backend `e15a7a4`, frontend `b030ac0`, 2026-11-06): `lib/emr` manifests + `resolveEmrTabs` (Core tabs always; a module's tabs only when the clinic enabled its specialty and the open visit is of it; read-only unless the visit is open); one seam in `PatientEMR.tsx`; visit specialty shown in Visit history |
| Dental shell: manifest, Dental Overview, summary card, `GET /patients/:id/dental/summary` (409 `MODULE_NOT_ENABLED` when Dental is off), odontogram mounted read-only and empty | Gate 4 | **Implemented** on `main` (same commits as Gate 3). No dental data is stored |
| Persisted odontogram: `DentalFindingType` (vocabulary, inserted by the server) + `DentalFinding` (FDI tooth / surface, never edited: superseded or voided with a reason, contradictions refused, per-tooth lock, audited) | Gate 5 | **Implemented** on `main` (backend `76e135c`, frontend `4083068`, 2026-11-06); table applied to Railway by the product owner |
| Dental examination: one per dental visit (`DentalExamination`), form defined once in code (`src/modules/dental/examination.ts`, English + Arabic) and served to the screen; server-validated, revision-checked (no silent overwrite), changed fields audited | Gate 6 | **Implemented** on `main` (backend `295eb80`, frontend `f4c317f`, 2026-11-06) |
| Treatment plan (`DentalPlanItem`) on the **Core service catalog**: tooth / surfaces / phase / diagnosis, estimate only; “Done” records a Core `Procedure`; only an explicit **“Add to invoice”** (invoices:manage) adds one Core charge through `invoiceService.addInvoiceCharge`, at the bill branch's price of the day (decision 7) | Gate 7 | **Implemented** on `main` (backend `d11923a`, frontend `020f6c4`, 2026-11-06) |
| Dental imaging: `DentalImageLink` links **Core radiology orders** (`ServiceOrder` / `ServiceResult`) and **Core patient files** to teeth and image kind; dental forms are Core form templates assigned to the Dental specialty (two drafts created in the development clinic). **No image bytes** stored or served — decision D4 (storage in Egypt) kept | Gate 8 | **Implemented** on `main` (backend `0671d8d`, frontend `00decfa`, 2026-11-06). Image upload / preview: Planned, after D4 storage |
| Security and audit hardening: every audit row carries its clinic (`src/lib/auditScope.ts`, plus the viewer reading older unstamped rows by actor — no data rewritten); `GET /audit` patient / visit / action-family filters; security matrix of every Dental route (401 / other clinic / 403 / branch wall / closed visit); Dental activity card for audit readers | Gate 9 | **Implemented** on `main` (backend `bca2909`, frontend `5463e84`, 2026-11-06) |
| Clinical review of the dental vocabulary, examination, plan phases and workflow (decision 8) | After Gate 9 | Planned — needs a practising dentist |
| Periodontal and advanced dental | Later | Deferred pending clinical validation |

Nothing in this document may be read as saying that a Planned item exists in code.

---

# Context

LOUTAS Care serves single-specialty and multi-specialty outpatient clinics. Before this decision:

- No specialty model existed. `Appointment.specialty` and `Visit.specialtyKey` were unvalidated free-text columns.
- Seven unrelated hard-coded specialty lists existed in the frontend and seed data, and none matched another.
- The Specialties admin screen and the dental odontogram were mock/prototype screens with no backend.
- The Patient EMR showed a fixed list of tabs for every visit.

The product requires one patient record that serves every specialty, with specialty-specific functionality layered in — never a second EMR or a second patient file.

---

# Decision

## 1. One Patient → One Patient EMR → Many Visits → Visit Specialty

A patient has exactly one identity and one longitudinal record per clinic. Specialty is a property of the **visit**, not of the patient. There are no specialty-specific patient files, patient identities or copies of core medical history.

## 2. Core EMR + code-defined specialty modules

The EMR is the **Core EMR** plus **code-defined specialty modules**. The Core EMR is the shared patient and clinical foundation (patient, visits, diagnoses, prescriptions, orders, results, files, notes, history).

- The Core EMR is **always available and cannot be disabled**.
- The Core EMR is **not a specialty** and is not an entry in the registry.
- **General Practice is an ordinary configurable specialty.** It can be disabled (for example in a dental-only clinic). No code may make Core EMR availability depend on the `general-practice` key or on any other specialty key.
- The Core never references a specialty by name. Modules are discovered through a manifest (Gate 3).
- This is controlled configurability by specialty. A database-driven EMR builder (arbitrary tabs, fields or workflows defined by administrators) is explicitly **out of scope**.

## 3. Specialty Registry (platform reference data)

The registry is the **single source of truth** for specialty selection and validation across the system. It is System Reference Data: seeded, FK-backed, no Prisma enum, no CRUD screen.

The initial registry has exactly nine specialties, all selectable:

| Key | Name | Module in V1 |
|---|---|---|
| `general-practice` | General Practice | Core EMR only |
| `dental` | Dental | Dedicated Dental module (Gates 4–8, planned) |
| `nutrition` | Nutrition | Core EMR only |
| `fitness` | Fitness | Core EMR only |
| `endoscopy` | Endoscopy | Core EMR only |
| `psychiatry` | Psychiatry | Core EMR only |
| `cardiology` | Cardiology | Core EMR only |
| `dermatology` | Dermatology | Core EMR only |
| `pediatrics` | Pediatrics | Core EMR only |

Being in the registry does not imply a dedicated module. A specialty without a module uses the Core EMR.

## 4. Clinic Specialty and Primary Specialty

- **Clinic Specialty = what the clinic provides.** Each clinic enables any subset of the registry.
- **Visit Specialty = what one visit is about.** It is one of the clinic's enabled specialties.
- Every clinic has **exactly one Primary Specialty**, representing the clinic's primary orientation. The primary:
  - is always enabled;
  - cannot be disabled directly — another enabled specialty must be made primary first;
  - can be changed to any other enabled specialty;
  - never prevents or restricts the other enabled specialties.
- It follows that at least one specialty is always enabled.

## 5. Legacy specialty values

Existing `Appointment.specialty` and `Visit.specialtyKey` values are **not rewritten**. Stored values are normalised to a registry key **when read** (exact key → case-insensitive key or name → known legacy label such as `General` or `Paediatrics`; anything else is unknown). A reviewed one-time backfill may be considered later, after the architecture is stable.

## 6. Reuse of the Core

Specialty modules reuse the Core Service Catalog, Billing, Files, Radiology, Forms, Diagnosis and Prescription infrastructure. There is no specialty-specific billing, catalog, prescription or file-storage system. A treatment plan is a clinical object; it never creates a billing charge automatically (V1 uses an explicit "Add to invoice" through Core Billing).

## 7. Security and audit

- Existing RBAC is reused. No specialty-specific permission model in V1.
- Access to module data is governed by user permission, clinic specialty configuration, visit specialty and visit writability.
- Clinical history (what happened clinically) and audit (who did what, when) are separate.
- `AuditLog` carries nullable scope columns `clinicId`, `patientId`, `visitId` from Gate 1. The audit viewer, record-access auditing and break-glass are Gate 9 topics.

## 8. Clinical review

Dental clinical content in Gates 5–7 is provisional and is reviewed clinically after implementation. Clinical vocabularies are held as reference data so that review changes data, not architecture. Periodontal functionality is deferred until its measurement model is clinically validated.

---

# Gate 1 — what is implemented

Backend only. No frontend, appointment, visit, EMR, dental or billing change.

## Data model

| Model | Purpose | Notes |
|---|---|---|
| `Specialty` | Platform registry | `key` (id), `nameEn`, `nameAr`, `descriptionEn`, `descriptionAr`, `icon`, `readiness` (`available` \| `coming_soon`), `sortOrder`, `active`, `createdAt` |
| `ClinicSpecialty` | A clinic's enabled and primary state | `clinicId` (scalar tenant scope, no relation to `Clinic`), `specialtyKey` → `Specialty.key`, `enabled`, `isPrimary`, `enabledById`, `enabledAt`, `disabledById`, `disabledAt`, `createdAt`, `updatedAt`; unique (`clinicId`, `specialtyKey`); index (`clinicId`, `enabled`) |
| `AuditLog` (existing) | Scope for new audit rows | Added nullable `clinicId`, `patientId`, `visitId`; index (`clinicId`, `createdAt`). Existing rows keep null |

All changes are additive. Gate 1 was applied to the development database by the Prisma migration `20261010_specialty_registry_gate1` (`prisma/migrations/20261010_specialty_registry_gate1/migration.sql`). No existing column is renamed, retyped or removed.

This departs from the `prisma db push` convention used for earlier schema changes. The earlier migrations in the repository do not reflect the full schema, so the migrations folder alone cannot rebuild the database. A policy for later gates is proposed, not yet approved, in the section "Proposed schema-change policy" below.

## Specialty service — the single server authority

`src/lib/specialtyService.ts` is the only code that answers specialty questions or changes specialty state. Route handlers do not re-implement its rules.

| Function | Responsibility |
|---|---|
| `listForClinic` | Registry plus the clinic's enabled/primary state |
| `normalizeKey` | Read-only mapping of a stored value to a registry key, or `null` |
| `setEnabled` | Enable or disable a specialty for a clinic |
| `setPrimary` | Change the clinic's primary specialty |
| `seedSpecialtyRegistry`, `ensureClinicConfigured` | Idempotent bootstrap of the registry and of a clinic that has no rows |

Guaranteed invariants: exactly one primary per configured clinic; the primary is enabled; at least one specialty is enabled. Each write runs in one serializable transaction together with its audit row, and the invariants are re-checked inside that transaction.

## Initial clinic configuration

A clinic with no registry rows receives: General Practice enabled and primary; Cardiology, Dermatology and Pediatrics enabled; Dental, Nutrition, Fitness, Endoscopy and Psychiatry disabled. An already-configured clinic is never changed by the seed.

On the development database the registry was initialised by a specialty-only run of the two service functions (`seedSpecialtyRegistry`, `ensureClinicConfigured`), not by the full seed script, so no user, role or permission data was touched.

## APIs

See `Registry/API_REGISTRY.md` for the three Gate 1 endpoints.

## Audit events

`SPECIALTY_ENABLED`, `SPECIALTY_DISABLED`, `SPECIALTY_PRIMARY_CHANGED` — each with actor, timestamp, `clinicId`, entity (`ClinicSpecialty`) and a `changes` object.

---

# Gate 1 boundary

Gate 1 does **not**:

- change any screen (the Specialties screen still shows mock data until Gate 2);
- validate or change `Appointment.specialty`, or call `normalizeKey` on any stored value;
- change visits, the Patient EMR or the Doctor Workspace;
- add any dental, forms, imaging, file-storage or billing functionality;
- add an audit viewer, record-access auditing, or scope to audit rows written by other modules;
- backfill or rewrite any existing data.

---

# Schema-change policy (approved 2026-11-06)

**Status: Approved** by the Product Owner on 2026-11-06 (with ADR-012). The policy is in force from that date. Gates 5–8 already followed point 1 (migrations `20261106_dental_gate5` … `gate8`).

**Policy**

1. Future schema changes are made through reviewed Prisma migration files, as Gate 1 was.
2. A separate baseline task is prepared for the existing database schema, so that the migration history can describe the whole schema.
3. Before anything is created or baselined, the current development and production schemas and the existing migration history are inspected and compared.
4. A baseline is never applied, a migration is never marked as applied, and no schema-changing command is run against a shared or production database without a separate, explicit approval for that action.

**What does not exist yet**

- No baseline migration has been created.
- No baseline has been applied or marked as applied on any database.
- The migrations folder still cannot rebuild the database on its own: on an empty database it stops at `20261025_schema_batch_a` (`ClinicalHistoryItem` and other tables were created by `db push`).

The baseline task is approved for **preparation** (2026-11-06). Applying it, or marking any migration as applied, on the development or any shared database needs a separate, explicit approval. It must be done before ADR-012 Gate E2.

---

# Alternatives considered

| Alternative | Why rejected |
|---|---|
| Separate EMR or patient file per specialty | Breaks the one-record principle; duplicates identity and history |
| Specialty as a property of the patient | A patient may have medical, dental and other visits |
| General Practice as a locked, always-on "core" specialty | Conflates the Core EMR with a specialty; blocks dental-only clinics |
| Specialty keys only for specialties that have a module | A clinic provides specialties (for example Dermatology) that need no module |
| Database-driven tab and field builder | Unbounded scope; UI structure as data drifts from code |
| Feature-flag system | None exists; the clinic registry already answers the question |
| Rewriting legacy specialty values | Risk to existing data with no functional need |
| Dental-specific permissions in V1 | Existing EMR permissions plus clinic and visit checks are sufficient |

---

# Consequences

**Positive**

- One specialty vocabulary for the whole platform.
- New specialties are a data insert; new modules are code, added without touching the Core.
- Dental-only and multi-specialty clinics are both first-class.
- Every specialty configuration change is tenant-scoped and audited.

**Negative / to manage**

- Until Gate 2, the frontend still uses its hard-coded specialty lists.
- Legacy values that match no registry entry resolve to "unknown" and receive the Core EMR.
- Until the proposed schema-change policy is approved, each schema gate must state how its change is applied (migration or `db push`) and be coordinated on the shared database.

---

# Related documents

- `Registry/API_REGISTRY.md`
- `ADR-001-Multi-Tenant-Architecture.md`
- `ADR-004-Authorization-RBAC.md`
- `ADR-008-Audit-Logging-Architecture.md`
- `04-Clinical/CLN-008-Care-Plan-Architecture.md` (treatment plan, Gate 7)
- `04-Clinical/CLN-009-Follow-Up-Architecture.md`

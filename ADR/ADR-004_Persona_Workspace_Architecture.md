# ADR-004 — Persona-Based Workspace Architecture (Reception · Doctor · MPI · Administration)

| Field | Value |
|-------|-------|
| ADR ID | ADR-004 |
| Title | Persona-Based Workspace Architecture |
| Status | Proposed (for approval) |
| Date | 2026-08-08 |
| Owner | Chief Solution Architect / CPO |
| Supersedes | — |
| Relates to | ADR-002 (schema/`db push` discipline) · ADR-003 v1.1 (Appointment & domain isolation) · ADR-EMR-011 (encounter lifecycle) |
| Governs | Frontend workspace organization and the sequencing of IP-001 Slices S10–S13 |
| Blocking debt | TD-EMR-001 (EMR `Visit.status`/signing lifecycle) — gates "pending signatures" only |

---

## 1. Context

Slices S1–S9 delivered a complete appointment/reception **operational backend**: configurable policy (S1), lifecycle & transitions (S2), conflict/overbooking (S3), reschedule (S4), visit-owner reassignment (S5), the **Queue Engine** as a read-model emitting both a reception queue and a per-provider `byDoctor` projection (S6), walk-in capacity & insertion (S7), check-in window & no-show enforcement (S8), and terminal reconciliation to `COMPLETED` (S9). These engines are cleanly separated at the domain level per ADR-003 (Appointment owns scheduling; Billing owns invoices; EMR owns clinical docs).

The **frontend has not kept pace with this separation**, and three screens now carry responsibilities that belong elsewhere:

- **`appointments_page.tsx`** is already ~80% a reception operational surface (list + calendar views, a client-side "queue/schedule" sort, cancel + reason, confirmed/cancelled counts, a no-show proxy, the lifecycle stepper) — but it does **not yet consume** the S6 queue endpoint, S7 walk-in, S5 reassign, or Open-EMR.
- **`patients_page.tsx`** does **double duty**: its `filterDate==='today'` path builds a **doctor worklist** (`listAppointments({doctorId})`, `__apptId/__visitId/__apptStatus` tagging, `startVisit()`), while it also imports the real longitudinal components `PatientEMR` and `PatientForms` — but over a **mock `PATIENTS` array**, not real data.
- **`dashboard_page.tsx`** is a **static KPI page** (`GET /dashboard/summary`, appointment/revenue cards) with **no operational wiring** — no queue, no `startVisit`, no EMR.

Meanwhile the **S6 `byDoctor` projection has no frontend consumer at all** — a doctor's daily queue was built in the backend but is currently dormant.

The question is whether to keep patching these historical screen layouts, or to re-organize the product around **user personas and operational responsibility**.

---

## 2. Decision

**LOUTAS Care will be organized around four persona-based workspaces**, each owning a distinct operational responsibility, each mapping onto boundaries the S1–S9 backend already enforces:

1. **Reception Workspace** — *"What must happen today?"* (the whole clinic's operational activity today).
2. **Doctor Workspace** — *"What are my patients today, and what should I do next?"* (one physician's clinical workflow today).
3. **Master Patient Index (MPI)** — *"What is this patient's complete longitudinal history?"* (patient identity & history).
4. **Administration Workspace** — *"How is the clinic performing?"* (executive KPIs, analytics, finance).

The current `appointments_page` evolves into the **Reception Workspace**; the current `dashboard_page` **bifurcates** — its operational future becomes the **Doctor Workspace**, and its KPI content relocates into the **Administration Workspace**; the current `patients_page` becomes the **MPI** and **ceases to be a daily worklist**.

Workspaces are **composition/view layers over the existing domains**, not new domains. They read from a single source of truth per concern: operational state from the appointment/queue engines, identity/history from patient/visit, KPIs from the analytics/summary endpoints. ADR-002 and ADR-003 are unchanged.

---

## 3. Architectural rationale

- **The backend is already three-shaped; the UI should mirror it.** S1–S9 separated *today-operations* (scheduling/queue/walk-in/check-in), *per-doctor work* (S6 `byDoctor`), and *patient identity*. The four-workspace model is the frontend expression of that existing boundary — not a new architecture imposed on the code.
- **It activates a dormant, already-built projection.** S6 was deliberately built to emit `byDoctor`; the Doctor Workspace is simply its missing consumer. No new queue engine is required.
- **It removes a genuine anti-pattern.** The doctor worklist living inside the Patient page conflates *identity* with *today's operations*. Relocating it gives that logic a correct home (Doctor Workspace) and lets the Patient page become a true MPI.
- **It converts, not demolishes.** The Administration Workspace is the current KPI dashboard's content relocated; the Doctor Workspace is additive wiring onto a screen that has nothing operational to unwind. Low-risk evolution.
- **It preserves domain isolation (ADR-003).** Workspaces never own business rules; they orchestrate reads/actions against Appointment, EMR, Billing, and Analytics through existing endpoints and the shared `appointmentsApi` mappers.

---

## 4. Responsibility boundaries

**Reception Workspace** (whole-clinic, today): calendar; daily schedule; reception board; reception queue (S6 `reception`); walk-in (S7); check-in / confirm / cancel / reschedule / no-show (S2/S8); reassign (S5); start visit; open EMR; reception operational dashboard.

**Doctor Workspace** (one physician, today): today's patients; doctor queue (S6 `byDoctor`); waiting patients; in-room patients; quick patient search; open EMR; continue active visit; personal schedule; pending documentation. *Pending signatures is deferred to the future EMR signing slice (TD-EMR-001).*

**Master Patient Index** (longitudinal identity/history): demographics; MRN/Name/Phone search; patient banner; visit history; clinical timeline; episodes of care; previous encounters; previous physicians; diagnoses; medications; prescriptions; laboratory history; radiology history; allergies; chronic conditions; clinical documents; administrative documents. **No daily-worklist or appointment-transition logic.**

**Administration Workspace** (clinic performance): executive KPIs; operational analytics; financial overview; performance metrics; branch statistics; productivity dashboards.

**Exclusivity rules.** Appointment-transition actions live only in Reception (clinic-wide) and Doctor (own patients); the MPI is read-only with respect to *today's* operational state. Longitudinal history lives only in the MPI; the other workspaces **deep-link into** it rather than duplicating it. KPIs live only in Administration.

---

## 5. Interaction between the four workspaces

- **Reception → EMR/Doctor:** Reception starts a visit (`IN_ROOM` → `getOrCreateOpenVisit`, `appointment.visitId`) and can Open EMR; the same appointment then surfaces in the owning doctor's queue (S6 `byDoctor`) — one appointment, two views, one backend state.
- **Doctor → EMR/MPI:** the Doctor Workspace opens the active visit's EMR and can deep-link to the patient's MPI for history; it never becomes the history browser itself.
- **Any workspace → MPI:** a stable `patientId` deep-links to the MPI patient banner from Reception, Doctor, or Administration.
- **Operations → Administration:** Administration **aggregates** the same appointment/visit/invoice data the operational workspaces act on; it reads summaries, never mutates operational state.
- **Single source of truth:** operational status is read from the appointment/queue engines (never recomputed per screen); identity/history from patient/visit; KPIs from analytics. No workspace caches another's authoritative state.

---

## 6. Navigation principles

- **Persona-first entry.** A user's role determines the default workspace (reception staff → Reception; physician → Doctor; records/clinical → MPI; management → Administration). Role gating is enforced on both routes and the underlying endpoints (RBAC already exists per-route).
- **The patient is the cross-workspace key.** `patientId` (and, where relevant, `visitId`/`appointmentId`) are the deep-link currency between workspaces; navigating "to a patient" always resolves to the MPI banner.
- **Actions stay with their owner.** "Open EMR" and "Start/Continue Visit" appear wherever operationally needed (Reception, Doctor) but always drive the same appointment/visit endpoints; history navigation always routes to the MPI.
- **No duplicated logic across workspaces.** Shared behavior (status mapping, transitions, queue consumption) is centralized in `appointmentsApi` + shared components (`PatientEMR`, `PatientForms`), consumed by each workspace.
- **Backward-compatible routes.** The four screens already exist as routes; evolution changes their *responsibilities*, not their existence, so navigation remains stable through migration.

---

## 7. Benefits

- **Faithful to the built backend** — the UI finally mirrors the S1–S9 domain boundaries; the S6 `byDoctor` projection stops being dormant.
- **Eliminates the identity/operations conflation** in the Patient page.
- **Clear ownership** — each responsibility has exactly one home, removing the "which screen does X?" ambiguity that currently spans three pages.
- **Reuse-heavy, low-risk** — `PatientEMR`/`PatientForms` reused in the MPI; `appointments_page` extended (not rebuilt) for Reception; the Dashboard converted, not demolished.
- **Cleaner S10–S13 sequencing** — each slice consumes an engine that already exists.
- **Scales to future personas** (lab, pharmacy, radiology) — the persona-workspace pattern generalizes.

---

## 8. Migration strategy from the current UI

Incremental and backward-compatible — no big-bang rewrite:

1. **Reception Workspace (extend `appointments_page`):** consume the S6 `/appointments/queue` endpoint (replace the client-side sort), add walk-in (S7), reassign (S5), and Open-EMR/`visitId` wiring. The existing shell (views, cancel, counts, stepper) stays.
2. **Doctor Workspace (evolve `dashboard_page`):** consume `/appointments/queue?doctorId=<me>` (S6 `byDoctor`), waiting/in-room split, quick search, Open EMR, continue visit, personal schedule, pending documentation. Relocate the current KPI cards to Administration.
3. **MPI (convert `patients_page`):** **remove** the `filterDate==='today'` worklist path (`listAppointments({doctorId})`, `startVisit()` at the worklist), reuse `PatientEMR`/`PatientForms`, and **replace the mock `PATIENTS` array with real patient/visit data**. Retain demographics + MRN/Name/Phone search.
4. **Administration Workspace:** house the relocated KPIs + analytics (existing `GET /dashboard/summary` and future analytics endpoints).
5. **Retire legacy last** — the old worklist path is removed only after the Doctor Workspace supersedes it, preserving backward compatibility throughout.

Discipline reminders (from prior sessions): apply frontend files only under `C:\LoutasCare\frontend` (avoid the duplicate folder), clear stale `.next` cache on activation, keep `appointmentsApi` mappers as the single canonical/display boundary.

---

## 9. Impact on S10–S13

| Slice | Scope under ADR-004 | Consumes (already built) |
|------|---------------------|--------------------------|
| **S10** | Reception Workspace | S6 `reception` queue, S7 walk-in, S5 reassign, S2/S8 transitions, Open EMR |
| **S11** | Doctor Workspace | **S6 `byDoctor`**, `startVisit`, Open EMR (pending-signatures deferred to EMR signing slice) |
| **S12** | MPI conversion + legacy worklist retirement | `PatientEMR`/`PatientForms`, real patient/visit data |
| **S13** | Administration Workspace | `dashboard/summary` + analytics |

Net effect: **simplifies** sequencing — each slice is a consumer of an existing engine with a single, unambiguous responsibility, rather than an entanglement of identity + operations across screens.

---

## 10. Risks

- **Pending-signatures dependency (TD-EMR-001).** The EMR encounter Signed/close lifecycle is not implemented (schema `OPEN|SIGNED|LOCKED` vs code `IN_PROGRESS`; no `SIGNED` write path). The Doctor Workspace ships without "pending signatures"; that widget lands with the future EMR signing slice. *Not a blocker for the rest of the workspace.*
- **MPI data migration.** `patients_page` currently renders a **mock** `PATIENTS` array; the MPI needs real patient/visit data wiring — a discrete task within S12, not a same-day change.
- **KPI relocation.** Moving KPIs from the Dashboard to Administration must preserve existing summary consumers; a placement decision, low risk.
- **Logic duplication.** Reception and Doctor both act on appointment state; mitigated by centralizing in `appointmentsApi` + shared components (no per-workspace re-implementation).
- **Role gating.** Persona-first navigation must be enforced on endpoints (RBAC exists) as well as routes, to prevent a workspace exposing actions outside its persona.
- **Deferred defects unchanged.** Issue 2 (timezone day-boundary) remains parked for the Appointment Regression Phase; Care Team (TD-S5-001) remains deferred — neither is reopened by this ADR.

---

## 11. Recommendation

**Adopt ADR-004.** The persona-based four-workspace architecture is the faithful frontend expression of the S1–S9 backend boundaries, is reuse-heavy and low-risk (extend/convert rather than rebuild), and it activates the already-built S6 `byDoctor` projection. Approve this ADR as the binding architecture for S10–S13, with two explicit carve-outs recorded: **pending signatures** depends on the future EMR signing slice (TD-EMR-001), and the **MPI real-data migration** is scoped within S12. On approval, S10 (Reception Workspace) is implemented against this record under the standard governance gate.

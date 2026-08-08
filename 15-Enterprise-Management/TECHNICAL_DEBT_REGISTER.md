# Engineering Technical Debt Register

---

## Document Information

| Item          | Value |
| ------------- | ----- |
| Document ID   | PM-005 (provisional — pending Enterprise Management registry confirmation) |
| Document Name | Engineering Technical Debt Register |
| Area          | 15-Enterprise-Management |
| Version       | 1.2 |
| Status        | Active (ongoing governance artifact) |
| Milestone     | M-018 — Established |
| Owner         | Lead Engineer / Chief Solution Architect |
| Last Updated  | 2026-08-06 |
| Related       | ADR-003 v1.1; ADR-EMR-011 v1.0; IP-001; PM-006; EMR Visit.status reconciliation |

---
## Purpose & governance

This register is the single, centralized record of accepted engineering technical debt. Every deferred decision, backward-compatibility shim, or dormant capability introduced during implementation is recorded here with its impact, its planned resolution, the slice/ADR that owns it, and cross-references. Items are added at the point the debt is accepted (usually a slice completion report) and closed only when the resolution ships and is verified.

The PMO Risk/Debt Register will be synchronized from this document after the Appointment implementation phase completes; until then, this register takes precedence for engineering debt.

Severity: **High** (risks correctness/data), **Medium** (limits a feature or creates inconsistency), **Low** (cosmetic/maintenance).

---

## Register (summary)

| ID | Title | Origin | Severity | Resolution owner | Status |
|----|-------|--------|----------|------------------|--------|
| TD-S2-001 | Persisted terminal `DONE` → canonical `COMPLETED` | S2 | Medium | S9 | **Resolved (S9)** |
| TD-EMR-001 | EMR `Visit.status` vocabulary reconciliation (schema `OPEN/SIGNED/LOCKED` vs code `IN_PROGRESS/COMPLETED/CANCELLED`) | EMR | Medium | Future EMR slice | Open |
| TD-S2-002 | Legacy appointment/patient pages spot-patched for the canonical lifecycle | S2 | Medium | S10 — Reception Workspace | Open |
| TD-S3-001 | S3 conflict-detection deferrals (room dormant; legacy exact-slot guard vs config overbooking) | S3 | Medium | Future Room domain / conflict-engine consolidation | Open |
| TD-S5-001 | Operational Care Team deferred (no consumer until S6/S10) | S5 | Medium | S6 Queue / S10 Reception | Open |
| TD-S5-002 | `PATCH doctorId` deprecation in favour of `/reassign` | S5 | Low | S10 (UI migration) | Open |

---

## TD-S2-001 — Persisted terminal status remains `DONE`

**Status:** Resolved (S9) · **Severity:** Medium · **Origin:** Slice S2 · **Resolved by:** S9

**Description.** The canonical appointment lifecycle (ADR-003 v1.1) names the completion terminal `COMPLETED`, but the running system persists it as `DONE`. S2 kept `DONE` as the stored value and treats `COMPLETED` as an alias (`STATUS_CANON`), to preserve backward compatibility for existing readers. No stored value was changed.

**Impact.** The appointment vocabulary is not yet fully canonical: dependent readers (billing projections, calendar/labels, reports) still key on `DONE`. Low correctness risk, but the KB and code are not literally aligned on the terminal name.

**Why deferred.** Renaming `DONE → COMPLETED` touches Billing and UI (both outside S2's scope) and must migrate all dependent modules together to avoid a mixed vocabulary. It is naturally owned by S9, where the EMR-close → appointment-`COMPLETED` behaviour (BR-APP-008) is implemented.

**Resolution plan (S9), as one coordinated change set.**
1. Flip the stored appointment terminal `DONE → COMPLETED`; retire the `completed → DONE` alias in `STATUS_CANON`.
2. Update Billing projections that read the terminal token.
3. Update frontend labels (calendar, Reception/Doctor views — S10 surfaces).
4. Update reports/analytics reading the terminal.
5. One-time reversible backfill of existing `DONE` rows → `COMPLETED` (ADR-002 `prisma db push`, no data loss).
6. Reconcile jointly with the coupled EMR `Visit.status` vocabulary item below.

**Closure criterion.** No reader references `DONE`; stored value is `COMPLETED`; backfill applied; alias removed — all in a single S9 change set.

**Resolution (S9).** Stored terminal flipped `DONE → COMPLETED` with backward-compatible dual-read (`canonStatus` maps legacy `done/DONE → COMPLETED`) and a one-time reversible backfill; `doneAt` retained; the appointment→visit completion side-effect repointed to `COMPLETED`. Verified no consumer outside the appointment domain reads the token (billing relates by `appointmentId`, not status); frontend maps `COMPLETED → 'Done'`.

**Cross-references.** ADR-003 v1.1 §3.1 · IP-001 Slice S9 · TD-EMR-001 (the coupled EMR reconciliation, now split out — see below).

---

## TD-S2-002 — Legacy pages spot-patched for the canonical lifecycle

**Status:** Open · **Severity:** Medium · **Origin:** Slice S2 · **Resolution owner:** S10 (Reception Workspace)

**Description.** S2 introduced the canonical lifecycle backend-first. The pre-existing appointment/patient front-end surfaces were built against the old status vocabulary and have been spot-patched for compatibility rather than rebuilt: `lib/appointmentsApi.ts` (BOOKED/NO_SHOW display↔canonical maps, `startVisit` wrapper), `app/appointments/page.tsx` (`statusStyle`, `STATUSES`, `rank`, and workflow-button branches for Booked/No-show), and `app/patients/page.tsx` (Start-Visit binding).

**Impact.** The old pages work but are transitional. Other surfaces that hard-code pre-canonical statuses (`Confirmed`/`Done`) may still surface gaps until migrated. Coupling between `appointmentsApi` maps and page button branches must stay in sync.

**Why deferred.** Full front-end alignment to the canonical lifecycle is owned by the Reception Workspace (S10) and Doctor View (S11), which replace these transitional surfaces. Rebuilding them in S2 was out of scope.

**Resolution plan.** S10/S11 deliver the unified Reception and Doctor workspaces against the canonical lifecycle, retiring the spot-patched surfaces. Until then, any new status-keyed UI must use the canonical display map in `appointmentsApi`.

**Closure criterion.** Reception and Doctor workspaces (S10/S11) render the full canonical lifecycle; no surface hard-codes pre-canonical status tokens.

**Cross-references.** ADR-003 v1.1 (canonical lifecycle) · IP-001 Slices S10 (Reception Workspace), S11 (Doctor View) · LAEF-IMPL-001 (S2 record).

---

## TD-S3-001 — S3 conflict-detection deferrals

**Status:** Open · **Severity:** Medium · **Origin:** Slice S3 · **Resolution owner:** Future Room domain / conflict-engine consolidation

**Description.** S3 delivered doctor double-booking with configurable buffers and overbooking. Two deferrals were accepted:

- **(a) Room availability is engine-only, live-dormant.** `conflictDetectionService` implements and unit-tests room-conflict logic, but the `Appointment` model has no `roomId` and there is no Room/Resource model, so no live appointment carries a room. No dead schema field was added (ADR-002: additive only if required), so room-conflict enforcement is inactive until a Room domain exists.
- **(b) Legacy exact-slot guard coexists with the config-driven engine.** The create endpoint's pre-existing exact-slot guard (which also covers free-text `doctorName`) was left in place. Consequently, when `overbookingAllowed = true`, an *exact* same-slot booking is still hard-blocked by the old guard; config-driven overbooking currently applies to buffer-overlap (non-exact) cases.

**Impact.** (a) Two appointments can occupy the same room without warning (no room data today, so latent). (b) Overbooking is not uniformly honoured — exact same-slot doubles remain blocked regardless of policy.

**Why deferred.** (a) Adding a Room/Resource domain is a separate design out of S3 scope. (b) Folding the legacy exact-slot guard into the conflict engine would modify validated existing behaviour mid-slice; deferred to a focused consolidation to keep S3 additive.

**Resolution plan.**
- (a) When a Room/Resource domain is introduced, add `roomId` (additive) and populate it at booking; the engine's room path activates with no logic change.
- (b) Consolidate the exact-slot guard into `conflictDetectionService` so exact and overlap conflicts are resolved through one config-aware path (preserving free-text `doctorName` protection), making overbooking uniform.

**Closure criterion.** Room conflicts are enforced live once rooms exist; a single conflict path governs exact and overlap cases so overbooking policy applies uniformly.

**Cross-references.** ADR-003 v1.1 AD-013 (conflict validation) · IP-001 Slice S3 · IP-001 Slice S7 (walk-in capacity — related configurable-limit work) · Future Room / Resource Management domain.

---

## TD-S5-001 — Operational Care Team deferred

**Status:** Open · **Severity:** Medium · **Origin:** Slice S5 · **Resolution owner:** S6 (Queue) / S10 (Reception)

**Description.** Under the approved lean S5 (Option B), Visit-Owner reassignment was delivered but the **operational Care Team** (an additive `AppointmentCareTeam` table + assign/remove/list) was deferred. No consumer exists yet — its readers are the Queue Engine (S6) and the Reception/Doctor workspaces (S10/S11) — so building it now would be dormant capability (same discipline as the room deferral in TD-S3-001).

**Impact.** AD-014's operational-Care-Team half is not yet modeled; only the single Visit Owner (`doctorId`) is assigned at booking.

**Resolution plan.** Introduce `AppointmentCareTeam` (additive, ADR-002) when S6/S10 need it; assign at booking; attributed + audited.

**Closure criterion.** Care Team members can be assigned/managed and are consumed by the queue/reception surfaces.

**Cross-references.** ADR-003 AD-014 / AD-018 · IP-001 S5 / S6 / S10 · ADR-EMR-011 (clinical Care Team stays with the EMR).

---

## TD-S5-002 — Deprecate `PATCH doctorId` in favour of `/reassign`

**Status:** Open · **Severity:** Low · **Origin:** Slice S5 · **Resolution owner:** S10 (UI migration)

**Description.** S5 introduced `POST /appointments/:id/reassign` as the first-class, audited reassignment path (mandatory reason, conflict re-validation, `APPOINTMENT_REASSIGN` event). For backward compatibility, `PATCH /appointments/:id` still accepts `doctorId` changes (the current edit dialog uses it). `PATCH doctorId` is S3-conflict-gated and audited as `APPOINTMENT_UPDATE`, but lacks the first-class reassignment semantics.

**Impact.** Two paths can change the Visit Owner; the `PATCH` path lacks the mandatory-reason + `APPOINTMENT_REASSIGN` event.

**Resolution plan.** When the UI migrates to `/reassign` (S10), remove `doctorId` handling from `PATCH` (reject with "use /reassign"), so Visit-Owner changes flow exclusively through the first-class path.

**Closure criterion.** `PATCH` rejects `doctorId`; all reassignment goes through `/reassign`.

**Cross-references.** ADR-003 AD-015 · IP-001 S5 / S10 · TD-S2-002 (frontend spot-patch migration).

---

## TD-EMR-001 — EMR `Visit.status` vocabulary reconciliation

**Status:** Open · **Severity:** Medium · **Origin:** identified at S9 · **Resolution owner:** future EMR slice

**Description.** The EMR `Visit` model has a stale/inconsistent status vocabulary: schema declares `@default("OPEN") // OPEN | SIGNED | LOCKED`, while `visits.ts` creates/queries with `IN_PROGRESS` and sets `COMPLETED`/`CANCELLED` (with `closedAt`) on the appointment-driven completion side-effect. No `SIGNED`/`LOCKED` write path exists (the ADR-EMR-011 clinical signing lifecycle is not implemented in code).

**Why separate (not blocking S9).** Appointment completion already drives visit closure (`status='COMPLETED', closedAt`) via the existing appointment→visit side-effect, so S9's business objective is met without touching EMR clinical code. Reconciling the `Visit.status` vocabulary is a clinical-domain cleanup, independent of the appointment terminal rename.

**Resolution plan.** In a dedicated EMR slice, reconcile `Visit.status` to one canonical set aligned with ADR-EMR-011 (implement the Signed/close lifecycle), correct the schema default/comment, and — if desired — add EMR-driven completion. No ADR change is implied by this record.

**Cross-references.** ADR-EMR-011 v1.0 (encounter lifecycle) · ADR-003 v1.1 · TD-S2-001 (resolved; formerly coupled) · IP-001 (future EMR slice).

---

## Revision History

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-08-06 | Register created; recorded TD-S2-001, TD-S2-002, TD-S3-001. |
| 1.1 | 2026-08-06 | Recorded TD-S5-001 (Care Team deferred) and TD-S5-002 (PATCH doctorId deprecation). |
| 1.2 | 2026-08-08 | TD-S2-001 marked Resolved (S9); EMR Visit.status reconciliation split out as TD-EMR-001 (future EMR cleanup). |

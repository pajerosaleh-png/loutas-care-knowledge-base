# Engineering Technical Debt Register

| Field | Value |
|-------|-------|
| Document ID | TDR-001 |
| Title | Engineering Technical Debt Register |
| Status | Active — Pending Product Owner Review |
| Version | 1.0 |
| Owner | Lead Engineer / Chief Solution Architect |
| Last Updated | 2026-08-06 |
| Scope | Centralized register of engineering technical debt. Authoritative source until the PMO Risk/Debt Register is synchronized after the Appointment implementation phase. |
| Related | ADR-003 v1.1; ADR-EMR-011 v1.0; IP-001; LAEF-IMPL-001 |

---

## Purpose & governance

This register is the single, centralized record of accepted engineering technical debt. Every deferred decision, backward-compatibility shim, or dormant capability introduced during implementation is recorded here with its impact, its planned resolution, the slice/ADR that owns it, and cross-references. Items are added at the point the debt is accepted (usually a slice completion report) and closed only when the resolution ships and is verified.

The PMO Risk/Debt Register will be synchronized from this document after the Appointment implementation phase completes; until then, this register takes precedence for engineering debt.

Severity: **High** (risks correctness/data), **Medium** (limits a feature or creates inconsistency), **Low** (cosmetic/maintenance).

---

## Register (summary)

| ID | Title | Origin | Severity | Resolution owner | Status |
|----|-------|--------|----------|------------------|--------|
| TD-S2-001 | Persisted terminal kept as `DONE` (canonical `COMPLETED` deferred) | S2 | Medium | S9 — EMR Completion Integration | Open |
| TD-S2-002 | Legacy appointment/patient pages spot-patched for the canonical lifecycle | S2 | Medium | S10 — Reception Workspace | Open |
| TD-S3-001 | S3 conflict-detection deferrals (room dormant; legacy exact-slot guard vs config overbooking) | S3 | Medium | Future Room domain / conflict-engine consolidation | Open |

---

## TD-S2-001 — Persisted terminal status remains `DONE`

**Status:** Open · **Severity:** Medium · **Origin:** Slice S2 · **Resolution owner:** S9 (EMR Completion Integration)

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

**Cross-references.** ADR-003 v1.1 §3.1 (state reconciliation) · ADR-EMR-011 v1.0 (encounter `COMPLETED`) · IP-001 Slice S9 · **EMR `Visit.status` vocabulary reconciliation** (schema `OPEN/SIGNED/LOCKED` vs code `IN_PROGRESS/COMPLETED/CANCELLED`) — this appointment-terminal rename and the encounter-status reconciliation are coupled and should be resolved in the same S9 pass to avoid two migrations on the same seam.

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

## Revision History

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-08-06 | Register created; recorded TD-S2-001, TD-S2-002, TD-S3-001. |

# ADR-012 — Technical Decision Register (Endoscopy)

**Document ID:** ADR-012-TDR  
**Linked to:** `ADR/ADR-012-Endoscopy-Module.md`, `ADR/ADR-012-E2-Implementation-Plan.md`  
**Scope:** technical decisions only. Clinical decisions are in `ADR-012-CR1-Decision-Matrix.md` and stay pending clinical validation.

| ID | Decision | Status | Date | Notes |
|---|---|---|---|---|
| TD-01 | **Case creation:** the Endoscopy case is created only when an authorised user explicitly selects **Start Visit** on an endoscopy visit, in the same transaction as `VISIT_STARTED`, through a module hook in the Core Start Visit seam. Opening the Endoscopy tab never creates a case. Idempotent: one case per visit; a reopen adds none | **Approved (changed by the Product Owner)** | 2026-11-07 | Replaces the proposal "create when the tab is first opened". Matches ADR-012 E-P2 |
| TD-02 | **Timeline:** each step is its own append-only `EndoscopyEvent` row with its own server time and actor; no time inferred from another; history and audit kept. Sedation start ≠ procedure start; scope insertion ≠ scope withdrawal | **Approved** | 2026-11-07 | |
| TD-03 | **Clearance permission:** Cleared / Not cleared requires `procedures:manage` (a gated clinical permission held by the doctor role by default, not by nurses), Endoscopy enabled for the clinic, the member's branch, an open visit; one audit row per decision in the same transaction | **Approved** (permission key defined here) | 2026-11-07 | No new permission key (E-P5) |
| TD-04 | **Starter forms:** added per clinic on request (`forms:build`), always as drafts; only the clinic's designated clinical lead reviews, approves and publishes them; the system never publishes them | **Approved** | 2026-11-07 | |
| TD-05 | **Current medications:** from existing prescriptions; only `PrescriptionItem.status = active` items, read-only, each with its status and source (visit, date, prescriber); discontinued items and items of cancelled visits are never shown as current | **Approved with conditions** | 2026-11-07 | Conditions as written |
| TD-06 | Clearance decision kept as the case `state` plus a `cleared` / `not_cleared` event (reason in the event note), not as a column overwritten in place | Proposed (in PR #27) | 2026-11-07 | Keeps every decision change in the timeline |
| TD-07 | Anticoagulant review versioned: each save is a new `EndoscopyAssessment` row (unique `caseId + revision`) | Proposed (in PR #27) | 2026-11-07 | |
| TD-08 | Assisting staff (several, each with a role) in a separate `EndoscopyCaseStaff` table in E3; unique Specimen ID per specimen in `EndoscopySpecimen` in E4 | Planned | 2026-11-07 | Not in the E2 migration |
| TD-09 | INR and platelet results shown when present on the record, with their date; no threshold, no eligibility rule | Approved as a display requirement | 2026-11-07 | Which labs and how recent: clinical, pending |
| TD-10 | Every schema change ships with its reviewed migration and a rollback script; CI proves empty-database build, Railway-shaped apply, and rollback; nothing is run on Railway without the Product Owner's separate approval | Approved policy (ADR-011) | 2026-11-07 | Applied to PR #27 |

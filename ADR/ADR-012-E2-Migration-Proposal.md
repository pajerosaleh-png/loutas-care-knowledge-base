# ADR-012 — Gate E2 Database Migration Proposal

**Document ID:** ADR-012-E2-MIG  
**Status:** **Pending Separate Review and Approval** — not merged, not deployed, not run on Railway  
**Pull request:** backend #27, branch `gate-e2-migration` (CI green)  
**Migration:** `prisma/migrations/20261107120000_endoscopy_e2_case/migration.sql`  
**Rollback:** `prisma/rollback/20261107120000_endoscopy_e2_case.sql`  
**Date:** 2026-11-07

---

## 1. Schema changes

Three new tables; three back-relation fields in `schema.prisma` on `Clinic`, `Patient`, `Visit` (no SQL change to those tables).

| Table | Columns | Keys and indexes | Foreign keys |
|---|---|---|---|
| `EndoscopyCase` | `id`, `clinicId`, `branchId?`, `patientId`, `visitId`, `procedureTypeCode?`, `procedureTypesVersion?`, `otherProcedureName?`, `otherDescription?`, `state` (default `PREP`), `endoscopistId?`, `endoscopistName?`, `openedById`, `openedByName?`, `revision` (default 1), `createdAt`, `updatedAt`, `shared?` | PK `id`; **unique `visitId`** (one case per visit); index `clinicId, patientId` | `Clinic`, `Patient`, `Visit` — `ON DELETE RESTRICT` |
| `EndoscopyEvent` | `id`, `caseId`, `clinicId`, `branchId?`, `patientId`, `visitId`, `code`, `codesVersion`, `at` (server time), `actorId`, `actorName?`, `note?` | PK `id`; index `caseId, at`; index `clinicId, patientId` | `EndoscopyCase` — `RESTRICT` |
| `EndoscopyAssessment` | `id`, `caseId`, `clinicId`, `branchId?`, `patientId`, `visitId`, `revision`, `anticoagulants` (JSONB), `note?`, `recordedById`, `recordedByName?`, `recordedAt` | PK `id`; **unique `caseId, revision`**; index `clinicId, patientId` | `EndoscopyCase` — `RESTRICT` |

No clinical rule, threshold or required clinical field is encoded in the database. `state` and event `code` are text; their allowed values live in versioned code lists in the module.

## 2. Data impact

- **Existing data:** none. No existing table, column, index or row is changed or deleted.
- **New tables:** empty after the migration. Rows appear only when a case is opened at Start Visit by the E2 implementation (a separate PR).
- **Locks:** `CREATE TABLE` and `ADD CONSTRAINT` on the new tables only; the foreign keys take a short lock on `Clinic`, `Patient`, `Visit` to validate — no rows reference them yet, so it is immediate.
- **Size:** negligible.

## 3. Tests (CI, PostgreSQL 16)

| Check | Result |
|---|---|
| Empty database: all migrations build the full schema; schema equals `schema.prisma` | Pass |
| Railway-shaped database (baseline recorded): baseline not re-applied; E2 applies; schema equals `schema.prisma` | Pass |
| Rollback script runs (tables 71 → 68) and the migration re-applies cleanly | Pass |
| The migration equals Prisma's own diff from `main@7c776f1` to the new schema | Pass |
| Additive only: three tables; no DROP / UPDATE / DELETE / ALTER of an existing table; one case per visit; tenant columns | Pass |
| Full backend suite (58 files) and typecheck | Pass |

## 4. Rollback

`prisma/rollback/20261107120000_endoscopy_e2_case.sql`:
1. Refuses if any `EndoscopyCase` exists (it would delete clinical records).
2. Drops `EndoscopyAssessment`, `EndoscopyEvent`, `EndoscopyCase`.
3. Removes the migration's row from `_prisma_migrations`.

After real cases exist, rollback is not a drop: the change would be reverted by a new forward migration, reviewed the same way.

## 5. Deployment steps (only after the Product Owner's separate approval)

1. Merge backend #27 (Product Owner).
2. Back up Railway (read-only copy, as for the baseline).
3. `npx prisma migrate deploy` — applies only `20261107120000_endoscopy_e2_case`.
4. `npx prisma migrate status` → up to date; `npx prisma migrate diff --from-config-datasource --to-schema prisma/schema.prisma --exit-code` → no difference.

Claude does not run any of these on Railway without that approval.

## 6. For the reviewer

- Is `ON DELETE RESTRICT` right for every foreign key (no cascade deletion of clinical records)? Proposed: yes.
- Are free-text `state` and `code` (validated in the module, not by a database enum) acceptable? Proposed: yes — the code lists are versioned and grow by gate without schema changes.

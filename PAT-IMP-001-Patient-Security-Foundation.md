# PAT-IMP-001 --- Patient Security Foundation

**Product:** LOUTAS Care\
**Domain:** Patient / Patient Index / Patient 360\
**Document Type:** Implementation Specification\
**Version:** 1.0\
**Status:** Approved for Repository / Implementation Preparation\
**Current State Baseline:** PAT-CSR-001 v1.0\
**Target State Reference:** PAT-TGT-001\
**Feasibility Reference:** PAT-TGT-FR-001

------------------------------------------------------------------------

## 1. Purpose

This document defines the first implementation stage for the LOUTAS Care
Patient domain.

The objective is to close the confirmed Patient authorization/security
gap and align the frontend and backend permission vocabulary before
expanding Patient 360 capabilities.

This stage is intentionally limited to the **Patient Security
Foundation**.

No MPI, Merge, Unmerge, Patient Lifecycle redesign, or Patient 360
redesign is included in this work item.

------------------------------------------------------------------------

## 2. Governing Principle

Patient identity and patient demographic information are sensitive data.

Access must follow:

-   Authentication
-   Explicit RBAC authorization
-   Clinic isolation
-   Applicable branch restrictions
-   Least privilege
-   Auditability for sensitive changes

The Patient domain owns patient identity.

Appointment, Encounter, Clinical/EMR, Orders/Results, Billing, and
Documents remain owners of their respective domains.

------------------------------------------------------------------------

## 3. Approved Scope

### In Scope

1.  Apply existing Patient permissions to Patient identity APIs.
2.  Protect Patient history access.
3.  Reconcile frontend/backend Patient permission vocabulary.
4.  Verify role grants against the existing RBAC catalog.
5.  Preserve existing clinic scoping.
6.  Verify applicable branch-scope behavior and document any remaining
    product decision.
7.  Add automated authorization tests.
8.  Add regression tests for existing Patient workflows.
9.  Verify audit behavior remains intact.

### Out of Scope

-   MPI implementation
-   Enterprise Patient ID schema change
-   Duplicate matching engine
-   Patient Merge
-   Patient Unmerge
-   New Patient lifecycle/status model
-   Patient 360 aggregation endpoint
-   Patient 360 UI redesign
-   Broad Patient schema redesign
-   File storage implementation
-   Cross-clinic identity resolution
-   Unrelated refactoring

------------------------------------------------------------------------

## 4. Existing Permission Catalog

The backend RBAC catalog already defines Patient permissions:

-   `patients:read`
-   `patients:create`
-   `patients:edit`
-   `patients:archive`

A working `requirePermission` middleware already exists.

The implementation task is therefore primarily to **apply and verify the
existing authorization model**, not invent a new permission framework.

------------------------------------------------------------------------

## 5. Permission Matrix

  -----------------------------------------------------------------------
  Operation                           Required Permission
  ----------------------------------- -----------------------------------
  List/search patients                `patients:read`

  View one patient                    `patients:read`

  View patient history                `patients:read`

  Create patient                      `patients:create`

  Edit patient                        `patients:edit`

  Archive/deactivate operation, if an `patients:archive`
  existing writer is exposed
  -----------------------------------------------------------------------

Do not introduce alternative permission names unless required by an
approved architecture decision.

------------------------------------------------------------------------

## 6. Backend Authorization

Review and secure the existing Patient identity routes.

Expected Patient routes include:

-   `GET /patients`
-   `GET /patients/:id`
-   `POST /patients`
-   `PATCH /patients/:id`
-   `GET /patients/:id/history`

Each route must retain:

-   Authentication
-   Clinic scoping
-   Existing validation
-   Existing business rules
-   Existing audit behavior

Authorization must be enforced using the existing RBAC middleware and
the approved permission keys.

Do not remove or weaken existing authorization on clinical/Encounter
routes.

------------------------------------------------------------------------

## 7. Patient History

`GET /patients/:id/history` is a sensitive identity/audit read.

It must:

1.  Require authentication.
2.  Require `patients:read`.
3.  Preserve clinic isolation.
4.  Return only history belonging to the authorized patient context.
5.  Preserve the existing audit records and response contract unless a
    change is strictly required for security.

Do not create a second audit mechanism.

------------------------------------------------------------------------

## 8. Frontend Permission Catalog Alignment

The repository contains a divergence between frontend role-management
permission vocabulary and the backend seeded RBAC vocabulary.

Examples identified during review include differences such as:

-   Backend: `patients:create` / `patients:edit`
-   Frontend: `patients:write`

And analogous vocabulary differences exist in other domains.

For this stage:

1.  Identify the canonical backend permission catalog.
2.  Compare frontend role-management keys against it.
3.  Align Patient permission display/configuration with the canonical
    keys.
4.  Do not redesign the complete RBAC system.
5.  Do not silently rename unrelated domain permissions unless required
    and separately approved.

The final implementation must have one authoritative Patient permission
vocabulary.

------------------------------------------------------------------------

## 9. Role Verification

Review the existing seeded role grants for Patient permissions.

At minimum verify the roles currently expected to interact with
patients, including applicable:

-   Reception
-   Doctor
-   Nurse
-   Administrative roles

Do not assume that a role should receive a permission merely because it
currently reaches a screen.

Verify the intended least-privilege behavior against the existing RBAC
catalog.

Any role-grant change beyond correcting an obvious catalog mismatch must
be explicitly documented.

------------------------------------------------------------------------

## 10. Clinic Isolation

Existing Patient queries are clinic-scoped.

This behavior must remain intact.

For every Patient endpoint verify:

-   Authenticated user's `clinicId` is used.
-   A patient from another clinic cannot be retrieved.
-   A patient from another clinic cannot be modified.
-   Search cannot return another clinic's patients.
-   Patient history cannot expose another clinic's audit records.

Do not weaken tenant isolation in order to simplify authorization.

------------------------------------------------------------------------

## 11. Branch Scope

Current Patient access is primarily clinic-scoped.

Branch-level Patient access remains an architecture/product decision
identified in PAT-TGT-FR-001.

Therefore:

-   Do not invent a new branch policy in this stage.
-   Do not silently convert all Patient access to branch-scoped access.
-   Preserve current clinic-level behavior unless an existing approved
    rule already requires branch restriction.
-   Document any branch-level restriction that cannot yet be enforced as
    an open decision.

------------------------------------------------------------------------

## 12. Audit Requirements

Existing Patient audit behavior must be preserved.

The implementation must verify:

### Patient Create

`PATIENT_CREATE`

### Patient Update

`PATIENT_UPDATE`

Including:

-   Reason where required
-   Field-level changes
-   Old value
-   New value

### Patient History

Existing Patient history must remain readable only by authorized users.

Do not replace the existing audit model with a new Patient audit table.

------------------------------------------------------------------------

## 13. Frontend Behavior

Frontend changes in this stage are limited to authorization-aware
behavior and permission vocabulary alignment.

The frontend should:

-   Hide or disable actions when the current user lacks the
    corresponding permission.
-   Never rely on frontend checks as the security boundary.
-   Handle backend `401/403` responses correctly.
-   Preserve current Patient search/register/edit workflows for
    authorized roles.
-   Avoid introducing a new Patient 360 UI.

Backend authorization remains authoritative.

------------------------------------------------------------------------

## 14. API Contract Preservation

Unless a security change requires otherwise, preserve existing API
contracts:

-   Existing route paths
-   Existing request shapes
-   Existing response envelope
-   Existing validation behavior
-   Existing MRN behavior
-   Existing Egyptian National ID handling
-   Existing audit behavior

Do not refactor APIs solely for style in this stage.

------------------------------------------------------------------------

## 15. Required Tests

Add or update tests covering at least:

### Read

1.  Authorized user with `patients:read` can list patients.
2.  Authorized user with `patients:read` can view a patient.
3.  Authorized user with `patients:read` can view patient history.
4.  User without `patients:read` receives `403`.

### Create

5.  Authorized user with `patients:create` can create a patient.
6.  User without `patients:create` receives `403`.

### Edit

7.  Authorized user with `patients:edit` can edit a patient.
8.  User without `patients:edit` receives `403`.
9.  Existing identity-change reason validation remains intact.
10. Existing Patient update audit remains intact.

### Tenant Isolation

11. User cannot read another clinic's patient.
12. User cannot edit another clinic's patient.
13. Search cannot return another clinic's patient.
14. Patient history cannot expose another clinic's records.

### Regression

15. Existing appointment patient-registration flow still works for an
    authorized role.
16. Existing walk-in flow still works.
17. Doctor Patient/EMR access remains functional under the appropriate
    permissions.
18. Existing clinical Encounter write-lock behavior is unchanged.

------------------------------------------------------------------------

## 16. Acceptance Criteria

### AC-01 --- Patient Read Authorization

A user without `patients:read` cannot list or retrieve patients.

### AC-02 --- Patient Create Authorization

A user without `patients:create` cannot create a patient.

### AC-03 --- Patient Edit Authorization

A user without `patients:edit` cannot modify a patient.

### AC-04 --- Patient History Authorization

A user without `patients:read` cannot access patient history.

### AC-05 --- Clinic Isolation

No Patient endpoint can expose or modify a patient belonging to another
clinic.

### AC-06 --- Existing Business Rules

MRN immutability, Egyptian National ID validation, required fields, and
existing Patient update behavior remain intact.

### AC-07 --- Audit

Existing Patient create/update audit records remain generated correctly.

### AC-08 --- Permission Vocabulary

Patient permissions used by backend authorization and frontend role
administration use one canonical vocabulary.

### AC-09 --- Regression Safety

Existing Appointment, Walk-in, Doctor Workspace, Visit, and EMR
workflows continue to function for authorized users.

### AC-10 --- No Scope Expansion

No MPI, Merge, Unmerge, Patient 360 redesign, lifecycle redesign, or
unrelated refactoring is introduced under this work item.

------------------------------------------------------------------------

## 17. Definition of Done

This work item is complete only when:

-   [ ] All Patient identity routes have explicit authorization.
-   [ ] Patient history has explicit authorization.
-   [ ] Clinic isolation is verified.
-   [ ] Patient permission vocabulary is reconciled.
-   [ ] Existing role grants are verified.
-   [ ] Automated authorization tests pass.
-   [ ] Tenant-isolation tests pass.
-   [ ] Patient regression tests pass.
-   [ ] Existing audit behavior is verified.
-   [ ] No unrelated Patient redesign is included.
-   [ ] No MPI/Merge/Unmerge work is included.
-   [ ] No unapproved Prisma migration is introduced.
-   [ ] No unrelated files are modified.
-   [ ] Final implementation review confirms scope compliance.

------------------------------------------------------------------------

## 18. Implementation Constraints for Claude

Claude may modify code only after this specification has been explicitly
approved for execution.

During implementation:

1.  Inspect the repository before editing.
2.  Follow existing project conventions.
3.  Reuse existing `requirePermission`.
4.  Reuse existing permission keys.
5.  Do not invent a new authorization framework.
6.  Do not change domain ownership.
7.  Do not implement MPI.
8.  Do not implement Merge/Unmerge.
9.  Do not implement Patient 360 aggregation.
10. Do not redesign Patient UI.
11. Do not perform unrelated refactoring.
12. Do not create destructive migrations.
13. Run the relevant tests after changes.
14. Report every changed file.
15. Report every test executed and its result.
16. Report any issue requiring a decision instead of silently changing
    scope.

------------------------------------------------------------------------

## 19. Required Claude Implementation Report

After implementation, Claude must provide:

### A. Files Changed

Exact file paths.

### B. Authorization Changes

Route-by-route description.

### C. Permission Catalog Changes

Exact permission keys changed or reconciled.

### D. Role Changes

Exact role grants changed, if any.

### E. Tests Added/Changed

Exact test files and cases.

### F. Test Results

Commands executed and results.

### G. Regression Results

Appointment, Walk-in, Doctor Workspace, Visit/EMR impact.

### H. Scope Confirmation

Explicit confirmation that:

-   MPI was not implemented.
-   Merge/Unmerge was not implemented.
-   Patient 360 redesign was not implemented.
-   Lifecycle redesign was not implemented.
-   No unrelated refactoring was performed.

### I. Remaining Risks

Any remaining security, architecture, or product decisions.

------------------------------------------------------------------------

## 20. References

-   `PAT-CSR-001` --- Patient / MPI / Patient 360 Current State
    Assessment
-   `PAT-TGT-001` --- Patient / MPI / Patient 360 Target State
-   `PAT-TGT-FR-001` --- Patient / MPI / Patient 360 Feasibility Review
-   Existing LOUTAS Care RBAC catalog
-   Existing Patient / Appointment / Encounter architecture

------------------------------------------------------------------------

## 21. Status

**Specification Status:** Approved for repository upload and
implementation preparation.

**Implementation Status:** Not started.

**Code Changes Under This Document:** None.

**Next Stage:** Execute PAT-IMP-001 after repository upload and explicit
implementation authorization.

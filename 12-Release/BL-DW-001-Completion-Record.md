# BL-DW-001 — Doctor Start Visit Authorization Completion Record

**Document ID:** BL-DW-001-COMP  
**Document Classification:** Release / Acceptance Record  
**Status:** CLOSED / PASS  
**Scope:** Doctor Start Visit authorization only  
**Recorded:** 2026-08-14

---

## 1. Purpose

This record documents the completed implementation and manual acceptance of the Doctor Start Visit authorization slice (BL-DW-001).

The slice is intentionally limited to the authorization path that allows a doctor to start a visit for an appointment owned by that doctor.

It does **not** mark the entire Doctor Workspace as complete.

---

## 2. Approved Authorization Model

The approved Option B policy is:

- A doctor may perform `WAITING → IN_ROOM` when the caller has `visit:start`.
- The appointment must be owned by the authenticated doctor:
  `appointment.doctorId === req.auth.userId`.
- A null or mismatched `doctorId` fails closed.
- The doctor-specific allowance is scoped only to `WAITING → IN_ROOM`.
- Other lifecycle transitions continue to use the normal permission path.
- Doctors are not granted `appointments:checkin` as part of this slice.
- The existing `TRANSITIONS` map remains unchanged.

This preserves the existing Reception/Admin/Nurse check-in authorization model while providing the required doctor-owned Start Visit path.

---

## 3. Implementation Record

### Backend

- `appointments.ts` calls `canPerformTransition(...)` for lifecycle authorization.
- `appointmentLifecycleService.ts` contains the pure doctor-owned authorization branch.
- The branch is restricted to `WAITING → IN_ROOM`, `visit:start`, and ownership equality.
- Visit creation/linking occurs on the successful `IN_ROOM` transition.

### Frontend

`transitionAppointment` now preserves the authoritative top-level `data.visitId` returned by the transition endpoint when the mapped appointment snapshot does not contain the visit id.

This fixes the Start Visit → EMR binding path without changing the backend visit contract.

### Automated Tests

- Frontend focused suite: **23/23 PASS**.
- Backend suite: **123 PASS**.

No schema or migration changes were introduced by this slice.

---

## 4. RBAC / Runtime Verification

Doctor test user:

- Role: `DOCTOR`
- Active: Yes
- Licensed clinician: Yes

The Doctor role's `visit:start` RolePermission was verified in the live database.

Important record-keeping detail: the authorized one-off verification script returned **NO-OP** because the relation already existed. No new RolePermission row was created by that script.

Verified relation:

- Role: `doctor`
- Role ID: `0a5b405f-ddde-455e-abed-fd1e3119d0bf`
- Permission: `visit:start`
- Permission ID: `5b01885b-6a8f-4f6a-be3e-a4ad2d069f98`
- Existing RolePermission ID: `6ab8232d-1d6d-4d4e-a6ce-7c3a6f3f3dc8`

The Doctor was **not** granted `appointments:checkin`.

---

## 5. Manual Acceptance Evidence

### 5.1 Positive — Doctor-owned Start Visit

Ahmed Saleh, acting as the owning doctor, successfully performed:

`WAITING → IN_ROOM`

Result:

- HTTP 200
- Visit created/linked
- EMR opened using the returned visit id

**PASS**

### 5.2 Negative — Missing Doctor Ownership

A `WAITING` appointment with `doctorId = null` was attempted through the doctor Start Visit path.

Result:

- HTTP 403
- `You do not have permission to do this`

**PASS — fail closed**

### 5.3 Negative Control — Different Lifecycle Edge

An Ahmed-owned `IN_ROOM` appointment was used to attempt:

`IN_ROOM → COMPLETED`

The request used the same authenticated doctor and demonstrated that the doctor-specific allowance is not a general `appointments:checkin` grant.

Result:

- HTTP 403
- `You do not have permission to do this`
- Appointment remained `IN_ROOM`

**PASS — edge scoped**

### 5.4 Regression Control — Admin Check-in

An Admin performed:

`WAITING → IN_ROOM`

Result:

- HTTP 200
- `ok: true`
- Visit id returned

**PASS — existing check-in path preserved**

### 5.5 EMR Binding

An existing `IN_ROOM` doctor-owned visit was opened through the Doctor Workspace.

Result:

- EMR opened against the real visit
- No `no started visit` guard message

**PASS**

---

## 6. Runtime Incident Resolved During Acceptance

The initial 403 contradiction was traced to a stale backend process occupying port `4000`.

The stale process was stopped and the backend was restarted from the current source. After the clean restart, the approved Doctor Start Visit path passed its positive acceptance test.

This was a runtime/process-state issue, not a change to the approved authorization model.

---

## 7. Scope Boundary

The following remain outside this closed slice and were intentionally **not changed**:

- Complete Doctor Workspace implementation
- Patient tab-strip behavior
- Complete Visit / Close Visit UI
- MPI
- RBAC `/me` frontend action gating
- Clinic Schedule backend
- Treatment Episode
- Deferred S11 work

These items must be handled under their own approved backlog slices.

---

## 8. Final Status

**BL-DW-001 — CLOSED / PASS**

The Doctor Start Visit authorization path has been implemented, tested, and manually accepted for the approved Option B policy.

The result does not constitute a production-readiness approval for the whole LOUTAS Care platform or completion of the entire Doctor Workspace.

---

## 9. Traceability

This record should be read alongside the approved roadmap and release governance documentation. Roadmap governance establishes the relationship between strategic planning and release execution, while backlog governance requires implementation, testing, security validation, documentation, and acceptance before an item is considered complete.

---

**End of Record**

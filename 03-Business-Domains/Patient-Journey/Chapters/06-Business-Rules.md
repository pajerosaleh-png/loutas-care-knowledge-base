# Patient Journey Business Rules

**Document ID:** PJ-006

**Module:** Patient Journey

**Status:** Draft

**Version:** 1.0

**Last Updated:** 2026-07-22

---

# Business Rules

The following business rules govern every Patient Journey within the LOUTAS Care platform.

## BR-PJ-001 — Journey Creation

Every Patient Journey shall be created for one identified patient only.

---

## BR-PJ-002 — Single Active Owner

Every Patient Journey shall have one operational owner at any given time.

---

## BR-PJ-003 — Controlled State Transitions

Patient Journey states shall change only through approved business workflows.

---

## BR-PJ-004 — Complete Audit Trail

Every significant Patient Journey event shall be recorded in the audit history.

---

## BR-PJ-005 — Clinical Order Association

Every clinical order shall remain linked to its originating Patient Journey throughout its lifecycle.

---

## BR-PJ-006 — Clinical Order Context

Every clinical order shall include the clinical information required for safe and effective execution of the requested service.

---

## BR-PJ-007 — Service Fulfillment

Every clinical order shall define its fulfillment method as Internal, External, or Referral.

---

## BR-PJ-008 — Continuity of Care

All operational and clinical activities performed during the patient's visit shall remain associated with the same Patient Journey unless a new journey is intentionally created according to approved business policies.

---

## BR-PJ-009 — Journey Completion

A Patient Journey shall be marked as Completed only after all required operational and clinical activities have been finalized according to organizational policies.

---

## BR-PJ-010 — Data Integrity

The Patient Journey shall remain the single authoritative operational record connecting all activities performed during the patient's visit.


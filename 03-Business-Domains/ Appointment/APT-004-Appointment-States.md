# Appointment States

**Document ID:** APT-004

**Module:** Appointment

**Status:** Updated — Pending Product Owner Review

**Version:** 1.1

**Last Updated:** 2026-08-04

**Related ADRs:** ADR-003 v1.1 (Appointment Architecture — canonical lifecycle); ADR-EMR-011 v1.0 (clinical handover / Visit Owner)

**Reason for Update:** Synchronize the appointment state machine with the approved canonical lifecycle in ADR-003 v1.1.

---

# Change Summary (v1.0 → v1.1)

Synchronized with ADR-003 v1.1. No workflow is redesigned and no new business rule is introduced. Modifications:

- State **Planned** renamed to **Booked** (canonical — ADR-003 AD-009).
- State **In Progress** renamed to **In Room** (canonical — ADR-003 AD-009).
- **Rescheduled** reclassified from a state to a **validated in-place operation**; removed from the state list, ownership table and transition graph (ADR-003 AD-009 / AD-013).
- **Booked** recorded as the mandatory initial state (ADR-003 AD-010).
- **Check-In** recorded as a mandatory gate before **In Room** (ADR-003 AD-011).
- **No Show** confirmed as a persisted state governed by a configurable grace period (ADR-003 AD-012).
- Visit Owner and operational Care Team assignment, and audited Physician Reassignment, recorded as operational events (ADR-003 AD-014 / AD-015 / AD-018).
- State Ownership table and Allowed State Transitions updated to the canonical vocabulary.

---

# Purpose

This document defines the operational state machine governing appointment management within the LOUTAS Care platform.

The objective is to standardize appointment behavior across all healthcare specialties while maintaining clear ownership boundaries between Appointment Management and Patient Journey.

The appointment state machine governs operational workflow only and shall not represent clinical workflow.

---

# State Machine Principles

The appointment state machine follows these principles:

* Each appointment shall have one current operational state.
* State transitions shall follow predefined business rules.
* Every transition shall be auditable.
* Operational policies may enable or restrict transitions.
* Clinical workflow begins after responsibility is transferred to Patient Journey.
* States shall represent long-lived operational conditions.
* Short-lived operational actions shall be represented as Events rather than States.

---

# Appointment States

## Booked

The appointment has been successfully created.

Booked is the mandatory initial state; appointments are never created as Confirmed (ADR-003 AD-010).

Characteristics:

* Resources allocated.
* Schedule reserved.
* Visit Owner and operational Care Team assigned.
* Patient Journey has not started.
* Patient has not arrived.

---

## Confirmed

The appointment has been confirmed according to organizational policy.

Confirmation may occur:

* Manually.
* Automatically.
* Via patient confirmation.
* Via communication channels.

Confirmation is configurable and not mandatory.

---

## Waiting

The patient has completed Reception Check-In and is waiting for service.

Check-In is a mandatory gate; an appointment reaches Waiting only through Check-In (ADR-003 AD-011).

Characteristics:

* Patient physically present.
* Administrative verification completed.
* Waiting for clinical handover.

This represents the final operational state managed primarily by the Appointment workflow before service delivery.

---

## In Room

Operational responsibility has been transferred to Patient Journey / EMR at Start Visit.

Examples include:

* Physician consultation.
* Physiotherapy session.
* Radiology examination.
* Laboratory collection.
* Dental procedure.

Appointment scheduling responsibility ends.

Clinical responsibility begins, owned by the Visit Owner (ADR-EMR-011).

---

## Completed

The scheduled service has been completed successfully.

The appointment is operationally closed.

---

## Cancelled

The appointment has been cancelled before service delivery.

Cancellation may occur by:

* Patient.
* Reception.
* Organization.
* Automated operational policy.

---

## No Show

The patient failed to attend within the configured operational policy.

No Show is a persisted operational state governed by a configurable grace period (ADR-003 AD-012).

No clinical encounter is created.

---

# Reschedule (Operation)

Reschedule is a **validated in-place operation**, not a distinct state (ADR-003 AD-009 / AD-013).

A reschedule revalidates provider availability, time-slot occupancy, and scheduling conflicts (double-booking) before commit, preserves the complete audit history, and keeps the original schedule historically traceable.

---

# State Ownership

| State     | Primary Owner             |
| --------- | ------------------------- |
| Booked    | Appointment               |
| Confirmed | Appointment               |
| Waiting   | Appointment / Reception   |
| In Room   | Patient Journey / EMR     |
| Completed | Patient Journey / EMR     |
| Cancelled | Appointment               |
| No Show   | Appointment Policy Engine |

---

# Allowed State Transitions

The following operational transitions are supported:

```text
Booked
 ├──► Confirmed
 ├──► Cancelled
 └──► No Show          (per grace policy)

Confirmed
 ├──► Waiting          (via Check-In — mandatory gate)
 ├──► Cancelled
 └──► No Show          (per grace policy)

Waiting
 ├──► In Room          (Start Visit)
 └──► Cancelled

In Room
 └──► Completed
```

Reschedule is a validated in-place operation applicable to Booked and Confirmed appointments (see Reschedule (Operation) above); it is not a state-transition edge.

Transitions outside these rules require explicit organizational policy support.

---

# State Transition Rules

Transitions shall be governed by operational policies.

Examples include:

* Early Check-In Policy
* Confirmation Policy
* Cancellation Policy
* Rescheduling Policy
* No Show Policy
* Walk-In Policy
* Waiting List Policy

Policies are maintained by the Scheduling Policy Engine and may vary between healthcare organizations.

---

# Events

The following operational events may occur without changing the appointment state:

* Reminder Sent
* Patient Arrived
* Patient Called
* Resource Changed
* Schedule Updated
* Notification Delivered
* Staff Assignment Updated
* Visit Owner Assigned
* Visit Owner Reassigned (audited — ADR-003 AD-015)

Events shall be recorded within the operational timeline.

---

# Operational Timeline

Every appointment shall maintain a chronological operational timeline recording significant events from creation until closure.

The timeline supports operational visibility while remaining independent from clinical documentation.

---

# Audit Requirements

Every state transition shall record:

* Previous State
* New State
* Transition Time
* Responsible User or System
* Triggering Policy (if applicable)
* Reason (where required)

Audit history shall remain permanently available.

---

# Architecture Notes

Appointment owns operational scheduling activities only.

Ownership transfers to Patient Journey when the appointment enters clinical execution (In Room).

Clinical workflow shall never be managed by the Appointment Business Domain.

The canonical state vocabulary is defined by ADR-003 v1.1; the Reception queue experience is governed by Reception Workflow (REC-006).

This separation ensures clear business boundaries, simplified maintenance, and long-term scalability across all supported healthcare specialties.

---

# Revision History

| Version | Date | Description |
|---------|------|-------------|
| 1.0 | 2026-07-22 | Initial Draft |
| 1.1 | 2026-08-04 | Synchronized with ADR-003 v1.1: Planned → Booked; In Progress → In Room; Rescheduled reclassified as a validated operation; Booked mandatory initial state; Check-In mandatory gate; No Show persisted; Visit Owner / operational Care Team / audited reassignment recorded; ownership table and transitions updated to canonical vocabulary. No workflow redesign. |

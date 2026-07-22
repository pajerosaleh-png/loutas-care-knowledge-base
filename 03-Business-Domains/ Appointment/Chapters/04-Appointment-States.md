# Appointment States

**Document ID:** APT-004

**Module:** Appointment

**Status:** Draft

**Version:** 1.0

**Last Updated:** 2026-07-22

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

## Planned

The appointment has been successfully created.

Characteristics:

* Resources allocated.
* Schedule reserved.
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

Characteristics:

* Patient physically present.
* Administrative verification completed.
* Waiting for clinical handover.

This represents the final operational state managed primarily by the Appointment workflow before service delivery.

---

## In Progress

Operational responsibility has been transferred to Patient Journey.

Examples include:

* Physician consultation.
* Physiotherapy session.
* Radiology examination.
* Laboratory collection.
* Dental procedure.

Appointment scheduling responsibility ends.

Clinical responsibility begins.

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

## Rescheduled

The original appointment has been replaced by another scheduled appointment.

The original appointment remains historically traceable.

---

## No Show

The patient failed to attend within the configured operational policy.

No clinical encounter is created.

---

# State Ownership

| State       | Primary Owner             |
| ----------- | ------------------------- |
| Planned     | Appointment               |
| Confirmed   | Appointment               |
| Waiting     | Appointment / Reception   |
| In Progress | Patient Journey           |
| Completed   | Patient Journey           |
| Cancelled   | Appointment               |
| Rescheduled | Appointment               |
| No Show     | Appointment Policy Engine |

---

# Allowed State Transitions

The following operational transitions are supported:

```text
Planned
 ├──► Confirmed
 ├──► Cancelled
 └──► Rescheduled

Confirmed
 ├──► Waiting
 ├──► Cancelled
 ├──► Rescheduled
 └──► No Show

Waiting
 └──► In Progress

In Progress
 └──► Completed
```

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

Ownership transfers to Patient Journey when the appointment enters clinical execution.

Clinical workflow shall never be managed by the Appointment Business Domain.

This separation ensures clear business boundaries, simplified maintenance, and long-term scalability across all supported healthcare specialties.


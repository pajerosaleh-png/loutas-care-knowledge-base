# APT-005 — Appointment Business Rules

| Property | Value |
|----------|-------|
| Document ID | APT-005 |
| Domain | Appointment |
| Status | Draft |
| Version | 1.0 |
| Owner | Business Architecture |
| Last Updated | 2026-07-22 |
| Depends On | APT-001, APT-002, APT-003, APT-004 |
| Related Documents | APT-006, APT-009 |
APT-005 — Appointment Business Rules

Document ID: APT-005
Module: Appointment
Status: Draft
Version: 1.0

1. Purpose

This document defines the business rules governing appointment scheduling within the LOUTAS Care platform.

These rules ensure consistent operational behavior across all healthcare organizations while allowing organization-specific behavior through configurable policies rather than software customization.

Business Rules are mandatory operational constraints and must be respected by all user interfaces, APIs, integrations, and automated processes.

2. Core Business Principles

The Appointment Business Domain shall operate according to the following principles:

Appointment manages scheduling only.
Clinical care belongs to the Patient Journey domain.
Every appointment represents one scheduled healthcare service.
Business rules shall be configuration-driven whenever possible.
All operational activities shall be traceable and auditable.
Appointment behavior shall remain specialty-independent.
3. Patient Rules
BR-001

An appointment cannot exist without a registered patient.

BR-002

Each appointment belongs to one and only one patient.

BR-003

A patient may have multiple future appointments unless restricted by organizational policy.

BR-004

Duplicate appointments for the same patient, service, and time shall be prevented unless explicitly allowed.

BR-005

Patient demographic updates shall not modify appointment history.

Appointments reference the patient but do not own patient information.

4. Scheduling Rules
BR-006

Appointments shall only be scheduled within valid scheduling templates.

BR-007

Scheduling outside working hours requires an approved operational policy.

BR-008

Organization holidays and blocked schedules shall prevent booking unless overridden by authorized users.

BR-009

Appointment duration shall follow the configured service duration unless manually overridden according to policy.

BR-010

Buffer time before and after appointments shall follow scheduling policies.

5. Resource Rules
BR-011

Every appointment shall reserve all required scheduling resources.

Resources may include:

Physician
Therapist
Dentist
Nurse
Room
Chair
Equipment
Device
BR-012

Resources cannot be double-booked unless overbooking is permitted by policy.

BR-013

Resource availability shall be validated before appointment confirmation.

BR-014

Changes to allocated resources shall preserve appointment history.

6. Confirmation Rules
BR-015

Appointment confirmation is optional and governed by organizational policy.

BR-016

Confirmation may occur:

Manually
Automatically
Patient initiated
Integration initiated
BR-017

Automatic reminders shall not change appointment status unless defined by policy.

7. Check-In Rules
BR-018

Reception Check-In is required before clinical service begins.

BR-019

Early Check-In is controlled by configurable policy.

BR-020

Late arrival handling shall follow organization policy.

Possible outcomes include:

Continue appointment
Shorten visit
Reschedule
Cancel
Mark No Show
BR-021

Completion of Check-In prepares operational handover to Patient Journey.

8. Clinical Handover Rules
BR-022

Appointment does not create clinical documentation.

BR-023

Patient Journey becomes operational owner when service begins.

BR-024

Appointment status changes after clinical handover shall be initiated by Patient Journey where applicable.

9. Cancellation Rules
BR-025

Cancelled appointments shall remain permanently stored.

BR-026

Deletion of cancelled appointments is prohibited.

BR-027

Cancellation reason may be mandatory according to organizational policy.

BR-028

Cancellation shall immediately release reserved scheduling resources.

10. Rescheduling Rules
BR-029

Rescheduling shall preserve the complete audit history.

BR-030

The original appointment shall remain historically traceable.

BR-031

Resource availability shall be revalidated after rescheduling.

11. No Show Rules
BR-032

A No Show appointment shall not create a Patient Journey.

BR-033

No Show determination shall follow configurable grace periods.

BR-034

Automatic No Show processing shall be controlled by policy.

12. Treatment Course Rules

These rules support future healthcare services delivered over multiple sessions.

BR-035

A treatment course consists of multiple independent appointments.

BR-036

Each session shall maintain its own operational lifecycle.

BR-037

Rescheduling one session shall not affect remaining sessions.

BR-038

Cancelling one session shall not automatically cancel the entire treatment course.

BR-039

Course completion rules shall be configurable by organization.

13. Audit Rules
BR-040

Every operational action shall be auditable.

BR-041

Every state transition shall record:

Previous State
New State
Timestamp
User/System
Triggering Policy
Reason (when applicable)
BR-042

Audit history shall never be deleted.

14. Timeline Rules
BR-043

Every appointment shall maintain an operational timeline.

Timeline events may include:

Appointment Created
Confirmation
Reminder Sent
Patient Arrived
Check-In Completed
Patient Called
Resource Changed
Rescheduled
Cancelled
Completed
BR-044

Timeline records operational history only.

Clinical documentation belongs to Patient Journey.

15. Policy Rules
BR-045

Operational behavior shall be configurable through Scheduling Policies.

Examples include:

Working Hours
Appointment Duration
Buffer Times
Overbooking
Confirmation
Cancellation
No Show
Walk-In
Early Check-In
Late Arrival
Waiting List
BR-046

Hardcoded operational behavior shall be avoided whenever configuration is possible.

16. Enterprise Rules
BR-047

Appointment shall remain independent from medical specialty.

BR-048

Appointment shall remain independent from billing implementation.

BR-049

Appointment shall remain independent from EMR implementation.

BR-050

Appointment shall support future Resource Reservation capabilities without architectural redesign.

Architecture Notes

The Appointment Business Domain is responsible for planning, organizing, and coordinating healthcare services. It does not own clinical care, billing, or patient demographics.

This separation of responsibilities ensures:

Clear ownership boundaries.
Scalable enterprise architecture.
High maintainability.
Reusable scheduling capabilities across all healthcare specialties.
Future extensibility for advanced scheduling and resource management.
---

# Change History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-22 | Initial Draft |

---

# Approval

| Role | Name | Status |
|------|------|--------|
| Solution Architect | Ahmed Saleh | Pending |

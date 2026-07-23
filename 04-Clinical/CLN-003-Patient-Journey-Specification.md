# CLN-003 — Patient Journey Specification

**Document ID:** CLN-003
**Title:** Patient Journey Specification
**Status:** Approved
**Priority:** Critical
**Category:** Clinical
**Implementation Status:** Ready
**Owner:** Clinical Architecture
**Last Updated:** July 2026

---

# Purpose

This document defines the complete outpatient patient journey within the LOUTAS Care Platform.

The patient journey represents the sequence of business events, clinical activities, operational workflows, and system interactions that occur from the moment a patient requests healthcare services until the completion of care.

This document is the authoritative reference for workflow design across all clinical and operational modules.

---

# Scope

This specification applies to:

- Reception
- Appointment
- Waiting Queue
- Clinical EMR
- Laboratory
- Radiology
- Pharmacy
- Billing
- Follow-Up

---

# Business Objective

The patient journey shall:

- Provide a consistent patient experience.
- Minimize waiting time.
- Improve physician productivity.
- Ensure complete clinical documentation.
- Ensure accurate billing.
- Maintain full auditability.

---

# Journey Actors

Primary Actors

- Patient
- Receptionist
- Physician
- Nurse

Supporting Actors

- Cashier
- Pharmacist
- Laboratory Technician
- Radiology Technician

System Actors

- Notification Service
- Billing Engine
- Audit Service
- Reporting Engine

---

# Standard Patient Journey

## Stage 1 — Patient Registration

Business Purpose

Create or identify the patient.

Possible Outcomes

- Existing patient found.
- New patient registered.

---

## Stage 2 — Appointment Booking

Business Purpose

Reserve healthcare resources.

Outputs

- Appointment
- Notification

---

## Stage 3 — Appointment Confirmation

Appointment status becomes Confirmed.

Patient receives reminder.

---

## Stage 4 — Patient Check-In

Business Purpose

Confirm patient arrival.

Outputs

- Waiting Queue Entry

---

## Stage 5 — Waiting Queue

Patient waits according to clinic workflow.

The system continuously tracks queue position.

---

## Stage 6 — Encounter Started

Business Event

Clinical Encounter is created or activated.

Encounter status becomes:

In Progress

---

## Stage 7 — Clinical Assessment

Activities

- Chief Complaint
- History
- Vital Signs
- Examination

Outputs

SOAP documentation begins.

---

## Stage 8 — Clinical Decision

Possible outcomes:

Diagnosis

Observation

Referral

Procedure

Admission Recommendation (Future)

---

## Stage 9 — Clinical Orders

Possible orders:

Laboratory

Radiology

Procedures

Medications

Referrals

---

## Stage 10 — Prescription

Provider issues medications.

Prescription becomes available for Pharmacy.

---

## Stage 11 — Encounter Completion

Clinical documentation completed.

Encounter status becomes Completed.

---

## Stage 12 — Billing

Billable Events generated.

Invoice created.

Payment collected.

Receipt issued.

---

## Stage 13 — Follow-Up

Possible outcomes

No Follow-Up

Scheduled Follow-Up

Referral

Remote Follow-Up

---

## Stage 14 — Journey Completed

The outpatient episode ends.

The patient's longitudinal history is updated.

Analytics updated.

Audit finalized.

---

# Business Rules

BR-001

A patient journey starts with either:

- Registration

or

- Existing Patient Identification

---

BR-002

One appointment may create one Encounter.

---

BR-003

Walk-In patients may bypass appointment booking.

---

BR-004

Billing begins only after billable events exist.

---

BR-005

Encounter must be completed before final billing.

---

BR-006

Every journey shall be auditable.

---

# Journey Events

PatientCreated

AppointmentBooked

AppointmentConfirmed

PatientCheckedIn

QueueEntered

EncounterStarted

SOAPStarted

DiagnosisRecorded

OrderCreated

PrescriptionIssued

EncounterCompleted

InvoiceCreated

PaymentCollected

FollowUpScheduled

JourneyCompleted

---

# Exception Scenarios

Cancelled Appointment

Patient No Show

Emergency Walk-In

Incomplete Payment

Encounter Reopened

Order Cancellation

---

# Quality Indicators

Average Waiting Time

Average Encounter Duration

Patient Throughput

Provider Productivity

Visit Completion Rate

Follow-Up Compliance

---

# Future Extensions

Telemedicine

Home Healthcare

Remote Monitoring

AI Assisted Journey

Patient Mobile Journey

Insurance Authorization Workflow

---

# Related Documents

CLN-001

CLN-002

CLN-004

ARCH-003

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
|1.0.0|July 2026|Initial Release|

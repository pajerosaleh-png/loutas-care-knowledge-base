# Reception Functional Requirements

**Document ID:** REC-006

**Module:** Reception

**Status:** Draft

**Version:** 1.0

**Last Updated:** 2026-07-22

---

# Functional Requirements

## Patient Management

### FR-REC-001 — Search Patient

The system shall allow Reception to search for an existing patient using one or more approved identifiers.

Examples include:

* Medical Record Number (MRN)
* National ID
* Passport Number
* Mobile Number
* Full Name
* Date of Birth

---

### FR-REC-002 — Register Patient

The system shall allow Reception to register a new patient after verifying that no matching patient already exists.

---

### FR-REC-003 — Update Patient Information

The system shall allow authorized Reception users to update demographic information according to organizational policies.

---

### FR-REC-004 — Verify Patient Identity

The system shall support patient identity verification before initiating a Patient Journey.

---

## Appointment Management

### FR-REC-005 — Manage Appointments

The system shall support appointment creation, confirmation, rescheduling, and cancellation.

---

### FR-REC-006 — Walk-In Registration

The system shall support registration of patients without prior appointments.

---

## Patient Arrival

### FR-REC-007 — Patient Check-In

The system shall support patient check-in upon arrival.

---

### FR-REC-008 — Queue Management

The system shall assign and maintain queue positions where applicable.

---

## Patient Journey

### FR-REC-009 — Initiate Patient Journey

The system shall initiate a Patient Journey according to approved business workflows.

---

### FR-REC-010 — Transfer Operational Ownership

The system shall transfer operational ownership to the next responsible department.

---

## Communication

### FR-REC-011 — Patient Communication

The system shall support operational communication with patients through approved communication channels.

---

## Documentation

### FR-REC-012 — Print Documents

The system shall support printing authorized operational documents including patient labels, visit slips, queue tickets, and other approved reception documents.

---

## Future Readiness

### FR-REC-013 — Eligibility Verification Readiness

The system architecture shall support future administrative eligibility verification workflows without requiring redesign.

---

### FR-REC-014 — Multi-Channel Reception

The system shall support multiple patient communication channels while maintaining a single standardized Reception workflow.

---

### FR-REC-015 — Patient Journey Compliance

Every Reception function shall remain fully integrated with the Patient Journey architecture.


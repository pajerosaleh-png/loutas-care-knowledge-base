# PAT-003 — Patient Lifecycle

| Property | Value |
|----------|-------|
| Document ID | PAT-003 |
| Domain | Patient Management |
| Document Type | Business Architecture |
| Classification | Business Lifecycle |
| Status | Draft |
| Version | 1.0 |
| Owner | Business Architecture |
| Last Updated | 2026-07-22 |
| Depends On | PAT-001, PAT-002 |
| Related Documents | PAT-004, PAT-005, PAT-006 |

---

# Purpose

This document defines the complete business lifecycle of a patient within the LOUTAS Care platform.

It describes the business states, lifecycle transitions, ownership rules, and governance principles that manage a patient's identity throughout their relationship with the organization.

The lifecycle is independent of appointments, clinical visits, billing activities, or any other operational workflows.

---

# Business Principle

A patient is a long-lived enterprise entity.

Appointments, visits, invoices, and clinical encounters may begin and end, but the patient identity remains persistent throughout the lifetime of the healthcare relationship.

---

# Lifecycle Overview

```text
Prospective
      │
      ▼
Registered
      │
      ▼
Verified
      │
      ▼
Active
      │
 ┌────┼───────────────┐
 ▼    ▼               ▼
Updated Merged    Deceased
 │
 ▼
Active
```

---

# Lifecycle States

## 1. Prospective

### Description

A person who has expressed interest in receiving healthcare services but has not yet completed registration.

### Entry Criteria

- Initial inquiry
- Online pre-registration
- Referral
- Walk-in inquiry

### Exit Criteria

- Registration completed
- Inquiry cancelled

---

## 2. Registered

### Description

A patient record has been created and assigned an Enterprise Patient ID.

The patient can now be scheduled for appointments.

### Entry Criteria

- Registration completed
- Required demographic information collected

### Exit Criteria

- Identity verification completed
- Registration cancelled before activation

---

## 3. Verified

### Description

The patient's identity has been validated using approved identification documents.

### Entry Criteria

- National ID
- Passport
- Residency ID
- Other approved identification

### Exit Criteria

- Verification approved
- Verification requires correction

---

## 4. Active

### Description

The patient is fully active within the LOUTAS Care ecosystem and may receive healthcare services.

This is the normal operational state.

### Business Capabilities

- Book appointments
- Attend visits
- Receive treatment
- Generate invoices
- Access EMR
- Laboratory requests
- Radiology requests
- Pharmacy services

---

## 5. Updated

### Description

Patient demographic information has been modified.

Examples include:

- Address
- Phone number
- Emergency contact
- Preferred language
- Marital status

Updating demographic information does not create a new patient identity.

After successful update, the lifecycle returns to Active.

---

## 6. Merged

### Description

Two or more duplicate patient records have been consolidated into a single enterprise identity.

The surviving patient record becomes the official enterprise record.

The obsolete identities remain available only for audit purposes.

### Business Rules

- Merge never deletes historical information.
- Merge is fully auditable.
- Merge requires authorized permissions.
- Clinical history is preserved.

---

## 7. Deceased

### Description

The patient has been confirmed deceased.

The patient record remains permanently stored.

### Business Rules

- No deletion permitted.
- Historical clinical information remains available.
- Future appointments are not allowed.
- New clinical encounters cannot be created.
- Reporting remains available.

---

# Invalid Lifecycle Actions

The following actions are prohibited:

- Delete an active patient.
- Reuse an Enterprise Patient ID.
- Create duplicate enterprise identities intentionally.
- Remove historical audit information.
- Permanently erase clinical history.

---

# Lifecycle Governance

The Patient Management Domain owns the lifecycle.

Other domains may consume patient status but may not change lifecycle states directly.

Examples:

- Appointment cannot deactivate a patient.
- Billing cannot merge patients.
- EMR cannot delete a patient.
- Laboratory cannot modify demographic information.

---

# Lifecycle Events

Examples of lifecycle events include:

- Patient Registered
- Identity Verified
- Demographics Updated
- Duplicate Detected
- Patient Merged
- Patient Deceased

Each event shall generate an audit trail according to organizational governance policies.

---

# Success Criteria

The lifecycle is considered successful when:

- Every patient progresses through controlled lifecycle states.
- Duplicate records are minimized.
- Identity changes remain auditable.
- Historical information is never lost.
- Business ownership remains centralized.

---

# Architecture Notes

Patient Lifecycle is independent from:

- Appointment Lifecycle
- Visit Lifecycle
- Invoice Lifecycle
- Payment Lifecycle

Each business domain maintains its own lifecycle while referencing the enterprise patient identity.

This separation ensures loose coupling, maintainability, and long-term scalability.

---

# Change History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-22 | Initial Patient Lifecycle |

---

# Approval

| Role | Name | Status |
|------|------|--------|
| Solution Architect | Ahmed Saleh | Approved |

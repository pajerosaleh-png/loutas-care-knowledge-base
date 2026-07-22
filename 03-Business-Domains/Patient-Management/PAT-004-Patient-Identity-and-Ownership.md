# PAT-004 — Patient Identity & Ownership

| Property | Value |
|----------|-------|
| Document ID | PAT-004 |
| Domain | Patient Management |
| Document Type | Business Architecture |
| Classification | Identity & Ownership |
| Status | Draft |
| Version | 1.0 |
| Owner | Business Architecture |
| Last Updated | 2026-07-22 |
| Depends On | PAT-001, PAT-002, PAT-003 |
| Related Documents | PAT-005, PAT-006, PAT-009 |

---

# Purpose

This document defines the ownership, governance, and management principles of patient identity within the LOUTAS Care platform.

It establishes how patient identities are created, maintained, protected, and consumed across all business domains.

The objective is to ensure that every patient has one trusted enterprise identity while preserving data integrity, patient safety, and organizational scalability.

---

# Enterprise Patient Identity

Within LOUTAS Care, every patient is represented by a single Enterprise Patient Identity.

This identity is permanent, unique, and shared across all authorized clinics and branches within the organization.

Patient identity is independent from appointments, visits, invoices, or clinical encounters.

---

# Identity Components

Every patient identity consists of:

- Enterprise Patient ID (System Identifier)
- Medical Record Number (MRN)
- Full Legal Name
- Date of Birth
- Gender
- National ID / Passport / Residency ID
- Mobile Number(s)
- Email Address (Optional)
- Demographic Information
- Emergency Contacts
- Preferred Language
- Preferred Communication Method
- Identity Verification Status

---

# Identity Ownership

The Patient Management Domain is the authoritative owner of patient identity.

Only this domain may:

- Create patient identities
- Update demographic information
- Merge duplicate identities
- Maintain identity verification
- Manage patient status

All other domains consume patient identity but do not own or modify it.

---

# Enterprise Patient ID

The Enterprise Patient ID is:

- System-generated
- Globally unique
- Immutable
- Never reused
- Internal to the platform

Business Rules

- Generated once.
- Cannot be edited.
- Cannot be reassigned.
- Remains valid for the patient's lifetime.
- Survives branch transfers and organizational expansion.

---

# Medical Record Number (MRN)

The MRN is the primary business identifier used by healthcare professionals.

Business Rules

- One MRN per patient.
- Unique across the enterprise.
- Never reused.
- Visible throughout the application.
- Remains associated with the patient permanently.

---

# Identity Verification

Patient identity should be verified using approved official documents.

Accepted identifiers may include:

- National ID
- Passport
- Residency Permit
- Other organization-approved documents

Verification status shall be recorded as part of the patient profile.

---

# Duplicate Prevention

Duplicate identities should be prevented whenever possible.

The system should support duplicate detection using combinations of:

- National ID
- Passport
- Mobile Number
- Full Name
- Date of Birth
- Gender

Potential duplicates should be reviewed before creating a new patient record.

---

# Patient Merge

If duplicate patient identities are confirmed:

- One record becomes the surviving identity.
- The remaining records become historical references.
- Clinical history is preserved.
- Audit records remain intact.
- Enterprise Patient ID is retained only for the surviving record.

Patient merge shall require elevated permissions.

---

# Data Ownership Matrix

| Information | Owner |
|--------------|-------|
| Patient Identity | Patient Management |
| MRN | Patient Management |
| Demographics | Patient Management |
| Contact Information | Patient Management |
| Emergency Contacts | Patient Management |
| Appointments | Appointment Domain |
| Visits | Patient Journey |
| Clinical Notes | EMR |
| Invoices | Billing |
| Payments | Billing |
| Laboratory Orders | Laboratory |
| Radiology Orders | Radiology |
| Prescriptions | Pharmacy |

---

# Identity Consumers

The following domains consume patient identity:

- Appointment
- Patient Journey
- EMR
- Billing
- Laboratory
- Radiology
- Pharmacy
- Reporting
- AI Services

These domains shall reference patient identity without creating independent copies.

---

# Security Principles

Patient identity shall be protected according to enterprise security policies.

Key principles include:

- Role-Based Access Control (RBAC)
- Audit Logging
- Data Integrity
- Controlled Updates
- Traceability
- Least Privilege Access

---

# Future Architecture

The Patient Identity model is designed to support future implementation of:

- Master Patient Index (MPI)
- Multi-Branch Organizations
- Healthcare Networks
- Shared Electronic Health Records
- National Health Information Exchange
- External Interoperability Standards

No redesign of patient identity shall be required to support these future capabilities.

---

# Success Criteria

The Patient Identity model is successful when:

- Every patient has one enterprise identity.
- Duplicate identities are minimized.
- Identity ownership is clearly defined.
- Downstream domains consume identity consistently.
- Identity changes remain fully auditable.
- Enterprise scalability is preserved.

---

# Architecture Notes

Patient Identity is classified as an Enterprise Shared Asset.

No business domain may create, duplicate, or own patient identity outside the Patient Management Domain.

This ownership model ensures consistency, interoperability, and long-term maintainability across the LOUTAS Care platform.

---

# Change History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-22 | Initial Patient Identity & Ownership |

---

# Approval

| Role | Name | Status |
|------|------|--------|
| Solution Architect | Ahmed Saleh | Approved |


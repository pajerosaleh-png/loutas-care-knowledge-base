# PAT-006 — Functional Requirements

| Property | Value |
|----------|-------|
| Document ID | PAT-006 |
| Domain | Patient Management |
| Document Type | Business Architecture |
| Classification | Functional Requirements |
| Status | Draft |
| Version | 1.0 |
| Owner | Business Architecture |
| Last Updated | 2026-07-22 |
| Depends On | PAT-001, PAT-002, PAT-003, PAT-004, PAT-005 |
| Related Documents | PAT-007, PAT-008, PAT-009 |

---

# Purpose

This document defines the functional capabilities required for the Patient Management Domain.

The requirements describe **what** the system shall do from a business perspective without prescribing implementation details.

These requirements provide the foundation for UI design, API development, database implementation, testing, and future enhancements.

---

# Functional Modules

The Patient Management Domain consists of the following functional modules:

1. Patient Registration
2. Patient Search
3. Patient Profile Management
4. Identity Verification
5. Duplicate Detection & Merge
6. Contact Management
7. Consent Management
8. Alerts & Flags
9. Lifecycle Management
10. Audit & History

---

# Module 1 — Patient Registration

## FR-001 Create Patient

The system shall allow authorized users to create a new patient record.

---

## FR-002 Generate Enterprise Patient ID

The system shall automatically generate a unique Enterprise Patient ID.

---

## FR-003 Generate MRN

The system shall generate an organization-approved Medical Record Number.

---

## FR-004 Validate Mandatory Fields

The system shall validate all required registration fields before saving.

---

## FR-005 Duplicate Check

Before registration is completed, the system shall perform duplicate detection.

---

# Module 2 — Patient Search

## FR-006 Search by MRN

Users shall be able to search using MRN.

---

## FR-007 Search by Enterprise Patient ID

Users shall search using Enterprise Patient ID.

---

## FR-008 Search by Name

Users shall search by full or partial patient name.

---

## FR-009 Search by National ID

Users shall search using National ID.

---

## FR-010 Advanced Search

The system shall support combining multiple search criteria.

---

# Module 3 — Patient Profile Management

## FR-011 View Patient Profile

Authorized users shall view the complete patient profile.

---

## FR-012 Edit Demographics

Authorized users shall update demographic information.

---

## FR-013 Maintain Contact Information

Users shall manage:

- Mobile Numbers
- Email
- Address
- Emergency Contacts

---

## FR-014 View Registration History

Users shall access patient registration history.

---

# Module 4 — Identity Verification

## FR-015 Record Verification

Users shall record identity verification status.

---

## FR-016 Manage Identification Documents

The system shall support multiple identity document types.

---

## FR-017 Verification Audit

Identity verification changes shall be audited.

---

# Module 5 — Duplicate Detection & Merge

## FR-018 Detect Duplicate Patients

The system shall identify potential duplicate patients.

---

## FR-019 Review Possible Duplicates

Users shall review suggested duplicate records.

---

## FR-020 Merge Duplicate Patients

Authorized users shall merge confirmed duplicate records.

---

## FR-021 Preserve Historical Information

Patient merge shall preserve all historical records.

---

# Module 6 — Contact Management

## FR-022 Manage Addresses

Users shall maintain patient addresses.

---

## FR-023 Manage Communication Methods

Users shall manage:

- Phone
- Email
- Preferred Language
- Preferred Communication Method

---

## FR-024 Emergency Contacts

Users shall manage one or more emergency contacts.

---

# Module 7 — Consent Management

## FR-025 Record Consent

Users shall record patient consent information.

---

## FR-026 Update Consent

Consent information shall be editable by authorized users.

---

## FR-027 Consent History

Consent changes shall remain historically available.

---

# Module 8 — Alerts & Flags

## FR-028 Patient Alerts

Users shall record patient alerts.

Examples include:

- VIP
- Communication Assistance
- Administrative Notes

---

## FR-029 Display Alerts

Important alerts shall be displayed wherever the patient is accessed.

---

# Module 9 — Lifecycle Management

## FR-030 Activate Patient

The system shall activate registered patients.

---

## FR-031 Update Patient Status

Authorized users shall manage lifecycle status according to PAT-003.

---

## FR-032 Record Deceased Status

Authorized users shall record deceased status.

---

## FR-033 Prevent Invalid Operations

The system shall prevent lifecycle actions prohibited by business rules.

---

# Module 10 — Audit & History

## FR-034 Audit Registration

Registration shall generate audit events.

---

## FR-035 Audit Demographic Changes

All demographic changes shall be audited.

---

## FR-036 Audit Merge Operations

Merge operations shall generate complete audit records.

---

## FR-037 View Patient History

Authorized users shall review patient administrative history.

---

# Non-Functional Considerations

The implementation should support:

- High search performance
- Enterprise scalability
- Data integrity
- High availability
- Role-Based Access Control (RBAC)
- Auditability
- Future interoperability

---

# Success Criteria

The Patient Management Domain is considered functionally complete when:

- Patient registration is standardized.
- Patient search is reliable.
- Duplicate management is available.
- Identity is protected.
- Administrative history is auditable.
- All downstream domains can safely consume patient identity.

---

# Architecture Notes

These functional requirements describe business capabilities only.

Technical implementation, UI behavior, APIs, database schema, and workflows shall be defined within the Solution Architecture and Technical Design documentation.

---

# Change History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-22 | Initial Functional Requirements |

---

# Approval

| Role | Name | Status |
|------|------|--------|
| Solution Architect | Ahmed Saleh | Approved |

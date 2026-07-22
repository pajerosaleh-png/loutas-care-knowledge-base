

| Property | Value |
|----------|-------|
| Document ID | PAT-007 |
| Domain | Patient Management |
| Document Type | Business Architecture |
| Classification | Business Integration |
| Status | Draft |
| Version | 1.0 |
| Owner | Business Architecture |
| Last Updated | 2026-07-22 |
| Depends On | PAT-001, PAT-002, PAT-003, PAT-004, PAT-005, PAT-006 |
| Related Documents | PAT-008, PAT-009 |

---

# Purpose

This document defines how the Patient Management Domain integrates with other Business Domains within the LOUTAS Care platform.

The objective is to establish clear ownership, responsibilities, and business interactions while maintaining loose coupling between domains.

This document describes business integration only and does not prescribe technical implementation mechanisms.

---

# Integration Principles

Patient Management is the authoritative owner of patient identity.

Other domains shall reference patient identity without creating or maintaining independent patient records.

All integrations shall preserve the Single Source of Truth principle.

---

# Business Integration Matrix

| Domain | Relationship | Role |
|---------|--------------|------|
| Appointment | Provides patient identity | Consumer |
| Patient Journey | Provides patient identity | Consumer |
| EMR | Provides patient demographics | Consumer |
| Billing | Provides patient identity | Consumer |
| Laboratory | Provides patient identity | Consumer |
| Radiology | Provides patient identity | Consumer |
| Pharmacy | Provides patient identity | Consumer |
| Reporting | Provides trusted patient data | Consumer |
| AI Services | Provides structured patient profile | Consumer |
| Security & RBAC | Uses identity for authorization | Shared Service |
| Audit & Timeline | Records identity events | Shared Service |
| Notification Center | Uses contact information | Shared Service |
| Configuration | Defines registration policies | Shared Service |

---

# Appointment Integration

Patient Management provides:

- Enterprise Patient ID
- MRN
- Patient demographics
- Patient status

Appointment Management:

- References patient identity.
- Shall not create patient records.
- Shall not modify demographic information.

---

# Patient Journey Integration

Patient Journey consumes patient identity to create clinical encounters.

Patient Journey owns:

- Visits
- Waiting Queue
- Encounter progression

Patient Management remains the owner of patient identity.

---

# EMR Integration

The EMR Domain consumes:

- Enterprise Patient ID
- Demographics
- Alerts
- Preferred Language
- Consent Status

The EMR Domain owns:

- Clinical Notes
- Diagnoses
- Allergies
- Medications
- Clinical History

Patient Management does not own clinical information.

---

# Billing Integration

Billing references patient identity for:

- Invoice creation
- Payment processing
- Financial reporting

Billing owns:

- Invoices
- Payments
- Financial Transactions

Patient Management shall never own financial records.

---

# Laboratory Integration

Laboratory references patient identity for:

- Test Orders
- Sample Tracking
- Result Reporting

Laboratory owns all laboratory workflows and results.

---

# Radiology Integration

Radiology references patient identity for:

- Imaging Requests
- Imaging Results
- Diagnostic Reports

Radiology owns all imaging activities.

---

# Pharmacy Integration

Pharmacy references patient identity when:

- Dispensing medications
- Recording prescriptions
- Managing medication history

Medication management remains owned by the Pharmacy Domain.

---

# Reporting Integration

Reporting consumes patient information to generate:

- Operational Reports
- Statistical Reports
- Executive Dashboards
- Quality Indicators

Reporting shall consume data without modifying patient identity.

---

# AI Services Integration

AI Services may consume patient identity for:

- Clinical summaries
- Administrative insights
- Predictive analytics
- Intelligent recommendations

AI Services shall never become the source of truth for patient identity.

---

# Security & RBAC Integration

Security services control:

- Authentication
- Authorization
- Role permissions

Patient Management supplies patient identity but does not control user authentication.

---

# Audit & Timeline Integration

The Audit Domain records events such as:

- Registration
- Demographic Updates
- Merge Operations
- Identity Verification
- Deactivation

Audit services preserve complete traceability.

---

# Notification Center Integration

Notification services consume:

- Mobile Number
- Email Address
- Preferred Language
- Communication Preferences

Notification delivery is outside the ownership of Patient Management.

---

# Configuration Integration

Configuration defines organization-specific settings including:

- Required registration fields
- Identity verification policies
- Consent requirements
- Numbering formats
- Validation rules

Patient Management consumes these policies during registration.

---

# Ownership Summary

| Information | Owner |
|-------------|-------|
| Patient Identity | Patient Management |
| MRN | Patient Management |
| Demographics | Patient Management |
| Appointments | Appointment |
| Visits | Patient Journey |
| Clinical Records | EMR |
| Billing | Billing |
| Laboratory Results | Laboratory |
| Radiology Reports | Radiology |
| Prescriptions | Pharmacy |
| User Permissions | Security |
| Audit History | Audit |

---

# Integration Principles Summary

- Single Source of Truth
- Enterprise Patient Identity
- Loose Coupling
- Clear Ownership
- Shared Enterprise Services
- Auditable Data Exchange
- Secure Information Sharing
- Scalable Architecture

---

# Architecture Notes

Patient Management is one of the Core Enterprise Domains.

Nearly every Business Domain depends on patient identity, but no domain may assume ownership of patient demographic information.

This architecture minimizes duplication, improves maintainability, and supports future enterprise interoperability.

---

# Change History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-22 | Initial Business Integration |

---

# Approval

| Role | Name | Status |
|------|------|--------|
| Solution Architect | Ahmed Saleh | Approved |

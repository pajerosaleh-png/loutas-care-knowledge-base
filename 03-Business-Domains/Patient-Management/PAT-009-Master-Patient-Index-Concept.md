# PAT-009 — Master Patient Index (MPI) Concept

| Property | Value |
|----------|-------|
| Document ID | PAT-009 |
| Domain | Patient Management |
| Document Type | Business Architecture |
| Classification | Strategic Architecture Concept |
| Status | Draft |
| Version | 1.0 |
| Owner | Business Architecture |
| Last Updated | 2026-07-22 |
| Depends On | PAT-001, PAT-003, PAT-004, PAT-007, PAT-008 |
| Related Documents | PAT-ADR-001 |

---

# Purpose

This document introduces the Master Patient Index (MPI) concept as the long-term strategic identity model for the LOUTAS Care platform.

The objective is to ensure that patient identity can scale beyond a single clinic and support enterprise healthcare organizations, healthcare networks, and future interoperability initiatives without requiring architectural redesign.

This document defines the business concept only and does not require immediate implementation.

---

# What is a Master Patient Index?

A Master Patient Index (MPI) is a centralized mechanism that ensures every patient has one trusted identity across the healthcare enterprise.

Instead of maintaining multiple independent patient records, the MPI establishes a single enterprise identity that is referenced by all participating business domains and authorized organizations.

---

# Business Vision

LOUTAS Care is designed around the principle that:

> One Patient = One Enterprise Identity

Regardless of:

- Clinic
- Branch
- Physician
- Department
- Number of Visits

The patient's identity remains consistent throughout their relationship with the organization.

---

# Why MPI Matters

Implementing the platform around MPI principles provides:

- Improved patient safety
- Reduced duplicate records
- Better continuity of care
- Reliable clinical history
- Consistent reporting
- Simplified interoperability
- Enterprise scalability

---

# Enterprise Identity Model

The Enterprise Identity consists of:

- Enterprise Patient ID
- Medical Record Number (MRN)
- Verified Identity Attributes
- Demographic Information
- Contact Information
- Identity Verification Status

All downstream business domains reference this identity rather than creating independent patient records.

---

# Relationship with Business Domains

The Patient Management Domain owns the enterprise identity.

Other domains consume that identity.

| Domain | Uses MPI |
|---------|----------|
| Appointment | Yes |
| Patient Journey | Yes |
| EMR | Yes |
| Billing | Yes |
| Laboratory | Yes |
| Radiology | Yes |
| Pharmacy | Yes |
| Reporting | Yes |
| AI Services | Yes |

---

# Duplicate Management

The architecture supports identifying possible duplicate patients using configurable matching criteria.

Potential identifiers include:

- National ID
- Passport
- Residency ID
- Mobile Number
- Full Name
- Date of Birth
- Gender

Confirmed duplicates may be merged according to approved business rules.

---

# Multi-Branch Support

The architecture supports organizations operating:

- Single Clinic
- Multiple Branches
- Medical Groups
- Outpatient Networks

Patients shall not require a new identity when visiting another authorized branch.

---

# Future Healthcare Networks

The architecture is prepared to support future scenarios where multiple healthcare organizations participate in a trusted identity ecosystem.

Examples include:

- Healthcare Groups
- Referral Networks
- Shared Care Programs
- Regional Health Networks

Implementation of these capabilities requires future governance decisions.

---

# External Interoperability

The architecture is designed to accommodate future interoperability initiatives, including integration with external healthcare systems and nationally recognized identity frameworks.

Specific technical standards will be defined in future Solution Architecture documents.

---

# Business Benefits

Adopting the MPI concept enables:

- Single Source of Truth
- Enterprise Identity
- Better Data Quality
- Reduced Administrative Costs
- Improved Patient Safety
- Better Clinical Continuity
- Long-Term Scalability

---

# Implementation Strategy

The first release of LOUTAS Care shall implement:

- Enterprise Patient ID
- Enterprise MRN
- Duplicate Detection
- Merge Operations

The following capabilities are intentionally deferred:

- Cross-Organization Identity Resolution
- Federated MPI
- National Identity Exchange
- External Identity Synchronization

This phased approach reduces implementation complexity while preserving future extensibility.

---

# Architectural Decisions

The MPI concept is based on the following approved architectural principles:

- Enterprise Patient Identity
- Single Source of Truth
- Clear Domain Ownership
- Loose Coupling
- Enterprise Scalability
- Future Interoperability

---

# Success Criteria

The architecture is considered successful when:

- Every patient has one trusted enterprise identity.
- Duplicate identities are minimized.
- Business domains consume patient identity consistently.
- Organizational growth does not require redesign.
- Future interoperability can be introduced without changing the business model.

---

# Architecture Notes

This document defines the strategic direction of patient identity management within LOUTAS Care.

It is not an implementation specification and does not introduce mandatory functionality for the current release.

Implementation priorities shall be determined by the Product Roadmap and Solution Architecture.

---

# Change History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-22 | Initial MPI Concept |

---

# Approval

| Role | Name | Status |
|------|------|--------|
| Solution Architect | Ahmed Saleh | Approved |

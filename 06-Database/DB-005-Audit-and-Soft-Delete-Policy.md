# Audit and Soft Delete Policy

| Field | Value |
|--------|-------|
| Document ID | DB-005 |
| Document Title | Audit and Soft Delete Policy |
| Book | LC-BOOK-001 Product Specification |
| Version | 1.0 |
| Status | Draft |
| Owner | Enterprise Architecture |
| Last Updated | 2026-07-25 |

---

# Purpose

This document defines the governance principles for audit information and soft deletion within the LOUTAS Care platform.

The objective is to preserve historical data, support traceability, maintain business continuity, and ensure accountability across all business modules.

This document establishes architectural principles only. Module-specific implementation details shall be defined within their respective documentation.

---

# Scope

This policy applies to all persistent business entities including:

- Patient
- Appointment
- Visit
- EMR
- Invoice
- Payment
- Laboratory
- Radiology
- Inventory
- Pharmacy
- Administration
- Security

---

# Audit Objectives

Audit information exists to support:

- Business traceability
- Operational accountability
- Regulatory compliance
- Historical analysis
- Incident investigation
- Data recovery

Audit information shall not modify business behaviour.

---

# Standard Audit Information

Where applicable, business entities should preserve audit information describing:

- Record creation
- Record modification
- Record deletion
- Responsible user
- Timestamp of each action

The exact implementation is defined by the application architecture.

---

# Soft Delete Policy

Business records should remain recoverable whenever business or regulatory requirements require historical preservation.

Soft deletion is intended to:

- Preserve historical references.
- Prevent accidental loss of information.
- Maintain audit history.
- Support reporting.

Soft deletion does not imply permanent removal from storage.

---

# Permanent Deletion

Permanent deletion should only occur when explicitly permitted by approved business rules or applicable legal requirements.

Where permanent deletion is supported, the corresponding module documentation shall define:

- Authorization requirements
- Business justification
- Retention policy
- Recovery limitations

---

# Referential Integrity

Soft deleted records shall not compromise referential integrity.

Relationships between historical records should remain valid whenever historical traceability is required.

---

# Historical Reporting

Historical reports should distinguish between:

- Active records
- Soft deleted records

Reporting requirements are governed by the owning business module.

---

# User Visibility

Application behaviour regarding soft deleted records shall be determined by business requirements.

Unless explicitly required, deleted records should not appear in normal operational workflows.

---

# Audit Integrity

Audit information should represent factual historical events.

Audit records shall not be altered except where explicitly permitted by approved governance procedures.

---

# Business Responsibility

Each business module is responsible for defining:

- Which entities support soft deletion.
- Retention requirements.
- Recovery process.
- Operational visibility.

This document defines the architectural policy only.

---

# Compliance

This policy supports:

- Enterprise traceability
- Information governance
- Business accountability
- Long-term maintainability

It does not replace legal or regulatory retention requirements applicable to a deployment.

---

# Dependencies

This document depends on:

- DB-001 Database Architecture
- DB-002 Database Naming Standards
- DB-003 Entity Relationship Guidelines
- DB-004 Primary Key and Identifier Policy

---

# Related Documents

- Product Constitution
- Architecture Documentation
- Clinical Documentation
- Billing Documentation

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-25 | Initial document |

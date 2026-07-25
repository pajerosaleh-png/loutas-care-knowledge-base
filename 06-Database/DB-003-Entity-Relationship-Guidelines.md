# Entity Relationship Guidelines

| Field | Value |
|--------|-------|
| Document ID | DB-003 |
| Document Title | Entity Relationship Guidelines |
| Book | LC-BOOK-001 Product Specification |
| Version | 1.0 |
| Status | Draft |
| Owner | Enterprise Architecture |
| Last Updated | 2026-07-25 |

---

# Purpose

This document defines the principles governing relationships between business entities within the LOUTAS Care database.

Its purpose is to ensure consistency, maintain data integrity, reduce redundancy, and provide a common design approach across all modules.

This document complements DB-001 (Database Architecture) and DB-002 (Database Naming Standards).

---

# Scope

This standard applies to all business entities including, but not limited to:

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

# Design Principles

Entity relationships shall:

- Represent real business relationships.
- Maintain referential integrity.
- Avoid unnecessary duplication.
- Support future expansion.
- Remain independent from user interface implementation.

---

# Relationship Types

The following relationship types are permitted.

## One-to-One (1:1)

Used only when one business entity owns exactly one related entity.

Examples may include:

- User → UserProfile

This relationship should be used sparingly.

---

## One-to-Many (1:N)

This is the most common relationship type.

Examples include:

Patient → Appointments

Patient → Visits

Patient → Invoices

Invoice → InvoiceItems

Branch → Users

Doctor → Appointments

---

## Many-to-Many (N:M)

Many-to-many relationships shall be implemented using junction tables.

Examples include:

UserRole

PatientAllergy

PatientDiagnosis

PatientMedication

InvoicePayment

---

# Junction Tables

Junction tables shall:

- Contain only relationship data.
- Include foreign keys for participating entities.
- Use meaningful names.
- Follow the DB-002 naming standards.

---

# Referential Integrity

Foreign keys shall enforce valid business relationships.

Business records shall not reference non-existing entities.

---

# Cascading Rules

Relationship behavior shall be explicitly defined during implementation.

Possible behaviors include:

- Restrict
- Cascade
- Set Null
- No Action

The selected behavior must support the corresponding business workflow.

---

# Optional Relationships

Optional relationships shall allow NULL foreign keys only when supported by business rules.

Mandatory business relationships should not allow NULL references.

---

# Circular Relationships

Circular references should be avoided whenever possible.

If unavoidable, they shall be fully documented and justified.

---

# Historical Data

Business history shall be preserved according to the applicable module policies.

Relationship design should support historical records without compromising referential integrity.

---

# Soft Delete Considerations

Where soft delete is implemented, relationships shall continue to preserve historical consistency.

Deleted records should remain traceable for audit purposes where applicable.

---

# Shared Reference Data

Reference entities that are shared across multiple modules should avoid unnecessary duplication.

Examples include:

- Branch
- User
- Role
- Specialty
- Service Catalog

---

# Entity Independence

Each business entity should own its data.

Entities should communicate through relationships rather than duplicating business information.

---

# Business Integrity

Database relationships shall reflect approved business processes documented within the Knowledge Base.

Database structure shall not introduce relationships that are not supported by approved business documentation.

---

# Documentation Requirement

Each new business entity introduced into the system should document:

- Business purpose
- Parent entity (if applicable)
- Child entities (if applicable)
- Relationship type
- Business constraints
- Related modules

---

# Related Documents

- Product Constitution
- ARCH-001
- ARCH-002
- DB-001 Database Architecture
- DB-002 Database Naming Standards

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-25 | Initial document |

# Database Naming Standards

| Field | Value |
|--------|-------|
| Document ID | DB-002 |
| Document Title | Database Naming Standards |
| Book | LC-BOOK-001 Product Specification |
| Version | 1.0 |
| Status | Draft |
| Owner | Enterprise Architecture |
| Last Updated | 2026-07-25 |

---

# Purpose

This document defines the official database naming conventions used across the LOUTAS Care platform.

The objective is to ensure consistency, readability, maintainability, and long-term governance of all database objects.

These standards apply to all current and future modules.

---

# Scope

This standard applies to:

- Tables
- Columns
- Primary Keys
- Foreign Keys
- Indexes
- Constraints
- Views
- Database Functions
- Database Procedures
- Audit Tables
- Migration Files

---

# Naming Principles

All database object names shall be:

- Clear
- Predictable
- Human readable
- Consistent across all modules
- Independent of implementation language

Names shall describe the business concept rather than technical implementation.

---

# Language

All database object names shall be written in English.

Abbreviations should be avoided unless they are universally recognized.

Examples:

Preferred

Patient

Appointment

Invoice

Payment

Laboratory

Radiology

Avoid

Pat

Inv

TblPatient

PatientDataTable

---

# Table Naming

Tables shall use singular business entity names.

Examples

Patient

Appointment

Invoice

InvoiceItem

Payment

Visit

ClinicalNote

Medication

LaboratoryOrder

RadiologyOrder

---

# Junction Tables

Relationship tables shall describe both participating entities.

Examples

UserRole

PatientAllergy

PatientDiagnosis

InvoicePayment

---

# Primary Keys

Each table shall contain one primary identifier.

Naming convention:

Id

Examples

Patient.Id

Appointment.Id

Invoice.Id

Visit.Id

---

# Foreign Keys

Foreign key columns shall reference the related business entity.

Examples

PatientId

AppointmentId

VisitId

InvoiceId

DoctorId

BranchId

CreatedByUserId

UpdatedByUserId

---

# Boolean Columns

Boolean fields should use descriptive names.

Examples

IsActive

IsDeleted

IsConfirmed

IsCancelled

IsPaid

IsLocked

---

# Date and Time Columns

Recommended naming:

CreatedAt

UpdatedAt

DeletedAt

ScheduledAt

CompletedAt

PaidAt

ConfirmedAt

CancelledAt

---

# Status Columns

Status fields shall represent business workflow state.

Examples

Status

PaymentStatus

AppointmentStatus

InvoiceStatus

VisitStatus

---

# Monetary Values

Currency-related fields should clearly identify the stored value.

Examples

Subtotal

DiscountAmount

TaxAmount

TotalAmount

PaidAmount

RemainingAmount

CurrencyCode

---

# Audit Columns

Business tables should support standard audit information where applicable.

Recommended fields:

CreatedAt

CreatedByUserId

UpdatedAt

UpdatedByUserId

DeletedAt

DeletedByUserId

---

# Index Naming

Recommended format:

IX_<TableName>_<ColumnName>

Examples

IX_Patient_NationalId

IX_Appointment_ScheduledAt

IX_Invoice_InvoiceNumber

---

# Primary Key Naming

Recommended format:

PK_<TableName>

Example

PK_Patient

PK_Appointment

PK_Invoice

---

# Foreign Key Naming

Recommended format:

FK_<Table>_<ReferencedTable>

Examples

FK_Appointment_Patient

FK_Invoice_Patient

FK_Payment_Invoice

---

# Unique Constraints

Recommended format:

UQ_<Table>_<Column>

Example

UQ_User_Email

---

# Check Constraints

Recommended format:

CK_<Table>_<Rule>

Examples

CK_Invoice_TotalPositive

CK_Appointment_ValidTime

---

# Migration Files

Migration names should describe the business change.

Examples

CreatePatientTable

AddInvoiceStatus

CreateBillingModule

AddAuditColumns

Avoid generic names such as:

Migration1

UpdateSchema

FixTable

---

# Related Documents

- ARCH-001
- ARCH-002
- DB-001 Database Architecture
- Product Constitution

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-25 | Initial document |

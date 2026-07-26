# STD-002 — Naming Conventions

**Document Classification:** Enterprise Standard
**Priority:** Critical
**Status:** Approved
**Version:** 1.0

---

# 1. Purpose

This document defines the official naming conventions for the LOUTAS Care platform.

Its objective is to ensure consistency, readability, maintainability, discoverability, and traceability across all software artifacts, including source code, database objects, APIs, infrastructure resources, documentation, and configuration files.

These naming standards are mandatory for all current and future project contributors.

---

# 2. Scope

This standard applies to:

- Source Code
- Git Repository
- Documentation
- APIs
- Database Objects
- Infrastructure
- Configuration Files
- UI Components
- AI Services
- Test Assets
- CI/CD Pipelines

---

# 3. General Principles

All names shall be:

- Descriptive
- Consistent
- Predictable
- Readable
- Unambiguous
- English language only
- Singular where representing a single entity
- Plural only where representing collections

Abbreviations shall be avoided unless they are industry-standard.

---

# 4. Repository Naming

Repositories shall use lowercase kebab-case.

Examples:

- loutas-care
- loutas-care-api
- loutas-care-web
- loutas-care-mobile
- loutas-care-knowledge-base

---

# 5. Folder Naming

Folders shall use lowercase kebab-case.

Examples:

- patient-management
- billing
- appointments
- ai-services
- clinical-library

---

# 6. File Naming

Markdown files:

- Pascal Case or approved numeric prefixes.

Examples:

- README.md
- CONTRIBUTING.md
- STD-001-Documentation-Standards.md
- FR-EMR-001.md

Source code:

- React Components → PascalCase
- Utility files → camelCase or kebab-case according to project conventions

Examples:

- PatientCard.tsx
- AppointmentCalendar.tsx
- dateFormatter.ts

---

# 7. Database Naming

## Tables

Plural snake_case.

Examples:

- patients
- appointments
- invoices
- invoice_items
- laboratory_orders

---

## Columns

Lowercase snake_case.

Examples:

- patient_id
- created_at
- updated_at
- appointment_date

---

## Primary Keys

```
id
```

---

## Foreign Keys

```
patient_id
doctor_id
invoice_id
branch_id
```

---

## Junction Tables

Alphabetical order.

Examples:

```
role_permissions
user_roles
patient_allergies
```

---

# 8. API Naming

REST endpoints shall use:

- lowercase
- plural nouns
- kebab-case only when required

Examples:

```
/patients
/appointments
/invoices
/pharmacy/orders
```

Nested resources:

```
/patients/{id}/appointments
/patients/{id}/allergies
```

HTTP verbs shall not appear in endpoint names.

Incorrect:

```
/getPatients
/createInvoice
```

---

# 9. Service Naming

Services shall use PascalCase.

Examples:

```
PatientService
BillingService
AppointmentService
InventoryService
```

Interfaces:

```
IPatientService
IBillingService
```

---

# 10. DTO Naming

DTO classes shall end with:

```
Dto
```

Examples:

```
CreatePatientDto
UpdateAppointmentDto
InvoiceResponseDto
```

---

# 11. React Components

Components shall use PascalCase.

Examples:

```
PatientBanner
AppointmentCard
BillingSummary
MedicationTable
```

Custom Hooks:

```
useAppointments
usePatients
useBilling
```

---

# 12. Environment Variables

Environment variables shall use uppercase with underscores.

Examples:

```
DATABASE_URL
JWT_SECRET
NEXT_PUBLIC_API_URL
SMTP_HOST
AI_PROVIDER
```

---

# 13. Git Branch Naming

Feature:

```
feature/patient-registration
```

Bug Fix:

```
bugfix/invoice-calculation
```

Hotfix:

```
hotfix/login-timeout
```

Release:

```
release/v1.2.0
```

Documentation:

```
docs/security-standards
```

---

# 14. Docker Resources

Images:

```
loutas-care-api
loutas-care-web
```

Containers:

```
api
web
postgres
redis
```

Networks:

```
loutas-network
```

Volumes:

```
postgres-data
```

---

# 15. Kubernetes Resources

Namespaces:

```
production
staging
development
```

Deployments:

```
patient-service
billing-service
```

Services:

```
patient-api
billing-api
```

---

# 16. Logging & Audit Events

Event names shall use uppercase with underscores.

Examples:

```
PATIENT_CREATED
PATIENT_UPDATED
APPOINTMENT_BOOKED
VISIT_STARTED
VISIT_COMPLETED
INVOICE_CREATED
PAYMENT_RECEIVED
LOGIN_SUCCESS
LOGIN_FAILED
```

---

# 17. AI Services

AI services shall use descriptive PascalCase names.

Examples:

```
ClinicalAssistant
MedicalSummarizer
DiagnosisAssistant
BillingAssistant
CodingAssistant
```

AI model identifiers shall include the provider and version where applicable.

---

# 18. Test Naming

Unit Tests:

```
PatientService.test.ts
```

Integration Tests:

```
appointment.integration.test.ts
```

End-to-End Tests:

```
patient-registration.e2e.ts
```

---

# 19. Documentation Naming

Standards:

```
STD-001
STD-002
```

Architecture:

```
ADR-001
```

Change Requests:

```
RFC-001
```

Functional Requirements:

```
FR-EMR-001
```

Business Rules:

```
BR-BILL-014
```

---

# 20. Compliance

All project artifacts shall comply with this naming standard.

Automated validation through linting, code review, or CI/CD is recommended where technically feasible.

---

# 21. Exceptions

Any deviation from this standard shall be documented and approved through the project's governance process.

---

# 22. Related Documents

- STD-001 Documentation Standards
- STD-003 Coding Standards
- STD-004 API Design Standards
- STD-005 Database Standards
- Development Standards
- Architecture
- Governance

---

**End of STD-002**

# DEV-018-API-Naming-Conventions.md

**Document ID:** DEV-018  
**Document Classification:** Development Standard  
**Owner:** Engineering Team  
**Status:** Approved  
**Version:** 1.0  
**Created:** 2026-07-27  
**Last Updated:** 2026-07-27  
**Next Review:** 2027-07-27  
**Approval Authority:** Chief Software Architect

---

# Purpose

This document defines the official API naming conventions for the LOUTAS Care Platform.

Its purpose is to ensure that all REST APIs are predictable, consistent, scalable, and easy to consume by frontend applications, mobile applications, integrations, and future external systems.

---

# Objectives

- Standardize endpoint naming.
- Improve API readability.
- Simplify integration.
- Reduce ambiguity.
- Maintain consistency across all modules.

---

# General Principles

APIs shall:

- Follow REST principles.
- Use nouns instead of verbs.
- Be resource-oriented.
- Be predictable.
- Be version-ready.

---

# Base URL

All APIs shall use:

```text
/api/
```

Example:

```text
/api/patients
/api/appointments
/api/invoices
```

---

# Resource Naming

Resources shall:

- Use lowercase.
- Use plural nouns.
- Use hyphens only when required.

Examples:

```text
patients
appointments
invoices
payments
branches
users
```

Avoid:

```text
Patient
PatientList
GetPatient
patient_data
```

---

# HTTP Methods

Use standard HTTP methods.

| Method | Purpose |
|----------|----------|
| GET | Retrieve data |
| POST | Create resource |
| PUT | Replace resource |
| PATCH | Partial update |
| DELETE | Remove resource |

---

# Resource Examples

Patients

```text
GET /api/patients
GET /api/patients/{id}
POST /api/patients
PUT /api/patients/{id}
DELETE /api/patients/{id}
```

Appointments

```text
GET /api/appointments
POST /api/appointments
PATCH /api/appointments/{id}
```

Invoices

```text
GET /api/invoices
POST /api/invoices
```

---

# Query Parameters

Use query parameters for filtering.

Example:

```text
GET /api/patients?page=1
GET /api/patients?search=Ahmed
GET /api/appointments?status=waiting
```

---

# Nested Resources

Use nested resources only when ownership is clear.

Example:

```text
GET /api/patients/{id}/visits
GET /api/patients/{id}/allergies
```

Avoid excessive nesting.

---

# Action Endpoints

Business actions should remain explicit.

Examples:

```text
POST /api/appointments/{id}/confirm
POST /api/appointments/{id}/cancel
POST /api/invoices/{id}/collect-payment
POST /api/visits/{id}/close
```

---

# API Versioning

Future public APIs should support versioning.

Example:

```text
/api/v1/patients
```

---

# Response Format

Responses should follow a consistent structure.

Example:

```json
{
  "success": true,
  "data": {},
  "message": null
}
```

Errors:

```json
{
  "success": false,
  "error": {
    "code": "PATIENT_NOT_FOUND",
    "message": "Patient not found."
  }
}
```

---

# Status Codes

Use standard HTTP status codes.

Examples:

- 200 OK
- 201 Created
- 204 No Content
- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 409 Conflict
- 422 Validation Error
- 500 Internal Server Error

---

# Security

Protected APIs shall require authentication.

Authorization shall be enforced using RBAC.

Sensitive endpoints shall generate Audit Log entries.

---

# Documentation

Every public endpoint shall include:

- Purpose
- Parameters
- Request example
- Response example
- Error responses
- Authorization requirements

---

# Related Documents

- DEV-017-Backend-Development-Standards.md
- DEV-019-Database-Naming-Conventions.md
- DEV-014-TypeScript-Standards.md
- DEV-006-Configuration-Management.md

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-27 | Initial API Naming Conventions |

---

**End of Document**

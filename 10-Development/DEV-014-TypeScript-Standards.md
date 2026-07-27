# DEV-014-TypeScript-Standards.md

**Document ID:** DEV-014  
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

This document defines the official TypeScript development standards for the LOUTAS Care Platform.

These standards ensure that all TypeScript code across the platform is consistent, readable, maintainable, scalable, and type-safe.

This standard applies to all Frontend, Backend, Shared Libraries, APIs, and utility modules.

---

# Objectives

The objectives of this document are to:

- Establish a unified TypeScript coding style.
- Improve readability and maintainability.
- Reduce runtime errors through strong typing.
- Encourage reusable and scalable code.
- Improve collaboration between developers.

---

# General Principles

Developers shall:

- Enable strict typing.
- Prefer explicit types.
- Write self-documenting code.
- Keep functions small and focused.
- Avoid duplicate logic.
- Prefer composition over inheritance.

---

# TypeScript Compiler Configuration

The project shall always enable Strict Mode.

Recommended options include:

- strict
- noImplicitAny
- strictNullChecks
- noUnusedLocals
- noUnusedParameters
- noFallthroughCasesInSwitch

---

# Naming Conventions

| Element | Convention | Example |
|----------|------------|----------|
| Interface | PascalCase | PatientDto |
| Type Alias | PascalCase | VisitStatus |
| Enum | PascalCase | AppointmentStatus |
| Variable | camelCase | patientName |
| Function | camelCase | createInvoice |
| Constant | UPPER_SNAKE_CASE | MAX_LOGIN_ATTEMPTS |
| File | kebab-case | patient-service.ts |

---

# Interfaces

Use interfaces for:

- DTOs
- API contracts
- Service contracts
- Repository contracts
- Object definitions

Example:

```typescript
interface PatientDto {
    id: string;
    fullName: string;
}
```

---

# Type Aliases

Use type aliases for:

- Union types
- Utility types
- Generic compositions
- Mapped types

Example:

```typescript
type VisitStatus =
    | "Waiting"
    | "InRoom"
    | "Completed";
```

---

# DTO Standards

DTO names shall end with:

```text
Dto
```

Examples:

- PatientDto
- InvoiceDto
- AppointmentDto
- VisitSummaryDto

---

# Service Naming

Service classes shall end with:

```text
Service
```

Examples:

- PatientService
- BillingService
- AppointmentService

---

# Repository Naming

Repository interfaces shall end with:

```text
Repository
```

Examples:

- PatientRepository
- InvoiceRepository

---

# Enum Standards

Enums should have meaningful names.

Example:

```typescript
enum AppointmentStatus {
    BOOKED,
    CONFIRMED,
    WAITING,
    IN_ROOM,
    COMPLETED,
    CANCELLED
}
```

---

# Use of any

The use of `any` is prohibited except in documented exceptional cases.

Whenever possible, use:

- unknown
- Generics
- Interfaces
- Type Aliases

instead.

---

# Async Programming

Prefer:

```typescript
async / await
```

Avoid chained `.then()` and `.catch()` unless necessary.

---

# Optional Chaining

Preferred:

```typescript
patient?.address?.city
```

---

# Nullish Coalescing

Preferred:

```typescript
patient.name ?? "Unknown"
```

instead of logical OR when preserving valid falsy values.

---

# Imports

Imports shall be grouped as:

1. External packages
2. Internal shared modules
3. Relative imports

---

# Exports

Named exports are preferred.

Default exports should only be used where required by framework conventions.

---

# Prisma Types

Developers shall use generated Prisma types whenever possible.

Duplicating Prisma model definitions manually is prohibited.

---

# Shared Types

Shared interfaces shall reside inside shared modules to prevent duplication.

---

# Error Handling

Custom error classes should be preferred.

Sensitive information shall never be exposed in error messages.

---

# Comments

Comments should explain business rules or architectural decisions.

Avoid comments that merely repeat what the code already expresses.

---

# Formatting

Formatting shall be enforced using:

- ESLint
- Prettier

Manual formatting should not override project standards.

---

# Security

Developers shall never:

- Disable type checking.
- Ignore compiler errors.
- Suppress warnings without justification.

---

# Related Documents

- DEV-003-Coding-Standards.md
- DEV-004-Error-Handling-Standards.md
- DEV-017-Backend-Development-Standards.md
- DEV-018-API-Naming-Conventions.md

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-27 | Initial release |

---

**End of Document**

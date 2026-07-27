# ADR-004-Authorization-RBAC.md

**Document ID:** ADR-004  
**Document Classification:** Architecture Decision Record  
**Owner:** Chief Software Architect  
**Status:** Approved  
**Version:** 1.0  
**Created:** 2026-07-26  
**Last Updated:** 2026-07-26  
**Next Review:** 2027-07-26  
**Approval Authority:** Architecture Review Board

---

# ADR-004 — Role-Based Access Control (RBAC) Architecture

---

# Status

**Approved**

This Architecture Decision Record defines the official authorization architecture for the **LOUTAS Care Platform**. All authorization mechanisms shall comply with this decision unless superseded by a future ADR.

---

# Context

LOUTAS Care is a multi-tenant healthcare SaaS platform serving multiple organizations, branches, and clinical roles.

The platform includes modules such as:

- Reception
- Patient Management
- Appointment Scheduling
- Electronic Medical Record (EMR)
- Billing
- Pharmacy
- Laboratory
- Radiology
- Inventory
- Administration
- Artificial Intelligence Services

Each module contains operations that require controlled access based on the authenticated user's responsibilities.

---

# Problem Statement

The platform requires an authorization model capable of:

- Supporting multiple organizations
- Supporting multiple branches
- Providing granular permissions
- Enforcing least-privilege access
- Scaling across future modules
- Supporting regulatory compliance
- Maintaining centralized governance
- Remaining easy to manage

---

# Decision

LOUTAS Care shall implement a centralized **Role-Based Access Control (RBAC)** architecture.

Authorization decisions shall be based on:

- Authenticated User
- Tenant
- Branch
- Assigned Role(s)
- Granted Permission(s)

Permissions shall be evaluated on every protected request.

Authentication establishes identity.

Authorization determines what the authenticated user is allowed to do.

---

# Authorization Architecture

```
                User
                  │
                  ▼
          Authentication
                  │
                  ▼
          Identity Verified
                  │
                  ▼
        Authorization Engine
                  │
      ┌───────────┴───────────┐
      ▼                       ▼
 Assigned Roles         Permissions
      │                       │
      └───────────┬───────────┘
                  ▼
          Access Decision
```

---

# RBAC Hierarchy

Authorization follows this hierarchy:

```
Tenant
   │
   ▼
Branch
   │
   ▼
User
   │
   ▼
Role
   │
   ▼
Permission
```

A user may belong to one tenant while having access to one or more branches, depending on administrative configuration.

---

# Core Concepts

## User

Represents an authenticated individual.

Examples:

- Physician
- Receptionist
- Nurse
- Pharmacist
- Cashier
- Administrator
- Laboratory Technician

---

## Role

A role groups permissions.

Examples:

- Super Administrator
- Organization Administrator
- Branch Manager
- Physician
- Receptionist
- Cashier
- Pharmacist
- Laboratory Staff
- Radiology Staff
- Inventory Manager
- Auditor

Users may be assigned multiple roles when justified by business requirements.

---

## Permission

A permission authorizes a specific operation.

Examples:

- View Patient
- Create Patient
- Edit Patient
- Delete Patient
- View Appointment
- Schedule Appointment
- Cancel Appointment
- Create Invoice
- Collect Payment
- Dispense Medication
- View Reports
- Configure System

Permissions are the smallest unit of authorization.

---

# Permission Model

Permissions shall be action-oriented.

Typical permission naming convention:

```
patient.view
patient.create
patient.update
patient.delete

appointment.view
appointment.create
appointment.update
appointment.cancel

invoice.create
invoice.collect
invoice.refund

inventory.adjust

reports.export

settings.manage
```

This naming convention shall remain consistent across the platform.

---

# Authorization Flow

Every protected request shall verify:

1. User authentication
2. Active account
3. Tenant membership
4. Branch authorization
5. Assigned roles
6. Required permission

Access shall only be granted if all validation steps succeed.

---

# Multi-Tenant Enforcement

Authorization shall always respect tenant boundaries.

Users:

- Cannot access another tenant's data.
- Cannot assign roles outside their tenant.
- Cannot elevate privileges beyond granted permissions.

Cross-tenant administration is reserved exclusively for approved platform-level administrators.

---

# Branch-Level Authorization

Organizations may restrict permissions to selected branches.

Examples:

- Reception staff assigned to Branch A cannot manage appointments in Branch B.
- Inventory managers may only manage stock within their assigned branches.

Branch validation shall occur before permission evaluation.

---

# Least Privilege Principle

Users shall receive only the permissions necessary to perform their responsibilities.

Unused administrative permissions shall not be granted.

Privilege escalation shall require formal approval.

---

# Administrative Roles

Platform administrators may perform:

- User management
- Role assignment
- Permission assignment
- Branch assignment
- Organization configuration

Administrative activities shall always be audited.

---

# Audit Requirements

Authorization events shall be recorded for:

- Permission changes
- Role assignment
- Role removal
- Login failures
- Privilege escalation
- Administrative actions
- Access denials

Audit records shall support compliance and incident investigations.

---

# Security Considerations

The authorization system shall:

- Deny access by default.
- Validate permissions server-side.
- Ignore client-side permission claims.
- Prevent privilege escalation.
- Validate tenant context.
- Validate branch context.
- Record authorization failures.

Authorization shall never rely solely on frontend controls.

---

# Alternatives Considered

## Option 1 — Static Role Checks

### Advantages

- Simple implementation

### Disadvantages

- Poor scalability
- Difficult maintenance
- Limited flexibility

**Decision:** Rejected.

---

## Option 2 — Role-Based Access Control (RBAC)

### Advantages

- Mature design
- Easy administration
- Scalable
- Widely adopted
- Supports enterprise governance

### Disadvantages

- Requires disciplined permission management

**Decision:** **Approved.**

---

## Option 3 — Attribute-Based Access Control (ABAC)

### Advantages

- Highly flexible
- Fine-grained decisions

### Disadvantages

- Increased complexity
- Higher maintenance cost
- More difficult governance

**Decision:** Deferred for future evaluation where advanced policy requirements emerge.

---

# Consequences

Positive outcomes include:

- Centralized authorization
- Improved security
- Easier administration
- Better compliance
- Simplified auditing
- Future scalability

Potential challenges include:

- Permission proliferation
- Role management complexity
- Governance overhead

These shall be addressed through periodic RBAC reviews.

---

# Implementation Requirements

The implementation shall ensure:

- Centralized authorization service
- Permission-based access checks
- Multi-role support
- Tenant-aware authorization
- Branch-aware authorization
- Server-side enforcement
- Complete audit logging
- Version-controlled permission catalog

---

# Compliance Considerations

The RBAC architecture supports:

- Least Privilege
- Separation of Duties
- Healthcare privacy requirements
- Regulatory compliance
- Auditability
- Secure administrative governance

---

# Risks

Primary risks include:

- Excessive permissions
- Incorrect role assignments
- Privilege escalation
- Missing authorization checks
- Inconsistent permission naming

These risks shall be mitigated through governance, testing, and periodic access reviews.

---

# Related Documents

- ADR-001-Multi-Tenant-Architecture.md
- ADR-003-Authentication-Architecture.md
- Security Standards
- API Standards
- Identity Management Documentation
- Architecture-Roadmap.md
- Audit Logging ADR

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-26 | Initial approved version |

---

**End of Document**

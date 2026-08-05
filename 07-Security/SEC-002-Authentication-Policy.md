# Authentication Policy

| Field | Value |
|--------|-------|
| Document ID | SEC-002 |
| Document Title | Authentication Policy |
| Book | LC-BOOK-001 Product Specification |
| Version | 1.1 |
| Status | Updated — Pending Product Owner Review |
| Owner | Enterprise Architecture |
| Last Updated | 2026-08-04 |
| Related Architecture Decision | Security Architecture Review — Phase 2.1 Production Security Hardening (approved) |

---

# Change Summary (v1.0 → v1.1)

This revision synchronizes the Authentication Policy with the approved Phase 2.1 Production Security Hardening decisions. No new policy is introduced and no authentication workflow is redesigned. Added: **Authentication Secret Governance** (mandatory signing secret, no default, fail-fast startup) and **Brute-Force Protection** (approved login rate-limit baseline). Existing principles are preserved unchanged.

---

# Purpose

This document defines the governance principles for user authentication within the LOUTAS Care platform.

The objective is to ensure that every authenticated session is established through approved identity verification mechanisms while maintaining consistency with the Enterprise Security Architecture.

This document defines governance principles only and does not prescribe authentication technologies.

---

# Scope

This policy applies to:

- Interactive users
- Administrative users
- Clinical users
- Reception users
- Financial users
- Platform services
- Future platform modules

---

# Authentication Objectives

Authentication shall support:

- Identity verification
- Secure access
- Operational accountability
- Session establishment
- Enterprise governance

Authentication shall occur before protected business resources become available.

---

# Authentication Principles

Authentication shall follow these principles:

- Every user shall have an identifiable account.
- Authentication shall occur before authorization.
- Successful authentication shall establish an authenticated session.
- Authentication shall support auditability.
- Authentication shall not expose sensitive credentials.

---

# Identity Verification

Authentication mechanisms shall verify the identity of users before granting access.

Identity verification methods shall be determined by the approved implementation architecture.

---

# Authentication Lifecycle

The authentication process consists of:

1. Identity submission
2. Credential verification
3. Authentication decision
4. Session establishment
5. Continuous session governance
6. Session termination

---

# Authentication Secret Governance

*(Synchronized — Phase 2.1 Production Security Hardening)*

Where authentication establishes a signed session token, the signing secret shall be governed as follows:

- The signing secret shall be **mandatory**. The platform shall obtain it exclusively from secure runtime configuration.
- No default, embedded, placeholder, or fallback secret shall exist in source code or configuration.
- If the signing secret is absent or empty at startup, the application shall **fail to start** (fail-fast), preventing operation in an insecure state.
- Secret provisioning, storage, and rotation are governed by the Password and Credential Policy (SEC-004), the Security Standards secrets-management requirements (STD-006 §10), and System Configuration governance (PLT-002).

This governance ensures no environment can run with an unprotected or shared default signing secret.

---

# Authentication Failure

Authentication failures should:

- Prevent access to protected resources.
- Preserve audit information.
- Avoid disclosure of sensitive system information.
- Follow approved security governance.

## Brute-Force Protection

*(Synchronized — Phase 2.1 Production Security Hardening)*

The authentication (login) endpoint shall be protected against brute-force and credential-stuffing attempts through rate limiting:

- The **approved baseline** is a maximum of **5 failed authentication attempts per source IP per 15-minute window**.
- Requests exceeding the limit shall receive **HTTP 429 (Too Many Requests)**.
- Successful authentications shall **not** count toward the limit, so legitimate users are not penalized.
- The threshold and window are organization-configurable and shall never be disabled below the approved baseline.

Rate-limiting enforcement at the API layer is governed by API Security (SEC-008). Account-level lockout after repeated failures remains governed by the Authentication functional requirements (FR-SEC-001.6).

---

# Account Responsibility

Each authenticated account shall represent an identifiable business responsibility.

Shared operational accounts should be avoided unless explicitly approved by organizational governance.

---

# Credential Protection

Credential management shall follow approved security policies.

Credential storage, transmission, and lifecycle management are governed by dedicated security documentation.

---

# Audit Requirements

Authentication events should be traceable.

Examples include:

- Successful authentication
- Failed authentication
- Rate-limited authentication attempts
- Session establishment
- Session termination

Audit implementation is defined by the logging architecture.

---

# Security Considerations

Authentication shall support:

- Confidentiality
- Integrity
- Accountability
- Traceability

Authentication mechanisms should remain consistent with enterprise security objectives.

---

# Compliance

This document supports:

- Enterprise Security Architecture
- Identity Governance
- Access Governance
- Information Protection

---

# Dependencies

- SEC-001 Security Architecture
- Database Documentation
- Enterprise Architecture

---

# Related Documents

- SEC-003 Authorization and RBAC
- SEC-004 Password and Credential Policy
- SEC-005 Session Management
- SEC-006 Audit and Logging
- SEC-008 API Security

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-25 | Initial Release |
| 1.1 | 2026-08-04 | Synchronized with approved Phase 2.1 Production Security Hardening: added Authentication Secret Governance (mandatory secret, no fallback, fail-fast) and Brute-Force Protection (login rate-limit baseline 5 failed / IP / 15 min → HTTP 429). No workflow redesign. |

# PAT-005 — Business Rules

| Property | Value |
|----------|-------|
| Document ID | PAT-005 |
| Domain | Patient Management |
| Document Type | Business Architecture |
| Classification | Business Rules |
| Status | Draft |
| Version | 1.0 |
| Owner | Business Architecture |
| Last Updated | 2026-07-22 |
| Depends On | PAT-001, PAT-002, PAT-003, PAT-004 |
| Related Documents | PAT-006, PAT-007, PAT-009 |

---

# Purpose

This document defines the official business rules governing patient identity, registration, maintenance, lifecycle management, and enterprise ownership within the LOUTAS Care platform.

These rules are mandatory for all implementations unless superseded by approved governance decisions.

---

# BR-001 Enterprise Identity

Every patient shall have exactly one Enterprise Patient Identity.

A patient identity:

- Is created once.
- Exists for the patient's lifetime.
- Cannot be duplicated intentionally.
- Cannot be reassigned.
- Cannot be reused.

---

# BR-002 Enterprise Patient ID

The Enterprise Patient ID shall:

- Be generated automatically.
- Be globally unique.
- Never change.
- Never be editable.
- Never be recycled.

---

# BR-003 Medical Record Number (MRN)

Each patient shall have one MRN.

Business Rules

- One MRN per patient.
- One patient per MRN.
- MRN is permanent.
- MRN cannot be reused.
- MRN cannot be transferred.

---

# BR-004 Patient Registration

A patient may only be registered once.

Minimum registration information shall be defined by organizational configuration.

Duplicate checking should occur before registration is completed.

---

# BR-005 Mandatory Demographics

The organization shall define mandatory demographic fields.

Typical mandatory fields include:

- Full Name
- Date of Birth
- Gender
- Mobile Number
- National ID (when applicable)

---

# BR-006 Identity Verification

Patient identity verification is recommended before the first clinical encounter.

Verification may use:

- National ID
- Passport
- Residency ID
- Other approved identification

Verification status shall be recorded.

---

# BR-007 Duplicate Detection

The system shall attempt to identify possible duplicate patients using combinations of:

- National ID
- Passport
- Mobile Number
- Full Name
- Date of Birth
- Gender

Potential duplicates require user review before creating a new patient.

---

# BR-008 Patient Merge

Duplicate patients may be merged only by authorized users.

Merge Rules

- Clinical history is preserved.
- Billing history is preserved.
- Appointments remain linked.
- Audit history remains intact.
- The surviving Enterprise Patient ID is retained.

Deletion of historical information is prohibited.

---

# BR-009 Patient Update

Authorized users may update demographic information.

Updates shall:

- Be audited.
- Preserve historical traceability.
- Not create a new patient identity.

---

# BR-010 Patient Deactivation

Patients shall not be physically deleted.

If organizational policy requires deactivation:

- The record remains available.
- Historical information remains intact.
- Future operations may be restricted.

---

# BR-011 Deceased Patients

When a patient is confirmed deceased:

- Identity remains permanent.
- Clinical history is retained.
- Billing history is retained.
- Future appointments are prohibited.
- New visits cannot be initiated.

---

# BR-012 Search Rules

Patient search should support:

- MRN
- Enterprise Patient ID
- National ID
- Passport
- Mobile Number
- Full Name
- Date of Birth

Search shall prioritize exact matches before partial matches.

---

# BR-013 Privacy

Patient information shall only be accessible by authorized users.

Access shall follow:

- RBAC
- Organizational policies
- Audit requirements

---

# BR-014 Consent

Patient consent shall be managed according to organizational policy.

Consent information should be available to authorized clinical users.

Consent history shall remain auditable.

---

# BR-015 Audit Trail

The following events shall generate audit records:

- Registration
- Identity verification
- Demographic update
- Merge
- Deactivation
- Consent changes

Audit history shall never be deleted.

---

# BR-016 Enterprise Ownership

Patient identity is owned exclusively by the Patient Management Domain.

Other business domains:

- Shall reference patient identity.
- Shall not duplicate patient records.
- Shall not modify enterprise identity.

---

# BR-017 Multi-Branch Organizations

Patient identity shall remain valid across all authorized branches.

Patients shall not require re-registration when receiving services at another branch within the same organization.

---

# BR-018 External Integrations

External systems shall reference the Enterprise Patient ID or approved identifiers.

External integrations shall not create independent patient identities without approved synchronization rules.

---

# BR-019 Data Retention

Patient identity records shall be retained according to applicable healthcare regulations and organizational retention policies.

Historical records shall remain available for authorized auditing and reporting purposes.

---

# BR-020 Business Rule Governance

Changes to these business rules require:

- Business approval.
- Architecture review.
- Documentation update.
- Version increment.
- Change history entry.

---

# Business Rule Summary

| Category | Rules |
|----------|-------|
| Identity | BR-001 → BR-003 |
| Registration | BR-004 → BR-006 |
| Duplicate Management | BR-007 → BR-008 |
| Data Management | BR-009 |
| Lifecycle | BR-010 → BR-011 |
| Search | BR-012 |
| Privacy & Security | BR-013 → BR-015 |
| Enterprise Governance | BR-016 → BR-020 |

---

# Architecture Notes

These business rules define the authoritative business behavior of the Patient Management Domain.

Implementation details may vary by technology, but the business behavior defined in this document shall remain consistent across all implementations.

---

# Change History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-22 | Initial Business Rules |

---

# Approval

| Role | Name | Status |
|------|------|--------|
| Solution Architect | Ahmed Saleh | Approved |

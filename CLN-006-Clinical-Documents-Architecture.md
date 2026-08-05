# CLN-006 — Clinical Documents Architecture Specification

**Document ID:** CLN-006
**Title:** Clinical Documents Architecture Specification
**Status:** Updated — Pending Product Owner Review
**Priority:** Critical
**Category:** Clinical Architecture
**Implementation Status:** Ready
**Owner:** Clinical Architecture Team
**Version:** 1.1.0
**Last Updated:** 2026-08-04
**Related ADR:** ADR-EMR-011 v1.0 — Encounter Closure, Electronic Signature, Read-Only Enforcement & Amendment

---

# Change Summary (v1.0.0 → v1.1.0)

Synchronized with the approved ADR-EMR-011. The **Electronic Signature** section is restated as a **technology-independent** capability (attest · integrity binding · verify · status · provenance-ready), signed documents are confirmed **read-only** with changes made only through a new signed version / Addendum, an **exceptional supervised reopen** and **superseded-signature** status are recorded, and co-signature is noted. No document workflow is redesigned and no rule beyond ADR-EMR-011 is introduced. All categories, metadata, and other sections are preserved.

---

# Purpose

This document defines the enterprise architecture for Clinical Documents within the LOUTAS Care Platform.

Clinical Documents represent the official medical record produced during patient care and are considered legal, clinical, and operational records.

This specification standardizes document creation, ownership, lifecycle, security, versioning, signatures, and interoperability.

---

# Scope

This specification applies to:

- Outpatient Clinics
- Specialty Clinics
- Multi-Branch Organizations

Applicable Modules

- EMR
- Clinical Encounter
- Laboratory
- Radiology
- Pharmacy
- Billing
- Audit
- Patient Portal (Future)

---

# Objectives

Clinical Documents shall:

- Standardize medical documentation.
- Preserve medico-legal integrity.
- Support structured and narrative content.
- Enable secure sharing.
- Support electronic signatures.
- Maintain version history.
- Integrate with clinical workflows.

---

# Enterprise Decision

## EA-007 — Clinical Documents are Enterprise Records

Every Clinical Document shall be treated as an enterprise record with lifecycle management, ownership, auditability, and legal retention.

---

# Definition

A Clinical Document is any formal medical document generated, imported, or finalized as part of patient care.

Clinical Documents become part of the patient's longitudinal medical record.

---

# Ownership

Patient

↓

Clinical Encounter

↓

Clinical Document

↓

Document Version

↓

Electronic Signature

Clinical Documents shall always belong to a Patient.

Most clinical documents shall also belong to a Clinical Encounter.

Some administrative documents may belong only to the Patient.

---

# Document Categories

## Clinical Notes

- SOAP Note
- Progress Note
- Follow-up Note
- Consultation Note

---

## Reports

- Laboratory Report
- Radiology Report
- Procedure Report
- Endoscopy Report
- Ultrasound Report

---

## Certificates

- Sick Leave Certificate
- Medical Fitness Certificate
- Return-to-Work Certificate

---

## Referral Documents

- Referral Letter
- Specialist Referral
- External Referral

---

## Consent Forms

- Procedure Consent
- Surgery Consent
- Data Sharing Consent

---

## Attachments

- PDF
- Images
- ECG
- External Reports
- Insurance Documents

---

# Document Lifecycle

Draft

↓

Under Review

↓

Finalized

↓

Electronically Signed

↓

Locked

↓

Archived

Possible Alternative States

Cancelled

Void

Superseded

---

## Draft

Document is editable.

---

## Under Review

Clinical review is in progress.

---

## Finalized

Content completed.

Waiting for signature if required.

---

## Electronically Signed

Author officially signs the document.

Legal integrity established.

---

## Locked

Document becomes read-only.

Post-signature changes are made only through a new signed version or Addendum; the original signed content is preserved *(ADR-EMR-011)*.

---

## Archived

Document retained according to retention policy.

---

# Document Metadata

Every Clinical Document shall contain:

Document ID

Patient

Encounter

Author

Organization

Branch

Department

Document Type

Creation Date

Last Updated

Status

Version

Language

Electronic Signature Status

Confidentiality Level

---

# Version Management

Every modification after finalization shall create a new document version where applicable.

Example

Version 1

↓

Version 2

↓

Version 3

The complete history shall remain auditable.

The original signed version shall never be overwritten or deleted *(ADR-EMR-011)*.

---

# Electronic Signature

*(Restated as technology-independent — ADR-EMR-011)*

The electronic signature is an abstract capability defined by a service contract; it is **not** tied to any specific signing technology. Signature mechanisms (for example password re-authentication, PKI / digital certificate, OTP, biometric, or a national e-signature gateway) are deployment choices and shall not be embedded in clinical logic.

The signature capability provides:

- **Attest** — bind signer identity, professional role, timestamp, document/version, and intent (sign · co-sign · addendum).
- **Integrity Binding** — a reference over the exact signed content, so later change is detectable.
- **Verify** — validate a signature and its binding at any future time.
- **Status** — valid | superseded | void, with the superseding reference where applicable.
- **Provenance** — mappable to FHIR Provenance / DocumentReference for future interoperability.

Supported Signatories

- Physician
- Consultant
- Authorized Healthcare Professional

Co-signature (e.g., supervising physician) shall be supported where organizational policy requires.

---

# Business Rules

## BR-001

Every Clinical Document belongs to a Patient.

---

## BR-002

Encounter-based documentation shall reference one Clinical Encounter.

---

## BR-003

Only authorized users may finalize documents.

---

## BR-004

Signed documents become read-only; changes are made only through a new signed version or Addendum. *(Updated — ADR-EMR-011)*

---

## BR-005

Document revisions shall preserve previous versions; the original signed version is never overwritten. *(Updated — ADR-EMR-011)*

---

## BR-006

Every document action shall be audited.

---

## BR-007

Templates shall not alter document integrity.

---

## BR-008

Generated PDFs shall faithfully represent the signed document.

---

## BR-009

Clinical Documents shall support multilingual content.

---

## BR-010

Document retention shall follow organizational and legal policies.

---

# Templates

Supported Template Types

- General Medicine
- Pediatrics
- Cardiology
- Orthopedics
- Dermatology
- Ophthalmology
- Dentistry
- Psychiatry
- Gastroenterology

Templates may define

Sections

Mandatory Fields

Clinical Checklists

Specialty Measurements

Default Instructions

---

# Integration

Clinical Encounter

↓

SOAP

↓

Clinical Orders

↓

Clinical Documents

↓

Clinical Timeline

↓

Patient History

↓

Billing

↓

Analytics

↓

Audit

---

# Security

Role-Based Access Control applies.

Access permissions depend on:

Role

Department

Organization

Branch

Document Confidentiality

Every access shall be logged.

---

# Audit Events

Document Created

Document Modified

Document Finalized

Document Signed

Document Locked

Document Signature Superseded

Document Addendum Created

Document Archived

Document Printed

Document Exported

Document Shared

Version Created

---

# AI Readiness

Clinical Documents support:

AI Draft Generation

Clinical Summarization

Referral Letter Generation

Patient Instructions

Discharge Summary Drafting

AI-generated content shall always require physician review before finalization.

---

# Future Extensions

FHIR DocumentReference

FHIR Provenance

Digital Signature Standards

National e-Signature Gateways

Patient Portal Document Sharing

Cross-Organization Document Exchange

---

# Related Documents

CLN-001 — Clinical Architecture Overview

CLN-002 — Clinical Encounter Specification

CLN-004 — SOAP Architecture Specification

CLN-005 — Clinical Orders Architecture Specification

CLN-007 — Clinical Timeline Architecture Specification

ADR-EMR-011 — Encounter Closure, Electronic Signature, Read-Only Enforcement & Amendment

ARCH-003 — Domain Architecture

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |
| 1.1.0 | 2026-08-04 | Synchronized with ADR-EMR-011: electronic signature restated as a technology-independent capability (attest · integrity binding · verify · status · provenance); signed documents read-only with amendment via new signed version / Addendum; superseded-signature status and co-signature recorded; BR-004/BR-005 updated; audit events extended. No workflow redesign. |

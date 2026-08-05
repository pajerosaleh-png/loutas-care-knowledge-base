# CLN-002 — Clinical Encounter Specification

**Document ID:** CLN-002
**Title:** Clinical Encounter Specification
**Status:** Updated — Pending Product Owner Review
**Version:** 1.1.0
**Owner:** Clinical Architecture
**Last Updated:** 2026-08-04
**Related ADR:** ADR-EMR-011 v1.0 — Encounter Closure, Electronic Signature, Read-Only Enforcement & Amendment

---

# Change Summary (v1.0.0 → v1.1.0)

Synchronized with the approved ADR-EMR-011. No workflow is redesigned and no rule beyond ADR-EMR-011 is introduced. Changes: the lifecycle read-only state is renamed from **Locked** to the canonical **Signed**; closure is defined as electronic signature; the amendment model is recorded as **addendum-default with an exceptional supervised reopen** (replacing an unqualified "unlock"); the **Visit Owner, Care Team, Clinical Contributors** and **Physician Reassignment** concepts are recorded; the read-only boundary and configurable closure policies are stated. All original sections and business-rule numbering are preserved.

---

# Purpose

This document defines the Clinical Encounter, the core business entity of the Clinical Domain.

The Clinical Encounter represents the official record of healthcare services delivered to a patient during a single episode of care.

Every clinical activity performed within LOUTAS Care shall belong to exactly one Clinical Encounter.

---

# Scope

This specification applies to:

- Outpatient Clinics
- Multi-Specialty Clinics
- Day Surgery Centers
- Future Telemedicine Encounters

---

# Business Purpose

The Clinical Encounter exists to:

- Record clinical care.
- Organize all medical documentation.
- Link healthcare professionals to clinical activities.
- Support legal medical documentation.
- Create a complete longitudinal medical history.

---

# Definition

A Clinical Encounter is a time-bound interaction between a patient and one or more healthcare providers for the purpose of delivering healthcare services.

The Encounter is the authoritative clinical container for all medical documentation generated during that interaction.

---

# Ownership

## Business Owner

Clinical Domain

---

## Primary Actor

Physician

---

## Supporting Actors

- Nurse
- Medical Assistant
- Reception (Limited)
- Administrator (Administrative Only)

---

## Visit Owner *(ADR-EMR-011)*

Each Encounter has exactly one **Visit Owner** — the responsible Provider (see BR-003) accountable for the Encounter and the only party who may complete and electronically sign it.

The Visit Owner is the single operational owner at any time.

---

## Care Team *(ADR-EMR-011)*

The **Care Team** is the set of providers and roles participating in the Encounter (for example the responsible Provider, nurse, and assisting staff).

Care Team membership is operational context; it does not transfer ownership.

---

## Clinical Contributors *(ADR-EMR-011)*

**Clinical Contributors** are Care Team members who author clinical content while the Encounter is **In Progress**.

Each contribution is attributed to its author.

Contributors may write; only the Visit Owner may complete and sign.

---

# Encounter Lifecycle

The official lifecycle is:

Draft

↓

Ready

↓

In Progress

↓

Completed

↓

Signed

↓

Archived

An Encounter ended before any clinical work may terminate as **Cancelled** *(ADR-EMR-011)*.

---

# Lifecycle Description

## Draft

Encounter created.

No clinical documentation yet.

---

## Ready

Patient has arrived.

Clinical staff may prepare the encounter.

---

## In Progress

Clinical documentation has started.

SOAP, diagnoses, orders and prescriptions become available.

This is the only state in which clinical content may be written.

---

## Completed

Clinical work has finished and completeness validation has passed.

The Encounter awaits electronic signature.

Before signature the Visit Owner may revert to **In Progress** for correction *(ADR-EMR-011)*.

---

## Signed *(ADR-EMR-011)*

The Encounter has been electronically signed by the Visit Owner and becomes **read-only**.

The signed Encounter is the legal clinical record.

(Previously named "Locked". Renamed to align with the approved electronic-signature closure model.)

---

## Archived

Historical record.

No modification allowed.

---

# Encounter Closure & Electronic Signature *(ADR-EMR-011)*

Closure is a deliberate two-step action inside the EMR and is never an appointment side-effect:

1. **Complete** — the Visit Owner marks the Encounter Completed after completeness validation (including at least one diagnosis with a designated primary and mandatory fields); the Encounter Summary is generated.
2. **Sign** — the Visit Owner applies an electronic signature, transitioning Completed → Signed and locking the record.

Electronic signature is mandatory: an unsigned Encounter is not closed and not read-only.

The electronic-signature capability is technology-independent and is governed by CLN-006.

---

# Amendment & Supervised Reopen *(ADR-EMR-011)*

- **Addendum (normal path).** A change after signature is a new, separately-signed Addendum that references the original signed Encounter; the original content is never overwritten or deleted, and the Encounter status remains **Signed**.
- **Supervised Reopen (exceptional).** For an erroneous closure only, a Supervisor may reopen the Encounter. Reopen requires a reason, is fully audited, supersedes (never deletes) the prior signature, and forces re-signature. It is not a routine editing path.

---

# Read-Only Boundary *(ADR-EMR-011)*

At signature, all Encounter-owned clinical content becomes read-only.

Independent domains continue under their own ownership after signature:

- Order fulfilment continues (CLN-005).
- Billing continues (BR-008; ARCH-003).
- The longitudinal Problem List (CLN-011) and patient-level records continue.

---

# Physician Reassignment *(ADR-EMR-011)*

The Visit Owner may be reassigned through an audited ownership-transfer:

- **Before signature** — reassignment transfers the responsible-Provider role and the duty to complete and sign; prior contributors retain their attribution.
- **After signature** — the signed authorship is immutable; reassignment then governs only ongoing care and addenda.

Every reassignment records actor, reason, and previous → new owner.

---

# Configurable Closure Policies *(ADR-EMR-011)*

Closure parameters are organization-configurable and are not hard-coded, including: whether a diagnosis / primary diagnosis is mandatory, mandatory specialty fields, whether a co-signature is required, whether and by whom a supervised reopen is permitted, and pre-signature revert.

Electronic signature and audit can never be disabled by configuration.

---

# Business Rules

## BR-001

Every Encounter belongs to exactly one Patient.

---

## BR-002

Every Encounter may originate from one Appointment.

Walk-in encounters are allowed.

---

## BR-003

Every Encounter must have one responsible Provider (the Visit Owner).

---

## BR-004

SOAP Notes belong only to an Encounter.

---

## BR-005

Diagnoses belong only to an Encounter.

---

## BR-006

Orders belong only to an Encounter.

---

## BR-007

Prescriptions belong only to an Encounter.

---

## BR-008

Billing shall consume billable events generated by the Encounter.

Billing never owns clinical documentation.

---

## BR-009

Clinical documentation shall not exist outside an Encounter.

---

## BR-010

A signed Encounter is read-only. Modification after signature shall occur only through a signed Addendum, or, for an erroneous closure, through an exceptional supervised reopen that supersedes the prior signature. *(Updated — ADR-EMR-011)*

---

## BR-011 *(Synchronized — ADR-EMR-011)*

Only the Visit Owner may complete and electronically sign the Encounter; a supervising provider may co-sign where organizational policy requires.

---

## BR-012 *(Synchronized — ADR-EMR-011)*

The original signed record shall never be overwritten or deleted; addenda are preserved alongside the original.

---

## BR-013 *(Synchronized — ADR-EMR-011)*

Visit Owner reassignment shall be audited; after signature, signed authorship is immutable.

---

## BR-014 *(Synchronized — ADR-EMR-011)*

Closure preconditions and co-signature/reopen permissions are organization-configurable; electronic signature and audit shall never be disabled by configuration.

---

# Encounter Owns

The Encounter owns:

- SOAP Notes
- Vital Signs
- Diagnoses
- Procedures
- Prescriptions
- Laboratory Orders
- Radiology Orders
- Clinical Attachments
- Care Plans
- Follow-Up Plans
- Clinical Tasks

---

# Relationships

Clinical Encounter relates to:

Patient

Appointment

Provider (Visit Owner)

Care Team

Department

Branch

Clinical Documents

Billing Events

Audit Logs

---

# Security

Reception

- Create Encounter
- View Status

Nurse

- Record Vitals
- Nursing Notes

Physician

- Full Clinical Access
- Complete & Sign (when Visit Owner)

Administrator

- Administrative Management (Read-Only clinical)

Supervisor

- Co-signature
- Supervised Reopen (exceptional)
- Clinical Audit

---

# Audit Requirements

The following events shall be audited:

Encounter Created

Encounter Started

Encounter Completed

Encounter Signed

Encounter Reopened (Supervised)

Signature Superseded

Addendum Created

Owner Reassigned

Diagnosis Added

Prescription Created

Order Submitted

Document Signed

---

# Future Extensions

The architecture shall support:

- Telemedicine
- Multi-provider Encounter
- AI Documentation Assistant
- Clinical Coding
- Voice Dictation
- Clinical Templates
- Digital Consent
- Remote Monitoring

---

# Implementation Notes

The Clinical Encounter shall act as the Aggregate Root of the Clinical Domain.

All child clinical entities shall reference the Encounter.

No clinical entity shall exist independently.

---

# Enterprise Decisions

EA-002

Clinical information belongs to the Encounter.

EA-004

Clinical Encounter is the authoritative source of all clinical activities.

---

# Related Documents

CLN-001 — Clinical Architecture Overview

CLN-003 — Patient Journey

CLN-004 — SOAP Architecture

CLN-006 — Clinical Documents Architecture

CLN-007 — Clinical Timeline Architecture

CLN-014 — Clinical Task Architecture

ADR-EMR-011 — Encounter Closure, Electronic Signature, Read-Only Enforcement & Amendment

ARCH-003 — Domain Architecture

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial release |
| 1.1.0 | 2026-08-04 | Synchronized with ADR-EMR-011: lifecycle read-only state renamed Locked → Signed; closure defined as electronic signature; addendum-default with exceptional supervised reopen; Visit Owner / Care Team / Clinical Contributors and Physician Reassignment recorded; read-only boundary and configurable closure policies stated; BR-010 updated; BR-011–BR-014 added (synchronized); audit and security updated. No workflow redesign. |

# CLN-004 — SOAP Architecture Specification

**Document ID:** CLN-004
**Title:** SOAP Architecture Specification
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

Synchronized with the approved ADR-EMR-011. The SOAP lifecycle read-only state is renamed from **Locked** to **Signed** to match the Encounter's electronic-signature closure; SOAP content becomes read-only when the **Encounter** is signed, and post-signature changes occur only through a signed Addendum. Business rules BR-003/BR-004 are clarified accordingly. No documentation workflow is redesigned and no rule beyond ADR-EMR-011 is introduced. All SOAP components and other sections are preserved.

---

# Purpose

This document defines the enterprise architecture for SOAP documentation within LOUTAS Care.

SOAP is the standard clinical documentation model used during every Clinical Encounter and serves as the authoritative source of medical documentation.

This specification standardizes how clinical information is captured, stored, validated, secured, and extended across all specialties.

---

# Scope

This specification applies to:

- Outpatient Clinics
- Specialty Clinics
- Telemedicine (Future)
- Emergency Walk-In (Future)
- Mobile EMR (Future)

Applicable Modules

- Clinical Encounter
- EMR
- Clinical Templates
- Clinical Timeline
- Clinical Decision Support
- AI Documentation
- Billing
- Audit

---

# Objectives

The SOAP architecture shall:

- Standardize clinical documentation.
- Reduce physician documentation time.
- Improve documentation quality.
- Support structured medical data.
- Support narrative clinical notes.
- Enable AI-assisted documentation.
- Enable specialty-specific templates.
- Maintain medico-legal compliance.

---

# Enterprise Decision

## EA-005 — Hybrid Clinical Documentation Model

LOUTAS Care shall support:

- Structured Clinical Data

AND

- Narrative Clinical Documentation

Structured data shall be preferred whenever clinically appropriate.

Narrative text shall be available whenever physician judgment requires additional explanation.

---

# SOAP Ownership

SOAP belongs exclusively to one Clinical Encounter.

Relationships

Patient
    ↓
Encounter
    ↓
SOAP

SOAP shall never exist independently from a Clinical Encounter.

---

# SOAP Lifecycle

Draft

↓

In Progress

↓

Completed

↓

Signed

↓

Archived

SOAP follows the Encounter lifecycle. SOAP becomes read-only when the Encounter is **Signed** *(ADR-EMR-011)*.

---

## Draft

SOAP has been created but no clinical documentation has been entered.

---

## In Progress

The provider is documenting the clinical encounter.

Updates are allowed.

---

## Completed

The provider confirms clinical documentation is complete.

Clinical decisions become available for downstream workflows.

---

## Signed *(ADR-EMR-011)*

The Encounter has been electronically signed; SOAP becomes read-only.

Post-signature changes occur only through a signed Addendum; the original SOAP content is preserved.

(Previously named "Locked". Renamed to align with the Encounter electronic-signature model.)

---

## Archived

Encounter is retained for legal and historical purposes.

---

# SOAP Components

## S — Subjective

Purpose

Capture information reported by the patient.

Typical Elements

- Chief Complaint
- History of Present Illness
- Review of Systems
- Past Medical History
- Surgical History
- Family History
- Social History
- Allergies
- Current Medications

Data Type

Hybrid

Structured fields

+

Narrative Notes

---

## O — Objective

Purpose

Document measurable clinical findings.

Examples

Vital Signs

Height

Weight

BMI

Temperature

Blood Pressure

Pulse

Respiratory Rate

Oxygen Saturation

Physical Examination

Clinical Measurements

Attachments

Images

ECG

Laboratory Results

Radiology Results

Data Type

Primarily Structured

---

## A — Assessment

Purpose

Clinical interpretation.

Possible Elements

Primary Diagnosis

Secondary Diagnoses

Differential Diagnosis

Clinical Impression

Problem List

Risk Assessment

Coding References

ICD-10

SNOMED CT (Future)

---

## P — Plan

Purpose

Document future management.

Examples

Medication Orders

Laboratory Orders

Radiology Orders

Procedures

Patient Education

Lifestyle Advice

Follow-Up

Referral

Care Plan

---

# Business Rules

## BR-001

SOAP documentation requires an active Clinical Encounter.

---

## BR-002

Only authorized providers may complete SOAP documentation.

---

## BR-003

SOAP is editable only while the Encounter is In Progress; upon Encounter signature SOAP becomes read-only. *(Updated — ADR-EMR-011)*

---

## BR-004

A signed Encounter's SOAP is amended only through a signed Addendum; the original is preserved. Reopening a signed Encounter is an exceptional supervised action (CLN-002). *(Updated — ADR-EMR-011)*

---

## BR-005

Every modification shall be audited.

---

## BR-006

Each SOAP belongs to one Encounter only.

---

## BR-007

Clinical Orders shall originate from the Plan section.

---

## BR-008

Diagnoses shall originate from the Assessment section.

---

## BR-009

Structured clinical fields shall be preferred whenever available.

---

## BR-010

Narrative documentation shall remain available for physician discretion.

---

# Template Architecture

SOAP supports reusable templates.

Template Types

- General Medicine
- Internal Medicine
- Pediatrics
- Cardiology
- Orthopedics
- Dermatology
- ENT
- Ophthalmology
- Dentistry
- Psychiatry

Templates may define:

Visible Sections

Required Fields

Default Values

Clinical Checklists

Custom Measurements

---

# AI Readiness

SOAP architecture supports AI-assisted workflows.

Examples

Clinical Summary

Suggested Diagnoses

Coding Suggestions

Clinical Documentation Assistance

Visit Summary

Patient Instructions

AI shall never finalize documentation automatically.

Final approval always belongs to the treating physician.

---

# Integration

SOAP integrates with:

Clinical Encounter

↓

Diagnosis

↓

Clinical Orders

↓

Laboratory

↓

Radiology

↓

Pharmacy

↓

Billing

↓

Clinical Timeline

↓

Analytics

---

# Security

Only authorized clinical users may access SOAP.

Permissions shall be role-based.

Sensitive documentation shall follow clinic privacy policies.

Every access shall be logged.

---

# Audit Events

SOAP Created

SOAP Modified

SOAP Completed

SOAP Read-Only (Encounter Signed)

SOAP Addendum Added

Encounter Reopened (Supervised)

Diagnosis Added

Diagnosis Modified

Medication Added

Order Created

Template Applied

AI Suggestion Accepted

AI Suggestion Rejected

---

# Quality Indicators

Average Documentation Time

SOAP Completion Rate

Incomplete Encounter Rate

Template Usage Rate

Clinical Coding Completeness

AI Assistance Adoption

---

# Future Extensions

Voice Recognition

Medical Dictation

AI Ambient Documentation

Clinical Decision Support

FHIR Clinical Notes

Remote Documentation

Offline Documentation

---

# Related Documents

CLN-001 — Clinical Architecture Overview

CLN-002 — Clinical Encounter Specification

CLN-003 — Patient Journey Specification

CLN-005 — Clinical Orders Architecture

CLN-006 — Clinical Documents Architecture

ADR-EMR-011 — Encounter Closure, Electronic Signature, Read-Only Enforcement & Amendment

ARCH-003 — Domain Architecture

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |
| 1.1.0 | 2026-08-04 | Synchronized with ADR-EMR-011: SOAP lifecycle state Locked → Signed; SOAP becomes read-only at Encounter signature; post-signature amendment via signed Addendum; BR-003/BR-004 clarified; audit events updated. No workflow redesign. |

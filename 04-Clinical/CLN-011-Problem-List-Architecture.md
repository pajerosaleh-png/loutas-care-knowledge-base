# CLN-011 — Problem List Architecture Specification

**Document ID:** CLN-011
**Title:** Problem List Architecture Specification
**Status:** Approved
**Priority:** Critical
**Category:** Clinical Architecture
**Implementation Status:** Ready
**Owner:** Clinical Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the enterprise architecture for the Problem List within the LOUTAS Care Platform.

The Problem List is the authoritative longitudinal record of a patient's active and historical health problems.

Unlike encounter diagnoses, the Problem List persists across multiple encounters and serves as the foundation for long-term clinical management.

---

# Scope

Applicable Modules

- Patient
- Clinical Encounter
- SOAP
- Care Plan
- Clinical Orders
- Follow-Up
- Clinical Timeline
- Clinical Decision Support
- Pharmacy
- Laboratory
- Radiology
- Analytics

---

# Objectives

The Problem List shall:

- Maintain a longitudinal record of health problems.
- Support continuity of care.
- Improve multidisciplinary collaboration.
- Enable clinical decision support.
- Support population health reporting.
- Reduce duplicated documentation.
- Improve chronic disease management.

---

# Enterprise Decision

## EA-012 — Longitudinal Problem Management

The Problem List is the authoritative source of ongoing clinical problems.

Encounter diagnoses document today's visit.

The Problem List documents the patient's continuing health conditions.

A diagnosis may exist without becoming a Problem List item.

A Problem List item may remain active across many encounters.

---

# Definition

A Problem is any clinically significant condition, diagnosis, symptom, syndrome, chronic disease, or health issue requiring monitoring, treatment, or follow-up.

The Problem List represents the patient's continuous clinical history.

---

# Encounter Diagnosis vs Problem List

| Encounter Diagnosis | Problem List |
|---------------------|--------------|
| Visit-specific | Longitudinal |
| May be temporary | Persistent |
| Documents today's assessment | Documents ongoing health status |
| May never appear again | May remain for years |
| Linked to one encounter | Linked to multiple encounters |

Example

Encounter Diagnosis

- Acute Sinusitis

Problem List

- Diabetes Mellitus Type 2
- Hypertension
- Chronic Kidney Disease
- Obesity

---

# Problem Categories

Chronic Disease

Acute Condition

Resolved Condition

Past Medical History

Family History (Reference)

Behavioral Health

Pregnancy-related

Functional Problem

Lifestyle Risk

Other Clinical Conditions

---

# Problem Lifecycle

Identified

↓

Confirmed

↓

Active

↓

Monitored

↓

Resolved

↓

Inactive

Alternative States

Entered in Error

Merged

Archived

---

## Identified

Clinical suspicion exists.

---

## Confirmed

Diagnosis confirmed.

---

## Active

Requires ongoing management.

---

## Monitored

Condition remains stable and under review.

---

## Resolved

No active treatment required.

---

## Inactive

Historical record retained.

---

# Problem Attributes

Each Problem shall contain:

Problem Identifier

Patient

Clinical Name

Standard Code (ICD-10 / SNOMED CT)

Category

Severity

Clinical Status

Verification Status

Onset Date

Resolution Date

Responsible Provider

Associated Encounter

Associated Care Plan

Associated Medications

Associated Orders

Notes

Version

---

# Clinical Status

Active

Inactive

Resolved

Recurrence

Unknown

---

# Verification Status

Suspected

Confirmed

Refuted

Entered in Error

---

# Severity

Mild

Moderate

Severe

Life Threatening

Unknown

---

# Relationships

Patient

↓

Problem List

↓

Care Plan

↓

Encounter

↓

Orders

↓

Medications

↓

Laboratory

↓

Radiology

↓

Timeline

↓

Follow-Up

↓

Clinical Decision Support

---

# Business Rules

## BR-001

Each Problem belongs to one Patient.

---

## BR-002

A Patient may have multiple Problems.

---

## BR-003

Problems may span multiple encounters.

---

## BR-004

Resolved Problems shall remain historically visible.

---

## BR-005

Deleting Problems is prohibited.

Entered-in-error workflow shall be used instead.

---

## BR-006

Problems may be linked to Care Plans.

---

## BR-007

Problems may trigger Clinical Decision Support rules.

---

## BR-008

Problem modifications shall be fully audited.

---

## BR-009

One encounter may update multiple Problems.

---

## BR-010

Problem coding should support international coding standards.

---

# Integration

Patient

↓

Problem List

↓

Encounter

↓

SOAP

↓

Care Plan

↓

Orders

↓

Laboratory

↓

Radiology

↓

Timeline

↓

Follow-Up

↓

Clinical Decision Support

↓

Analytics

---

# Security

Only authorized clinical users may create or modify Problems.

Historical integrity shall be preserved.

Administrative users cannot modify clinical content.

---

# Audit Events

Problem Created

Problem Updated

Problem Confirmed

Problem Resolved

Problem Inactivated

Problem Reopened

Problem Merged

Problem Marked as Error

---

# Quality Indicators

Active Problem Accuracy

Problem Coding Completeness

Chronic Disease Registry Coverage

Problem Resolution Rate

Duplicate Problem Rate

Clinical Documentation Quality

---

# AI Readiness

Future AI capabilities

Suggested Problems

Duplicate Problem Detection

Problem Severity Prediction

Coding Assistance

Automatic Problem Extraction from Clinical Notes

Clinical Relationship Discovery

AI-generated suggestions require physician approval.

---

# Future Extensions

FHIR Condition Resource

SNOMED CT Integration

ICD-11 Support

National Disease Registries

Population Health Dashboards

Clinical Quality Programs

---

# Implementation Impact

## Frontend Impact

Dedicated Problem List panel in the EMR.

Quick Add / Edit workflow.

Problem timeline view.

Status and severity indicators.

Filters for Active / Resolved problems.

---

## Backend Impact

Problem List Service.

Coding Service.

Clinical Relationship Engine.

Audit Integration.

Decision Support Integration.

---

## Database Impact (Conceptual)

Patient

↓

Problem

↓

Encounter

↓

Care Plan

↓

Medication

↓

Order

↓

Audit

---

## API Impact

Create Problem

Update Problem

Resolve Problem

Reopen Problem

Merge Problems

Retrieve Active Problems

Retrieve Problem History

---

## RBAC Impact

Physician

Create / Update / Resolve

Nurse

View / Add observations (configurable)

Clinical Administrator

Coding configuration only

System Administrator

Configuration without editing clinical content

---

# Related Documents

CLN-002 — Clinical Encounter Specification

CLN-004 — SOAP Architecture Specification

CLN-005 — Clinical Orders Architecture

CLN-007 — Clinical Timeline Architecture

CLN-008 — Care Plan Architecture

CLN-009 — Follow-Up Architecture

CLN-010 — Clinical Decision Support Architecture

ARCH-003 — Domain Architecture

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

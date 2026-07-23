# RAD-004 — Image Acquisition Workflow

**Document ID:** RAD-004
**Title:** Image Acquisition Workflow
**Status:** Approved
**Priority:** Critical
**Category:** Radiology Domain
**Implementation Status:** Ready
**Owner:** Enterprise Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the enterprise workflow for Image Acquisition within the LOUTAS Care Platform.

Image Acquisition is the operational process through which a scheduled Imaging Study is performed, validated, and prepared for radiologist interpretation while ensuring patient safety, image quality, and workflow traceability.

---

# Scope

Applies to:

- X-Ray
- Ultrasound
- CT
- MRI
- Mammography
- Fluoroscopy
- Future Imaging Modalities

Includes:

- Patient arrival
- Patient verification
- Preparation
- Acquisition
- Image quality review
- Repeat acquisition
- Study completion

---

# Objectives

The Image Acquisition Workflow shall:

- Ensure positive patient identification.
- Ensure correct study execution.
- Maintain complete acquisition history.
- Support image quality review.
- Allow controlled repeat acquisitions.
- Prepare studies for interpretation.

---

# Enterprise Decision

## EA-038 — Positive Patient Identification Before Acquisition

No imaging acquisition shall begin until patient identity, scheduled study, and requested procedure have been successfully verified.

---

# Workflow Overview

Scheduled Study

↓

Patient Arrival

↓

Patient Identification

↓

Procedure Verification

↓

Preparation Completed

↓

Equipment Ready

↓

Acquisition Started

↓

Images Acquired

↓

Image Quality Review

↓

Repeat Required?

├── Yes → Repeat Acquisition
└── No

↓

Study Completed

↓

Available for Interpretation

---

# Acquisition Stages

## Stage 1 — Patient Arrival

Responsibilities:

- Confirm appointment
- Verify demographics
- Confirm scheduled study

---

## Stage 2 — Patient Identification

Verification shall include:

Patient

Study

Body Region

Laterality

Modality

Organization

Branch

---

## Stage 3 — Preparation

Preparation may include:

Contrast preparation

Patient positioning

Protective equipment

Removal of metal objects

Breath-hold instructions

Sedation (Future)

Consent verification

Laboratory prerequisite verification

---

## Stage 4 — Equipment Preparation

Technologist shall verify:

Analyzer/Modality availability

Calibration status

Quality status

Protocol availability

Worklist synchronization

---

## Stage 5 — Image Acquisition

Supports:

Single image

Multi-image acquisition

Multi-series acquisition

Dynamic imaging

Contrast phases

Real-time imaging

---

## Stage 6 — Image Quality Review

Review criteria include:

Patient positioning

Image completeness

Motion artifacts

Exposure quality

Coverage

Technical adequacy

Only acceptable studies proceed to interpretation.

---

## Stage 7 — Repeat Acquisition

Repeat acquisition may occur due to:

Motion artifacts

Incorrect positioning

Incomplete anatomy

Equipment issue

Protocol adjustment

Patient movement

Each repeat shall be documented.

---

## Stage 8 — Study Completion

Completion includes:

Finalize acquisition

Close acquisition session

Update study status

Notify reporting queue

Synchronize with PACS

---

# Acquisition Status

Scheduled

Patient Arrived

Verified

Preparation Pending

Ready

Acquiring

Quality Review

Repeat Required

Completed

Available for Interpretation

Cancelled

---

# Business Rules

## BR-001

Patient identification is mandatory before acquisition.

---

## BR-002

Only scheduled and validated studies may enter acquisition.

---

## BR-003

Repeat acquisitions shall preserve previous images.

---

## BR-004

Quality review must be completed before interpretation.

---

## BR-005

Every acquisition shall record the responsible technologist.

---

## BR-006

Study completion automatically updates the reporting worklist.

---

## BR-007

All acquisition activities shall be audit logged.

---

# Roles and Responsibilities

## Receptionist

Confirm patient arrival.

Verify appointment.

---

## Radiology Technologist

Verify patient identity.

Prepare patient.

Perform acquisition.

Review technical quality.

Document repeats.

---

## Radiologist

Review completed studies.

Request repeat acquisition if clinically justified.

Interpret finalized studies.

---

## Department Supervisor

Monitor acquisition workflow.

Review quality issues.

Approve exceptional cases.

---

# Audit Events

Patient Arrived

Patient Verified

Preparation Completed

Equipment Ready

Acquisition Started

Image Captured

Repeat Acquisition

Quality Review Completed

Study Completed

Study Cancelled

---

# Security

Image Acquisition shall enforce:

Role-Based Access Control

Organization Isolation

Branch Isolation

Complete Audit Trail

Immutable Acquisition History

---

# AI Readiness

Future AI capabilities:

Automatic positioning guidance

Image quality scoring

Motion artifact detection

Protocol optimization

Repeat prediction

Workflow optimization

AI recommendations require technologist validation.

---

# Future Extensions

Automated patient positioning

Robotic imaging assistance

Real-time acquisition optimization

Voice-guided workflow

Wearable device integration

Remote acquisition supervision

---

# Implementation Impact

## Frontend Impact

Acquisition worklist

Patient verification screen

Preparation checklist

Acquisition console

Quality review panel

Repeat acquisition dialog

---

## Backend Impact

Acquisition Service

Quality Review Service

Workflow Engine

Notification Service

Audit Service

---

## Database Impact (Conceptual)

Radiology Order

↓

Imaging Study

↓

Acquisition Session

↓

Image Series

↓

Quality Review

↓

Audit

---

## API Impact

Start Acquisition

Complete Acquisition

Record Repeat

Submit Quality Review

Finalize Study

Retrieve Acquisition History

---

## RBAC Impact

Receptionist

Patient arrival management

Radiology Technologist

Acquisition workflow management

Radiologist

Interpret completed studies

Department Supervisor

Workflow monitoring and quality oversight

Administrator

Workflow configuration

---

# Related Documents

RAD-002 — Radiology Order Architecture

RAD-003 — Imaging Study Architecture

RAD-005 — PACS & DICOM Integration

RAD-006 — Radiology Reporting Architecture

ARCH-004 — Shared Clinical Services

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

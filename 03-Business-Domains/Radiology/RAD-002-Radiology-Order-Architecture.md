# RAD-002 — Radiology Order Architecture

**Document ID:** RAD-002
**Title:** Radiology Order Architecture
**Status:** Approved
**Priority:** Critical
**Category:** Radiology Domain
**Implementation Status:** Ready
**Owner:** Enterprise Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the enterprise architecture for Radiology Orders within the LOUTAS Care Platform.

A Radiology Order represents the formal clinical request for one or more imaging procedures. It extends the enterprise Clinical Order architecture while introducing imaging-specific workflows, preparation requirements, modality selection, and scheduling readiness.

---

# Scope

Applies to:

- Diagnostic Imaging Orders
- Screening Imaging Orders
- Follow-up Imaging
- Multi-study Orders
- Emergency Imaging
- Future Interventional Radiology Orders

---

# Objectives

The Radiology Order Architecture shall:

- Extend Clinical Orders.
- Support multiple imaging studies per order.
- Enable scheduling workflows.
- Capture preparation instructions.
- Support imaging protocols.
- Integrate with PACS and modality worklists.
- Maintain complete auditability.

---

# Enterprise Decision

## EA-034 — Radiology Orders Extend Clinical Orders

Radiology Orders shall be implemented as a specialization of the enterprise Clinical Order.

No duplicate order model shall exist.

---

# Order Components

Every Radiology Order shall contain:

Order Identifier

Clinical Order Reference

Patient Reference

Encounter Reference

Ordering Provider

Organization

Branch

Priority

Clinical Indication

Requested Modality

Requested Study

Body Region

Laterality (if applicable)

Contrast Requirement

Preparation Requirement

Requested Date & Time

Status

Version

---

# Supported Modalities

The architecture supports:

- X-Ray
- Ultrasound
- CT
- MRI
- Mammography
- Fluoroscopy
- Dental Imaging
- Bone Densitometry
- Future Imaging Modalities

---

# Order Lifecycle

Clinical Request

↓

Radiology Order Created

↓

Clinical Validation

↓

Scheduling Ready

↓

Appointment Scheduled

↓

Patient Preparation

↓

Image Acquisition

↓

Reporting

↓

Verification

↓

Completed

Alternative Path

Cancelled

Expired

Rejected

---

# Order Status

Draft

Requested

Validated

Scheduling Pending

Scheduled

Patient Arrived

Preparation Pending

Ready for Acquisition

In Progress

Reporting

Verified

Completed

Cancelled

Rejected

Expired

---

# Clinical Information

A Radiology Order may include:

Clinical indication

Diagnosis

Symptoms

Medical history

Previous imaging references

Pregnancy status (where applicable)

Allergy information

Contrast contraindications

Special instructions

---

# Preparation Requirements

Supports configurable preparation including:

Fasting

Hydration

Contrast preparation

Medication adjustment

Full bladder

Empty bladder

Laboratory prerequisites

Consent required

Each preparation rule shall be configurable by imaging study.

---

# Contrast Management

Supports:

Contrast Required

Contrast Optional

Non-Contrast Study

Contrast Type

Contrast Route

Contrast Status

Future contrast inventory integration

---

# Priority Levels

Routine

Urgent

STAT

Emergency

Priority shall influence scheduling and worklist ordering.

---

# Business Rules

## BR-001

Every Radiology Order shall reference one Clinical Order.

---

## BR-002

Orders requiring preparation shall not proceed until preparation is complete.

---

## BR-003

Cancelled orders shall remain available for audit.

---

## BR-004

Multiple imaging studies may belong to a single Radiology Order.

---

## BR-005

Order modifications shall create a new version while preserving history.

---

## BR-006

Priority shall determine scheduling precedence.

---

## BR-007

Radiology Orders shall be available to modality worklists only after validation.

---

# Roles and Responsibilities

## Ordering Physician

Create imaging orders.

Provide clinical indication.

Set priority.

---

## Receptionist

Schedule imaging appointments.

Verify patient information.

---

## Radiology Technician

Review preparation status.

Perform acquisition.

---

## Radiologist

Review clinical indication.

Interpret studies.

Verify reports.

---

## Administrator

Configure:

Modalities

Preparation rules

Priority rules

Protocols

---

# Audit Events

Order Created

Order Updated

Order Validated

Order Scheduled

Preparation Completed

Order Cancelled

Order Expired

Order Completed

---

# Security

Radiology Orders shall enforce:

Role-Based Access Control

Organization Isolation

Branch Isolation

Audit Logging

Version History

---

# AI Readiness

Future AI capabilities

Protocol recommendation

Preparation validation

Priority optimization

Duplicate order detection

Clinical appropriateness review

Workflow optimization

AI recommendations require clinician approval.

---

# Future Extensions

Clinical decision support integration

Electronic consent

Automatic protocol assignment

Insurance pre-authorization

National imaging exchange

---

# Implementation Impact

## Frontend Impact

Radiology order form

Preparation checklist

Scheduling readiness panel

Priority indicators

Order history

---

## Backend Impact

Radiology Order Service

Preparation Service

Validation Service

Scheduling Integration Service

---

## Database Impact (Conceptual)

Clinical Order

↓

Radiology Order

↓

Imaging Study

↓

Appointment

↓

Radiology Report

↓

Audit

---

## API Impact

Create Radiology Order

Update Radiology Order

Validate Order

Cancel Order

Retrieve Order

Retrieve Preparation Status

---

## RBAC Impact

Ordering Physician

Create and update orders

Receptionist

Schedule validated orders

Radiology Technician

View assigned orders

Radiologist

Interpret completed studies

Administrator

Configure radiology workflows

---

# Related Documents

RAD-001 — Radiology Architecture Overview

RAD-003 — Imaging Study Architecture

CLN-002 — Clinical Orders

CLN-007 — Clinical Timeline

ARCH-004 — Shared Clinical Services

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

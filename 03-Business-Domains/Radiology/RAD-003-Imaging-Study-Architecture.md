# RAD-003 — Imaging Study Architecture

**Document ID:** RAD-003
**Title:** Imaging Study Architecture
**Status:** Approved
**Priority:** Critical
**Category:** Radiology Domain
**Implementation Status:** Ready
**Owner:** Enterprise Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the enterprise architecture for Imaging Studies within the LOUTAS Care Platform.

An Imaging Study represents the complete diagnostic imaging examination performed for a patient. It serves as the central business entity linking Radiology Orders, image acquisition, reporting, and enterprise clinical services.

The Imaging Study is independent of any specific imaging vendor or storage technology.

---

# Scope

Applies to:

- Diagnostic Imaging Studies
- Multi-Series Studies
- Multi-Modality Studies
- Follow-up Studies
- Comparison Studies
- Future AI-assisted Imaging Studies

---

# Objectives

The Imaging Study Architecture shall:

- Represent the complete imaging examination.
- Support one or more image series.
- Support multiple imaging instances.
- Maintain complete traceability.
- Support enterprise interoperability.
- Remain independent of DICOM implementation details.

---

# Enterprise Decision

## EA-036 — Imaging Study is the Core Radiology Business Entity

The Imaging Study shall be the primary business entity within the Radiology Domain.

Image files, DICOM metadata, and storage mechanisms are implementation details attached to the Imaging Study.

---

# Imaging Study Components

Every Imaging Study shall contain:

Study Identifier

Radiology Order Reference

Clinical Order Reference

Patient Reference

Encounter Reference

Organization

Branch

Modality

Study Description

Body Region

Laterality (if applicable)

Priority

Acquisition Date & Time

Performing Technician

Interpreting Radiologist

Study Status

Version

---

# Imaging Hierarchy

Radiology Order

↓

Imaging Study

↓

Imaging Series

↓

Imaging Instance

---

# Imaging Series

Each Imaging Study may contain one or more Imaging Series.

Examples:

Scout Images

Axial Series

Coronal Reconstruction

Sagittal Reconstruction

Contrast Phase

Post-processing Series

Series belong exclusively to one Imaging Study.

---

# Imaging Instances

Each Imaging Series contains one or more Imaging Instances.

Examples:

Single X-ray image

CT Slice

MRI Slice

Ultrasound Frame

Mammography Image

Future 3D Objects

Instances are immutable once acquired.

---

# Study Lifecycle

Scheduled

↓

Patient Arrived

↓

Preparation Completed

↓

Acquisition Started

↓

Acquisition Completed

↓

Quality Review

↓

Available for Interpretation

↓

Reporting

↓

Verified

↓

Published

Alternative Path

Cancelled

Repeated

Archived

---

# Study Status

Scheduled

Patient Arrived

Preparation Pending

Ready

Acquiring

Acquired

Quality Review

Reporting

Verified

Published

Cancelled

Repeated

Archived

---

# Metadata

Supports:

Study Description

Clinical Indication

Procedure Code

Modality

Body Region

Laterality

Priority

Acquisition Duration

Contrast Usage

Equipment Identifier

Location

---

# Business Rules

## BR-001

Every Imaging Study shall belong to exactly one Radiology Order.

---

## BR-002

An Imaging Study may contain multiple Imaging Series.

---

## BR-003

Each Imaging Series may contain multiple Imaging Instances.

---

## BR-004

Published studies shall remain immutable.

---

## BR-005

Repeated studies shall reference the original study.

---

## BR-006

Study history shall preserve all previous versions.

---

## BR-007

Quality review must be completed before interpretation.

---

# Roles and Responsibilities

## Radiology Technician

Perform acquisition.

Complete study.

Document acquisition issues.

---

## Radiologist

Interpret study.

Request repeat if required.

Verify findings.

---

## Department Supervisor

Review study quality.

Monitor workflow.

Approve exceptional cases.

---

## Administrator

Configure:

Modalities

Study templates

Workflow rules

Retention policies

---

# Audit Events

Study Created

Study Updated

Acquisition Started

Acquisition Completed

Quality Review Completed

Study Repeated

Study Verified

Study Published

Study Archived

---

# Security

Imaging Studies shall enforce:

Role-Based Access Control

Organization Isolation

Branch Isolation

Immutable Published Studies

Complete Audit Trail

---

# AI Readiness

Future AI capabilities:

Automatic anatomy detection

Image quality assessment

Series classification

Duplicate study detection

Workflow prioritization

Study completeness validation

AI-assisted interpretation support

AI outputs require radiologist validation.

---

# Future Extensions

3D Reconstruction

Structured Imaging Studies

Multi-site Imaging

Cloud Imaging Repository

Enterprise Imaging Exchange

Digital Pathology Integration

---

# Implementation Impact

## Frontend Impact

Study worklist

Study details

Series browser

Study timeline

Quality review workspace

---

## Backend Impact

Imaging Study Service

Series Management Service

Study Validation Service

Quality Review Service

---

## Database Impact (Conceptual)

Radiology Order

↓

Imaging Study

↓

Imaging Series

↓

Imaging Instance

↓

Radiology Report

↓

Audit

---

## API Impact

Create Study

Update Study

Retrieve Study

Retrieve Series

Retrieve Instances

Publish Study

Archive Study

---

## RBAC Impact

Radiology Technician

Manage acquisition workflow

Radiologist

Interpret and verify studies

Supervisor

Quality oversight

Administrator

Configure imaging services

---

# Related Documents

RAD-001 — Radiology Architecture Overview

RAD-002 — Radiology Order Architecture

RAD-004 — Image Acquisition Workflow

RAD-005 — PACS & DICOM Integration

ARCH-004 — Shared Clinical Services

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

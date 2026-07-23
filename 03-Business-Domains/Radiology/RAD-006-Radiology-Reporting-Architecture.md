# RAD-006 — Radiology Reporting Architecture

**Document ID:** RAD-006
**Title:** Radiology Reporting Architecture
**Status:** Approved
**Priority:** Critical
**Category:** Radiology Domain
**Implementation Status:** Ready
**Owner:** Enterprise Radiology Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the enterprise architecture for Radiology Reporting within the LOUTAS Care Platform.

A Radiology Report represents the verified clinical interpretation of an Imaging Study and serves as the authoritative diagnostic document delivered to clinicians and patients.

The architecture supports structured reporting, narrative reporting, version control, report verification, and enterprise interoperability.

---

# Scope

Applies to:

- Diagnostic Reports
- Screening Reports
- Follow-up Reports
- Preliminary Reports
- Final Reports
- Corrected Reports
- Future AI-assisted Reports

---

# Objectives

The Radiology Reporting Architecture shall:

- Produce standardized clinical reports.
- Support structured and narrative reporting.
- Preserve complete report history.
- Support multi-stage verification.
- Enable interoperability.
- Maintain full auditability.

---

# Enterprise Decision

## EA-042 — One Verified Report Per Imaging Study

Each Imaging Study shall have one active verified Radiology Report.

Historical versions shall remain permanently available for audit and clinical reference.

---

# Report Components

Every Radiology Report shall contain:

Report Identifier

Imaging Study Reference

Radiology Order Reference

Clinical Order Reference

Patient Reference

Encounter Reference

Reporting Radiologist

Verification Status

Report Version

Creation Date & Time

Verification Date & Time

Publication Date & Time

---

# Clinical Sections

The report may include:

Clinical Indication

Technique

Contrast Information

Comparison Studies

Findings

Impression

Recommendations

Critical Findings

Follow-up Recommendation

---

# Reporting Models

Supports:

Narrative Reporting

Structured Reporting

Template-Based Reporting

Organ-Based Reporting

Future Voice Recognition Reporting

Future AI-assisted Drafting

---

# Report Lifecycle

Draft

↓

In Progress

↓

Ready for Review

↓

Verified

↓

Published

Alternative Path

Corrected

↓

Republished

Archived

---

# Report Status

Draft

In Progress

Pending Review

Verified

Published

Corrected

Superseded

Archived

Cancelled

---

# Version Management

Supports:

Major revisions

Minor revisions

Corrections

Amendments

Every published version shall remain immutable.

New corrections create a new version while preserving previous releases.

---

# Findings Management

Findings may include:

Normal Findings

Abnormal Findings

Incidental Findings

Critical Findings

Comparison Findings

Recommendations

Structured observations where applicable.

---

# Business Rules

## BR-001

Every Radiology Report shall reference one Imaging Study.

---

## BR-002

Only verified reports may be published.

---

## BR-003

Published reports are immutable.

---

## BR-004

Corrected reports shall preserve prior versions.

---

## BR-005

Critical Findings shall trigger the Critical Findings workflow.

---

## BR-006

Reports shall automatically appear in the Clinical Timeline after publication.

---

## BR-007

Structured report templates shall be configurable without application code changes.

---

# Roles and Responsibilities

## Radiologist

Interpret imaging studies.

Prepare reports.

Verify reports.

Issue corrections.

---

## Department Supervisor

Review exceptional reports.

Approve quality corrections where required.

Monitor reporting quality.

---

## Referring Physician

View published reports.

Review recommendations.

Access historical reports.

---

## Administrator

Configure:

Templates

Report sections

Verification workflow

Publication policies

---

# Audit Events

Report Created

Report Updated

Report Verified

Report Published

Report Corrected

Report Archived

Template Updated

---

# Security

Radiology Reports shall enforce:

Role-Based Access Control

Organization Isolation

Branch Isolation

Version Protection

Immutable Published Reports

Complete Audit Trail

---

# AI Readiness

Future AI capabilities

AI-generated report drafts

Structured finding extraction

Terminology normalization

Recommendation suggestions

Quality consistency review

Speech-to-text reporting

AI outputs require radiologist review and approval.

---

# Future Extensions

Voice recognition

Structured reporting standards

FHIR DiagnosticReport integration

Natural language search

Automated coding

Clinical research integration

---

# Implementation Impact

## Frontend Impact

Radiology reporting workspace

Structured reporting editor

Template selector

Version comparison

Report history viewer

---

## Backend Impact

Reporting Service

Template Engine

Version Management Service

Publication Service

Audit Service

---

## Database Impact (Conceptual)

Imaging Study

↓

Radiology Report

↓

Report Version

↓

Clinical Timeline

↓

Audit

---

## API Impact

Create Report

Update Report

Verify Report

Publish Report

Retrieve Report

Retrieve Version History

Correct Report

---

## RBAC Impact

Radiologist

Create, edit, verify, and correct reports

Department Supervisor

Review quality and exceptional reports

Referring Physician

View published reports

Administrator

Manage templates and reporting workflow

---

# Related Documents

RAD-003 — Imaging Study Architecture

RAD-004 — Image Acquisition Workflow

RAD-005 — PACS & DICOM Integration Architecture

RAD-007 — Critical Findings Management

CLN-007 — Clinical Timeline

ARCH-004 — Shared Clinical Services

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

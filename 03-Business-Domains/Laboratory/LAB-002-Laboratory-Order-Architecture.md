# LAB-002 — Laboratory Order Architecture

**Document ID:** LAB-002
**Title:** Laboratory Order Architecture
**Status:** Approved
**Priority:** Critical
**Category:** Laboratory Domain
**Implementation Status:** Ready
**Owner:** Laboratory Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the enterprise architecture for Laboratory Orders within the LOUTAS Care Platform.

A Laboratory Order extends the shared Clinical Order and represents the complete diagnostic workflow required to fulfill laboratory investigations.

Laboratory Orders manage requested tests, specimen requirements, processing status, verification, and result release.

---

# Scope

Applicable Modules

- Clinical Encounter
- Clinical Orders
- Laboratory
- Specimen Management
- Quality Control
- Clinical Timeline
- Clinical Decision Support
- Billing

---

# Objectives

Laboratory Orders shall:

- Extend Clinical Orders.
- Manage laboratory workflow.
- Support single and grouped investigations.
- Track specimen requirements.
- Support multiple specimens.
- Enable complete order traceability.
- Integrate with billing and reporting.

---

# Enterprise Decision

## EA-019 — Laboratory Orders Extend Clinical Orders

A Laboratory Order is a specialization of the shared Clinical Order.

It shall never duplicate the Clinical Order business model.

Laboratory-specific attributes extend the shared model.

---

# Laboratory Order Lifecycle

Requested

↓

Accepted

↓

Specimen Pending

↓

Collected

↓

Received

↓

Processing

↓

Result Entry

↓

Verification

↓

Released

Alternative States

Cancelled

Rejected

Expired

---

# Laboratory Order Components

Every Laboratory Order shall include:

Laboratory Order Identifier

Related Clinical Order

Patient

Encounter

Ordering Provider

Priority

Requested Tests

Requested Panels

Required Specimens

Collection Status

Processing Status

Verification Status

Release Status

Laboratory Notes

Attachments

Version

---

# Priority Levels

Routine

Urgent

STAT

Critical

---

# Order Types

Single Test

Panel

Profile

Repeat Order

Reflex Order (Future)

Standing Order (Future)

---

# Test Organization

One Laboratory Order may contain:

Single Tests

or

Laboratory Panels

Examples

CBC

HbA1c

Lipid Profile

Liver Function Tests

Renal Profile

Thyroid Profile

---

# Specimen Requirements

One Order

↓

One or More Specimens

Example

CBC → EDTA Blood

Glucose → Fluoride Tube

Urine Analysis → Urine Sample

Culture → Sterile Container

---

# Relationships

Clinical Order

↓

Laboratory Order

↓

Specimen

↓

Analysis

↓

Result

↓

Clinical Timeline

---

# Business Rules

## BR-001

Every Laboratory Order references one Clinical Order.

---

## BR-002

Every Laboratory Order belongs to one Patient.

---

## BR-003

One Laboratory Order may require multiple specimens.

---

## BR-004

Released Orders become read-only.

---

## BR-005

Cancellation shall preserve audit history.

---

## BR-006

Specimen collection is mandatory before processing.

---

## BR-007

Result verification is mandatory before release.

---

## BR-008

Critical results shall activate Critical Result workflow.

---

## BR-009

Laboratory Orders may generate Clinical Tasks.

---

## BR-010

Laboratory Orders participate in the Clinical Timeline.

---

# Integration

Clinical Encounter

↓

Clinical Order

↓

Laboratory Order

↓

Specimen

↓

Laboratory Processing

↓

Result

↓

Timeline

↓

Decision Support

↓

Billing

---

# Security

Only authorized laboratory personnel may process Laboratory Orders.

Ordering physicians may view order progress and results.

---

# Audit Events

Order Created

Order Accepted

Specimen Requested

Specimen Collected

Processing Started

Result Entered

Result Verified

Result Released

Order Cancelled

---

# Quality Indicators

Turnaround Time

Collection Delay

Verification Delay

Cancelled Orders

Rejected Specimens

Critical Result Response Time

---

# AI Readiness

Future AI capabilities

Recommended Test Panels

Duplicate Test Detection

Reflex Test Suggestions

Priority Optimization

Turnaround Prediction

AI recommendations require laboratory or physician approval.

---

# Future Extensions

HL7 ORM

FHIR ServiceRequest

External Laboratory Routing

Barcode Integration

Standing Orders

Reflex Testing

---

# Implementation Impact

## Frontend Impact

Laboratory Order workspace.

Order tracking timeline.

Priority badges.

Panel visualization.

---

## Backend Impact

Laboratory Order Service.

Workflow Engine.

Status Management.

Billing Integration.

---

## Database Impact (Conceptual)

Clinical Order

↓

Laboratory Order

↓

Requested Test

↓

Specimen

↓

Audit

---

## API Impact

Create Laboratory Order

Update Laboratory Order

Retrieve Order Status

Cancel Order

Retrieve Order History

---

## RBAC Impact

Physician

Create / View

Laboratory Technician

Accept / Process

Laboratory Supervisor

Verify / Release

Administrator

Configuration

---

# Related Documents

ARCH-004 — Shared Clinical Services

CLN-005 — Clinical Orders Architecture

LAB-001 — Laboratory Architecture Overview

LAB-003 — Specimen Architecture

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

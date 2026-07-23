# LAB-003 — Specimen Architecture

**Document ID:** LAB-003
**Title:** Specimen Architecture
**Status:** Approved
**Priority:** Critical
**Category:** Laboratory Domain
**Implementation Status:** Ready
**Owner:** Laboratory Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the enterprise architecture for Specimen Management within the LOUTAS Care Platform.

A Specimen represents the physical biological sample collected from a patient for one or more laboratory investigations.

The Specimen is an independent business entity responsible for identification, traceability, collection lifecycle, transportation, processing, storage, and disposal.

---

# Scope

This architecture applies to:

- Laboratory Orders
- Specimen Collection
- Specimen Transportation
- Laboratory Reception
- Laboratory Processing
- Quality Control
- Result Verification
- Laboratory Analytics

Supported specimen categories include:

- Whole Blood
- Serum
- Plasma
- Urine
- Stool
- Sputum
- Swab
- Tissue (Future)
- Body Fluids
- Other Biological Samples

---

# Objectives

The Specimen Architecture shall:

- Provide unique specimen identification.
- Ensure complete traceability.
- Support barcode identification.
- Support multiple specimens per order.
- Support chain of custody.
- Reduce specimen errors.
- Support accreditation requirements.

---

# Enterprise Decision

## EA-020 — Specimen as an Independent Business Entity

A Specimen shall exist as an independent business entity.

It shall not be implemented as a simple child record of Laboratory Order.

The specimen owns its own lifecycle, audit history, and operational workflow.

---

# Specimen Lifecycle

Requested

↓

Collection Pending

↓

Collected

↓

Labeled

↓

Transported

↓

Received

↓

Accepted

↓

Processing

↓

Stored

↓

Disposed

Alternative States

Rejected

Lost

Damaged

Cancelled

---

# Specimen Components

Every specimen shall contain:

Specimen Identifier

Barcode

Laboratory Order Reference

Patient Reference

Encounter Reference

Collection Date & Time

Collector

Collection Site

Specimen Type

Container Type

Volume

Priority

Current Status

Storage Location

Expiration

Remarks

Version

---

# Specimen Classification

Blood

Urine

Stool

Swab

Sputum

Body Fluid

Serum

Plasma

Other

---

# Container Types

EDTA Tube

Plain Tube

Fluoride Tube

Citrate Tube

Culture Bottle

Urine Cup

Sterile Container

Other

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

Each Specimen has one unique identifier.

---

## BR-002

Each Specimen has one barcode.

---

## BR-003

One Laboratory Order may have multiple specimens.

---

## BR-004

One Specimen may support multiple laboratory tests.

---

## BR-005

Rejected specimens cannot proceed to processing.

---

## BR-006

Every status change shall be audited.

---

## BR-007

Specimen disposal shall be recorded.

---

## BR-008

Lost or damaged specimens require incident documentation.

---

# Chain of Custody

The following events shall be recorded:

Collection

Label Printing

Transportation

Laboratory Receipt

Acceptance

Processing

Storage

Release

Disposal

Every event shall include:

Timestamp

User

Location

Remarks (optional)

---

# Barcode Principles

Each specimen receives one barcode.

The barcode remains unchanged throughout its lifecycle.

Barcode scanning shall be supported during:

Collection

Transportation

Reception

Processing

Storage

Disposal

---

# Quality Objectives

Prevent specimen misidentification.

Reduce recollection rates.

Improve traceability.

Reduce processing delays.

Support laboratory accreditation.

---

# Security

Only authorized personnel may:

Collect specimens.

Receive specimens.

Modify specimen status.

Dispose specimens.

All actions require audit logging.

---

# AI Readiness

Future AI capabilities include:

Specimen rejection prediction.

Collection error detection.

Transport delay prediction.

Quality monitoring.

Operational optimization.

AI recommendations require human validation.

---

# Future Extensions

RFID specimen tracking.

Smart transport containers.

Cold-chain monitoring.

External laboratory transfer.

Automated storage systems.

---

# Implementation Impact

## Frontend Impact

Specimen dashboard.

Barcode printing.

Collection screen.

Tracking timeline.

Storage management.

---

## Backend Impact

Specimen Service.

Barcode Service.

Tracking Engine.

Storage Engine.

Audit Engine.

---

## Database Impact (Conceptual)

Laboratory Order

↓

Specimen

↓

Tracking Events

↓

Processing

↓

Result

↓

Audit

---

## API Impact

Create Specimen

Print Barcode

Receive Specimen

Update Status

Track Specimen

Dispose Specimen

Retrieve Specimen History

---

## RBAC Impact

Physician

View specimen status.

Laboratory Technician

Collect, receive and process specimens.

Laboratory Supervisor

Approve, reject and supervise specimen workflow.

Administrator

Configure specimen types, containers and storage.

---

# Related Documents

LAB-001 — Laboratory Architecture Overview

LAB-002 — Laboratory Order Architecture

LAB-004 — Specimen Collection Workflow

ARCH-004 — Shared Clinical Services

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

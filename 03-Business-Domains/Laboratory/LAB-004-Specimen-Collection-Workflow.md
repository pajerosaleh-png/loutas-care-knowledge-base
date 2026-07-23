# LAB-004 — Specimen Collection Workflow

**Document ID:** LAB-004
**Title:** Specimen Collection Workflow
**Status:** Approved
**Priority:** Critical
**Category:** Laboratory Domain
**Implementation Status:** Ready
**Owner:** Laboratory Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the standardized workflow for specimen collection within the LOUTAS Care Platform.

The workflow ensures that specimens are collected safely, accurately identified, transported correctly, and delivered to the laboratory while maintaining complete traceability and patient safety.

---

# Scope

This workflow applies to:

- Outpatient Clinics
- Multi-Branch Organizations
- Future Hospital Deployments
- Internal Collection Points
- External Collection Centers (Future)

Applicable specimen types include all specimen categories defined in LAB-003.

---

# Objectives

The workflow shall:

- Ensure correct patient identification.
- Ensure correct specimen collection.
- Prevent specimen mix-ups.
- Support barcode-based identification.
- Maintain chain of custody.
- Improve turnaround time.
- Support laboratory accreditation requirements.

---

# Enterprise Decision

## EA-021 — Positive Patient Identification

Specimen collection shall not begin until the patient's identity has been positively verified using approved identification methods.

Barcode generation shall occur immediately after specimen registration.

---

# Workflow Overview

Clinical Order

↓

Laboratory Order

↓

Specimen Registration

↓

Patient Identification

↓

Collection Preparation

↓

Specimen Collection

↓

Barcode Labeling

↓

Collection Verification

↓

Packaging

↓

Transportation

↓

Laboratory Reception

↓

Acceptance

↓

Processing

---

# Workflow Steps

## Step 1 — Laboratory Order Review

Responsible:

Laboratory Technician

Activities:

- Review Laboratory Order.
- Confirm requested tests.
- Verify specimen requirements.
- Check collection instructions.

---

## Step 2 — Patient Identification

Responsible:

Collector / Laboratory Technician

Verification methods may include:

- Patient Full Name
- Medical Record Number (MRN)
- National ID (if applicable)
- Date of Birth
- Wristband (Hospital Future)

The collector shall verify at least two patient identifiers before collection.

---

## Step 3 — Collection Preparation

Activities:

- Prepare required containers.
- Prepare collection equipment.
- Confirm specimen type.
- Verify patient preparation requirements (e.g., fasting).

---

## Step 4 — Specimen Collection

Activities:

- Collect specimen according to laboratory protocol.
- Record collection date and time.
- Record collector identity.
- Record collection site (if applicable).

---

## Step 5 — Barcode Labeling

Immediately after collection:

- Generate barcode.
- Label specimen container.
- Verify barcode readability.
- Match barcode with Laboratory Order.

Specimens shall never be transported without identification.

---

## Step 6 — Collection Verification

Verify:

Correct patient

Correct specimen

Correct container

Correct barcode

Correct collection time

Correct requested tests

---

## Step 7 — Packaging

Activities:

- Secure specimen.
- Apply transport requirements.
- Prepare transport documentation (if required).

---

## Step 8 — Transportation

Record:

Transport start time

Courier (if applicable)

Destination laboratory

Expected arrival time

Transport conditions (if required)

---

## Step 9 — Laboratory Reception

Activities:

- Scan barcode.
- Verify specimen integrity.
- Verify accompanying order.
- Register laboratory receipt.

---

## Step 10 — Acceptance Decision

Possible outcomes:

Accepted

Rejected

Recollection Required

Transferred

Rejected specimens shall follow the rejection workflow.

---

# Exception Workflows

## Specimen Rejection

Reasons may include:

Wrong container

Insufficient volume

Hemolyzed sample

Leaking container

Missing barcode

Expired specimen

Incorrect patient identification

Rejected specimens require documented reason and audit record.

---

## Recollection Workflow

When recollection is required:

- Create recollection request.
- Notify ordering provider.
- Notify collection point.
- Schedule recollection.

---

# Roles and Responsibilities

## Physician

- Order laboratory investigations.
- Review results.
- Respond to recollection requests when necessary.

---

## Laboratory Technician

- Review orders.
- Collect specimens.
- Label specimens.
- Verify collection.
- Transfer specimens.

---

## Laboratory Supervisor

- Supervise collection quality.
- Review rejected specimens.
- Monitor workflow compliance.

---

## Administrator

- Configure specimen types.
- Configure collection locations.
- Configure barcode settings.

---

# Business Rules

## BR-001

Specimens shall only be collected for valid Laboratory Orders.

---

## BR-002

Positive patient identification is mandatory.

---

## BR-003

Each collected specimen shall receive one unique barcode.

---

## BR-004

Specimens shall be labeled immediately after collection.

---

## BR-005

Unlabeled specimens shall not be accepted.

---

## BR-006

Rejected specimens require documented justification.

---

## BR-007

Every workflow step shall generate an audit event.

---

# Audit Events

Laboratory Order Reviewed

Patient Identified

Collection Started

Collection Completed

Barcode Printed

Barcode Scanned

Specimen Packaged

Specimen Transported

Specimen Received

Specimen Accepted

Specimen Rejected

Recollection Requested

---

# Quality Indicators

Patient identification compliance

Collection error rate

Rejected specimen rate

Recollection rate

Average collection time

Average transport time

Barcode scan success rate

---

# Security

Only authorized laboratory personnel may collect specimens.

Patient identification records shall be protected.

Audit logs shall be immutable.

---

# AI Readiness

Future AI capabilities:

Collection delay prediction

Specimen rejection prediction

Workflow optimization

Collector performance analytics

Transport risk prediction

AI recommendations require human review.

---

# Future Extensions

Self-service collection kiosks

Mobile phlebotomy

Home specimen collection

RFID specimen tracking

Cold-chain monitoring

---

# Related Documents

LAB-001 — Laboratory Architecture Overview

LAB-002 — Laboratory Order Architecture

LAB-003 — Specimen Architecture

LAB-005 — Laboratory Processing Architecture

ARCH-004 — Shared Clinical Services

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

# PHR-006 — Controlled Drug Management

**Document ID:** PHR-006
**Title:** Controlled Drug Management
**Status:** Approved
**Priority:** Critical
**Category:** Pharmacy Domain
**Implementation Status:** Ready
**Owner:** Enterprise Pharmacy Governance Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the Enterprise Controlled Drug Management Architecture for the LOUTAS Care Platform.

The architecture provides complete lifecycle management for controlled medications, ensuring regulatory compliance, secure handling, full traceability, inventory accountability, and comprehensive auditability.

Controlled drug management is implemented as an extension of the Enterprise Pharmacy Domain while allowing organization-specific regulatory configuration.

---

# Vision

To establish a secure, compliant, and fully traceable controlled medication management framework that protects patients, healthcare professionals, and organizations while satisfying national regulatory requirements.

---

# Scope

Applies to:

- Controlled Medication Catalog
- Controlled Prescriptions
- Controlled Dispensing
- Controlled Inventory
- Chain of Custody
- Controlled Returns
- Controlled Destruction
- Regulatory Reporting
- Audit & Compliance

Future Scope:

- National Narcotics Registry Integration
- Electronic Regulatory Reporting
- Smart Safe Integration
- Biometric Authentication
- Dual-Control Dispensing
- Automated Controlled Drug Cabinets

---

# Objectives

The Controlled Drug Architecture shall:

- Ensure regulatory compliance.
- Prevent diversion and misuse.
- Maintain complete chain of custody.
- Support configurable regulatory policies.
- Strengthen accountability.
- Maintain permanent audit records.

---

# Enterprise Decision

## EA-057 — Controlled Medications Require Enhanced Governance

Controlled medications shall follow dedicated governance workflows beyond standard medication management.

Enhanced controls shall be applied to prescribing, dispensing, inventory movement, reconciliation, and destruction.

---

# Enterprise Architecture

Medication Catalog

↓

Controlled Medication Classification

↓

Controlled Prescription

↓

Authorization Validation

↓

Controlled Dispensing

↓

Inventory Accountability

↓

Chain of Custody

↓

Regulatory Reporting

↓

Audit

---

# Controlled Medication Classification

Supports configurable classifications such as:

Schedule I

Schedule II

Schedule III

Schedule IV

Schedule V

Controlled classifications shall be configurable according to country-specific regulations.

---

# Controlled Prescription Components

Every controlled prescription shall include:

Prescription Identifier

Patient Reference

Prescribing Physician

License Identifier (optional)

Controlled Medication Reference

Controlled Schedule

Clinical Justification

Issue Date

Expiration Date

Electronic Signature Status

Approval Status

Version

---

# Chain of Custody

Every movement shall record:

Transaction Identifier

Medication

Quantity

Source Location

Destination Location

Responsible User

Witness User (if required)

Transaction Date & Time

Reason

Electronic Signature

Audit Reference

Every custody event shall be permanently retained.

---

# Controlled Dispensing Workflow

Controlled Prescription

↓

Authorization Verification

↓

Identity Verification

↓

Inventory Verification

↓

Dispensing

↓

Patient Confirmation

↓

Inventory Adjustment

↓

Audit Recording

↓

Regulatory Reporting

---

# Inventory Reconciliation

Supports:

Daily reconciliation

Shift reconciliation

Periodic reconciliation

Cycle count

Full inventory audit

Variance investigation

Corrective action

Reconciliation frequency shall be configurable.

---

# Controlled Returns

Supports:

Patient return

Expired medication return

Damaged medication return

Dispensing error return

Inventory correction

All returns require documented justification.

---

# Controlled Destruction

Destruction workflow:

Destruction Request

↓

Authorization

↓

Witness Verification

↓

Destruction

↓

Inventory Adjustment

↓

Audit Recording

↓

Regulatory Reporting

Every destruction shall require documented approval.

---

# Business Rules

## BR-001

Only authorized clinicians may prescribe controlled medications.

---

## BR-002

Controlled prescriptions shall require enhanced authentication according to organization policy.

---

## BR-003

Controlled dispensing shall require identity verification.

---

## BR-004

Every inventory movement shall update the Chain of Custody.

---

## BR-005

Controlled medication destruction shall require authorization and witness approval.

---

## BR-006

Controlled medication transactions shall never be physically deleted.

---

## BR-007

Regulatory reports shall be generated from immutable audit records.

---

# Roles and Responsibilities

## Physician

Prescribe controlled medications.

Provide clinical justification.

Electronically sign prescriptions.

---

## Pharmacist

Validate prescriptions.

Dispense medications.

Maintain custody records.

Perform reconciliations.

---

## Pharmacy Supervisor

Approve exceptional transactions.

Review discrepancies.

Authorize destruction.

Monitor compliance.

---

## Compliance Officer

Review regulatory reports.

Monitor audit findings.

Investigate irregularities.

---

## Administrator

Configure:

Controlled schedules

Authentication rules

Approval workflows

Reconciliation policies

Retention policies

---

# Audit Events

Controlled Prescription Created

Controlled Prescription Signed

Authorization Verified

Medication Dispensed

Chain of Custody Updated

Inventory Reconciled

Variance Detected

Medication Returned

Medication Destroyed

Regulatory Report Generated

---

# Security

Controlled Drug Management shall enforce:

Role-Based Access Control

Organization Isolation

Branch Isolation

Electronic Signature

Multi-Factor Authentication (Future)

Immutable Audit Records

Chain of Custody Protection

Regulatory Retention Policies

---

# AI Readiness

Future AI capabilities

Diversion detection

Abnormal dispensing pattern analysis

Inventory anomaly detection

Compliance risk prediction

Controlled medication forecasting

Operational audit recommendations

AI recommendations require human review.

---

# Future Extensions

National narcotics integration

Electronic regulatory submission

Biometric dispensing

Smart controlled drug safes

Blockchain-based custody tracking

Regional compliance exchange

---

# Implementation Impact

## Frontend Impact

Controlled prescription workspace

Authorization dashboard

Chain of custody viewer

Inventory reconciliation screen

Destruction management

Compliance dashboard

---

## Backend Impact

Controlled Drug Service

Authorization Service

Custody Service

Compliance Service

Reconciliation Engine

Audit Service

---

## Database Impact (Conceptual)

Controlled Medication

↓

Controlled Prescription

↓

Dispense Record

↓

Chain of Custody

↓

Inventory Reconciliation

↓

Regulatory Report

↓

Audit

---

## API Impact

Create Controlled Prescription

Authorize Dispensing

Record Chain of Custody

Perform Inventory Reconciliation

Register Medication Destruction

Generate Regulatory Report

---

## RBAC Impact

Physician

Prescribe controlled medications

Pharmacist

Dispense and maintain custody

Pharmacy Supervisor

Approve controlled workflows

Compliance Officer

Review compliance and investigations

Administrator

Configure governance policies

---

# Related Documents

PHR-002 — Medication Catalog Architecture

PHR-003 — Prescription Architecture

PHR-004 — Dispensing Workflow

PHR-005 — Medication Safety & Interaction

ARCH-004 — Shared Clinical Services

Future Inventory Book

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

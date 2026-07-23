# CLN-005 — Clinical Orders Architecture Specification

**Document ID:** CLN-005
**Title:** Clinical Orders Architecture Specification
**Status:** Approved
**Priority:** Critical
**Category:** Clinical Architecture
**Implementation Status:** Ready
**Owner:** Clinical Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the enterprise architecture for Clinical Orders within the LOUTAS Care Platform.

Clinical Orders represent the physician's formal request for diagnostic, therapeutic, pharmaceutical, procedural, or referral services during a Clinical Encounter.

This document standardizes the creation, lifecycle, ownership, execution, auditing, and integration of all clinical orders.

---

# Scope

This specification applies to:

- Outpatient Clinics
- Specialty Clinics
- Multi-Branch Organizations

Applicable Modules

- EMR
- Clinical Encounter
- Laboratory
- Radiology
- Pharmacy
- Procedures
- Billing
- Notifications
- Audit
- Analytics

---

# Objectives

Clinical Orders shall:

- Standardize physician requests.
- Ensure traceability.
- Support multidisciplinary workflows.
- Reduce medical errors.
- Enable downstream automation.
- Integrate with billing.
- Support future interoperability standards.

---

# Enterprise Decision

## EA-006 — Clinical Order as an Enterprise Entity

Clinical Orders shall be modeled as a unified enterprise entity.

Different order categories shall share one lifecycle, one governance model, and one audit framework.

Specialization occurs through the Order Type, not through separate architectures.

---

# Definition

A Clinical Order is a physician-authorized instruction requesting a healthcare service to be performed for a patient during an active Clinical Encounter.

Clinical Orders originate from the Plan section of SOAP documentation.

---

# Ownership

Relationship

Patient

↓

Clinical Encounter

↓

SOAP Plan

↓

Clinical Order

↓

Executing Department

↓

Clinical Result

Clinical Orders shall never exist without an active Clinical Encounter.

---

# Order Lifecycle

Draft

↓

Ordered

↓

Acknowledged

↓

In Progress

↓

Completed

↓

Verified

↓

Closed

Possible Alternative States

Cancelled

Rejected

Expired

---

## Draft

The provider is preparing the order.

No execution allowed.

---

## Ordered

Order officially submitted.

Department notification generated.

---

## Acknowledged

Receiving department accepts responsibility.

---

## In Progress

Execution has started.

---

## Completed

Requested activity finished.

Results available.

---

## Verified

Provider reviews results.

Clinical interpretation completed.

---

## Closed

Workflow completed.

Order becomes read-only.

---

# Order Types

Laboratory Order

Radiology Order

Medication Order

Procedure Order

Referral Order

Medical Certificate

Vaccination Order (Future)

Medical Device Order (Future)

Nutrition Order (Future)

Physiotherapy Order (Future)

---

# Order Components

Every Clinical Order shall contain:

Order Identifier

Patient

Encounter

Ordering Provider

Department

Priority

Clinical Indication

Requested Service

Clinical Notes

Status

Creation Date

Completion Date

Audit Information

---

# Priority Levels

Routine

Urgent

STAT

Emergency

Priority influences execution sequence.

---

# Business Rules

## BR-001

Orders require an active Clinical Encounter.

---

## BR-002

Orders originate from SOAP Plan.

---

## BR-003

Only authorized providers may create orders.

---

## BR-004

Completed orders become immutable.

---

## BR-005

Cancelled orders remain auditable.

---

## BR-006

Every order shall have one responsible executing department.

---

## BR-007

Results must remain linked to the originating order.

---

## BR-008

Billing events may be generated after order completion according to clinic policy.

---

## BR-009

Every order shall support electronic status tracking.

---

## BR-010

Duplicate active orders for the same service should trigger a clinical warning.

---

# Clinical Workflow

Physician

↓

SOAP Plan

↓

Clinical Order

↓

Department Queue

↓

Execution

↓

Result

↓

Provider Review

↓

Encounter Completion

↓

Billing

---

# Department Responsibilities

## Laboratory

Receive laboratory requests

Perform testing

Publish validated results

---

## Radiology

Schedule imaging

Perform examination

Attach reports

---

## Pharmacy

Validate prescription

Dispense medication

Record dispensing status

---

## Procedures

Schedule procedure

Perform intervention

Document completion

---

## Referrals

Generate referral

Assign destination

Track completion

---

# Integration

Clinical Encounter

↓

SOAP

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

↓

Audit

---

# Notifications

The system may generate notifications when:

Order Created

Order Accepted

Order Completed

Critical Result Available

Order Cancelled

Delayed Order

---

# Audit Events

Order Created

Order Updated

Order Cancelled

Order Completed

Order Verified

Department Assigned

Priority Changed

Result Attached

Billing Generated

---

# Security

Clinical Orders follow role-based access control.

Ordering permissions shall depend on professional role.

Execution permissions shall belong to the responsible department.

Audit logs are mandatory.

---

# Quality Indicators

Average Order Turnaround Time

Laboratory Completion Time

Radiology Completion Time

Medication Dispensing Time

Cancelled Order Rate

Duplicate Order Rate

Critical Result Response Time

---

# Future Extensions

FHIR ServiceRequest

HL7 Integration

Electronic Laboratory Interfaces

Radiology PACS Integration

Clinical Decision Support

AI Order Suggestions

Electronic Prior Authorization

---

# Related Documents

CLN-001 — Clinical Architecture Overview

CLN-002 — Clinical Encounter Specification

CLN-003 — Patient Journey Specification

CLN-004 — SOAP Architecture Specification

CLN-006 — Clinical Documents Architecture

ARCH-003 — Domain Architecture

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

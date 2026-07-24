# BILL-007 — Accounts Receivable

**Document ID:** BILL-007
**Title:** Accounts Receivable
**Status:** Approved
**Priority:** Critical
**Category:** Billing Domain
**Implementation Status:** Ready
**Owner:** Enterprise Billing Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the Enterprise Accounts Receivable (AR) Architecture for the LOUTAS Care Platform.

Accounts Receivable manages all outstanding financial obligations owed to the healthcare organization, tracks collections, supports reconciliation, and provides operational visibility into unpaid balances.

Receivables represent operational financial obligations and shall integrate with Accounting without replacing it.

---

# Vision

To establish a secure, auditable, scalable, and enterprise-ready Accounts Receivable platform supporting patient billing, insurance reimbursement, corporate accounts, and future financial expansion.

---

# Scope

Applies to:

- Patient Receivables
- Insurance Receivables
- Corporate Receivables
- Outstanding Balances
- Aging Analysis
- Collection Workflow
- Payment Allocation
- Write-Offs
- Bad Debt
- Financial Reconciliation

Future Scope:

- Installment Plans

- Automated Collections

- Payment Promises

- External Collection Agencies

- AI Collection Optimization

---

# Objectives

The Accounts Receivable Architecture shall:

- Track all outstanding balances.
- Support multiple debtor types.
- Enable structured collection workflows.
- Preserve complete financial traceability.
- Support financial reconciliation.
- Maintain enterprise auditability.

---

# Enterprise Decision

## EA-082 — Receivables Represent Outstanding Financial Obligations

Accounts Receivable shall maintain outstanding balances derived from approved invoices and approved financial adjustments.

Receivable balances shall never exist independently from billing records.

---

# Enterprise Architecture

Issued Invoice

↓

Outstanding Balance

↓

Accounts Receivable

↓

Collection Workflow

↓

Payment Allocation

↓

Balance Recalculation

↓

Financial Integration

↓

Audit

---

# Receivable Types

Supports:

Patient Receivable

Insurance Receivable

Corporate Receivable

Government Receivable (Future)

Research Grant Receivable (Future)

---

# Receivable Status

Open

↓

Partially Collected

↓

Collected

OR

Written Off

OR

Disputed

↓

Closed

---

# Aging Analysis

Supports configurable aging buckets:

Current

1–30 Days

31–60 Days

61–90 Days

91–180 Days

Over 180 Days

Organizations may configure additional aging intervals.

---

# Collection Workflow

Outstanding Balance

↓

Reminder

↓

Follow-up

↓

Collection Attempt

↓

Payment

OR

Dispute

OR

Write-Off

↓

Closure

---

# Write-Off Policy

Supports:

Administrative Write-Off

Financial Write-Off

Bad Debt

Charity Adjustment

Organizations shall configure approval thresholds.

Write-Offs shall preserve historical traceability.

---

# Collection Rules

Supports:

Automatic Reminder Scheduling

Manual Collection Workflow

Priority-Based Collection

Insurance Follow-Up

Corporate Follow-Up

Configurable Escalation Rules

---

# Business Rules

## BR-001

Receivables shall originate only from approved invoices.

---

## BR-002

Receivable balances shall automatically update after every approved payment.

---

## BR-003

Write-Offs shall require authorization.

---

## BR-004

Closed receivables shall remain immutable.

---

## BR-005

Receivable history shall never be physically deleted.

---

## BR-006

Patient and insurance receivables shall remain independently traceable.

---

## BR-007

Every receivable activity shall generate audit records.

---

# Roles and Responsibilities

## Billing Officer

Monitor receivables.

Initiate collection workflow.

Review outstanding balances.

---

## Insurance Officer

Manage insurance receivables.

Follow payer responses.

Track reimbursement.

---

## Finance Manager

Approve write-offs.

Review aging reports.

Monitor collection performance.

---

## Administrator

Configure:

Collection policies

Write-off limits

Reminder schedules

Aging intervals

---

# Audit Events

Receivable Created

Payment Allocated

Receivable Updated

Reminder Sent

Collection Attempt

Write-Off Approved

Receivable Closed

---

# Security

Accounts Receivable services shall enforce:

Role-Based Access Control

Organization Isolation

Branch Isolation

Immutable Financial History

Audit Logging

Future Electronic Signatures

---

# AI Readiness

Future AI capabilities

Collection prioritization

Payment prediction

Bad debt prediction

Insurance reimbursement forecasting

Collection performance analytics

AI recommendations require managerial approval.

---

# Future Extensions

Installment Management

Self-Service Payment Portal

External Collection Integration

Predictive Collection Models

Automated Reminder Engine

National Financial Reporting

---

# Implementation Impact

## Frontend Impact

Receivable dashboard

Aging analysis

Collection workspace

Insurance receivable dashboard

Write-off approval screen

Collection analytics

---

## Backend Impact

Receivable Service

Collection Engine

Reminder Service

Write-Off Service

Analytics Service

Audit Service

---

## Database Impact (Conceptual)

Invoice

↓

Receivable

↓

Collection Activity

↓

Payment

↓

Write-Off

↓

Audit

---

## API Impact

Retrieve Receivables

Retrieve Aging Report

Allocate Payment

Approve Write-Off

Retrieve Collection History

Retrieve Outstanding Balances

---

## RBAC Impact

Billing Officer

Insurance Officer

Finance Manager

Administrator

---

# Related Documents

BILL-003 — Invoice Lifecycle

BILL-004 — Payment Processing

BILL-005 — Discounts, Refunds & Credit Notes

BILL-006 — Insurance Billing

Future BILL-008 — Revenue Recognition

Future BILL-009 — Financial Integration

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

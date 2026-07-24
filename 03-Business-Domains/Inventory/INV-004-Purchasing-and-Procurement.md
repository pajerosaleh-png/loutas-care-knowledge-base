# INV-004 — Purchasing & Procurement

**Document ID:** INV-004
**Title:** Purchasing & Procurement
**Status:** Approved
**Priority:** Critical
**Category:** Inventory Domain
**Implementation Status:** Ready
**Owner:** Enterprise Procurement Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the Enterprise Purchasing and Procurement Architecture for the LOUTAS Care Platform.

The Procurement Domain governs how inventory items are requested, approved, sourced, ordered, and delivered while ensuring financial control, supplier governance, auditability, and regulatory compliance.

Procurement is responsible for acquiring inventory. Inventory is responsible for owning received stock.

---

# Vision

To establish a transparent, controlled, and scalable procurement process that supports healthcare organizations of every size while maintaining accountability, traceability, and cost optimization.

---

# Scope

Applies to:

- Purchase Requisitions
- Purchase Orders
- Supplier Management
- Vendor Evaluation
- Approval Workflow
- Request for Quotation (RFQ)
- Vendor Comparison
- Purchase Contracts (Future)
- Procurement Reporting

Future Scope:

- E-Procurement
- Supplier Portal
- Framework Agreements
- Reverse Auctions
- Contract Lifecycle Management
- AI Procurement Assistant

---

# Objectives

The Procurement Architecture shall:

- Standardize purchasing processes.
- Prevent unauthorized purchasing.
- Support approval workflows.
- Improve supplier management.
- Reduce procurement costs.
- Enable complete purchasing traceability.
- Support future ERP integration.

---

# Enterprise Decision

## EA-067 — Procurement Controls Purchasing, Inventory Controls Stock

Procurement manages purchasing activities.

Inventory manages stock ownership.

Receiving inventory shall never bypass the procurement workflow unless explicitly authorized by enterprise policy.

---

# Procurement Lifecycle

Purchase Requisition

↓

Approval

↓

RFQ (Optional)

↓

Quotation Evaluation

↓

Supplier Selection

↓

Purchase Order

↓

Supplier Delivery

↓

Goods Receipt

↓

Invoice Matching

↓

Payment (Financial System)

↓

Purchase Closed

---

# Core Components

The Procurement Domain consists of:

Purchase Requisition Service

Approval Workflow

Supplier Service

RFQ Service

Quotation Service

Purchase Order Service

Procurement Analytics

Audit Service

---

# Purchase Requisition

A Purchase Requisition shall include:

Requisition Number

Organization

Branch

Requesting Department

Requested By

Priority

Required Date

Requested Items

Business Justification

Approval Status

Version

---

# Requisition Priority

Supports:

Emergency

High

Normal

Low

Priority influences approval and procurement timelines.

---

# Supplier Management

Each supplier shall include:

Supplier Identifier

Supplier Code

Supplier Name

Tax Information

Contact Information

Address

Payment Terms

Currency

Status

Performance Rating

Preferred Supplier Flag

Version

---

# Request for Quotation (RFQ)

Supports:

Single Supplier RFQ

Multiple Supplier RFQ

Quotation Expiration

Vendor Comparison

Recommendation

Approval

---

# Purchase Order

Every Purchase Order shall contain:

PO Number

Supplier

Purchase Date

Expected Delivery Date

Items

Quantities

Prices

Discounts

Taxes

Currency

Approval Status

Order Status

Version

---

# Purchase Order Status

Draft

Pending Approval

Approved

Sent

Partially Received

Completed

Cancelled

Closed

---

# Approval Workflow

Approval may depend on:

Purchase Value

Item Category

Department

Emergency Priority

Organization Policy

Multiple approval levels shall be supported.

---

# Business Rules

## BR-001

Every Purchase Order shall originate from an approved Purchase Requisition unless policy allows otherwise.

---

## BR-002

Only approved Purchase Orders may be sent to suppliers.

---

## BR-003

Supplier performance shall be retained historically.

---

## BR-004

Purchase Orders shall never modify inventory directly.

---

## BR-005

Inventory increases only after Goods Receipt.

---

## BR-006

Cancelled Purchase Orders shall remain auditable.

---

## BR-007

Procurement records shall never be physically deleted.

---

# Roles and Responsibilities

## Requester

Create Purchase Requisitions.

Track request status.

---

## Procurement Officer

Manage RFQs.

Select suppliers.

Issue Purchase Orders.

Monitor deliveries.

---

## Procurement Manager

Approve procurement activities.

Review supplier performance.

Manage procurement strategy.

---

## Financial Reviewer

Review procurement budgets.

Validate financial policies.

---

## Administrator

Configure:

Approval workflows

Supplier policies

RFQ rules

Procurement settings

---

# Audit Events

Purchase Requisition Created

Requisition Approved

RFQ Issued

Quotation Received

Supplier Selected

Purchase Order Created

Purchase Order Approved

Purchase Order Cancelled

Purchase Closed

---

# Security

Procurement services shall enforce:

Role-Based Access Control

Organization Isolation

Branch Isolation

Approval Authorization

Complete Audit Trail

Future Electronic Signatures

---

# AI Readiness

Future AI capabilities:

Demand-based purchasing

Supplier recommendation

Price trend prediction

Quotation analysis

Procurement optimization

Budget forecasting

AI recommendations require procurement approval.

---

# Future Extensions

Supplier Portal

Electronic Purchase Orders

Contract Management

Automatic Vendor Scoring

National Supplier Registry

Marketplace Integration

---

# Implementation Impact

## Frontend Impact

Purchase Requisition screen

Purchase Order management

Supplier management

RFQ management

Approval dashboard

Procurement analytics

---

## Backend Impact

Procurement Service

Supplier Service

Approval Engine

RFQ Service

Purchase Order Service

Audit Service

---

## Database Impact (Conceptual)

Purchase Requisition

↓

Approval

↓

RFQ

↓

Quotation

↓

Supplier

↓

Purchase Order

↓

Goods Receipt

↓

Audit

---

## API Impact

Create Purchase Requisition

Approve Requisition

Create RFQ

Receive Quotation

Create Purchase Order

Approve Purchase Order

Retrieve Procurement History

---

## RBAC Impact

Requester

Create requisitions

Procurement Officer

Manage procurement workflow

Procurement Manager

Approve procurement

Financial Reviewer

Financial validation

Administrator

Configure procurement

---

# Related Documents

INV-001 — Inventory Architecture Overview

INV-002 — Warehouse Architecture

INV-003 — Product & Stock Model

Future Accounting Book

Future Finance Book

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

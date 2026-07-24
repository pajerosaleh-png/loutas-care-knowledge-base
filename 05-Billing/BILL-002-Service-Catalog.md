# BILL-002 — Service Catalog

**Document ID:** BILL-002
**Title:** Service Catalog
**Status:** Approved
**Priority:** Critical
**Category:** Billing Domain
**Implementation Status:** Ready
**Owner:** Enterprise Billing Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the Enterprise Service Catalog Architecture for the LOUTAS Care Platform.

The Service Catalog is the authoritative source of all billable services, products, procedures, and administrative charges used throughout the Billing Domain.

It standardizes pricing, classifications, billing rules, and service metadata while supporting future expansion without architectural redesign.

---

# Vision

To establish a centralized, configurable, and enterprise-ready catalog of billable items shared across all clinical and operational domains.

---

# Scope

Applies to:

- Clinical Services
- Consultations
- Procedures
- Laboratory Tests
- Radiology Services
- Pharmacy Items
- Medical Supplies
- Administrative Charges
- Packages
- Membership Services (Future)
- Insurance Billing Codes (Future)

---

# Objectives

The Service Catalog shall:

- Maintain a single source of truth for billable items.
- Standardize pricing across the organization.
- Support configurable billing rules.
- Enable integration with clinical domains.
- Preserve historical pricing.
- Support enterprise scalability.

---

# Enterprise Decision

## EA-077 — Every Billable Item Shall Exist in the Service Catalog

No invoice line may be created unless it references an approved Service Catalog item.

Manual free-text invoice lines shall be prohibited except through explicitly authorized administrative charges.

---

# Service Catalog Responsibilities

The Service Catalog owns:

Service Definition

Service Code

Service Category

Pricing Metadata

Tax Configuration

Discount Eligibility

Billing Rules

Activation Status

Version History

---

# Service Catalog Does NOT Own

Clinical Documentation

Appointments

Inventory Stock

Financial Posting

Patient Medical Records

These remain the responsibility of their respective domains.

---

# Enterprise Architecture

Clinical Domains

↓

Billable Events

↓

Service Catalog

↓

Pricing Engine

↓

Invoice Engine

↓

Billing Domain

---

# Service Categories

Supports:

Consultation

Procedure

Laboratory

Radiology

Pharmacy

Medical Supply

Vaccination

Administrative Charge

Package

Future Membership

Organizations may configure additional categories.

---

# Service Definition

Every catalog item shall include:

Service Code

Service Name

Short Description

Long Description

Category

Base Price

Currency

Tax Rule

Discount Rule

Billing Unit

Active Status

Effective Date

Expiration Date (Optional)

Version

---

# Pricing Model

Supports:

Fixed Price

Variable Price (Future)

Tiered Pricing (Future)

Organization-Level Pricing

Branch-Level Pricing

Future Contract Pricing

Historical prices shall remain reproducible.

---

# Billing Rules

Each service may define:

Requires Physician

Requires Inventory Consumption

Requires Laboratory Order

Requires Radiology Order

Requires Authorization

Requires Insurance Approval

Requires Appointment

Multiple rules may apply simultaneously.

---

# Versioning

Every pricing or billing rule change shall create a new version.

Historical invoices shall continue referencing the original service version.

---

# Business Rules

## BR-001

Every billable item shall have a unique service code.

---

## BR-002

Inactive services shall not be selectable for new invoices.

---

## BR-003

Historical invoices shall preserve historical pricing.

---

## BR-004

Service prices shall never be edited retroactively.

---

## BR-005

Service categories shall be configurable.

---

## BR-006

Billing rules shall be evaluated before invoice generation.

---

## BR-007

All catalog modifications shall be audit logged.

---

# Roles and Responsibilities

## Billing Administrator

Manage service catalog.

Configure pricing.

Maintain billing rules.

---

## Finance Manager

Approve pricing changes.

Review pricing policies.

---

## Clinical Administrator

Validate service definitions.

Coordinate with clinical departments.

---

## System Administrator

Manage catalog configuration.

Maintain security policies.

---

# Audit Events

Service Created

Service Updated

Price Changed

Category Changed

Service Activated

Service Deactivated

Billing Rule Updated

Catalog Version Published

---

# Security

The Service Catalog shall enforce:

Role-Based Access Control

Organization Isolation

Branch-Level Configuration

Immutable Historical Versions

Audit Logging

Future Electronic Approval Workflow

---

# AI Readiness

Future AI capabilities

Pricing recommendations

Service utilization analysis

Revenue optimization

Package recommendations

Demand forecasting

Pricing anomaly detection

AI recommendations require managerial approval.

---

# Future Extensions

Insurance Billing Codes

National Procedure Codes

CPT Mapping

ICD-Based Pricing Rules

Subscription Services

Dynamic Pricing

---

# Implementation Impact

## Frontend Impact

Service catalog management

Pricing configuration

Category management

Billing rule editor

Service search

Version history viewer

---

## Backend Impact

Service Catalog Service

Pricing Engine

Billing Rule Engine

Version Management

Audit Service

---

## Database Impact (Conceptual)

Service Category

↓

Service Catalog

↓

Pricing Version

↓

Billing Rule

↓

Invoice Line

↓

Audit

---

## API Impact

Retrieve Services

Create Service

Update Service

Deactivate Service

Retrieve Pricing

Retrieve Service Versions

---

## RBAC Impact

Billing Administrator

Finance Manager

Clinical Administrator

System Administrator

---

# Related Documents

BILL-001 — Billing Architecture Overview

Future BILL-003 — Invoice Lifecycle

Clinical Domain Documents

Laboratory Domain Documents

Radiology Domain Documents

Pharmacy Domain Documents

Inventory Domain Documents

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

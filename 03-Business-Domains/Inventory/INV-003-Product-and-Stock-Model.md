# INV-003 — Product & Stock Model

**Document ID:** INV-003
**Title:** Product & Stock Model
**Status:** Approved
**Priority:** Critical
**Category:** Inventory Domain
**Implementation Status:** Ready
**Owner:** Enterprise Inventory Architecture Team
**Version:** 1.0.0
**Last Updated:** July 2026

---

# Purpose

This document defines the Enterprise Product & Stock Model for the LOUTAS Care Platform.

The Product Model establishes a single authoritative definition for every inventory-managed item across the enterprise while separating product identity from stock ownership.

The model supports medications, laboratory supplies, radiology materials, medical consumables, equipment, office supplies, and future inventory categories.

---

# Vision

To establish a unified, reusable, and scalable product model that supports every inventory-consuming domain while maintaining consistency, traceability, and enterprise governance.

---

# Scope

Applies to:

- Product Master
- Stock Model
- SKU Management
- Units of Measure
- Product Categories
- Product Attributes
- Stock Quantities
- Reserved Stock
- Available Stock
- Reorder Policies
- Multi-UOM Conversion

Future Scope:

- Serialized Inventory

- GS1 Product Identification

- RFID Tags

- Medical Device Tracking

- UDI (Unique Device Identification)

---

# Objectives

The Product Model shall:

- Maintain one definition for every product.

- Separate products from inventory balances.

- Support multiple units of measure.

- Support configurable product categories.

- Enable enterprise-wide reuse.

- Support future serialization.

---

# Enterprise Decision

## EA-066 — One Product, Multiple Stock Records

A Product represents the business definition of an inventory item.

Stock quantities shall never be stored within the Product entity.

Each warehouse maintains independent stock records referencing the Product.

---

# Enterprise Architecture

Product Master

↓

Product Category

↓

SKU

↓

Unit of Measure

↓

Warehouse Stock

↓

Batch

↓

Inventory Transaction

↓

Analytics

---

# Product Components

Every Product shall contain:

Product Identifier

SKU

Product Code

Product Name

Scientific Name (optional)

Brand Name (optional)

Category

Subcategory

Inventory Type

Primary Unit of Measure

Status

Manufacturer

Supplier Reference (optional)

Version

---

# Product Categories

Supports:

Medication

Laboratory Reagent

Medical Consumable

Radiology Material

Medical Equipment

Office Supply

Cleaning Supply

Biomedical Item

Organizations may define additional categories.

---

# SKU Management

Each product shall have:

Enterprise SKU

Organization Code

Barcode (optional)

External Reference

Supplier Reference

National Reference (future)

SKU values shall remain unique within the organization.

---

# Units of Measure

Supports:

Piece

Box

Bottle

Pack

Strip

Kit

Milliliter

Liter

Gram

Kilogram

Meter

Roll

Organizations may define additional units.

---

# Multi-UOM Conversion

Supports:

Purchase Unit

Storage Unit

Dispensing Unit

Consumption Unit

Conversion Ratio

All conversions shall be configurable.

---

# Stock Model

Each stock record shall include:

Warehouse

Product

Current Quantity

Available Quantity

Reserved Quantity

Damaged Quantity

Expired Quantity

On Order Quantity

Minimum Stock

Maximum Stock

Reorder Level

Safety Stock

Version

---

# Stock Status

Available

Reserved

Allocated

On Hold

Damaged

Expired

Returned

Transferred

Archived

---

# Reorder Policy

Supports:

Minimum Stock Level

Maximum Stock Level

Safety Stock

Reorder Point

Economic Order Quantity (Future)

Automatic Replenishment (Future)

---

# Business Rules

## BR-001

Every inventory item shall reference one Product.

---

## BR-002

Stock balances shall exist only within Warehouse Stock records.

---

## BR-003

SKU values shall be unique.

---

## BR-004

Product definitions shall support versioning.

---

## BR-005

Unit conversions shall be configurable.

---

## BR-006

Product definitions shall never be physically deleted.

---

## BR-007

Stock calculations shall distinguish between Available and Reserved quantities.

---

# Roles and Responsibilities

## Inventory Officer

Manage stock.

Review quantities.

Receive products.

Issue products.

---

## Product Administrator

Maintain Product Master.

Manage categories.

Configure units of measure.

Maintain SKU definitions.

---

## Procurement Officer

Review reorder levels.

Maintain supplier references.

Support purchasing.

---

## Administrator

Configure:

Categories

Units

Product policies

Reorder rules

Conversion rules

---

# Audit Events

Product Created

Product Updated

SKU Assigned

Unit Conversion Updated

Reorder Level Updated

Category Updated

Stock Model Updated

---

# Security

Product services shall enforce:

Role-Based Access Control

Organization Isolation

Branch Isolation

Immutable Product History

Complete Audit Trail

---

# AI Readiness

Future AI capabilities

Demand prediction

Product classification

Automatic reorder recommendations

Stock optimization

Supplier recommendations

Consumption forecasting

AI recommendations require operational approval.

---

# Future Extensions

GS1 Product Registry

UDI Support

RFID Identification

Serialization

Global Product Registry

National Product Registry

---

# Implementation Impact

## Frontend Impact

Product management

SKU management

Category management

Unit conversion management

Stock overview

Reorder dashboard

---

## Backend Impact

Product Service

SKU Service

Category Service

Unit Conversion Service

Stock Service

Audit Service

---

## Database Impact (Conceptual)

Product

↓

SKU

↓

Unit of Measure

↓

Warehouse Stock

↓

Batch

↓

Inventory Transaction

↓

Audit

---

## API Impact

Create Product

Update Product

Retrieve Product

Retrieve Stock

Update Reorder Level

Retrieve Product Categories

Retrieve Units of Measure

---

## RBAC Impact

Inventory Officer

Manage stock

Product Administrator

Manage Product Master

Procurement Officer

Maintain procurement attributes

Administrator

Configure product framework

---

# Related Documents

INV-001 — Inventory Architecture Overview

INV-002 — Warehouse Architecture

PHR-007 — Pharmacy Inventory Integration

ARCH-001 — Enterprise Architecture Principles

Future Procurement Book

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0.0 | July 2026 | Initial Enterprise Release |

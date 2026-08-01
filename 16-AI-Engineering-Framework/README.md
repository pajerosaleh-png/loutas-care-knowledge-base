# 16-AI-Engineering-Framework

| Field | Value |
|-------|-------|
| Document ID | README-LAEF |
| Document Title | 16-AI-Engineering-Framework (Area Index) |
| Book | LAEF — LOUTAS AI Engineering Framework |
| Knowledge Base Area | 16-AI-Engineering-Framework |
| Version | 1.0 |
| Status | Approved |
| Owner | Enterprise AI Governance Office |
| Approval Authority | Product Owner |
| Review Cycle | Annual, and on every major LAEF milestone |
| Last Updated | 2026-07-29 |

---

# Overview

This README is the **entry point** to the **16-AI-Engineering-Framework** area of the LOUTAS Care Knowledge Base.

It orients any contributor — human or AI — to the LOUTAS AI Engineering Framework (LAEF): what this area contains, how to read it, how it relates to the rest of the Knowledge Base, and who governs it. Any contributor beginning work with LAEF should start here.

This area is model-agnostic. It refers to any contributing AI system as **"The AI Agent"** and is designed to remain valid across current and future AI systems.

---

# Purpose of the 16-AI-Engineering-Framework Area

The 16-AI-Engineering-Framework area exists to:

- Hold the authoritative LAEF governance documents in one place.
- Serve as the single, traceable index of those documents and their lifecycle status.
- Define the boundary between AI-engineering governance and the execution assets that apply it.
- Ensure every LAEF document complies with the LOUTAS Care documentation governance framework.

This area defines *what LAEF is and what it requires.* It does not contain execution assets.

---

# Relationship to LAEF

The 16-AI-Engineering-Framework area **is** the governance home of LAEF.

LAEF is the official engineering methodology governing every AI system that contributes to LOUTAS Care. Its governing documents — vision, principles, scope, architecture, governance, versioning, and roadmap — live here in 16-AI-Engineering-Framework. Every other LAEF asset, wherever it resides, derives its authority from the documents in this area.

---

# Relationship to 99-AI-Team (Execution Workspace)

LAEF is delivered through two complementary tiers. Neither replaces the other.

| Tier | Area | Role |
|------|------|------|
| **Governance** | **16-AI-Engineering-Framework** *(this area)* | Authoritative governance documents — *what LAEF is and requires* |
| **Execution** | **LAEF Workspace** *(evolved from 99-AI-Team)* | Agents, workflows, playbooks, templates, reviews — *how the work is performed* |

The 99-AI-Team area is evolving into the **LAEF Workspace**, the execution tier that applies LAEF governance in day-to-day engineering. Governance authority always resides in 16-AI-Engineering-Framework; the Workspace executes within the boundaries this area defines.

---

# Objectives

- Maintain LAEF as one enduring, model-agnostic engineering methodology.
- Preserve a single source of truth for AI-engineering governance.
- Keep governance (16-AI-Engineering-Framework) and execution (LAEF Workspace) clearly separated and cross-referenced.
- Ensure traceability of every LAEF document through its lifecycle.

---

# Document Index

| Document | Title | Status |
|----------|-------|--------|
| LAEF-001 | LAEF Framework Vision & Mission | Approved v1.0 |
| LAEF-002 | LAEF Core Principles & Philosophy | Approved v1.0 |
| LAEF-003 | LAEF Scope & Objectives | Approved v1.0 |
| LAEF-004 | LAEF Framework Architecture Overview | Approved v1.0 |
| LAEF-005 | LAEF Governance Overview | Approved v1.0 |
| LAEF-006 | LAEF Versioning Strategy | Approved v1.0 |
| LAEF-007 | LAEF Framework Roadmap | Approved v1.0 |
| LAEF-008 | Layer Architecture Foundations & Design Principles | Approved v1.0 |
| LAEF-009 | Foundation Tier Architecture | Approved v1.0 |
| LAEF-010 | Knowledge & Context Tier Architecture | Approved v1.0 |
| LAEF-011 | Execution Tier Architecture | Approved v1.0 |
| LAEF-012 | Assurance & Evolution Tier Architecture | Approved v1.0 |
| LAEF-013 | Engine-Set Design Specification | Approved v1.0 |
| LAEF-014 | Context Engine Specification | Approved v1.0 |
| LAEF-015 | Knowledge Engine Specification | Approved v1.0 |
| LAEF-016 | Task Engine Specification | Approved v1.0 |
| LAEF-017 | Decision Engine Specification | Approved v1.0 |
| LAEF-018 | Workflow Engine Specification | Approved v1.0 |
| LAEF-019 | Agent Engine Specification | Approved v1.0 |
| LAEF-020 | Quality Engine Specification | Approved v1.0 |
| LAEF-021 | Learning Engine Specification | Approved v1.0 |
| LAEF-022 | Governance Detail Design Specification | Approved v1.0 |
| LAEF-023 | Decision-Rights & Authority Matrix | Approved v1.0 |
| LAEF-024 | Approval Gates Specification | Approved v1.0 |
| LAEF-025 | Exception Handling Procedure | Approved v1.0 |
| LAEF-026 | Audit & Traceability Specification | Approved v1.0 |
| LAEF-027 | Compliance Verification Procedure | Approved v1.0 |

Only documents in **Approved** or **Active** status are authoritative, in accordance with the documentation lifecycle. Statuses in this index shall be updated as each document progresses.

---

# Reading Order

New contributors should read the LAEF governance documents in the following order. Each answers a distinct question:

1. **LAEF-001 — Framework Vision & Mission** — *why LAEF exists.*
2. **LAEF-002 — Core Principles & Philosophy** — *how engineers and the AI Agent should think.*
3. **LAEF-003 — Scope & Objectives** — *where LAEF begins, where it ends, and what success looks like.*
4. **LAEF-004 — Framework Architecture Overview** *(planned)* — *how LAEF is structured.*
5. **LAEF-005 — Governance Overview** *(planned)* — *how compliance is verified and enforced.*
6. **LAEF-006 — Versioning Strategy** *(planned)* — *how LAEF is versioned.*
7. **LAEF-007 — Framework Roadmap** *(planned)* — *how LAEF evolves over time.*

After the governance documents, contributors proceed to the **LAEF Workspace** (evolved from 99-AI-Team) for the execution assets — agents, workflows, and playbooks — that apply this governance.

---

# Related Knowledge Base Areas

- **02-Architecture — Architecture Repository.** LAEF governs *adherence* to approved architecture; the Architecture Repository owns architecture *content*. Authority does not overlap.
- **14-Standards — STD-016 AI-Governance-Standards (Planned).** STD-016 will reference LAEF (16-AI-Engineering-Framework) as the source of truth rather than redefining it.
- **05-Functional-Requirements — FR-AI-\*.** These define AI *product* features for end users (clinical assistant, decision support) — a distinct product domain, **out of scope** for LAEF.
- **01-Governance — GOV-001 through GOV-005.** All LAEF documents comply with the GOV documentation governance framework; they do not duplicate it.
- **15-Enterprise-Management — PROJECT_STATUS.** Tracks overall project and LAEF progress.

---

# Governance Ownership

| Role | Responsibility |
|------|----------------|
| **Owner — Enterprise AI Governance Office** | Prepares, maintains, and periodically reviews the LAEF governance documents in this area |
| **Approval Authority — Product Owner** | Approves all LAEF documents; no LAEF document is authoritative until approved |
| **Architecture Authority** | Resolves architecture and principle conflicts escalated in accordance with LAEF-002 |
| **Repository Maintainer** | Publishes approved LAEF documents into the repository and maintains cross-references |

These roles align with the responsibilities defined in GOV-004 Approval and Review Policy. One individual may hold multiple roles where governance integrity is preserved. Any exception to LAEF governance shall be documented, justified, approved, and traceable.

---

# Compliance

This area index is authoritative once approved by the Product Owner.

Every document in this area shall comply with the LOUTAS Care documentation governance framework, including the documentation lifecycle, GOV-002 Document Template Standard, and GOV-003 Document Numbering Standard. This document indexes and describes the area; it does not define framework content, which resides in the individual LAEF documents.

---

# Dependencies

- LAEF-001 Framework Vision & Mission
- LAEF-002 Core Principles & Philosophy
- LAEF-003 Scope & Objectives
- GOV-002 Document Template Standard
- GOV-003 Document Numbering Standard
- GOV-004 Approval and Review Policy
- GOV-005 Repository Governance

---

# Related Documents

- MASTER_INDEX
- LOUTAS Care Product Constitution (LC-BOOK-000)
- STD-016 AI-Governance-Standards *(planned)*
- LAEF Workspace — evolved from 99-AI-Team *(execution tier)*

---

# Revision History

| Version | Date | Description |
|---------|------|-------------|
| 1.0 (Draft) | 2026-07-29 | Initial draft issued for approval; expanded with Reading Order and Governance Ownership per Product Owner direction |
| 1.0 (Approved) | 2026-07-29 | Approved by Product Owner; closes the Foundation Governance milestone |

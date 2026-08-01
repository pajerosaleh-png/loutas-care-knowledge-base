# Changelog

All notable changes to the LOUTAS Care Knowledge Base are documented here.
The LOUTAS AI Engineering Framework (LAEF) follows the two-level versioning model defined in LAEF-006.

## Repository — 2026-07-31

### Added
- LICENSE — Proprietary, All Rights Reserved. Repository-level legal file; does **not** modify the LAEF v1.3 framework baseline or any LAEF document.

## LAEF v1.3 — 2026-07-31

**Phase 4 — Governance (complete). Governance baseline.**

### Added
- LAEF-022 Governance Detail Design Specification (Phase 4 scope map).
- Five detailed governance documents: LAEF-023 Decision-Rights & Authority Matrix, LAEF-024 Approval Gates Specification, LAEF-025 Exception Handling Procedure, LAEF-026 Audit & Traceability Specification, LAEF-027 Compliance Verification Procedure.

### Notes
- **Frozen 2026-07-31** as the permanent engineering baseline; future change only via approved ADR and a new release (v1.4+).
- Additive and backward-compatible; no v1.0–v1.2 document was changed.
- Each Phase 4 document expands exactly one LAEF-005 section under the LAEF-022 anti-duplication rules; LAEF-005 remains the authoritative overview.

## LAEF v1.2 — 2026-07-30

**Phase 3 — Core Engines (complete). Official stable baseline.**

### Added
- LAEF-013 Engine-Set Design Specification (engine architecture overview).
- Eight engine specifications: LAEF-014 Context, LAEF-015 Knowledge, LAEF-016 Task (cross-cutting), LAEF-017 Decision, LAEF-018 Workflow, LAEF-019 Agent, LAEF-020 Quality, LAEF-021 Learning.
- ADR-011 — Business Workflow Source of Truth (Workflow Engine engineering-only; Knowledge Engine reads the planned 17-Business-Workflows).

### Notes
- Additive and backward-compatible; no v1.0 or v1.1 document was changed.
- Engines reuse the Phase 2 layer contracts as their interfaces; no invariant, contract, or scope boundary altered.

## LAEF v1.1 — 2026-07-30

**Phase 2 — Framework Architecture (complete).**

### Added
- LAEF-008 Layer Architecture Foundations & Design Principles (architectural constitution).
- Tier architecture: LAEF-009 Foundation, LAEF-010 Knowledge & Context, LAEF-011 Execution, LAEF-012 Assurance & Evolution.

### Notes
- Additive and backward-compatible.

## LAEF v1.0 — 2026-07-30

**Phase 1 — Framework Foundation (complete).**

### Added
- New area 16-AI-Engineering-Framework with README-LAEF and LAEF-001 through LAEF-007.

### Changed
- Namespace migration of the LAEF documents from the 11-AI area to 16-AI-Engineering-Framework (LAEF- prefix), resolving a collision with the existing Artificial Intelligence Repository. The 11-AI area was left unchanged.

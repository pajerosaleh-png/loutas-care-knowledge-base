# LAEF v1.2 Release Manifest

| Field | Value |
|-------|-------|
| Document ID | LAEF-REL-1.2 |
| Document Title | LAEF v1.2 Release Manifest |
| Book | LAEF — LOUTAS AI Engineering Framework |
| Knowledge Base Area | 16-AI-Engineering-Framework |
| Release Version | LAEF v1.2 |
| Release Type | Minor (additive, backward-compatible) |
| Status | Released |
| Owner | Enterprise AI Governance Office |
| Approval Authority | Product Owner |
| Release Date | 2026-07-30 |

---

# Purpose

This manifest defines the **LAEF v1.2** release: the coherent, approved baseline produced by completing Phase 3 — Core Engines. It records the exact approved version of every document in the release, per LAEF-006 Versioning Strategy.

---

# Release Summary

LAEF v1.2 is a **minor, backward-compatible** release. It extends the v1.1 baseline with the Phase 3 engine documents (LAEF-013 through LAEF-021), which specify the architecture of the eight core engines that realize the layers defined in Phase 2. It reflects **ADR-011** (Workflow Engine engineering-only; Knowledge Engine reads the planned `17-Business-Workflows`).

No architectural invariant, layer contract, or scope boundary was changed; the engines reuse the Phase 2 layer contracts as their interfaces. LAEF v1.2 is therefore fully backward-compatible with v1.1 and v1.0; no migration is required.

---

# Included Documents

| Document | Title | Version | Status | Introduced |
|----------|-------|---------|--------|------------|
| README-LAEF | Area Index | 1.0 | Active | v1.0 |
| LAEF-001 | Framework Vision & Mission | 1.0 | Active | v1.0 |
| LAEF-002 | Core Principles & Philosophy | 1.0 | Active | v1.0 |
| LAEF-003 | Scope & Objectives | 1.0 | Active | v1.0 |
| LAEF-004 | Framework Architecture Overview | 1.0 | Active | v1.0 |
| LAEF-005 | Governance Overview | 1.0 | Active | v1.0 |
| LAEF-006 | Versioning Strategy | 1.0 | Active | v1.0 |
| LAEF-007 | Framework Roadmap | 1.0 | Active | v1.0 |
| LAEF-008 | Layer Architecture Foundations & Design Principles | 1.0 | Active | v1.1 |
| LAEF-009 | Foundation Tier Architecture | 1.0 | Active | v1.1 |
| LAEF-010 | Knowledge & Context Tier Architecture | 1.0 | Active | v1.1 |
| LAEF-011 | Execution Tier Architecture | 1.0 | Active | v1.1 |
| LAEF-012 | Assurance & Evolution Tier Architecture | 1.0 | Active | v1.1 |
| LAEF-013 | Engine-Set Design Specification | 1.0 | Active | **v1.2** |
| LAEF-014 | Context Engine Specification | 1.0 | Active | **v1.2** |
| LAEF-015 | Knowledge Engine Specification | 1.0 | Active | **v1.2** |
| LAEF-016 | Task Engine Specification | 1.0 | Active | **v1.2** |
| LAEF-017 | Decision Engine Specification | 1.0 | Active | **v1.2** |
| LAEF-018 | Workflow Engine Specification | 1.0 | Active | **v1.2** |
| LAEF-019 | Agent Engine Specification | 1.0 | Active | **v1.2** |
| LAEF-020 | Quality Engine Specification | 1.0 | Active | **v1.2** |
| LAEF-021 | Learning Engine Specification | 1.0 | Active | **v1.2** |

**Related decision:** ADR-011 — Business Workflow Source of Truth (Approved; recorded in the repository `ADR/` directory).

---

# Release Criteria (per LAEF-006)

| Criterion | Met |
|-----------|-----|
| Every included document is Approved / Active | Yes — all documents at v1.0 |
| All cross-references between included documents are consistent | Yes |
| Release Manifest complete with exact document versions | Yes — this manifest |
| Product Owner has approved the release as a baseline | Yes — approved 2026-07-30 |
| No open exception blocks the release | Yes — none open |

The release is **Active** as of Product Owner approval and is now immutable; any subsequent change produces a new release. LAEF v1.2 is the official stable baseline for Phase 4.

---

# Backward Compatibility

LAEF v1.2 is additive within the v1.x major line. All v1.0 and v1.1 documents are unchanged. The engines reuse the Phase 2 layer contracts as their interfaces; no invariant, contract, or scope boundary was altered. Contributors require no migration.

---

# Approval

| Field | Value |
|-------|-------|
| Prepared by | Enterprise AI Governance Office |
| Release Approval | Approved by Product Owner |
| Approval Date | 2026-07-30 |

---

# Revision History

| Version | Date | Description |
|---------|------|-------------|
| 1.2 (Prepared) | 2026-07-30 | Release manifest prepared on completion of Phase 3 (Core Engines); pending Product Owner release approval |
| 1.2 (Released) | 2026-07-30 | LAEF v1.2 approved by Product Owner as the official stable baseline (Phase 3 engines added to v1.1) |

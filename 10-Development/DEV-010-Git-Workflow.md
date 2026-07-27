# DEV-010-Git-Workflow.md

**Document ID:** DEV-010  
**Document Classification:** Development Standard  
**Owner:** Engineering Team  
**Status:** Approved  
**Version:** 1.0  
**Created:** 2026-07-27  
**Last Updated:** 2026-07-27  
**Next Review:** 2027-07-27  
**Approval Authority:** Chief Software Architect

---

# Git Workflow

## Purpose

This document defines the official Git workflow for the **LOUTAS Care Platform**.

The objective is to ensure:

- Consistent development practices
- Safe collaboration
- Stable releases
- Traceable changes
- High-quality source code
- Predictable deployment

This workflow is mandatory for all contributors.

---

# Objectives

The Git workflow shall:

- Protect production code
- Support parallel development
- Reduce merge conflicts
- Improve code quality
- Maintain complete history
- Enable safe releases

---

# Repository Model

The LOUTAS Care repositories use a **feature-based branching strategy** with protected long-lived branches.

```text
main
 │
 ├────────────── release/*
 │
 ├────────────── develop
 │                 │
 │                 ├──────── feature/*
 │                 ├──────── bugfix/*
 │                 ├──────── refactor/*
 │                 └──────── docs/*
 │
 └────────────── hotfix/*
```

---

# Branch Definitions

## main

Purpose:

Production-ready code only.

Characteristics:

- Always deployable
- Protected branch
- No direct commits
- Tagged releases only

---

## develop

Purpose:

Primary integration branch.

Characteristics:

- Active development
- Receives completed features
- Used for QA
- Stable enough for testing

---

## feature/*

Purpose:

Development of new functionality.

Examples:

```text
feature/appointment-calendar
feature/patient-banner
feature/billing-dashboard
feature/emr-ai-summary
```

Rules:

- Created from develop
- Merged back into develop
- Deleted after merge

---

## bugfix/*

Purpose:

Fix non-production issues.

Examples:

```text
bugfix/login-validation
bugfix/patient-search
bugfix/calendar-refresh
```

---

## hotfix/*

Purpose:

Critical production fixes.

Examples:

```text
hotfix/payment-crash
hotfix/security-patch
```

Workflow:

```text
main
   │
hotfix/*
   │
   ├──► main
   └──► develop
```

---

## refactor/*

Purpose:

Internal improvements without changing functionality.

Examples:

```text
refactor/appointment-service
refactor/prisma-layer
refactor/auth-module
```

---

## docs/*

Purpose:

Documentation updates only.

Examples:

```text
docs/adr-update
docs/readme
docs/api-reference
```

---

# Branch Protection Rules

The following branches shall be protected:

- main
- develop

Protection includes:

- No force push
- No direct commits
- Pull Request required
- Successful CI required
- Required approvals
- Conversation resolution required

---

# Feature Development Workflow

```text
Create Feature Branch
        │
        ▼
Develop Feature
        │
        ▼
Local Testing
        │
        ▼
Commit Changes
        │
        ▼
Push Branch
        │
        ▼
Create Pull Request
        │
        ▼
Code Review
        │
        ▼
CI Validation
        │
        ▼
Merge into develop
```

---

# Hotfix Workflow

```text
Production Issue
        │
        ▼
Create hotfix/*
        │
        ▼
Fix
        │
        ▼
Testing
        │
        ▼
Merge → main
        │
        ▼
Tag Release
        │
        ▼
Merge → develop
```

---

# Release Workflow

```text
develop
    │
    ▼
release/vX.Y.Z
    │
    ▼
QA
    │
    ▼
Bug Fixes
    │
    ▼
Approval
    │
    ▼
main
```

---

# Merge Strategy

The preferred merge strategy is:

**Squash and Merge**

Benefits:

- Cleaner history
- Easier rollback
- Better traceability
- Reduced commit noise

Merge commits may be used for release branches where preserving branch history is beneficial.

---

# Commit Frequency

Developers should:

- Commit frequently
- Keep commits small
- Ensure each commit builds successfully
- Avoid committing unrelated changes together

---

# Pull Request Requirement

Every code change must:

- Pass CI
- Pass automated tests
- Receive required approvals
- Resolve review comments
- Meet coding standards

Direct pushes to protected branches are prohibited.

---

# Conflict Resolution

Developers should:

- Rebase or merge from `develop` regularly
- Resolve conflicts locally
- Retest after conflict resolution
- Avoid large, long-lived branches

---

# Release Tags

Production releases shall follow Semantic Versioning.

Examples:

```text
v1.0.0
v1.1.0
v1.2.4
v2.0.0
```

---

# Forbidden Practices

The following are prohibited:

- Direct commits to `main`
- Force pushing protected branches
- Committing secrets
- Committing generated build artifacts
- Large unrelated commits
- Skipping code review
- Merging failing builds

---

# Integration with Repository Governance

This workflow complements:

- Development Principles
- Coding Standards
- Code Review Process
- Testing Strategy
- Release Documentation
- ADR Repository
- RFC Repository

All Git operations shall comply with the governance defined in those documents.

---

# Best Practices

Developers should:

- Keep branches short-lived
- Rebase frequently
- Write meaningful commit messages
- Review code before requesting approval
- Delete merged branches
- Sync with `develop` regularly
- Verify CI status before merging

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-27 | Initial Git Workflow standard |

---

**End of Document**

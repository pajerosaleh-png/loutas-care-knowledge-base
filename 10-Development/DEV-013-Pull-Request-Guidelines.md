# DEV-013-Pull-Request-Guidelines.md

**Document ID:** DEV-013  
**Document Classification:** Development Standard  
**Owner:** Engineering Team  
**Status:** Approved  
**Version:** 1.0  
**Created:** 2026-07-27  
**Last Updated:** 2026-07-27  
**Next Review:** 2027-07-27  
**Approval Authority:** Chief Software Architect

---

# Pull Request Guidelines

## Purpose

This document defines the official Pull Request (PR) process for the **LOUTAS Care Platform**.

All source code changes shall be introduced into protected branches through a Pull Request to ensure quality, consistency, security, and maintainability.

---

# Objectives

The Pull Request process aims to:

- Improve code quality.
- Enforce peer review.
- Prevent unstable code from reaching protected branches.
- Ensure coding standards are followed.
- Maintain a complete audit trail of changes.

---

# When to Create a Pull Request

A Pull Request shall be created when:

- A feature is complete.
- A bug has been fixed.
- Documentation changes are ready.
- Refactoring has been completed.
- A release branch is ready for review.
- A hotfix is prepared for production.

---

# Pull Request Workflow

```text
Feature Branch
      │
      ▼
Push Changes
      │
      ▼
Open Pull Request
      │
      ▼
Automated CI Checks
      │
      ▼
Code Review
      │
      ▼
Address Review Comments
      │
      ▼
Approval
      │
      ▼
Merge
```

---

# Pull Request Title

Titles should be concise and describe the purpose of the change.

Examples:

```text
Add patient timeline
Fix duplicate invoice generation
Improve appointment scheduler performance
Update billing documentation
```

---

# Pull Request Description

Every Pull Request should include:

- Summary of changes.
- Reason for the change.
- Related issue or task.
- Testing performed.
- Screenshots (if UI changes exist).
- Documentation updates (if applicable).

---

# Pull Request Checklist

Before requesting review, verify:

- Code builds successfully.
- Unit tests pass.
- No linting errors.
- Documentation updated if required.
- No secrets committed.
- No debug code remains.
- Related ADR/RFC references included when applicable.

---

# Draft Pull Requests

Draft Pull Requests should be used when:

- Development is still in progress.
- Feedback is requested before completion.
- Major architectural discussions are needed.

Draft Pull Requests shall not be merged.

---

# Code Review Requirements

Every Pull Request shall receive at least one approval before merging.

Reviewers should verify:

- Code quality.
- Readability.
- Performance.
- Security.
- Compliance with coding standards.
- Test coverage.

---

# Continuous Integration (CI)

A Pull Request shall not be merged unless:

- Build succeeds.
- Automated tests pass.
- Static analysis passes.
- Security checks pass.

---

# Merge Strategy

The preferred merge strategy is:

**Squash and Merge**

Advantages:

- Clean Git history.
- Easier rollback.
- Simplified release notes.
- Reduced commit noise.

---

# Merge Restrictions

Pull Requests shall not be merged if:

- CI fails.
- Required approvals are missing.
- Review comments remain unresolved.
- Merge conflicts exist.
- Branch is outdated and requires synchronization.

---

# After Merge

After successful merge:

- Delete the source branch.
- Verify CI completion.
- Confirm deployment pipeline execution.
- Update related work items if necessary.

---

# Related Documents

- DEV-009-Code-Review-Process.md
- DEV-010-Git-Workflow.md
- DEV-011-Branching-Strategy.md
- DEV-012-Commit-Message-Convention.md
- DEV-008-Testing-Strategy.md

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-27 | Initial Pull Request Guidelines |

---

**End of Document**

# DEV-012-Commit-Message-Convention.md

**Document ID:** DEV-012  
**Document Classification:** Development Standard  
**Owner:** Engineering Team  
**Status:** Approved  
**Version:** 1.0  
**Created:** 2026-07-27  
**Last Updated:** 2026-07-27  
**Next Review:** 2027-07-27  
**Approval Authority:** Chief Software Architect

---

# Commit Message Convention

## Purpose

This document defines the official Git commit message convention for the **LOUTAS Care Platform**.

A consistent commit history improves:

- Code traceability
- Release note generation
- Code review efficiency
- Change auditing
- Collaboration across development teams
- Future maintenance

This standard applies to all repositories within the LOUTAS Care ecosystem.

---

# Objectives

Commit messages shall:

- Clearly describe the change.
- Be easy to understand.
- Support automated tooling.
- Improve repository history.
- Enable efficient troubleshooting.
- Follow a consistent format.

---

# Commit Message Format

All commit messages shall follow the structure below.

```text
<type>: <short summary>

(optional body)

(optional footer)
```

Example:

```text
feat: add appointment confirmation workflow
```

---

# Commit Types

| Type | Description |
|------|-------------|
| feat | New functionality |
| fix | Bug fix |
| refactor | Internal code improvement without changing behavior |
| docs | Documentation changes |
| style | Formatting or styling changes only |
| test | Tests added or modified |
| perf | Performance improvement |
| build | Build system changes |
| ci | CI/CD configuration updates |
| chore | Maintenance tasks |
| revert | Revert previous commit |
| security | Security improvements |

---

# Examples

## New Feature

```text
feat: add patient insurance verification
```

---

## Bug Fix

```text
fix: prevent duplicate appointment creation
```

---

## Documentation

```text
docs: update billing workflow documentation
```

---

## Refactoring

```text
refactor: simplify appointment validation service
```

---

## Performance

```text
perf: optimize patient search query
```

---

## Testing

```text
test: add unit tests for invoice service
```

---

# Subject Line Rules

The subject line shall:

- Be written in English.
- Use the imperative mood.
- Start with a lowercase commit type.
- Not end with a period.
- Be concise.
- Preferably remain under 72 characters.

Correct:

```text
feat: add patient timeline
```

Incorrect:

```text
Added Patient Timeline.
```

---

# Commit Body

Use the body when additional explanation is required.

Example:

```text
feat: add invoice discount validation

Introduced configurable discount validation
based on clinic settings.

Added unit tests.
Updated documentation.
```

---

# Footer

The footer may reference:

- GitHub Issue
- Sprint
- ADR
- RFC
- Breaking Changes

Example:

```text
Closes #245
Sprint: Sprint-08
ADR: ADR-006
RFC: RFC-002
```

---

# Breaking Changes

Breaking changes shall include:

```text
BREAKING CHANGE:
```

Example:

```text
feat: redesign authentication middleware

BREAKING CHANGE:
Authentication tokens are now validated using
the new security service.
```

---

# Atomic Commits

Each commit shall represent one logical change.

Good:

```text
feat: add patient QR code
```

Bad:

```text
feat: patient QR + billing fix + login redesign
```

---

# Commit Frequency

Developers should:

- Commit frequently.
- Keep commits small.
- Avoid committing unfinished work.
- Push regularly to remote branches.

---

# Forbidden Commit Messages

The following commit messages are prohibited:

```text
update

changes

fix

temp

test

new

final

done

work

aaaa

123
```

These messages provide no meaningful project history.

---

# Linking to Work Items

When applicable, commits should reference:

- GitHub Issue
- Sprint Task
- Bug Report
- Feature Request
- ADR
- RFC

Example:

```text
feat: add billing configuration screen

Closes #178
ADR: ADR-001
```

---

# Merge Commits

Feature development should normally use **Squash and Merge**.

Automatically generated merge commit messages should be avoided unless preserving branch history is necessary.

---

# AI-Assisted Development

When AI tools contribute significantly to implementation:

- Developers remain responsible for reviewing all generated code.
- Commit messages shall describe the implemented change, not the tool used.
- Do not mention AI in commit messages unless explicitly required for audit purposes.

Correct:

```text
feat: add AI-generated clinical summary endpoint
```

Incorrect:

```text
feat: ChatGPT generated this code
```

---

# Best Practices

Developers should:

- Review commits before pushing.
- Use meaningful summaries.
- Keep one logical change per commit.
- Ensure code builds successfully before committing.
- Update documentation when applicable.

---

# Related Documents

- DEV-010-Git-Workflow.md
- DEV-011-Branching-Strategy.md
- DEV-009-Code-Review-Process.md
- DEV-003-Coding-Standards.md
- Release Documentation

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-27 | Initial Commit Message Convention standard |

---

**End of Document**

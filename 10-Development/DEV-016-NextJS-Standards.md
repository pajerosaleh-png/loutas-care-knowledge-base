# DEV-016-NextJS-Standards.md

**Document ID:** DEV-016  
**Document Classification:** Development Standard  
**Owner:** Engineering Team  
**Status:** Approved  
**Version:** 1.0  
**Created:** 2026-07-27  
**Last Updated:** 2026-07-27  
**Next Review:** 2027-07-27  
**Approval Authority:** Chief Software Architect

---

# Purpose

This document defines the official Next.js development standards for the LOUTAS Care Platform.

The objective is to establish a consistent project structure, improve maintainability, optimize performance, and ensure all developers follow the same architectural conventions.

These standards apply to all frontend modules developed using Next.js.

---

# Objectives

- Standardize application structure.
- Improve scalability.
- Reduce duplicated code.
- Improve maintainability.
- Follow modern Next.js best practices.
- Ensure consistent routing across modules.

---

# Project Structure

The application shall follow the App Router architecture.

Example:

```text
app/
components/
hooks/
lib/
services/
types/
utils/
styles/
```

Project folders should remain organized and modular.

---

# App Router

All new pages shall use the App Router.

Legacy Pages Router shall not be used for new development.

---

# Module Structure

Each feature module should contain:

```text
module/
    page.tsx
    layout.tsx
    loading.tsx
    error.tsx
    not-found.tsx
    components/
```

Only include files that are required by the module.

---

# Layouts

Shared layouts should be defined using layout.tsx.

Avoid duplicating navigation or page wrappers.

---

# Server Components

Server Components shall be used by default.

Use Client Components only when browser APIs or React Hooks are required.

---

# Client Components

Client Components must include:

```tsx
"use client";
```

Only use them when necessary.

---

# Routing

Use file-based routing.

Routes should remain descriptive.

Examples:

```text
/patients
/appointments
/billing
/settings
/reports
```

Avoid unnecessary nested routes.

---

# Loading UI

Modules performing asynchronous operations should implement:

```text
loading.tsx
```

to improve user experience.

---

# Error Handling

Modules should implement:

```text
error.tsx
```

to gracefully handle unexpected errors.

---

# Not Found Pages

Where appropriate, modules should provide:

```text
not-found.tsx
```

to improve navigation and usability.

---

# Data Fetching

Use Server Components for data fetching whenever possible.

Avoid unnecessary client-side requests.

---

# API Calls

API communication should be encapsulated inside service classes.

Do not call APIs directly from UI components unless justified.

---

# Environment Variables

Environment variables shall only be accessed through approved configuration mechanisms.

Sensitive values must never be exposed to Client Components.

---

# Performance

Developers should:

- Minimize unnecessary rendering.
- Optimize images.
- Lazy load large components.
- Keep bundle size small.

---

# Code Splitting

Use dynamic imports when loading large or rarely used components.

---

# Security

Never expose:

- API secrets
- Database credentials
- Authentication tokens

to the browser.

---

# Accessibility

Pages should:

- Use semantic HTML.
- Support keyboard navigation.
- Follow WCAG recommendations where practical.

---

# Related Documents

- DEV-014-TypeScript-Standards.md
- DEV-015-React-Standards.md
- DEV-017-Backend-Development-Standards.md
- DEV-006-Configuration-Management.md

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-27 | Initial Next.js Standards |

---

**End of Document**

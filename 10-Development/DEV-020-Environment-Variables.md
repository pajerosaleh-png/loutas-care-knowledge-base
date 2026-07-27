# DEV-020-Environment-Variables.md

**Document ID:** DEV-020  
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

This document defines the official standards for managing Environment Variables within the LOUTAS Care Platform.

The objective is to protect sensitive information, simplify deployments, and ensure consistent configuration across all environments.

These standards apply to all backend services, frontend applications, deployment pipelines, and third-party integrations.

---

# Objectives

- Protect sensitive credentials.
- Separate configuration from source code.
- Support multiple deployment environments.
- Improve security.
- Simplify infrastructure management.

---

# General Principles

Configuration values shall never be hardcoded inside the application source code.

All environment-specific settings shall be stored using Environment Variables.

---

# Supported Environments

The platform supports:

- Development
- Testing
- Staging
- Production

Each environment shall maintain its own configuration values.

---

# Naming Convention

Environment variable names shall:

- Use uppercase letters.
- Use underscores as separators.
- Be descriptive.

Examples:

```text
DATABASE_URL
JWT_SECRET
NEXTAUTH_SECRET
SMTP_HOST
SMTP_PORT
OPENAI_API_KEY
WHATSAPP_API_KEY
```

---

# Sensitive Variables

Sensitive values include:

- Database passwords
- API keys
- Authentication secrets
- Encryption keys
- Cloud credentials

These values shall never be committed to source control.

---

# Public Variables

Only variables intended for frontend use may use the approved public prefix.

Example:

```text
NEXT_PUBLIC_APP_NAME
NEXT_PUBLIC_API_BASE_URL
```

Public variables must never contain secrets.

---

# Local Development

Developers should use:

```text
.env.local
```

This file shall not be committed to Git.

---

# Example Template

Provide an example configuration file such as:

```text
.env.example
```

The example file shall include placeholders only.

Example:

```text
DATABASE_URL=
JWT_SECRET=
OPENAI_API_KEY=
SMTP_HOST=
SMTP_PORT=
```

No real credentials shall appear in this file.

---

# Source Control

The following files shall not be committed:

```text
.env
.env.local
.env.production
.env.staging
```

These files shall be included in `.gitignore`.

---

# Accessing Variables

Environment variables shall be accessed through approved configuration utilities.

Avoid scattering direct access throughout the application.

Where practical, centralize configuration access.

---

# Secret Rotation

Secrets should be rotated periodically.

If a secret is exposed:

- Generate a replacement immediately.
- Revoke the compromised secret.
- Update all affected environments.

---

# Logging

Environment variable values shall never be written to application logs.

---

# Deployment

Each deployment environment shall maintain independent configuration values.

Deployment pipelines should inject environment variables securely.

---

# Validation

Applications should validate required environment variables during startup.

Missing required variables should prevent application startup.

---

# Documentation

Every environment variable should be documented with:

- Name
- Purpose
- Required/Optional
- Default value (if applicable)

---

# Related Documents

- DEV-006-Configuration-Management.md
- DEV-017-Backend-Development-Standards.md
- SEC Security Documentation
- Deployment Documentation

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | 2026-07-27 | Initial Environment Variables Standard |

---

**End of Document**

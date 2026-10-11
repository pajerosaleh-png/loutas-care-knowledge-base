# LOUTAS Care API Registry

Status: Draft

Version: 1.2

Last Updated: 2026-10-11

This registry is being populated gate by gate. An endpoint listed here exists in
code on `main` unless stated otherwise. Endpoints of later gates are listed only under
"Planned" and are **not implemented**.

Conventions for every endpoint: JSON over HTTPS · `Authorization: Bearer <token>` ·
response envelope `{ ok: true, ... }` or `{ ok: false, code?, message }` · clinic
scope is taken from the authenticated session, never from client input.

---

## Specialty Registry (Gate 1)

Source: `loutascare-backend` · `src/routes/specialties.ts` · rules in
`src/lib/specialtyService.ts` · `main` (commit `6b13ac2`).
Architecture: `ADR/ADR-011-Multi-Specialty-Architecture.md`.

| Method | Path | Purpose | Authorisation |
|---|---|---|---|
| GET | `/specialties` | The platform registry with the caller's clinic state | Signed-in user |
| PATCH | `/clinic/specialties/:key` | Enable or disable a specialty for the caller's clinic | `specialties:manage` |
| PUT | `/clinic/specialties/primary` | Change the clinic's primary specialty | `specialties:manage` |

### GET /specialties

Response `200`:

```json
{
  "ok": true,
  "specialties": [
    {
      "key": "general-practice",
      "nameEn": "General Practice",
      "nameAr": "الممارسة العامة",
      "descriptionEn": "General medical consultations on the Core EMR.",
      "descriptionAr": "...",
      "icon": "🩺",
      "readiness": "available",
      "sortOrder": 10,
      "enabled": true,
      "isPrimary": true
    }
  ]
}
```

Returns the nine active registry entries ordered by `sortOrder`. `enabled` and
`isPrimary` are the state of the caller's clinic only.

### PATCH /clinic/specialties/:key

Request body: `{ "enabled": true }` or `{ "enabled": false }`.

Response `200`: `{ "ok": true, "changed": true, "specialties": [ ... ] }`.
`changed` is `false` when the specialty was already in the requested state (no
audit row is written in that case).

### PUT /clinic/specialties/primary

Request body: `{ "key": "dental" }`.

Response `200`: `{ "ok": true, "changed": true, "specialties": [ ... ] }`.
The previous primary becomes non-primary; exactly one primary remains.

### Errors

| Status | `code` | When |
|---|---|---|
| 400 | `ENABLED_REQUIRED` | PATCH body has no boolean `enabled` |
| 400 | `SPECIALTY_KEY_REQUIRED` | PUT body has no string `key` |
| 401 | — | Not signed in |
| 403 | — | Caller lacks `specialties:manage` |
| 404 | `SPECIALTY_NOT_FOUND` | Unknown or inactive specialty key |
| 409 | `PRIMARY_CANNOT_BE_DISABLED` | Disabling the primary specialty |
| 409 | `PRIMARY_MUST_BE_ENABLED` | Making a disabled specialty primary |
| 409 | `SPECIALTY_NOT_AVAILABLE` | Enabling a specialty whose readiness is `coming_soon` |
| 409 | `LAST_ENABLED_SPECIALTY` | The change would leave no enabled specialty |
| 409 | `SPECIALTY_WRITE_CONFLICT` | Concurrent change; retry |
| 409 | `PRIMARY_INVARIANT_VIOLATED` | Stored state does not have exactly one primary; the change is refused |

### Rules

- Tenant scope: `clinicId` comes from the session. A clinic id in the URL, query or body is ignored.
- Every successful change writes one audit row in the same transaction:
  `SPECIALTY_ENABLED`, `SPECIALTY_DISABLED` or `SPECIALTY_PRIMARY_CHANGED`,
  with `clinicId`, actor, entity `ClinicSpecialty` and a `changes` object.
- Invariants (enforced by the service): exactly one primary per clinic; the primary is enabled; at least one specialty is enabled.
- Legacy normalisation (`normalizeKey`) is a server-side, read-only helper. No Gate 1 endpoint rewrites a stored Appointment or Visit specialty value.

---

## Planned (not implemented)

| Gate | Endpoints | State |
|---|---|---|
| 2 | Specialty validation on `POST /appointments`, `PATCH /appointments/:id`, `POST /appointments/walk-in` | Planned |
| 3 | `specialtyKey` and `visitType` added to the visit summary of `GET /patients/:patientId/record` | Planned |
| 4–6 | Dental summary, chart, findings and examination endpoints | Planned |
| 7 | Treatment plan endpoints | Planned |
| 8 | Imaging metadata and forms integration | Planned |

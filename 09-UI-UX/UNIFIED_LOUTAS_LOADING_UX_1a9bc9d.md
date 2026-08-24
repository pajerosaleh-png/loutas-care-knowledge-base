# Implement unified LOUTAS loading UX

**Commit:** `1a9bc9d`  
**Commit Message:** `Implement unified LOUTAS loading UX`  
**Status:** Implemented, tested, committed, and pushed to GitHub.

## Purpose
This change introduced one unified LOUTAS loading experience. The standard pattern is:

```text
[ Animated LOUTAS Lotus ]

LOADING

SCREEN NAME...
```

Examples:

```text
LOADING
EMR...
```

```text
LOADING
DOCTOR WORKSPACE...
```

The dots animate during normal motion and remain static when reduced-motion is enabled.

## Previous Problems
Before this implementation, loading states were inconsistent:

- Doctor Workspace displayed `Loading your queue...`
- Doctor Workspace could display a full-screen teal overlay with a white Lotus
- Patient EMR used a separate loading presentation
- During overlapping conditions, two loaders could appear

## Final UX Decision
All content-area loading states use the reusable `LotusLoader` component.

Canonical content configuration:

```tsx
<LotusLoader
  tone="teal"
  size={118}
  screen="SCREEN NAME"
/>
```

The screen name is the only intentional visual difference.

## Doctor Workspace
Doctor Workspace now uses the inline content loader for:

- Initial load
- Refresh
- Start Visit refresh
- Complete Visit refresh
- Reopen Visit refresh
- Encounter save refresh

The final presentation is:

```text
[ Teal Animated Lotus ]

LOADING

DOCTOR WORKSPACE...
```

There should be:

- No teal full-screen scrim
- No white Lotus overlay
- No duplicate loader
- No old `Loading your queue...` text

## Patient EMR
While EMR data is loading, the EMR content area displays:

```text
[ Teal Animated Lotus ]

LOADING

EMR...
```

The loader is confined to the EMR content area and is driven by the existing loading state. It clears on both successful and failed requests through the existing `finally` behavior.

## Reusable Component
New file:

```text
components/LotusLoader.tsx
```

The component centralizes:

- Lotus visual
- Lotus animation
- `LOADING` label
- Screen label
- Animated dots
- Reduced-motion behavior
- Shared typography and spacing

Screens pass only the screen label:

```tsx
<LotusLoader tone="teal" size={118} screen="EMR" />
```

## Files Changed

### `components/LotusLoader.tsx`
New reusable loading component and shared loading infrastructure.

### `app/AppLayout.tsx`
Added `LoadingProvider` around the application layout. The provider remains available for future use. The Login animation itself was not changed.

### `app/doctor/page.tsx`
- Added `LotusLoader`
- Replaced the old queue loading text
- Preserved existing lifecycle refresh callers
- Removed Doctor Workspace use of the full-screen overlay
- Uses the inline loader for all workspace loading states

Current canonical usage:

```tsx
<LotusLoader
  tone="teal"
  size={118}
  screen="DOCTOR WORKSPACE"
/>
```

### `app/patients/PatientEMR.tsx`
- Added the reusable `LotusLoader`
- Replaced the previous loading presentation
- Removed duplicated loading markup

Current usage:

```tsx
<LotusLoader
  tone="teal"
  size={118}
  screen="EMR"
/>
```

Clinical rendering and business logic were not changed.

## Explicitly Not Changed
This implementation was limited to frontend loading UX.

Not changed:

- Backend
- API contracts
- Database
- Prisma schema
- RBAC
- Authentication
- Login animation
- Appointment lifecycle rules
- Start Visit business logic
- Complete Visit business logic
- Reopen Visit business logic
- Encounter business logic
- S12-A
- Clinical rendering logic

## Start Visit Experience

```text
START VISIT
    ↓
Local STARTING state
    ↓
No full-screen Doctor Workspace overlay
    ↓
Patient EMR opens
    ↓
Lotus / LOADING / EMR...
    ↓
Patient and visit data complete loading
    ↓
EMR content appears
```

## Governance Rule for Future Screens
Future screens should reuse `LotusLoader` instead of creating new loading markup.

Examples:

```tsx
<LotusLoader tone="teal" size={118} screen="PATIENTS" />
<LotusLoader tone="teal" size={118} screen="APPOINTMENTS" />
<LotusLoader tone="teal" size={118} screen="BILLING" />
<LotusLoader tone="teal" size={118} screen="INVENTORY" />
<LotusLoader tone="teal" size={118} screen="DASHBOARD" />
```

Do not create:

- New spinner designs
- Plain `Loading...` text
- Per-screen duplicate Lotus markup
- Different content-area sizes without an approved UX reason
- A full-screen overlay when an inline screen loader is the intended experience

## Accessibility and Reduced Motion
Reduced-motion behavior is preserved:

- Lotus becomes static
- Dots become static `...`
- Loading behavior and request completion remain unchanged

## Testing
Reported validation:

- esbuild clean
- Frontend test suite: `26/26`
- Doctor Workspace tests: `11`
- Reception API tests: `9`
- Appointment status tests: `4`
- Start Visit tests: `2`

The tested business logic was not modified.

## Manual Acceptance Checklist

### Patient EMR
- [x] Teal Lotus appears inside the EMR area
- [x] `LOADING` is visible
- [x] `EMR...` is visible
- [x] Loader clears after loading
- [x] Workspace navigation remains visible

### Doctor Workspace
- [x] Initial load uses the unified inline loader
- [x] Refresh uses the unified inline loader
- [x] Start Visit refresh uses the inline loader
- [x] Complete Visit refresh uses the inline loader
- [x] Reopen Visit refresh uses the inline loader
- [x] Encounter save refresh uses the inline loader
- [x] No full-screen teal overlay
- [x] No duplicate loaders
- [x] Old `Loading your queue...` text removed
- [x] Visual configuration matches EMR apart from the screen label

## Git Record

```text
1a9bc9d — Implement unified LOUTAS loading UX
```

Commit result:

```text
4 files changed
175 insertions(+)
12 deletions(-)
create mode 100644 components/LotusLoader.tsx
```

The commit was pushed successfully to the `main` branch.

Final verification:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

## Final Architecture Decision

LOUTAS Care uses a reusable Lotus-based component for content-area loading states.

Canonical pattern:

```text
LOUTAS LOTUS
LOADING
SCREEN NAME...
```

### Currently Implemented

| Screen | Loading Label |
|---|---|
| Patient EMR | `EMR...` |
| Doctor Workspace | `DOCTOR WORKSPACE...` |

### Canonical Component

```text
components/LotusLoader.tsx
```

### Canonical Content Configuration

```text
tone="teal"
size={118}
```

## Status

**Implementation Status:** Complete  
**Git Commit:** `1a9bc9d`  
**Commit Title:** `Implement unified LOUTAS loading UX`  
**Scope:** Frontend loading UX only  
**Repository Status:** Successfully pushed to GitHub  
**Working Tree:** Clean

Future LOUTAS Care screens should reuse the unified `LotusLoader` pattern.

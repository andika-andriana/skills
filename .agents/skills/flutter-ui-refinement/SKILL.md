---
name: flutter-ui-refinement
version: 1.0.0
description: Refine Flutter visual and motion implementation to match approved Figma without changing UX flow, layout hierarchy, business logic, or design tokens. Use when implementing or polishing screens/widgets/transitions in this repository according to UI_IMPLEMENTATION_GUIDE.md.
metadata:
  requires: []
---

# UI Implementation Refinement

## Overview

Implement UI polish only. Keep design intent, information architecture, and feature behavior unchanged.

Treat `references/UI_IMPLEMENTATION_GUIDE.md` as the canonical project standard and load only the sections needed for the current task.

## Project Structure Read Pass (Required)

Before any UI refinement, run a short structure read pass to avoid architecture drift:

1. Identify where the target screen/widget is composed (feature entry file and route binding).
2. Trace direct dependencies used by that screen/widget (shared widgets, constants, theme, motion helpers).
3. Confirm state/data flow boundaries so visual changes do not modify business logic or state management.
4. Note the minimum safe edit surface (files and widgets that can be refined without changing hierarchy/contracts).

If structure ownership is ambiguous, stop and ask for clarification before editing.

## Execute Workflow

1. Identify target UI scope (screen, reusable widget, transition, or modal behavior).
2. Complete the required project structure read pass.
3. Open only relevant guide sections using the map in `references/guide-section-map.md`.
4. Audit current implementation against mandatory rules (`MUST` / `MUST NOT`).
5. Apply visual refinements at widget composition level only (spacing, typography application, wrappers, shadows, icon rendering).
6. Apply motion refinements with existing project motion defaults and conventions.
7. Verify no architecture, token-source, layout hierarchy, or business-logic drift.
8. Run formatting and relevant analysis/tests for touched files.

## Enforce Non-Negotiables

- Preserve approved Figma structure and UX flow.
- Refine implementation only; do not redesign.
- Do not edit design token sources or generated token files.
- Do not introduce a new design system.
- Prefer existing theme/tokens/components before introducing new primitives.
- Escalate ambiguity as a blocker; do not make unilateral design decisions.

## Apply Project Conventions

- Render header back navigation with `Assets.images.icLeftDirectionalBlack` via `ImageIcon(...provider())`; avoid `Icons.arrow_back` unless explicitly approved.
- Use `showAppModalBottomSheet` instead of direct `showModalBottomSheet` for app modal sheets.
- For login/register-style content transitions, animate selected content blocks (ordered `FadeinSlide`) and do not animate the selector control.
- Respect reduced-motion accessibility settings and degrade to minimal/no motion when enabled.
- For immediate-input screens (OTP/PIN/code), request focus from post-frame callback in `initState`; avoid initial `autofocus: true` route-push behavior.

## Use Motion Defaults

When design does not specify exact motion values, use:

- `kMotionDurationBase` (500ms)
- `kMotionCurveStandard` (`Curves.easeInOutCubic`)

For modal bottom sheets, prefer shared helper defaults unless approved override is required.

## Edit Boundaries

Focus edits in:

- `lib/theme/`
- `lib/constants/`
- `lib/widgets/`
- `lib/features/`
- `lib/routes/`

Keep reusable component responsibilities intact. Refine appearance, transitions, and minor interaction states only.

## References

- Canonical guide: `references/UI_IMPLEMENTATION_GUIDE.md`
- Section lookup: `references/guide-section-map.md`

# UI Implementation: Visual & Motion Refinement — Scope Overview

This document defines a **project-specific foundation** for Flutter development in this codebase.
It is designed to guide **this project**, whether work is done by humans or AI Agents.

The guidelines focus on **clarity, consistency, maintainability, and UI slicing discipline**, without being tied to any specific application domain.

All rules marked with **MUST** are mandatory and non-negotiable.

---

## Normative Keywords

* **MUST** — mandatory
* **SHOULD** — strongly recommended unless it conflicts with existing code
* **MAY** — optional

---

## Purpose

This task focuses on **visual and motion refinement at the UI implementation level** so the app’s look and animations **match the approved Figma**, without changing:

* design intent
* UX flow
* layout hierarchy
* business logic

This task is **purely engineering execution**, not design decision-making.

---

## In-Scope Responsibilities

### 1. Visual Consistency (Implementation-Level)

**Primary touchpoints:**
`lib/theme/`, `lib/constants/`, `lib/widgets/`

* Align spacing, padding, margin, typography, color, border radius, elevation, and shadow to match the approved Figma.
* Use the existing theme and design tokens:

  * `ThemeData`, `ColorScheme`
  * `lib/constants/dimens.dart`
  * generated assets (`colors.gen.dart`, `fonts.gen.dart`)
* Visual adjustments **MUST happen at the widget implementation level**
  (e.g. padding, layout composition, wrapper widgets).
  * **MUST NOT** change sizes unless they match the approved Figma.
* **MUST NOT modify source design tokens** or generated files.
* **MUST NOT introduce a new design system or token set** unless there is a clear and approved technical requirement.

> Clarification:
> If an exact Figma value does not exist as a token, choose the **closest existing token** and adjust via widget composition — never by editing the token source.

#### Header Back Icon Consistency (Project Convention)

For screen headers that use a back button:

* **MUST** use the project asset icon `Assets.images.icLeftDirectionalBlack` for back navigation to keep visual consistency with approved UI.
* **MUST** render it via `ImageIcon(Assets.images.icLeftDirectionalBlack.provider(), ...)` (or equivalent generated asset usage).
* **SHOULD** keep icon size at `24` unless Figma explicitly specifies a different value.
* **MUST NOT** use default `Icons.arrow_back` on production screens unless explicitly required by approved design.

---

### 2. Motion & Animation Refinement

**Primary touchpoints:**
`lib/routes/`, `lib/widgets/`, `lib/features/`

* Refine animation duration, easing, and transitions for consistency and smoothness.
* If duration or curve is not explicitly provided, use defaults from `lib/constants/motion.dart`:
  * `kMotionDurationBase` (500ms)
  * `kMotionCurveStandard` (`Curves.easeInOutCubic`)
* Includes:

  * page transitions (`router.dart`)
  * micro-interactions (tap / press feedback)
  * implicit animations (`AnimatedSwitcher`, `AnimatedPositioned`, etc.)
* Focus on tuning and polish only.
* **MUST NOT introduce new motion patterns or experimental animations.**

#### Modal Bottom Sheet Motion (Project Convention)

To ensure consistent modal bottom sheet motion across the app:

* **MUST** use the shared helper `showAppModalBottomSheet` (from `lib/widgets/app_modal_bottom_sheet.dart`)
  for all modal bottom sheets instead of calling `showModalBottomSheet` directly.
* The helper applies the default `AnimationStyle` using:
  * `Duration(milliseconds: 400)` for `duration` and `reverseDuration`
  * `Curves.ease` for `curve` and `reverseCurve`
* **MAY** override `sheetAnimationStyle` only when a specific design requirement is approved.

#### Ordered Fade/Slide For Section Blocks (Project Convention)

When simulating tab transitions between Login/Register, the UI should feel
smooth without animating the tab control itself.

* Apply `FadeinSlide` per **selected content block** (ordered/staggered), not
  to the entire screen.
* **MUST NOT** animate the tab selector widget (it is manipulated separately).
* Use small, progressive delays (e.g., 60ms increments) to create an elegant
  ordering effect.
* Keep duration/curve aligned with `kMotionDurationBase` and
  `kMotionCurveStandard` unless explicitly specified.
* **SHOULD** prioritize animation only on high-impact interactive blocks
  (for example: primary input section, secondary input section, primary CTA).
* **SHOULD NOT** animate every section on a dense form screen, because
  over-animation can make the layout feel unstable/“bergoyang”.
* As a practical baseline for form screens, animate only **2-3 key blocks**
  unless Figma explicitly requires broader motion coverage.
* **SHOULD** keep slide distance subtle and consistent for form entrance
  animations (recommended `FadeinSlide.offset`: `Offset(0, 12)` to
  `Offset(0, 16)`).
* **SHOULD NOT** use large entrance offsets on form screens, because larger
  movement can reduce perceived stability and elegance.
* **MUST** respect reduced-motion accessibility preferences when available
  (for example platform/app-level “reduce motion” settings).
* When reduced-motion is enabled, entrance animations **SHOULD** degrade
  gracefully to minimal or no motion while preserving layout and hierarchy.

#### Input Auto Focus on Screen Entry (Project Convention)

For screens with immediate text input intent (e.g. OTP, PIN, code entry):

* **MUST NOT** trigger input focus using `autofocus: true` during initial route push.
* **MUST** schedule initial focus from `initState` using
  `WidgetsBinding.instance.addPostFrameCallback(...)`.
* **SHOULD** add a short post-transition delay for smoother keyboard appearance
  when required by design polish (example used in this project:
  `Durations.extralong4` before `requestFocus()`).
* **MUST** guard focus requests with mounted checks.
* **SHOULD** centralize focus logic in a helper method (for example,
  choosing between phone/email field based on current state) so the same
  behavior can be reused on mode switch.

---

### 3. Figma Alignment

**Primary touchpoints:**
All visible UI layers

* UI **MUST be implemented strictly based on the approved Figma**.
* **MUST NOT change:**

  * layout structure
  * visual hierarchy
  * information architecture
  * user flow
* If ambiguity or mismatch exists:

  * communicate with the UI/UX team
  * **MUST NOT make unilateral design decisions at the engineering level**

---

### 4. Reusable Component Enhancement

**Primary touchpoints:**
`lib/widgets/`

* Existing reusable components MAY be refined.
* Allowed improvements are limited to:

  * visual consistency
  * animation smoothness
  * minor UI behavior (hover, pressed, disabled states)
* **MUST NOT:**

  * change component responsibility
  * change layout hierarchy inside components
  * add or remove visible UI elements
* The existing component architecture **MUST remain intact**.

> Rule of thumb:
> You may refine *how a component looks and animates*,
> but not *what the component structurally is*.

---

## Status Bar Wrapper (Reusable Widget)

When a screen needs to control the system status bar style, use the shared
`StatusBar` widget stored in `lib/widgets/status_bar.dart`.

### Usage

```dart
import 'package:ocx_mobile/widgets/widgets.dart';

StatusBar(
  statusBar: SystemUiOverlayStyle.dark,
  child: Scaffold(
    body: Center(child: Text('Hello World')),
  ),
)
```

---

## Explicitly Out of Scope

The following are **strictly forbidden**:

* UI/UX design or visual exploration
* Redesign or UX flow changes
* New asset slicing from Figma
* Business logic or state management changes
* New feature additions

---

## Ownership Clarification

* Design authority remains fully with the UI/UX team.
* This task is **implementation polish**, not design iteration.
* All changes **MUST stay within**:

  * approved Figma
  * existing architecture
  * existing component contracts

---

## Outcome Expectation

* UI is precise, consistent, and visually refined.
* Motion feels intentional and cohesive.
* The app is ready for QA or release without visual debt.

---

# B. UI Slicing & Figma Workflow (Optional, Project-Dependent)

This section defines recommended practices for UI slicing and Figma-to-code
workflow. It focuses on **code structure and implementation discipline**, not
visual or UX changes.

---

## 1. Figma as the Source of Truth

* **MUST** use MCP when a Figma link is provided (if MCP is available in the workflow)
* **MUST** extract layout, spacing, typography, colors, radius, and UI states directly from Figma
* **MUST NOT** guess or infer design values that are not explicitly defined
* If a Figma font is unavailable in the project:
  * **MUST** keep using the existing project font(s) as the fallback
  * **MUST NOT** add new fonts or change font sources without explicit approval

> Any ambiguity or missing specification in Figma **MUST be escalated** to the UI/UX team before implementation.

---

## 2. UI Slicing Principles

* UI **MUST** be sliced based on **Figma sections or logical UI blocks**
* Screens **MUST** act as **composers**, not detailed renderers
* As a default, **one major UI section SHOULD map to one file**
* Reusable or shared sections **MUST** live in `lib/widgets/`

> UI slicing affects **code organization only** and **MUST NOT** alter layout
> structure, hierarchy, or visual behavior defined in Figma.

---

## 6A. UI State Handling via Reusable Containers (MANDATORY)

### Core Rule

**Loading state is a container concern — not a screen concern.**

### Prohibited Pattern

```dart
if (isLoading) ...
else ...
```

### Required Pattern

* Loading state **MUST** be handled by a reusable container (e.g. `ShimmerContainer`)
* Screens and sections **MUST NOT** branch on loading
* Loading **wraps content**, it does not replace it
* Error and empty states **live inside the content tree**
* Loading UI **MUST use project shimmer widgets**

### Canonical Widget Tree

```
ShimmerContainer
│
├── Loading State
│   └── ShimmerWidget / ShimmerCard
│
└── Content
    ├── Data State
    ├── Error State
    └── Empty State
```

---

## Final Note for AI Agents & Engineers

This document prioritizes:

* predictability
* consistency
* long-term maintainability

When in doubt:

* **do not guess**
* **do not redesign**
* **escalate ambiguity to UI/UX**

# Guide Section Map

Use this file to load only the required section(s) from `UI_IMPLEMENTATION_GUIDE.md`.

## Section Index

- `Visual Consistency (Implementation-Level)`
- `Header Back Icon Consistency (Project Convention)`
- `Motion & Animation Refinement`
- `Modal Bottom Sheet Motion (Project Convention)`
- `Ordered Fade/Slide For Section Blocks (Project Convention)`
- `Input Auto Focus on Screen Entry (Project Convention)`
- `Figma Alignment`
- `Reusable Component Enhancement`
- `Status Bar Wrapper (Reusable Widget)`
- `Explicitly Out of Scope`

## Fast Search Patterns

```bash
rg -n "Visual Consistency|Header Back Icon Consistency|Motion & Animation Refinement|Modal Bottom Sheet Motion|Ordered Fade/Slide|Input Auto Focus|Figma Alignment|Reusable Component Enhancement|Status Bar Wrapper|Out of Scope" UI_IMPLEMENTATION_GUIDE.md
```

## Load Strategy

1. Load only sections directly related to the active file/task.
2. Treat every `MUST` and `MUST NOT` as mandatory.
3. If a requirement conflicts with current code, keep architecture/logic stable and apply minimum UI-layer change.
4. If design ambiguity remains after reading relevant sections, stop and ask for design confirmation.

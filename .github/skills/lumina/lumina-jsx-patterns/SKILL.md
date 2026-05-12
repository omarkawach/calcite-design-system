---
name: lumina-jsx-patterns
description: Apply Lumina JSX conventions when authoring or refactoring component TSX render logic to keep events, refs, and rendering behavior correct.
---

# Lumina JSX Patterns

Apply Lumina-specific JSX patterns that compile cleanly to Lit templates and preserve performance/type-safety.

## Use when

- You are writing or refactoring `render()` output in Lumina.
- JSX event names/refs/list rendering are behaving unexpectedly.
- You need to embed custom elements from internal or external libraries.

## Steps

1. Ensure `.tsx` and `import { h } from "@arcgis/lumina"` are present.
2. Use `class` (not `className`) and camelCase aria props (for example `ariaCurrent`).
3. Use native event casing like `onClick` for DOM events.
4. Use lowercase custom event binding shape: `onarcgisOverflow`, `oncalciteAlertClose` (no capitalization after `on`).
5. Use stable refs via `createRef()` or stable methods; avoid inline ref lambdas when repeated calls matter.
6. Use `key={}` only for reorderable/heavy lists; skip it for simple stable lists.
7. Prefer explicit attributes over spread props for static analyzability and better diff performance.
8. Methods passed to ref, on jsx props and this.listen are bound automatically. For everything else, prefer class-field arrow handlers (for example `private onX = (...) => {}`) over manual `.bind(...)` fields to keep event callbacks stable and avoid duplicate bound members.
9. Avoid creating parallel `*Bound` handler fields just to preserve `this`; prefer the original method in JSX/listen contexts or a single arrow handler when binding is actually needed outside those contexts.
10. For external custom elements, keep package dependencies aligned and rely on compiler-managed element imports.

## Checks

- JSX events/refs work with correct casing and types.
- Re-renders do not churn refs or handlers.
- No spread-attribute typing/perf regressions.
- No duplicate bound-handler fields exist when Lumina already auto-binds the callback site.

## Example prompts

- "Refactor this Lumina `render()` to use correct custom event casing and stable refs."
- "Fix JSX typing errors for `calcite-*` and `arcgis-*` events in this TSX file."

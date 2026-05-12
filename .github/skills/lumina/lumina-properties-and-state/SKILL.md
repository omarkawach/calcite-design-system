---
name: lumina-properties-and-state
description: Design robust Lumina reactive properties and state when defining or refining component APIs in TSX, including defaults, typing, and update semantics.
---

# Lumina Properties and State

Design robust reactive property/state APIs in Lumina with correct defaults, typing, and update semantics.

## Use when

- Adding new public inputs or internal reactive state.
- Fixing re-render behavior tied to property changes.
- Hardening API ergonomics for HTML, JS, and JSX consumers.
- Choosing between component-local state, controllers, `Accessor`-style models, or deeper shared state.

## Steps

1. Choose the smallest state model that fits the job before adding fields:
   - `@property()` for public API.
   - `@state()` for component-local reactive UI state.
   - A controller when stateful behavior is reusable or lifecycle-heavy.
   - An `Accessor` or viewmodel-style model when you need Accessor capabilities (e.g., computed properties/dependency tracking) for an `@arcgis/core`-heavy part of the implementation; `reactiveUtils` can still be used directly in components/controllers.
   - Context or other shared-state plumbing only when prop passing is no longer reasonable for a deeper tree.
2. Use `@property()` for public API and `@state()` for internal reactive data.
3. Keep boolean public properties defaulted to `false`; invert naming if needed (`disabled`, `hideX`).
4. Do not reflect properties unless necessary for styling or interoperability.
5. Prefer explicit literal unions over broad `string` when values are finite.
6. For complex values, expect JS/JSX assignment; provide HTML-friendly alternatives if needed.
7. Use `@property({ readOnly: true })` + `bypassReadOnly()` or expose getter-based read-only wrappers.
8. Mark truly required properties and still provide runtime fallback/warning paths.
9. Use custom getter/setter for validation/casting and advanced change control.
10. Handle updates in `willUpdate(changes)` and remember first cycle includes initialized reactive values.
11. Avoid introducing global or singleton state for reusable components unless singleton semantics are intentional.
12. Determine the existing state approach before changing it:
    - Inspect 2-3 nearby components and related controllers/models/tests to identify where state is kept.
    - Treat these as the main patterns to look for: component-local Lumina state on the component class, controller-owned state, `Accessor`/viewmodel state, or shared context/store state.
    - Do not classify by decorator token alone (`@property()` also appears in `@arcgis/core` Accessor classes); confirm by file role and imports (e.g., Lumina component/controller).
    - Match the dominant pattern for the current change unless the task explicitly asks to migrate state patterns.

## Checks

- Property typing is strict and discoverable.
- HTML/JS/JSX usage works as intended.
- Re-renders occur only when meaningful changes happen.
- The chosen state model is no broader than the behavior requires.

## Example prompts

- "Define Lumina properties for a filter panel with strict unions and safe defaults."
- "Add validation setter logic for a numeric property without breaking reactivity."

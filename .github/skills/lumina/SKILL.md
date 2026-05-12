---
name: lumina
description: Top-level router for Lumina component work. Selects only the relevant sub-skill(s) and docs for Lumina packages, including migration, state, lifecycle, events, t9n, testing, SSR, and build concerns.
---

# Lumina Skill Router

Determine the correct Lumina sub-skills to use for a task, then apply only the relevant ones.

## What Lumina Is

Lumina is Esri's web component development system built on Lit and Vite.

It is composed of:

- `@arcgis/lumina`: runtime APIs for components, lifecycle, and controllers.
- `@arcgis/lumina-compiler`: build tooling (via `useLumina()` in Vite) that compiles components and generates docs/typings outputs.

Key capabilities include:

- JSX support on top of Lit.
- A controller system for reusable stateful logic.
- Build-time API docs and typings generation.
- Support for both lazy and non-lazy component loading outputs.
- Integration paths for `@arcgis/core` Accessor/viewmodel patterns when needed.

## Use when

- Work is in Lumina component packages or closely related Lumina controller/test surfaces.
- The task references Lumina-specific APIs or concepts (for example `@arcgis/lumina`, ElementInternals, Lumina lifecycle/event patterns).
- A `.tsx` file is involved and nearby code or imports indicate Lumina component work (`.tsx` alone is not enough).

Do **not** use this router for React-only work (for example app-level React files or React 18 wrapper-only changes).

## Routing rules

1. Always start by checking trigger conditions:
   - First, confirm target files are Lumina component/controller surfaces and not React-only surfaces.
   - If target files are mixed or unclear, require Lumina API/import evidence before activation.
     - Prefer `@arcgis/lumina` and Lumina lifecycle/event patterns as primary confirmation.
   - Treat `.tsx` and `@property()`/similar decorator usage as non-decisive signals only, since these patterns can also appear in Core and other non-Lumina code.
   - If still ambiguous, ask for exact target files before applying Lumina sub-skills.
2. Select sub-skills by change type:
   - New component scaffolding/setup → `lumina-new-component`
   - Stencil, widget, or legacy Lumina migration work → `lumina-migration`
   - Assets and asset paths → `lumina-assets`
   - Styling, shadow DOM, or external CSS → `lumina-styling`
   - JSX render/event/ref/list patterns → `lumina-jsx-patterns`
   - Public API reactivity (`@property()`) or internal state (`@state()`) → `lumina-properties-and-state`
   - Public methods or imperative API → `lumina-methods`
   - Custom events or `arcgisPropertyChange` behavior → `lumina-events-and-property-change`
   - Lifecycle hook placement or async loading flow → `lumina-lifecycle-and-async`
   - Race condition investigation or non-deterministic async bugs → `lumina-race-condition-detection`
   - Localization/messages/locale behavior → `lumina-t9n-localization`
   - Reusable controller design/extraction → `lumina-controller-authoring`
   - Vitest browser tests for Lumina components → `lumina-testing-vitest-browser`
   - ElementInternals or form association → `lumina-element-internals`
   - SSR compatibility or hydration issues → `lumina-ssr`
   - Workers or worker bundling → `lumina-workers`
   - Build config or publishing/exports → `lumina-build-and-publishing`
3. If the task spans multiple areas, compose multiple sub-skills in this order:
   - API and state
   - events
   - lifecycle/async
   - JSX/render
   - localization
   - tests
4. Keep scope minimal: apply only the sub-skills needed for the requested change.

## Cross-cutting Type Rules

- Do not define local replacement types for values from third-party libraries or other internal packages.
- Import source-of-truth types directly from the owning library/package.
- Do not use unsafe casts (for example `as unknown as MyType`) to force incompatible values through type checks.

## Checks

- Trigger conditions were checked.
- React-only targets were excluded.
- Selected sub-skill(s) match the change type.
- No unrelated Lumina guidance was applied.
- Type guidance imports source-of-truth types and avoids local replacement types.

## Example prompts

- "Update this component to emit `arcgisPropertyChange` for `value`."
- "Add T9N and tests for a component."

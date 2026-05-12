---
name: lumina-migration
description: Migrate Stencil, widget-based, or other legacy component code into repo-aligned Lumina patterns without carrying forward obsolete lifecycle or event behavior.
---

# Lumina Migration

Migrate legacy component code to Lumina in a way that preserves behavior while converging on current repo conventions instead of mechanically translating every old pattern.

## Use when

- Migrating Stencil components to Lumina.
- Moving widget-driven components toward native Lumina component structure.
- Refactoring older Lumina or Lit code that still carries pre-Lumina lifecycle, event, or DOM assumptions.

## Steps

1. Start with deterministic codemods for large-scale migration where applicable (for example `@arcgis/stencil-to-lumina` and the Widgets-to-Lumina codemod in `@arcgis/map-components-migration`), then use agents for TODOs, test migration, and remaining TypeScript/ESLint fixes.
2. Identify the migration surface first: public properties, methods, events, slots, styling hooks, t9n assets, and tests that define current behavior.
3. Preserve the public contract unless the task explicitly includes an API change; migration is not a license to redesign everything at once.
4. Replace DOM assumptions that are unsafe in Lumina lazy-loading flows; prefer `this.el` for host-element DOM work.
5. Replace manual add/remove listener bookkeeping with `this.listen()`, `this.listenOn()`, or lifecycle-safe controller wiring where possible.
6. Move property-reaction logic into `willUpdate(changes)` unless the code truly needs post-render DOM access.
7. For widget or viewmodel migration, treat `useWidget()` / `useViewModel()` as transitional helpers rather than the desired end state; prefer native Lumina logic when the scope allows it.
8. Re-evaluate state shape during migration: keep local UI state in `@state()`, reusable lifecycle-aware logic in controllers, and `@arcgis/core`-heavy state in `Accessor` or viewmodel-style models.
9. Do not leave hardcoded user-facing strings behind; migrate them to t9n assets and `useT9n()` when the component renders user-visible copy.
10. Keep the change incremental when the old component is large: preserve behavior first, then extract follow-up cleanup if deeper redesign is needed.
11. Update or add focused browser-mode tests around migrated behavior so the translation is behavior-backed rather than purely structural.

## Checks

- Codemods handled primitive/lifecycle translation first, and follow-up edits focused on behavior, architecture, and cleanup work.
- Public API behavior is preserved unless the task explicitly changes it.
- DOM-facing code uses Lumina-safe host access patterns.
- Migration did not leave user-visible strings, stale widget assumptions, or untested behavioral changes behind.

## Example prompts

- "Migrate this Stencil component to Lumina and preserve its public API."
- "Convert this widget-backed component to Lumina patterns without carrying over duplicate listener setup."
- "Review this partial migration and identify remaining non-Lumina lifecycle or event patterns."

---
name: lumina-t9n-localization
description: Implement Lumina localization with useT9n when adding translated strings to TSX components and wiring locale-aware t9n assets and message overrides.
---

# Lumina T9n Localization

Implement Lumina localization using `useT9n()` with correct asset layout, locale handling, and consumer overrides.

## Use when

- Adding localized UI strings to a component.
- Debugging missing/undefined messages at first render.
- Exposing a `messageOverrides` API for consumers.

## Steps

1. Create package-level `src/controllers/useT9n.ts` once for correct package runtime asset resolution.
2. Add `messages.en.json` at `src/components/<name>/assets/t9n/messages.en.json`.
3. Ensure naming conventions match component folder/file names and T9N file naming rules.
4. In component, initialize `_messages` with `useT9n()`.
5. Choose blocking mode (`{ blocking: true }`) only when first render must wait for strings.
6. In non-blocking mode, guard render paths so missing keys do not break first render.
7. For non-internal components, expose `messageOverrides` typed from `_messages._overrides`.

   ```ts
   import { property } from "@arcgis/lumina/decorators";

   @property() messageOverrides?: typeof this._messages._overrides;
   ```

8. Support localizable label pattern: optional `label` prop + `componentLabel` T9N key.
9. In non-monorepo setups, ensure T9N manifest registration is updated for translation discovery.

## Checks

- English strings load from expected assets path.
- Render does not assume blocking `useT9n` load.

## Example prompts

- "Add localization to this Lumina component."
- "Make render() not assume blocking useT9n load."

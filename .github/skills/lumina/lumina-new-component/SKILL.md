---
name: lumina-new-component
description: Scaffold a production-ready Lumina component in TSX when creating a new component in a Lumina package.
---

# Lumina New Component

Create a production-ready Lumina component skeleton that follows package/file conventions and avoids common startup mistakes.

## Use when

- You need a new component like `arcgis-foo-bar`.
- You want to scaffold structure before implementing behavior.

## Steps

1. Create the component at `src/components/<name>/<name>.tsx` where `<name>` matches the folder name.
2. Start from a minimal `LitElement` class and keep the file as `.tsx`.
3. Use JSX rendering and include `import { h } from "@arcgis/lumina"`.
4. Define initial `@property()` inputs with safe defaults; keep booleans defaulting to `false`.
5. Keep shadow DOM enabled unless there is a hard requirement; if disabled, set `static override shadowRootOptions = noShadowRoot` and plan manual style scoping.
6. Put component-specific static files in `src/components/<name>/assets`.
7. Add a basic render path that works even when optional inputs are unset.

## Checks

- File/folder names match the component name.
- Component renders without runtime errors.
- Public properties update reactively.

## Example prompts

- "Create `arcgis-status-pill` as a Lumina component with one `label` prop and a basic render."
- "Scaffold a new Lumina component under `src/components/results-panel/results-panel.tsx` with default shadow DOM."

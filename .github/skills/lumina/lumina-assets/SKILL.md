---
name: lumina-assets
description: Apply Lumina asset placement and getAssetPath usage for component and package assets.
---

# Lumina Assets

Use Lumina asset conventions to ensure component assets resolve correctly in dev and CDN builds.

## Use when

- Adding or referencing static assets (svg, json, images).
- Fixing broken asset URLs or CDN path issues.

## Steps

1. Put component-scoped assets in `src/components/<name>/assets/`.
2. Import `getAssetPath` from your package's local `src/runtime.ts` module (not directly from `@arcgis/lumina`).

   ```ts
   import { getAssetPath } from "../runtime";
   ```

   Use `../runtime`, `../../runtime`, or deeper relative paths based on file location.

3. Use `getAssetPath("assets/<component-name>/<file>")` in component code.
4. Put shared assets in the package root `assets/` folder.
5. Use `getAssetPath("assets/<file>")` for shared assets.

## Checks

- Asset URLs resolve in dev and CDN builds.
- Component assets are scoped to the component name.

## Example prompts

- "Add an icon asset and reference it with getAssetPath."
- "Fix this asset URL so it loads from the CDN."

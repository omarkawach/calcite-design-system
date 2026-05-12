---
name: lumina-build-and-publishing
description: Apply Lumina build, exports, and publishing conventions.
---

# Lumina Build and Publishing

Apply build configuration and publishing rules that affect runtime and packaging.

## Use when

- Updating `useLumina()` config or build outputs.
- Adjusting package `exports` or docs outputs.

## Steps

1. Use `useLumina()` for shared build defaults and Lumina-specific customization.
2. If a concern is not covered by `useLumina()` options, configure standard Vite/Vitest/ESBuild/Rollup options in the same `defineConfig()`.

   ```ts
   export default defineConfig({
     plugins: [
       useLumina({
         /* Lumina-specific options */
       }),
     ],
     build: {
       /* Vite/Rollup options */
     },
     test: {
       /* Vitest options */
     },
   });
   ```

3. Configure `assets.defaultUrl` and `css.globalStylesPath` when needed.
4. Keep `exports` limited to intended public entrypoints.
5. Ensure docs outputs (`dist/docs`) remain reachable via exports.

## Checks

- Build outputs and exports match the intended public API.
- Assets and global styles resolve in CDN and NPM builds.

## Example prompts

- "Update useLumina config for asset defaults and global styles."
- "Adjust package exports without exposing internals."

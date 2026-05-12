---
name: lumina-workers
description: Load workers correctly in dev and production with Lumina asset paths.
---

# Lumina Workers

Use a dual-path worker loading pattern that works in dev and CDN builds.

## Use when

- Adding a web worker to a Lumina component.
- Fixing worker bundling or asset path issues.

## Steps

1. Import workers with `?worker` for development.
2. In production, load the worker via `getAssetPath()` and a blob URL.
3. Ensure Vite config bundles worker files into assets.

## Checks

- Worker runs in dev (HMR) and production builds.
- Worker script is served from assets/CDN in production.

## Example prompts

- "Add a worker that loads in dev and CDN builds."
- "Fix a worker that works in dev but not production."

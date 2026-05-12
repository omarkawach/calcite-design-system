---
name: lumina-ssr
description: Author SSR-safe Lumina components and avoid hydration mismatches.
---

# Lumina SSR

Ensure Lumina components render safely in server-side rendering environments.

## Use when

- Components are used in SSR frameworks (Next.js, Astro).
- Fixing SSR errors or hydration mismatches.

## Steps

1. Assume `connectedCallback`, `load`, and `loaded` do not run on the server.
2. Guard DOM access with `if (!isServer) {}` and avoid DOM in module scope.
3. Keep first render deterministic to prevent hydration mismatch.
4. Guard for cases when async data is not available in `render()`, including `this.messages`.
5. Do not rely on light DOM for SSR (Lit SSR does not support it).

## Checks

- Server render does not access DOM APIs.
- Client first render matches server HTML.

## Example prompts

- "Make this component SSR-safe and prevent hydration mismatches."
- "Guard DOM usage for server rendering."

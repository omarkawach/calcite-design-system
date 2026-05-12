---
name: lumina-styling
description: Apply Lumina styling patterns for shadow DOM, global styles, and external CSS.
---

# Lumina Styling

Use Lumina styling conventions for shadow DOM, global styles, and external library CSS.

## Use when

- Adding or refactoring component styles.
- Styling a light DOM component or bundling external CSS.

## Steps

1. Import component styles and set `static override styles`.
2. Prefer shadow DOM defaults; scope styles when using light DOM.
3. Use `css.globalStylesPath` for global styles (variables, resets).
4. For external library CSS, ensure it is not externalized in `useLumina()` config.
5. Do not use BEM class-name conventions for component scoping; rely on shadow DOM encapsulation.

## Checks

- Styles apply correctly in shadow or light DOM.
- External CSS is bundled when required.

## Example prompts

- "Add component styles and wire static styles correctly."
- "Bundle external library CSS so it works in production."

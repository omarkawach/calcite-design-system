---
name: lumina-controller-authoring
description: Author reusable Lumina controllers when extracting lifecycle-aware logic from TSX components, including cases that also involve controller-only non-TSX modules.
---

# Lumina Controller Authoring

Author reusable Lumina controllers (functional or class-based) to encapsulate lifecycle-aware business logic and reduce component duplication.
Conceptually, controllers are similar to Vue composables for web components: reusable logic units that keep rendering in the component.

## Use when

- Multiple components share the same logic.
- Component files are mixing rendering and complex side effects.
- You need isolated testing for reusable logic.

## Steps

1. Pick controller style: functional for concise cases, class-based for advanced composition/extension.
2. Define explicit expected component interface (typing contracts) for safer usage.
3. Move non-render business logic/state into the controller.
4. Prefer controllers when logic is stateful, reused, or needs lifecycle hooks; do not extract one for a one-off local field that `@state()` can express cleanly.
5. When logic is primarily `@arcgis/core` state/reactivity, consider an `Accessor` class (viewmodel-style) instead of manually wiring many `reactiveUtils.watch` calls in a controller.
6. Hook lifecycle logic through controller APIs instead of duplicating component-level boilerplate.
7. Ensure one-time teardown uses controller destroy semantics where appropriate.
8. Keep controller public surface minimal and explicit.
9. For components using the controller, preserve clear boundaries: render in component, logic in controller.
10. Add isolated tests with `wrapController()` and dynamic test components when component contracts are required.

## References

- Code reuse patterns (including Accessor viewmodels): https://webgis.esri.com/references/lumina/code-reuse-patterns#accessor-viewmodels

## Checks

- Shared logic is removed from component duplication sites.
- Controller can be tested independently.
- Lifecycle attach/detach behavior is deterministic.
- A controller was chosen for reuse/lifecycle reasons rather than as a default abstraction.

## Example prompts

- "Extract this repeated load/watch logic into a Lumina functional controller."
- "Create a class-based controller for keyboard navigation used by three components."

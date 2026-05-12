---
name: lumina-lifecycle-and-async
description: Place logic in the correct Lumina lifecycle hooks and manage async flows safely when implementing or refactoring TSX component initialization and updates.
---

# Lumina Lifecycle and Async

Place logic in the correct Lumina lifecycle hooks and run async work safely without blocking initial rendering unnecessarily.

## Use when

- You are deciding between `constructor`, `connectedCallback`, `load`, `loaded`, and `willUpdate`.
- You need to fetch data based on initial setup or property changes.
- You are fixing lifecycle bugs from reconnects or duplicate listeners.

## Steps

1. Use `constructor()` only for one-time setup that does not require DOM or user-set props.
2. Use `connectedCallback()` for DOM-position-dependent setup, but assume it can run multiple times.
3. Use `load()` for one-time async initialization; keep it short to avoid delaying first paint.
4. For long requests, start work in `load()` but avoid hard blocking when possible; render loading UI and update later.
5. Use `loaded()` for one-time post-render work that needs rendered DOM and loaded children.
6. Use `willUpdate(changes)` as primary reaction point for property/state changes.
   - `willUpdate` runs before render; it is safe to derive or clamp related state here without triggering an extra render.
   - The first `willUpdate` includes all reactive properties with initial values (defaults included); use `this.hasUpdated` or getters/setters if you need to distinguish initial vs user-driven updates.
7. For side effects with teardown (listeners, watches, observers), prefer `this.manager.onLifecycle(...)` (similar to React's useEffect) so setup/cleanup stay paired across disconnect/reconnect cycles.
8. Do not pair one-time setup in `firstUpdated()` with teardown in `disconnectedCallback()`; that breaks reconnect behavior because `firstUpdated()` runs once while disconnect cleanup can run many times.
9. If an effect must exist whenever the element is connected, model it as a reconnect-safe lifecycle effect and re-establish it on each connection.
10. Prefer `this.listen()` / `this.listenOn()` over manual add/remove listener bookkeeping.
11. For property-change async tasks, use guarded patterns (request identity/abort) or `@lit/task` to avoid stale updates.
12. Use `updated()` only for post-render work (measurements, positioning, imperative DOM tweaks). Avoid setting reactive state in `updated()` unless absolutely necessary to prevent update-after-update warnings.

## Checks

- Initial render timing matches UX expectations.
- Reconnect/disconnect does not duplicate listeners.
- Reconnect/disconnect preserves behavior instead of leaving the component half-initialized after reattachment.
- Lifecycle-sensitive changes include at least one reconnect simulation in tests (detach and reattach) to verify setup/cleanup pairing.
- Property-driven async updates do not commit stale state.
- Side effects are reconnect-safe and cleanup is paired with setup.

## Example prompts

- "Move this fetch logic into the right Lumina lifecycle and prevent stale async updates."
- "Implement property-driven async loading using `willUpdate` plus loading/error state."

---
name: lumina-testing-vitest-browser
description: Create reliable Lumina component tests for TSX components using Vitest browser mode and mount-driven assertions for realistic DOM and shadow behavior.
---

# Lumina Testing with Vitest Browser

Create reliable Lumina component tests with Vitest browser mode, focusing on real DOM/shadow behavior and practical debugging.

## Use when

- Writing or updating `.e2e.` component tests.
- Debugging browser-only behavior in Lumina components.
- Migrating older Puppeteer-style tests toward browser mode.

## Steps

1. Prefer Vitest browser mode for component rendering/interactions and shadow DOM fidelity.
2. Name browser tests with `.e2e.` (or match project include pattern).
3. Use `mount()` and `await` it before interacting.

   ```tsx
   const { component, el } = await mount<"arcgis-arcade-editor">(<arcgis-arcade-editor />);
   ```

4. Use returned `component` for internal members and `el` for public API and DOM element assertions.
5. In shadow DOM components, treat `el.shadowRoot` as required and query from it directly so missing shadow roots fail fast.
6. Do not optional-chain `shadowRoot` in assertions for shadow DOM components; if the component should render shadow DOM, let missing shadow root fail the test immediately.
7. Keep tests behavior-focused (render baseline, interaction, emitted events, accessibility-critical states).
8. Make cleanup deterministic: use `afterEach()` or `try/finally` when mutating globals, shared config, timers, or DOM fixtures so early test failures do not pollute later tests.
9. Do not place required cleanup only after assertions in the happy path; if a test can fail before cleanup runs, move that cleanup into `afterEach()` or `finally`.
10. When setup APIs expose disposers, prefer TypeScript `using` so cleanup runs even when assertions fail (fall back to `try/finally` where `using` is not practical).
11. For agents and CI, use non-interactive runs (`vitest run` or `pnpm --filter <pkg> exec vitest run <file>`). Reserve watch mode for human interactive iteration, and use debug mode (`--inspect`, no parallel/isolation when needed) for deep diagnosis.
12. For controller-heavy logic, add isolated controller tests in addition to component E2E checks.
13. When component behavior depends on listeners, observers, or lifecycle-managed effects, include a detach/reattach assertion to verify reconnect-safe behavior.

```tsx
import { wrapController } from "@arcgis/lumina-compiler/testing";

const { component } = await mount(wrapController(useGreet));
```

## Checks

- Tests pass in browser mode locally and CI.
- Assertions target user-observable behavior.
- Flakes are minimized (awaited setup, stable selectors, deterministic async).
- Tests do not leak shared state when a prior assertion fails.
- Lifecycle side effects remain correct after host detach/reattach.

## Example prompts

- "Write a Vitest browser `.e2e.` test that mounts this component and verifies event emission + DOM update."
- "Stabilize these flaky browser-mode tests by fixing async timing and selectors."

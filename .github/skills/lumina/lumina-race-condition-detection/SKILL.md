---
name: lumina-race-condition-detection
description: Find and isolate race conditions in Lumina (Lit-based) frontend code by tracing async, lifecycle, and event-order interactions before proposing minimal, deterministic fixes.
---

# Lumina Race Condition Detection

Systematically detect race conditions in Lumina components by mapping trigger sources, async boundaries, and state writes, then proving ordering assumptions with targeted instrumentation and tests.

## Use when

- A component shows intermittent or non-deterministic UI state.
- Async data or derived state occasionally appears stale or out of order.
- Behavior differs between fast/slow network, reconnects, or rapid user interaction.
- Property changes, watchers, or events seem to fire in unexpected sequences.

## Do not use when

- The issue is a deterministic type/error-path bug with no timing dimension.
- The problem is purely visual/CSS and unrelated to state transition ordering.

## Investigation workflow

1. Define one failing timeline in plain language: trigger -> async boundary -> state write -> render symptom.
2. Enumerate all writers for each affected property/state field.
3. Identify overlap windows where two or more in-flight operations can write the same state.
4. Confirm lifecycle entry points involved (`connectedCallback`, `load`, `loaded`, `willUpdate`) and whether they can re-run.
5. Add temporary ordering instrumentation:
   - monotonic request/token id
   - start/finish timestamps
   - source tag (which hook/event started the work)
6. Reproduce under stress:
   - rapid repeated input
   - delayed responses (slow network / artificial delay)
   - connect/disconnect cycles
7. Prove race by capturing an invalid ordering (older request commits after newer intent).
8. Classify the race type, then choose the smallest fix strategy.

## Lumina/Lit race patterns to check first

- Stale async commit: request A starts, request B starts later, A resolves last and overwrites B.
- Hook re-entry duplication: setup in `connectedCallback` or watcher starts duplicate async/listener work.
- Property-event feedback loop: event updates property that re-triggers watcher and causes out-of-order state.
- Parent-child timing mismatch: child reads parent-provided data before parent async update settles.
- Multi-source writes: controller/store + local component logic both writing same state without arbitration.
- Unawaited branch races: fire-and-forget async blocks writing after newer UI intent.
- Teardown races: component disconnects but async continuation still mutates state.

## Preferred fix strategies (ordered)

1. Last-intent-wins guard: request identity/token check before commit.
2. Cancellation: `AbortController` (or equivalent) cancel prior in-flight request when intent changes.
3. Single writer rule: centralize writes to a property/state field in one path.
4. Lifecycle relocation: move logic to the hook that matches intent boundaries.
5. Task orchestration: adopt a task pattern (`@lit/task` where appropriate) for explicit pending/success/error transitions.
6. Idempotent guards: prevent duplicate setup/listener registration on reconnect.

## Checks

- Rapid-interaction sequence no longer shows stale UI.
- Slow/variable network does not violate intent ordering.
- Connect/disconnect does not duplicate side effects.

## Example prompts

- "Find why this Lumina selector sometimes shows stale options after rapid filter changes."
- "Trace race conditions between `load()` and `willUpdate()` in this TSX component."
- "Identify and fix out-of-order async commits after reconnect in a viewer component."

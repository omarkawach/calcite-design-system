---
name: lumina-events-and-property-change
description: Implement predictable Lumina event contracts and property-change signaling when defining or refactoring component events and TSX listeners.
---

# Lumina Events and Property Change

Implement predictable event contracts in Lumina, including `arcgisPropertyChange` for mutable public properties.

## Use when

- Defining new component output events.
- Wiring event listeners in TSX.
- Migrating ad-hoc change events to standardized property-change signaling.

## Steps

1. Listen to native events with `onClick`-style casing.
2. Listen to custom events with lowercase event names after `on` (for example `onarcgisPropertyChange`).
3. Type handlers with `ToEvents<...>` or known element event types.
4. Declare custom events via `createEvent()` and keep `arcgis` prefix, but do not prefix with the full component name.
5. Avoid naming custom events after native events (for example `click`).
6. For property-change style events, do not include redundant payloads; consumers should read the latest value from the component property.
7. For `arcgisPropertyChange` specifically, emit the signal without echoing the changed value in `detail`.
8. Only include custom event payload when the data is not otherwise available from component state/props at event time.
9. Use `usePropertyChange()` to emit `arcgisPropertyChange` for relevant public properties.
10. For internal-only event channels, use `arcgisInternal*`, stop propagation, and mark private in docs.
11. Use `bindEvent()` when passive/capture/once options are needed.

## Checks

- Event names/casing are consistent and typed.
- Consumers can subscribe from JSX and DOM APIs.
- `arcgisPropertyChange` fires only when intended.
- Property-change events do not carry duplicated state payloads.

## Example prompts

- "Standardize custom events in this Lumina component and fix JSX listener casing."
- "Replace this custom `valueChanged` event with Lumina `usePropertyChange()` pattern."

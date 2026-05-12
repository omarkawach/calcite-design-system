---
name: lumina-methods
description: Define public methods that are lazy-load safe and async-first.
---

# Lumina Methods

Expose public methods that work with lazy-loading and future async behavior.

## Use when

- Adding or refactoring component methods.
- Converting imperative APIs for consistency.

## Steps

1. Prefer async methods (`Promise` return) for all public APIs.
2. Keep methods stable even if component loads lazily.
3. Mark internal-only methods `@private` when not part of public API.

## Checks

- Public methods are async and safe to call before load.
- Internal methods are not exposed unintentionally.

## Example prompts

- "Convert this method to async for lazy-loading support."
- "Add a public setFocus method safely."

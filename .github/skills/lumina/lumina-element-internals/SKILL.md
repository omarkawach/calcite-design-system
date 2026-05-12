---
name: lumina-element-internals
description: Use ElementInternals and form-associated behavior in Lumina components.
---

# Lumina Element Internals

Implement ElementInternals and form-associated behavior safely in Lumina.

## Use when

- Adding form-associated behavior or ARIA reflection.
- Listening to form lifecycle callbacks.

## Steps

1. Access `this.elementInternals` for ARIA reflection and form data.
2. Set `static formAssociated = true` only when needed.
3. Listen to Lumina form lifecycle events with `this.listen()` (not bare `listen()`).
   - `luminaFormAssociatedCallback`
   - `luminaFormDisabledCallback`
   - `luminaFormResetCallback`
   - `luminaFormStateRestoreCallback`

   ```ts
   this.listen("luminaFormAssociatedCallback", ({ detail: [form] }) => {
     // form: HTMLFormElement | null
   });
   ```

4. Use `setFormValue()` and `setValidity()` to integrate with forms.

## References

- Lumina element internals docs: https://webgis.esri.com/references/lumina/element-internals

## Checks

- Form callbacks are handled via Lumina events.
- Validity and submission values behave like native inputs.

## Example prompts

- "Make this component form-associated and handle form reset."
- "Use ElementInternals for ARIA and validity."

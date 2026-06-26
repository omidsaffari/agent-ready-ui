# ARIA Patterns For Agent-Ready UI

Use native HTML first. Add ARIA when native semantics cannot express the control
or state clearly enough.

## Buttons

- Prefer visible text: `Save card`, `Create account`, `Download invoice`.
- For icon-only buttons, add a specific `aria-label`.
- Avoid duplicate names in the same scope. Use entity context when needed, such
  as `Archive invoice INV-1042`.

## Forms

- Prefer visible `<label htmlFor="...">` with a stable `id`.
- Do not rely on placeholders as labels.
- Add helper text with `aria-describedby` when the agent must understand format,
  limits, or error recovery.
- Keep disabled and invalid states observable with `disabled`, `aria-invalid`,
  visible errors, and stable error text.

## Dialogs And Drawers

- Give every dialog a programmatic name with `aria-labelledby` or `aria-label`.
- Scope locators under the dialog before selecting controls inside it.
- Keep close, cancel, and destructive actions uniquely named.
- Expose busy states with visible text, `disabled`, or `aria-busy`.

## Menus, Tabs, And Custom Selects

- Use native `<select>`, `<button>`, and links where possible.
- For custom controls, expose role, name, expanded or selected state, and
  keyboard behavior.
- Make option names unique inside the menu or listbox.

## Loading And Settled State

- Replace arbitrary waits with observable product states.
- Prefer visible completion signals: headings, toasts, saved values, route
  changes, or enabled buttons.
- If a spinner is present, pair it with text or `aria-busy` so an agent can
  detect both pending and settled states.

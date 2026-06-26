# Agent Action Map

Use this template before patching a UI. It turns a browser-agent task into a
small contract that can be implemented and verified.

## Template

| Step | User action | Preferred locator | Required state | Success signal |
| --- | --- | --- | --- | --- |
| 1 | Click checkout | `getByRole('button', { name: 'Checkout' })` | visible, enabled | URL contains `/checkout` |
| 2 | Enter email | `getByLabel('Email')` | editable | Input value matches the email |
| 3 | Submit form | `getByRole('button', { name: 'Create account' })` | enabled, not busy | Heading says `Check your inbox` |

## Fields

- `Step`: The smallest sequential action the browser agent needs to take.
- `User action`: A human-readable action, not an implementation detail.
- `Preferred locator`: The locator the agent should try first.
- `Required state`: The observable state before the action is safe.
- `Success signal`: The observable state after the action succeeds.

## Locator Priority

1. Role plus accessible name.
2. Associated label.
3. Stable visible text scoped to a region, dialog, row, or form.
4. Existing project test ID when semantic locators are not enough.
5. Coordinate or DOM fallback, only when documented and unavoidable.

## Review Questions

- Can every action be described without mentioning CSS classes or child indexes?
- Are repeated controls uniquely named inside their scope?
- Can the agent tell when the page is loading, disabled, expanded, selected, or
  settled?
- Does every success signal reflect product state rather than timing?

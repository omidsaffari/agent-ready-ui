# Checkout Before And After

This example shows the kind of patch `agent-ready-ui` should produce: small UI
affordance changes, stable accessible locators, and an action map a browser
agent can follow.

## Before

The UI looks usable, but the automation contract is weak:

- The icon-only close button has no accessible name.
- The dialog has no programmatic name.
- The card number input relies on placeholder text.
- The save button is only findable by DOM structure.
- Loading state is not exposed to the agent.

```tsx
export function PaymentModal({ saving, onClose, onSave }) {
  return (
    <div className="modal">
      <button className="icon" onClick={onClose}>
        X
      </button>
      <h2>Add card</h2>
      <input placeholder="Card number" />
      <button className="primary" disabled={saving} onClick={onSave}>
        Save
      </button>
    </div>
  );
}
```

The resulting locator is brittle because it encodes layout:

```ts
await page.locator('.modal > div:nth-child(2) button').click();
```

## After

The visible UI can stay the same, but the semantic contract becomes stable:

```tsx
export function PaymentModal({ saving, onClose, onSave }) {
  return (
    <section
      aria-labelledby="payment-method-title"
      aria-modal="true"
      aria-busy={saving}
      role="dialog"
    >
      <button aria-label="Close payment method" onClick={onClose}>
        X
      </button>

      <h2 id="payment-method-title">Payment method</h2>

      <label htmlFor="card-number">Card number</label>
      <input
        autoComplete="cc-number"
        id="card-number"
        inputMode="numeric"
        name="cardNumber"
      />

      <button
        disabled={saving}
        onClick={onSave}
        type="button"
      >
        {saving ? 'Saving...' : 'Save card'}
      </button>
    </section>
  );
}
```

The browser-agent locator now reads like a user task:

```ts
const paymentDialog = page.getByRole('dialog', {
  name: 'Payment method',
});

await paymentDialog.getByLabel('Card number').fill('4242424242424242');
await paymentDialog
  .getByRole('button', { name: 'Save card' })
  .click();
await expect(page.getByText('Card saved')).toBeVisible();
```

## Action Map

| Step | User action | Preferred locator | Required state | Success signal |
| --- | --- | --- | --- | --- |
| 1 | Open checkout | `getByRole('button', { name: 'Checkout' })` | visible, enabled | URL contains `/checkout` |
| 2 | Open payment dialog | `getByRole('button', { name: 'Add card' })` | visible, enabled | Dialog named `Payment method` is visible |
| 3 | Enter card number | `dialog.getByLabel('Card number')` | editable | Input value is populated |
| 4 | Save card | `dialog.getByRole('button', { name: 'Save card' })` | enabled, not busy | Toast says `Card saved` |

## Patch Notes

- Prefer `role="dialog"` plus `aria-labelledby` over an unnamed modal wrapper.
- Prefer `<label htmlFor>` over placeholder-only input names.
- Use scoped locators under the dialog so duplicate page buttons do not collide.
- Expose loading with `aria-busy`, disabled state, and visible button text.
- Keep `data-testid` as a fallback only if the app already uses test IDs or a
  target cannot have a meaningful accessible locator.

# Playwright Locators For Browser Agents

Browser agents and Playwright tests are more reliable when locators match how a
person understands the UI.

## Preferred Locators

```ts
await page.getByRole('button', { name: 'Checkout' }).click();
await page.getByLabel('Email').fill('reader@example.com');
await page.getByRole('heading', { name: 'Receipt' }).isVisible();
```

These locators survive common refactors because they depend on product meaning,
not DOM shape.

## Scoped Locators

Scope repeated controls to a stable parent:

```ts
const paymentDialog = page.getByRole('dialog', {
  name: 'Payment method',
});

await paymentDialog.getByRole('button', { name: 'Save card' }).click();
```

For tables and repeated cards, scope to the row or article:

```ts
const invoiceRow = page.getByRole('row', { name: /INV-1042/ });

await invoiceRow.getByRole('button', { name: 'Download invoice' }).click();
```

## Avoid

```ts
await page.locator('.modal > div:nth-child(2) button').click();
await page.locator('[class^="Button_button__"]').click();
await page.locator('xpath=//*[@id="root"]/div[3]/button[1]').click();
```

These selectors break when layout, generated classes, or wrapper elements
change.

## Test IDs

Use `data-testid` only when:

- The project already uses test IDs.
- The target has no honest accessible name, such as a canvas region.
- The test ID is stable and product-level, not layout-level.

Good:

```tsx
<canvas aria-label="Seat map" data-testid="seat-map-canvas" />
```

Avoid:

```tsx
<button data-testid="button-3" />
```

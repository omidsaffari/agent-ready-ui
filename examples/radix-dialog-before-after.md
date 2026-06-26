# Radix Dialog Before And After

This example uses a shadcn/ui-style wrapper around Radix Dialog primitives. The
goal is not to redesign the dialog; it is to make the existing component
reliable for browser agents and role/name locators.

## Before

The component works visually, but the automation contract is incomplete:

- The dialog content is not named through the dialog primitive.
- The close control is icon-only.
- The input is placeholder-only.
- The submit button has a generic name that may collide with other dialogs.
- The pending state changes text, but there is no scoped settled signal.

```tsx
import { Dialog, DialogContent } from '@/components/ui/dialog';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { X } from 'lucide-react';

export function InviteDialog({ open, saving, onOpenChange, onInvite }) {
  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent>
        <button className="absolute right-4 top-4" onClick={() => onOpenChange(false)}>
          <X />
        </button>

        <h2>Invite user</h2>
        <Input placeholder="Email address" />

        <Button disabled={saving} onClick={onInvite}>
          {saving ? 'Saving...' : 'Save'}
        </Button>
      </DialogContent>
    </Dialog>
  );
}
```

The resulting browser-agent script has to depend on vague text or layout:

```ts
await page.locator('[role="dialog"] input').fill('alex@example.com');
await page.getByRole('button', { name: 'Save' }).click();
```

## After

Keep the same component shape, but expose the dialog name, field label, pending
state, and scoped submit action.

```tsx
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogTitle,
} from '@/components/ui/dialog';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { X } from 'lucide-react';

export function InviteDialog({ open, saving, onOpenChange, onInvite }) {
  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent aria-busy={saving}>
        <button
          aria-label="Close invite teammate dialog"
          className="absolute right-4 top-4"
          onClick={() => onOpenChange(false)}
        >
          <X aria-hidden="true" />
        </button>

        <DialogTitle>Invite teammate</DialogTitle>
        <DialogDescription>
          Send a workspace invitation by email.
        </DialogDescription>

        <Label htmlFor="invite-email">Email address</Label>
        <Input
          autoComplete="email"
          id="invite-email"
          name="email"
          type="email"
        />

        <Button disabled={saving} onClick={onInvite}>
          {saving ? 'Sending invite...' : 'Send invite'}
        </Button>
      </DialogContent>
    </Dialog>
  );
}
```

The browser-agent script can now scope every action to the named dialog:

```ts
const inviteDialog = page.getByRole('dialog', {
  name: 'Invite teammate',
});

await inviteDialog.getByLabel('Email address').fill('alex@example.com');
await inviteDialog
  .getByRole('button', { name: 'Send invite' })
  .click();
await expect(page.getByText('Invitation sent')).toBeVisible();
```

## Action Map

| Step | User action | Preferred locator | Required state | Success signal |
| --- | --- | --- | --- | --- |
| 1 | Open invite dialog | `getByRole('button', { name: 'Invite teammate' })` | visible, enabled | Dialog named `Invite teammate` is visible |
| 2 | Enter email | `dialog.getByLabel('Email address')` | editable | Input value contains the email |
| 3 | Send invite | `dialog.getByRole('button', { name: 'Send invite' })` | enabled, not busy | Toast says `Invitation sent` |

## Patch Notes

- Use `DialogTitle` so the Radix dialog has a stable accessible name.
- Use `DialogDescription` for extra context instead of hiding instructions in
  placeholder text.
- Keep the close button icon-only visually, but give it a specific accessible
  name.
- Rename the submit action from generic `Save` to scoped `Send invite`.
- Use `aria-busy` and disabled state so agents can wait for the dialog to settle.

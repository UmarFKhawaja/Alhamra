# ADR-0020: Standardize form error surfacing

- Status: Proposed
- Date: 2026-06-22

## Context

Without a shared error-surfacing contract, forms can use inconsistent combinations of toast notifications, inline banners, and field-level errors. Feedback can disappear entirely when a server response omits a flag that the view requires before displaying a notification.

Users need a clear outcome for each submission and enough detail to correct invalid fields. A shared contract makes that behavior predictable and gives developers a consistent pattern for new forms.

## Decision

Every form in the application follows a single error-surfacing model:

### Three layers of error feedback

1. **Toast notification** — rendered once per submission outcome. The form displays a notification for every success or failure result. The server action always sets `hasToast: true` alongside `message` and `isSuccess`. The examples below call the notification component `ToastTrigger`; use the project's equivalent.
2. **Per-field invalid markers** — each input, text area, or picker receives `invalid={Boolean(errors?.fieldName)}` or its equivalent when the server returns field-level errors.
3. **Per-field error text** — a short `<p>` below the invalid field repeats the error message so the user does not need to correlate the toast text with the affected input.

### Form state contract

Every form state type includes these fields, or equivalent names with the same semantics:

```ts
type FormState = {
  message?: string;       // user-facing summary
  isSuccess?: boolean;    // true when the action succeeded
  hasToast: boolean;      // always set — true for both success and failure
  errors?: Record<string, string>;  // field-name → error message
  values?: Record<string, unknown>; // submitted values for repopulation
};
```

- Initial form state may use `hasToast: false`. Every returned submission result sets `hasToast: true` and includes `message` and `isSuccess`. The widget never needs to gate `ToastTrigger` on `isSuccess` being present — `hasToast` alone is sufficient.
- `errors` maps field names to messages. The view iterates the map to set `invalid` and render per-field text.
- `isSuccess` determines the `ToastTrigger` tone and summary title.

### Error language rules

- Messages address the user directly and describe the problem and the required action: `"A name is required."`, not `"field 'name' failed validation."`.
- Messages do not expose internal identifiers, stack traces, API error codes, or Zod raw errors.
- Server `catch` blocks wrap underlying errors with a stable user-facing message and log the original error server-side.

### Toast title convention

| Outcome | Title pattern |
|---|---|
| Create success | `"{Entity} created"` |
| Update success | `"{Entity} updated"` |
| Delete success | `"{Entity} deleted"` |
| Validation failure | `"Could not {action} {entity}"` |
| Access failure | `"Could not {action} {entity}"` |
| Unexpected failure | `"Could not {action} {entity}"` |

The `{action}` is present-tense verb form: `"save"`, `"create"`, `"delete"`, `"update"`.

## Consequences

Every form provides consistent visual feedback. Users see exactly which fields need correction and receive a confirming or error toast on every submission.

Adding a new form requires supporting all three layers. The server action must return `hasToast: true`, `isSuccess`, and `message` for every submission outcome, plus `errors` when field-level validation fails.

Forms that omit outcome feedback or field-level validation feedback must be brought into line with this contract.

Gate notification display on `hasToast` alone. A completed submission result always includes a message, so the view does not need a second message-presence check.

## Rejected alternatives

Keeping the ad-hoc mixture of toast, inline, and field-level patterns was rejected because it can leave failures invisible and forces developers to rediscover the convention.

Requiring only toast feedback was rejected because a toast alone does not tell the user which field to correct on a long form.

Requiring only inline feedback was rejected because the user may scroll away from the inline message before a slow submission completes, missing the feedback entirely.

## Related decisions

- [ADR-0003: Expose semantic component appearance APIs](./0003-expose-semantic-component-appearance-apis.md) — the `invalid` prop on inputs follows the same semantic-API pattern.
- [ADR-0006: Separate controllers from themed views](./0006-separate-controllers-from-themed-views.md) — the form state type belongs to the shared controller contract.

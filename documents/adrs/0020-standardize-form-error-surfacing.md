# ADR-0020: Standardize form error surfacing

- Status: Proposed
- Date: 2026-06-22

## Context

Form errors are surfaced inconsistently. Some widgets use toast notifications, some render inline banners, some mark individual fields, and two widgets silently swallow validation errors because the server omits the `toast` flag that the widget's `ToastTrigger` gate requires.

See [defect 0001](../defects/0001-error-notifications-are-absent-and-inconsistent.md) for the full audit.

The inconsistency means users sometimes receive no feedback when a form fails, sometimes see a toast, and sometimes see inline text. There is no rule that tells a developer which pattern to implement for a new form.

## Decision

Every form in the application follows a single error-surfacing model:

### Three layers of error feedback

1. **Toast notification** — rendered once per submission outcome. The form displays a `ToastTrigger` for every success or failure result. The server action always sets `toast: true` alongside `message` and `success`.
2. **Per-field invalid markers** — each `InlineInput`, `InlineTextArea`, `CountryPicker`, and similar input receives `invalid={Boolean(errors.fieldName)}` when the server returns field-level errors.
3. **Per-field error text** — a short `<p>` below the invalid field repeats the error message so the user does not need to correlate the toast text with the affected input.

### Form state contract

Every form state type includes:

```ts
type FormState = {
  message?: string;       // user-facing summary
  isSuccess?: boolean;    // true when the action succeeded
  hasToast: boolean;      // always set — true for both success and failure
  errors?: Record<string, string>;  // field-name → error message
  values?: Record<string, unknown>; // submitted values for repopulation
};
```

- `hasToast` is **always** set when the server returns a result. The widget never needs to gate `ToastTrigger` on `isSuccess` being present — `hasToast` alone is sufficient.
- `errors` maps field names to messages. The view iterates the map to set `invalid` and render per-field text.
- `isSuccess` determines the `ToastTrigger` tone and summary title.

### Error language rules

- Messages address the user directly and describe the problem and the required action: `"A clinic name is required."`, not `"field 'name' failed validation."`.
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

Adding a new form requires implementing all three layers. The server action must always populate `toast`, `success`, `message`, and `errors` in its return shape.

Existing forms that deviate from this pattern are defects. The `ManageClinicWidget` and `ManageRoomWidget` toast-gating bugs and the missing field-level markers in `ProfileView`, `CreateUserWidget`, and `ArticleEditorWidget` must be corrected.

The `toast` gate condition `message && toast` is replaced by `toast` alone — `toast` is always set, so the presence of a message is implied.

## Rejected alternatives

Keeping the ad-hoc mixture of toast, inline, and field-level patterns was rejected because it silently breaks two forms and forces every new developer to rediscover the convention.

Requiring only toast feedback was rejected because a toast alone does not tell the user which field to correct on a long form.

Requiring only inline feedback was rejected because the user may scroll away from the inline message before a slow submission completes, missing the feedback entirely.

## Related decisions

- [ADR-0003: Expose semantic component appearance APIs](./0003-expose-semantic-component-appearance-apis.md) — the `invalid` prop on inputs follows the same semantic-API pattern.
- [ADR-0006: Separate controllers from themed views](./0006-separate-controllers-from-themed-views.md) — the form state type belongs to the shared controller contract.

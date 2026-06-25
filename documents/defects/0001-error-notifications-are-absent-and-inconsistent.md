# Defect 0001: Form error notifications are absent and inconsistent

## Status

Open

## Summary

Form errors are surfaced in three different ways across the codebase — some forms use toast notifications, some show inline banners, and some provide field-level markers — with no consistent rule governing which style applies when. Two forms silently swallow validation errors entirely, leaving users with no feedback when a submission fails.

## Specific findings

### 1. Silently swallowed validation errors (critical)

`ManageClinicWidget` and `ManageRoomWidget` never display server-side validation errors. The server action returns `{ message, values }` on validation failure but omits `toast: true`. The widget's `ToastTrigger` gate is `effectiveForm?.message && effectiveForm.toast`, so the message is never rendered. The form silently re-renders with the submitted values and zero user feedback.

### 2. Inconsistent toast vs. inline display

| Form | Toast | Inline | Field markers |
|------|-------|--------|---------------|
| ManageClinicWidget | Broken for errors | No | No |
| ManageRoomWidget | Broken for errors | No | No |
| CreateUserWidget | Yes | No | No |
| ProfileView | Partial | Partial | No |
| ManageUserSecurityWidget | Yes | Yes | Yes |
| ArticleEditorWidget | Yes | No | No |
| AuthView | Yes | No | No |

`ManageUserSecurityWidget` is the only form that marks individual fields as invalid. `ProfileView` declares an `errors` map in its form state type but never reads it or passes `invalid` to any input.

### 3. Inconsistent `toast` flag usage

Some server actions set `toast: true` for all errors, some only for non-field errors, and some not at all. There is no rule defining when an error should appear as a toast versus inline versus per-field.

### 4. Vague and technical error language

Several catch-all messages are unhelpful: `"Invalid profile form values."`, `"Article content is invalid."`, `"Unexpected error while creating the user."`. Better Auth `APIError` messages may leak implementation details when passed through to the client via `error.message`.

### 5. `CreateUserWidget` always renders toast as `tone="error"`

The form state type lacks a `success` field, so even a hypothetical success message would display as a red error toast.

## Expected behavior

All forms should:
- Show a toast notification on submission result (success or failure)
- Mark erroneous form fields with a visible invalid state
- Display per-field error messages adjacent to the affected input
- Use straightforward, actionable error language — no stack traces, no opaque API error codes, no vague `"Invalid form values."`

## Resolution

Deferred to ADR-0020.

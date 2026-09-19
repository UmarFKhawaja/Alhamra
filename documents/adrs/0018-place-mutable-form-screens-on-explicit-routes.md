# ADR-0018: Place mutable form screens on explicit routes

- Status: Accepted
- Date: 2026-06-21

## Context

React Router applications support two distinct form patterns:

- `fetcher.Form` for in-place save workflows that should preserve the current screen without navigation churn;
- plain React Router `<Form method="post">` for workflows where full-route navigation or separate action targeting is intentional.

React Router treats index-route submissions specially. When a plain form posts to an index route, the framework appends `?index` so it can disambiguate the target action from the parent route.

That behavior is framework-correct, but it produces a canonical URL that differs from the visible screen URL and makes index-route form screens behave differently from other editable screens.

For example, submitting a plain form from an editable `/settings` index route can navigate to `/settings?index`.

## Decision

Navigable screens that own mutable form workflows must live on explicit non-index routes.

Screens that save in place must use `fetcher.Form` so submission feedback stays local to the screen and the URL remains stable.

Apply this pattern as follows:

- render the editable screen on an explicit path such as `/settings/profile`;
- use `fetcher.Form` for in-place save/update workflows as the default;
- use plain `<Form method="post">` only when navigation, nested action targeting, or non-fetcher semantics are intentionally required;
- let the route module own the loader and action for that screen;
- reserve index routes for redirects, neutral landing screens, or read-only overview content.

If a parent section needs a default child, its index route should redirect to the canonical explicit child route instead of owning the form action itself.

## Consequences

Mutable screens keep stable canonical URLs before and after submission.

Route modules remain easier to reason about because each mutating screen has one explicit address and one explicit action target.

In-place saves follow a consistent interaction pattern.

Contributors have a clear default: if a form saves on the current screen, reach for `fetcher.Form` unless there is a deliberate reason not to.

Contributors should treat `?index` in navigation after a normal save as a signal that the form is mounted on the wrong kind of route under this convention.

## Exceptions

Plain forms may still be appropriate for create/delete flows, cross-route actions, or screens that intentionally navigate after submit.

Using plain `<Form method="post">` for a normal in-place save is considered a deviation from this ADR and should be justified in code review or a follow-on ADR.

Read-only index routes may continue to exist where a section overview is useful.

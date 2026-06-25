# ADR-0019: Keep reusable UI in the elements tier

- Status: Accepted
- Date: 2026-06-22
- Revised: 2026-06-25

## Revision note (2026-06-25)

The reusable-UI tier was renamed from `app/components/shared` to `app/elements`. `app/elements` is now the single home for the basic components used to construct every screen, and `app/components/shared` no longer exists. Wherever this ADR originally said `app/components/shared`, read `app/elements`. The boundary rules are unchanged; only the directory name changed so that the basic building blocks live in one clearly named tier alongside the existing auth and input primitives that already lived under `app/elements`.

## Context

The repository already distinguishes between `app/elements`, `app/components/site`, and `app/themes/*`, but that boundary had drifted in practice.

Several reusable form and layout controls were stored under `app/components/site` even though they were used by user-management screens, article editing, route placeholders, and theme-shared views. A theme-local `CountryPicker` wrapper also existed under `app/themes/shared/UserProfileView/components` even though it only adapted props for a reusable shared control.

That drift made the architecture harder to read:

- `site` looked like a grab bag of generic and domain-specific components;
- theme packages appeared to own controls that were not actually theme-specific;
- reviewers had to inspect implementation details to determine whether a component was safe to reuse outside one screen or one management area.

## Decision

Reusable controls, form primitives, layout primitives, and generic composite UI live under `app/elements`. These are the basic components used to construct the UI; screens, widgets, pages, and themed views compose them.

`app/components/site` is reserved for components whose meaning is specific to site-management or public-content workflows.

`app/themes/*` packages must not define standalone reusable controls unless the control is genuinely theme-specific and equivalent behavior is made available across every shipped theme. Theme packages should primarily compose shared components and theme styling, not own reusable control logic.

There is no separate “theme-shared” tier for contract-bound views. If code is shared across themes, it must be theme-agnostic and live outside the theme layer. If a contract view needs different styling, markup, spacing, or composition per theme, each theme owns its own implementation inside its package.

Private subcomponents that only exist to make one themed view readable may remain inside that themed view package when they are not intended for broader reuse.

## Audit result

The first cleanup pass promotes these packages from `app/components/site` to the reusable tier (then `app/components/shared`, now `app/elements`):

- `FormSection`
- `FieldSection`
- `FormRow`
- `AddressFieldSection`
- `CountryPicker`
- `EmptyState`
- `IconButton`
- `Surface`

The theme-local `UserProfileView/components/CountryPicker` wrapper is removed. `ProfileView` now consumes the shared `CountryPicker` directly.

## Consequences

Shared UI is easier to discover because generic controls live in one obvious place.

Theme packages stay focused on composition and styling, which keeps the theme contract cleaner and reduces duplicate control logic.

`site` components now communicate domain intent more clearly. If a component remains under `app/components/site`, callers can assume it is tied to site-management or public-content concerns rather than being a general-purpose primitive.

Future component reviews should ask two questions:

1. Is this component reusable outside one domain workflow?
2. Is this component introducing control behavior that a theme should not own?

If the answer to the first is yes, it belongs in `app/elements`. If the answer to the second is yes, the control should move out of the theme layer.

## Related decisions

- [ADR-0005: Select themes through a typed runtime contract](./0005-select-themes-through-a-typed-runtime-contract.md)
- [ADR-0006: Separate controllers from themed views](./0006-separate-controllers-from-themed-views.md)
- [ADR-0007: Maintain one canonical rendering path per screen](./0007-maintain-one-canonical-rendering-path-per-screen.md)

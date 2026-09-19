# ADR-0019: Keep reusable UI in the elements tier

- Status: Accepted
- Date: 2026-06-22
- Revised: 2026-06-25

## Context

Applications need clear boundaries between reusable UI elements, domain components, and theme implementations.

When generic form or layout controls live inside a domain area or theme package, that boundary becomes harder to read:

- domain directories become a mixture of generic and workflow-specific components;
- theme packages appear to own controls that are not actually theme-specific;
- reviewers must inspect implementation details to determine whether a component is safe to reuse outside one screen or workflow.

## Decision

Reusable controls, form primitives, layout primitives, and generic composite UI live in one elements tier, represented here by `app/elements`. These are the basic components used to construct the UI; screens, widgets, pages, and themed views compose them. Adapt the directory paths to the project while preserving these boundaries.

Domain component directories, such as `app/components/projects`, are reserved for components whose meaning is specific to that domain's workflows.

`app/themes/*` packages must not define standalone reusable controls unless the control is genuinely theme-specific and equivalent behavior is made available across every shipped theme. Theme packages should primarily compose shared components and theme styling, not own reusable control logic.

There is no separate “theme-shared” tier for contract-bound views. If code is shared across themes, it must be theme-agnostic and live outside the theme layer. If a contract view needs different styling, markup, spacing, or composition per theme, each theme owns its own implementation inside its package.

Private subcomponents that only exist to make one themed view readable may remain inside that themed view package when they are not intended for broader reuse.

## Examples

Components such as these belong in the reusable tier when they have no domain-specific dependencies:

- `FormSection`
- `FieldSection`
- `FormRow`
- `AddressFieldSection`
- `CountryPicker`
- `EmptyState`
- `IconButton`
- `Surface`

A themed profile view should consume the shared `CountryPicker` directly when a local wrapper would only adapt props without adding theme-specific behavior or presentation.

## Consequences

Shared UI is easier to discover because generic controls live in one obvious place.

Theme packages stay focused on composition and styling, which keeps the theme contract cleaner and reduces duplicate control logic.

Domain components communicate their intent more clearly. For example, callers can assume a component under `app/components/projects` is tied to project workflows rather than being a general-purpose primitive.

Future component reviews should ask two questions:

1. Is this component reusable outside one domain workflow?
2. Is this component introducing control behavior that a theme should not own?

If the answer to the first is yes, it belongs in `app/elements`. If the answer to the second is yes, the control should move out of the theme layer.

## Related decisions

- [ADR-0005: Select themes through a typed runtime contract](./0005-select-themes-through-a-typed-runtime-contract.md)
- [ADR-0006: Separate controllers from themed views](./0006-separate-controllers-from-themed-views.md)
- [ADR-0007: Maintain one canonical rendering path per screen](./0007-maintain-one-canonical-rendering-path-per-screen.md)

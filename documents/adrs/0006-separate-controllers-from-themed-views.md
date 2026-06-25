# ADR-0006: Separate controllers from themed views

- Status: Accepted
- Date: 2026-06-19

## Context

Large components combined state, fetchers, workflow decisions, permissions, route interpretation, side effects, and detailed JSX.

That coupling made behavior difficult to reuse across themes. Creating a second visual treatment would either duplicate the workflow or force visual components to retain implementation-specific behavior.

## Decision

Complex screens follow this flow:

```text
Route loader/action -> controller -> themed view
```

Route modules remain responsible for server data, mutations, authorization, and redirects.

Controller hooks own client-side behavior and expose a view model. Cross-screen controllers live under `app/features`; component-specific controllers may live in a component's private `hooks/` directory.

Themed views receive data and actions through typed props and render the visual structure.

The refactor applies this pattern to:

- sign-in workflow state;
- management workspace navigation;
- user profile editing;
- image selection and upload behavior;
- managed content editor behavior.

A controller may expose React Router fetcher form components when the view must render a form, but the view does not own the fetcher lifecycle or workflow decisions.

Interaction state that exists only because of one theme's composition remains inside that theme component. For example, the Sapphire management shell owns its mobile drawer state because the Emerald shell uses a different navigation structure. The shared management controller exposes navigation meaning and active items, not shell-specific open and close behavior.

## Consequences

Multiple themes can render the same workflow without duplicating state or business rules.

Controllers can be reviewed and tested independently from detailed markup.

View-model contracts become an explicit dependency between behavior and presentation.

Not every component needs a controller. Simple presentational components should remain simple, and small private behavior may stay in `component.tsx` until extraction improves readability or reuse.

Controllers must not become unstructured collections of unrelated behavior. If a controller becomes too large, it should be split by workflow or responsibility.

## Rejected alternatives

Duplicating complete screen components for each theme was rejected because behavior would drift.

Passing raw route data directly into every themed view was rejected where the view would then need to reproduce state transitions and derived workflow decisions.

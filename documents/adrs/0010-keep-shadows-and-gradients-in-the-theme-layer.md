# ADR-0010: Keep shadows and gradients in the theme layer

- Status: Accepted
- Date: 2026-06-19

## Context

Semantic theme tokens can control colors, typography, shape, and elevation while components still apply raw effects. Examples include Tailwind shadow utilities such as `shadow-sm`, `shadow-lg`, and `shadow-xl`, or gradient utilities such as `bg-linear-to-r`, `from-*`, `via-*`, and `to-*`.

That approach pushes visual effect decisions back down into the component layer. It becomes harder to reason about why one theme appears flatter or louder than another, and it makes theme changes require component-by-component cleanup.

## Decision

Shadows and gradients are defined only in the theme layer through semantic tokens.

Components and shared views may consume only semantic effect utilities that resolve to theme-owned custom properties. They must not define effect values directly with:

- raw Tailwind shadow size utilities;
- raw Tailwind gradient stop utilities;
- explicit `box-shadow` declarations;
- explicit `linear-gradient(...)` or `radial-gradient(...)` declarations.

Theme CSS files define the reusable effect roles for every supported theme and color mode. The shared CSS layer exposes those roles as semantic utilities.

Call-to-action gradients are reserved for branded treatments. Utility controls remain solid fills and may use shadow tokens, but not gradient fills.

## Consequences

Changing the mood of a theme no longer requires searching through ordinary component styles for effect utilities.

Branded calls to action can remain expressive without teaching utility controls to behave like marketing surfaces.

Adding a new shadow or gradient role requires:

1. defining the token for every supported theme and color mode;
2. exposing a semantic utility for the token;
3. applying that utility from components instead of a raw effect value.

Effect role names should describe reusable intent such as `flat`, `utility`, `floating`, `accent`, `brand`, `placeholder`, or `scrim`, not a specific page or widget.

## Rejected alternatives

Allowing components to keep raw Tailwind shadow and gradient utilities was rejected because it recreates theme styling drift outside the theme contract.

Restricting only shadows and not gradients was rejected because gradients create the same kind of drift and should follow the same ownership boundary.

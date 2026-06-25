# ADR-0004: Use semantic design tokens

- Status: Accepted
- Date: 2026-06-19

## Context

Components referenced palette-specific names such as brand gold, blue, light, and dark values. Those names describe the current palette rather than the purpose of a color or measurement.

Changing the visual identity or color mode therefore required knowledge of palette choices throughout the component tree.

## Decision

The application defines semantic design tokens between components and theme palettes.

Semantic tokens describe roles including:

- canvas and surfaces;
- foreground, heading, muted, and inverse text;
- primary actions and hover states;
- action, accent, chrome, overlay, and their on-color foregrounds;
- hover, selected, disabled, and soft surfaces;
- subtle and strong borders;
- success, danger, and warning feedback;
- panel, branded, and utility radii;
- panel and floating shadows;
- heading and body typography.

Tailwind v4 registers semantic utilities in `app/app.css` through `@theme inline`. The utilities resolve to CSS custom properties such as `--theme-surface` and `--theme-fg-heading`.

Each theme maps the same custom property contract to its own values in:

```text
app/themes/<theme>/theme.css
```

Color mode is also represented by token substitution. Components should not need separate light and dark markup.

## Consequences

Components use role-based utilities such as `bg-surface`, `text-fg-heading`, `text-on-action`, and `border-border-subtle`.

A theme can alter color, radius, shadow, and typography without changing component behavior.

The component layer no longer registers or references the legacy `brand-*` palette. Color-mode-specific `dark:` utilities have also been removed from component styles; light, dark, and system modes are implemented by substituting semantic custom properties at the root.

Fixed black and white values remain only where they are part of an external provider's visual identity, such as Apple and Google sign-in controls.

Non-semantic palette colors such as `gray-900`, `gray-200`, and `gray-50` are also permitted for rendering the full visual identity of an external brand, including hover, pressed, and focus states. These values belong to the brand being rendered rather than to the application theme. The component is still responsible for the layout, shape, and spacing of the control, which should continue to use semantic tokens.

Semantic tokens must remain stable across themes. Adding a token requires defining it for every supported theme and color mode.

Tokens should describe reusable visual roles rather than individual pages or components.

Shape tokens describe the intended visual treatment rather than the HTML element that receives it:

- `branded` exposes more of a theme's character in public-facing and identity-oriented experiences;
- `utility` provides the quieter, denser treatment used in operational interfaces.

The distinction is not synonymous with input versus button, or public versus private routing. For example, a newsletter input can use the branded shape while a management action button uses the utility shape.

## Rejected alternatives

Replacing the values of brand-specific palette tokens alone was rejected because it cannot express different semantic uses of the same palette color.

Duplicating dark-mode utilities across components was rejected because color mode belongs in the theme mapping.

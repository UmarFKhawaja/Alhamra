# ADR-0014: Let themes declare font assets

- Status: Accepted
- Date: 2026-06-20

## Context

Themes already control semantic font tokens through `theme.css`, but custom font files were still an implicit global concern.

That made two things harder than necessary:

- loading a font only for the active theme;
- switching a theme such as `emerald` to a bundled font family without coupling every other theme to the same asset.

## Decision

The theme runtime contract now allows a theme definition to declare static stylesheet assets.

The active theme exposes those stylesheets through `ThemeDefinition.assets.stylesheets`. The root layout renders `<link rel="stylesheet">` tags for the active theme only.

Theme CSS files remain responsible for semantic font token assignments such as `--theme-font-heading` and `--theme-font-body`. The stylesheet asset layer is responsible only for loading font-face declarations and related static theme assets.

`emerald` now declares the Neue Haas stylesheet at `/fonts/neue-haas/stylesheet.css` and maps both heading and body font tokens to that family. `sapphire` also declares the same stylesheet because it already depends on the same font family.

## Consequences

Themes can opt into custom fonts without making those font files a global application default.

Inactive theme font stylesheets are no longer loaded unnecessarily.

Adding a custom theme font now requires two explicit steps:

1. declare the stylesheet in the theme definition;
2. reference the family through the theme's semantic font tokens.

The runtime contract now covers visual assets as well as component composition, which keeps theme-specific behavior in one place.

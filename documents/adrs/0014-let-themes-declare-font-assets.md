# ADR-0014: Let themes declare font assets

- Status: Accepted
- Date: 2026-06-20

## Context

A theme may control semantic font tokens through its stylesheet while custom font files remain an implicit global concern.

That makes two things harder than necessary:

- loading a font only for the active theme;
- switching one theme to a bundled font family without coupling every other theme to the same asset.

## Decision

The theme runtime contract allows a theme definition to declare static stylesheet assets.

The active theme exposes those stylesheets through a typed assets field, named `ThemeDefinition.assets.stylesheets` in this example. The root layout renders `<link rel="stylesheet">` tags for the active theme only.

Theme CSS files remain responsible for semantic font token assignments such as `--theme-font-heading` and `--theme-font-body`. The stylesheet asset layer is responsible only for loading font-face declarations and related static theme assets.

For example, a theme can declare a bundled stylesheet at `/fonts/display-family/stylesheet.css` and map its heading token to that font family. Another theme that uses the same family declares the same stylesheet in its own assets field. The path is illustrative; use the location of the project's actual font stylesheet.

## Consequences

Themes can opt into custom fonts without making those font files a global application default.

Font stylesheets needed only by inactive themes are not loaded unnecessarily.

Adding a custom theme font requires two explicit steps:

1. declare the stylesheet in the theme definition;
2. reference the family through the theme's semantic font tokens.

The runtime contract covers visual assets as well as component composition, which keeps theme-specific behavior in one place.

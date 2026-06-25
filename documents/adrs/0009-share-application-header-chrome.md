# ADR-0009: Share application header chrome across public and management areas

- Status: Accepted
- Date: 2026-06-19

## Context

Public and management pages previously rendered visually similar top and menu bars through separate component markup and separate CSS variables.

Their colors, heights, spacing, and responsive behavior consequently drifted when one implementation changed without the other.

## Decision

Public and management headers render their top and menu rows through the shared `NavBar` and `MenuBar` component packages.

Theme files define one application-wide chrome contract:

- `--app-top-bar-bg`;
- `--app-top-bar-fg`;
- `--app-menu-bar-bg`;
- `--app-menu-bar-fg`;
- `--app-menu-bar-accent`;
- `--app-menu-bar-muted`.

The shared components own bar height, padding, alignment, and responsive layout. Theme views and shells provide content through component props and may style that content, but they do not redefine the bar geometry or palette.

Theme-specific header composition may add navigation content below a shared bar where structurally necessary, but the bars themselves remain canonical.

## Consequences

Changing header bar dimensions or color treatment changes public and management pages together.

Adding a theme requires defining the application chrome variables alongside the other semantic theme tokens.

Theme shells must not introduce public-only or management-only bar color variables or duplicate the shared bar layout.

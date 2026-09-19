# ADR-0009: Share application header chrome across public and management areas

- Status: Accepted
- Date: 2026-06-19

## Context

Public and management pages often render visually similar top and menu bars. Separate component markup and CSS variables allow their colors, heights, spacing, and responsive behavior to drift when one implementation changes without the other.

## Decision

Public and management headers render their top and menu rows through shared component packages, named `NavBar` and `MenuBar` in these examples.

Theme files define one application-wide chrome contract. For example:

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

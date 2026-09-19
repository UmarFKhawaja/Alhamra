# ADR-0005: Select themes through a typed runtime contract

- Status: Accepted
- Date: 2026-06-19

## Context

Changing colors alone is not sufficient for a visual theme. A theme may need different page composition, navigation structure, layout shells, typography, spacing, and decorative elements while preserving the same application behavior.

The application also needs one validated place to select the active visual theme and color mode.

## Decision

Themes implement a shared typed TypeScript contract, for example in `app/themes/core/contract.ts`. Paths and identifiers here illustrate a possible project layout.

The contract covers the application's composition boundaries, which may include:

- authentication and management shells;
- home and article views;
- auth and profile views;
- shared branding data;
- supported theme identifiers and color modes.

Theme implementations are registered centrally, for example in `app/themes/core/registry.ts`. A baseline theme such as `default` and a structurally different implementation can verify that the contract supports changes in composition as well as palette.

A runtime provider, named `ThemeRuntimeProvider` in these examples, exposes the selected theme definition, color mode, and branding through React context. Components obtain it through the exported `useThemeRuntime` hook.

Use one root loader to validate theme configuration. This guideline uses the following environment-variable convention:

- `UI_THEME`, falling back to `default`;
- `UI_MODE`, accepting `light`, `dark`, or `system` and falling back to `system`.

The root document writes `data-ui-theme` and `data-ui-mode` attributes to `<html>`. CSS theme files use those attributes to select token mappings.

Theme and mode configuration use the `UI_THEME` and `UI_MODE` environment variables exclusively.

## Consequences

Routes and controllers do not import a concrete theme implementation.

Adding a theme requires:

1. adding its identifier to the theme contract;
2. implementing all required shells and views;
3. defining its CSS token mappings;
4. registering the definition;
5. configuring `UI_THEME`.

TypeScript reports an incomplete theme implementation.

The contract contains views whose composition is expected to differ structurally between themes. Shared widgets such as the article editor remain outside the view registry when semantic tokens are sufficient to theme them. A shared widget should be added to the contract only when a second theme requires materially different markup or composition, not merely different colors or measurements.

Contract-bound views belong to each theme package. A theme must not hide per-theme differences behind a single cross-theme implementation with `variant` switches or theme-ID branching. If a piece of UI is truly reusable across themes, it belongs outside the theme layer as a theme-agnostic shared component or feature component.

Theme selection is server-configured for the application environment under this decision. Per-user theme selection would require an additional persistence and request-resolution decision.

All registered theme components are present in the application bundle graph. If the number or size of themes grows significantly, lazy theme loading should be considered.

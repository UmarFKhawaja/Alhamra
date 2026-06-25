# ADR-0008: Resolve application branding at the root

- Status: Accepted
- Date: 2026-06-19

## Context

Public managed content already supplied branding, but authentication layouts and document metadata used hard-coded logo and favicon paths.

This produced multiple sources of truth and meant a branding update could affect public pages without updating authentication or other application shells.

## Decision

The React Router root loader resolves application branding from the stored home-page document.

The root normalizes the managed brand into a `ThemeBranding` value containing:

- brand name and home link;
- logo source and alternative text;
- icon source and alternative text.

The root document uses the resolved icon as the favicon and passes the full branding value into `ThemeRuntimeProvider`.

Theme shells consume branding from the runtime rather than importing hard-coded assets.

Nested route loaders do not repeat the branding query. In particular, the management workspace receives `ThemeBranding` from `ThemeRuntimeProvider`, so its shells use the same normalized value as the favicon and authentication shell.

A built-in `default` brand remains as a defensive fallback. The loader uses it when:

- stored content is unavailable;
- database access fails;
- a managed branding field is empty.

## Consequences

Public pages, authentication pages, management pages, and document metadata share one managed branding source.

Theme implementations can decide how to present the same branding data.

The root loader now has a read dependency on stored home-page content. Failure is deliberately non-fatal because fallback branding keeps the application renderable.

The fallback assets remain part of the deployed application and must continue to exist.

Branding is application-wide. Supporting different brands by tenant or request host would require extending root branding resolution with an explicit tenancy decision.

## Rejected alternatives

Keeping separate hard-coded authentication assets was rejected because it would preserve multiple sources of truth.

Requiring the database read to succeed was rejected because branding failure should not make administrative and authentication routes unavailable.

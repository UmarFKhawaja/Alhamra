# ADR-0007: Maintain one canonical rendering path per screen

- Status: Accepted
- Date: 2026-06-19

## Context

A screen can acquire more than one implementation: a page may contain a local copy of markup while a similarly named widget exists but is unused. Other widgets may become orphaned entirely.

Duplicate implementations make it unclear which code is authoritative and allow fixes or visual changes to be applied to only one path.

## Decision

Each screen has one canonical rendering path.

Pages coordinate route-facing concerns and delegate rendering to the canonical widget or themed view. They do not maintain a second copy of the same screen implementation.

For example:

- home sections delegate to shared home widgets;
- article editing delegates to a single editor widget;
- account setup and password reset delegate to their widgets within an authentication shell;
- sign-in delegates to a controller and the selected theme view.

When a themed view becomes canonical, remove any superseded widget after verifying that it is unused.

Unused implementations should be deleted rather than retained as speculative alternatives.

Theme view and shell components are exempt from this rule. Two themes may initially contain identical or near-identical implementations that share the same rendering path. These implementations are expected to diverge as each theme develops its own visual identity and composition. Forcing them to share a base or inherit from a common abstraction would introduce indirection that makes future divergence harder. The typed theme contract ensures each theme has a self-contained, independently editable implementation without cross-theme coupling.

## Consequences

There is one place to fix behavior and one intentional place per theme to change presentation.

Pages become smaller orchestration components.

Widgets and themed views have a clearer purpose: widgets represent reusable screen content, while theme views represent theme-selectable composition.

Deleting an apparently unused component requires verifying imports and route usage first.

If two implementations are genuinely required, they must have distinct names and documented responsibilities instead of sharing near-identical page and widget names.

## Rejected alternatives

Keeping duplicate implementations after a migration was rejected because it would preserve ambiguity and make later cleanup harder.

Selecting an implementation through ad hoc route conditionals was rejected in favor of the typed theme registry described in ADR-0005.

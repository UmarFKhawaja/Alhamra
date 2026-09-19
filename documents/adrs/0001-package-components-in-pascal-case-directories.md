# ADR-0001: Package each component in a PascalCase directory

- Status: Accepted
- Date: 2026-06-19

## Context

Storing components as individual `.tsx` files can leave their props, public types, private models, hooks, helper methods, subcomponents, and styles embedded in one large file or scattered without a consistent ownership boundary.

As components grow, that structure makes them harder to navigate and obscures which types and helpers form part of their public API.

## Decision

Each React component is stored in its own PascalCase directory.

Every component package contains:

- `index.ts`, which defines the package's public exports.
- `component.tsx`, which contains the component JSX and is exported by `index.ts`.

The following files are used when needed:

- `props.ts`, which contains the component props and is exported by `index.ts`.
- `types.ts`, which contains public component types and is exported by `index.ts`.
- `models.ts` for private types. It is not exported by `index.ts`.
- `hooks.ts` for a small number of short private hooks.
- `hooks/` for larger private hooks, with one hook per file.
- `hook.ts` for a hook that is deliberately part of the public API, such as a context wrapper. It is exported by `index.ts`.
- `methods.ts` and `methods/` for private helper methods, following the same size rule as hooks.
- `components.ts` and `components/` for private subcomponents used to make the main component easier to understand.
- `styles.module.css` for styles owned by the component, as described in [ADR-0002](0002-colocate-component-styles.md).

Private models, hooks, methods, and subcomponents must not be re-exported from the parent `index.ts`.

React Router route modules remain framework entry points rather than component packages. Components used by route modules follow this convention.

## Consequences

Component ownership and public API boundaries are explicit.

Imports can continue to target the component directory because `index.ts` acts as the package boundary.

This convention introduces more small files and directories. This is an intentional trade-off for discoverability, reviewability, and local ownership.

Creating a `types.ts` or moving a type between `models.ts` and `types.ts` is an architectural decision: it changes whether callers may depend on that type.

## Examples

For example, an `ImageSelect` component can keep upload behavior in `hooks/useImageSelectController.ts`, helper functions in `methods.ts`, private state types in `models.ts`, and public handle and asset types in `types.ts`.

A context provider such as `ThemeRuntimeProvider` can expose its public context hook through `hook.ts`, while its raw context remains private.

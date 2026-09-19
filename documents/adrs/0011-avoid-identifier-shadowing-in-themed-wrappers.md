# ADR-0011: Avoid identifier shadowing in themed wrappers

- Status: Accepted
- Date: 2026-06-20

## Context

A themed component may wrap a theme-agnostic shared component and export a function with the same name. Example paths below illustrate a layout with a shared component layer and a separate theme contract.

The following wrapper introduces a conflicting identifier:

```tsx
import { ContentPanel } from '../../../components/ContentPanel';
import type { ContentPanelProps } from './props';

export function ContentPanel(props: ContentPanelProps) {
  return <ContentPanel {...props}/>;
}
```

The local `function ContentPanel` declaration conflicts with the imported binding. This should be caught by type checking. If conflicting code reaches runtime and the JSX resolves to the local function, the component calls itself recursively instead of delegating to the imported component.

The same pattern affects type-only re-exports in theme `props.ts` files:

```ts
import type { AuthShellProps } from '../../core/contract';

export type AuthShellProps = AuthShellProps; // TS2440
```

## Decision

1. **Runtime component wrappers** must alias the delegated import so the local export name does not collide with the imported binding:

   ```tsx
   import { ContentPanel as SharedContentPanel } from '../../../components/ContentPanel';
   import type { ContentPanelProps } from './props';

   export function ContentPanel(props: ContentPanelProps) {
     return <SharedContentPanel {...props}/>;
   }
   ```

2. **Type-only re-exports** must use `export type` re-export syntax rather than importing into scope first:

   ```ts
   export type { AuthShellProps } from '../../core/contract';
   ```

3. New themed wrappers must follow these patterns from the start. Existing wrappers must be audited for shadowed identifiers.

These examples wrap a shared component outside the theme layer. Contract-bound theme views remain independently implemented by each theme, as described in [ADR-0005](0005-select-themes-through-a-typed-runtime-contract.md).

## Consequences

- Themed wrappers delegate to the imported implementation as intended. No infinite recursion can arise from shadowed imports.
- The delegated component remains in the build output and receives the intended props.
- TypeScript produces no TS2440 errors from conflicting identical type names.
- Reviewers can catch shadowed identifiers by looking for function names that match import names without an `as` alias.
- This rule is mechanical and requires no runtime overhead.

## Rejected alternatives

- **Renaming the local export to differ from the import name** (e.g. exporting `SharedContentPanel` instead of `ContentPanel`) was rejected because import aliasing resolves the conflict while preserving the wrapper's public API.
- **Suppressing TypeScript errors with `@ts-ignore` or `@ts-expect-error`** was rejected because it masks the problem and does not fix the runtime crash.
- **Restructuring the theme registry to resolve names differently** was rejected because naming conflicts can be resolved locally without changing the typed contract described in ADR-0005.

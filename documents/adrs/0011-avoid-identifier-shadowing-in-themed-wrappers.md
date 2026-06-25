# ADR-0011: Avoid identifier shadowing in themed wrappers

- Status: Accepted
- Date: 2026-06-20

## Context

Theme wrapper components sometimes reuse another implementation and export a function with the same name. For example:

```tsx
import { HomeView } from '../../default/HomeView';

export function HomeView(props: HomeViewProps) {
  return <HomeView {...props}/>;
}
```

The local `function HomeView` declaration shadows the imported binding. The JSX `<HomeView .../>` inside the body resolves to the local function rather than the imported component. The component calls itself recursively on every render until the call stack overflows.

Bundlers such as esbuild (used by Vite) treat the import binding and local declaration as the same identifier. The delegated component is dead-code eliminated from the output because its import is never referenced. The build produces a wrapper that calls itself, and the crash only surfaces at runtime during SSR.

The same pattern affects type-only re-exports in theme `props.ts` files:

```ts
import type { AuthShellProps } from '../../core/contract';

export type AuthShellProps = AuthShellProps; // TS2440
```

## Decision

1. **Runtime component wrappers** must alias the delegated import so the local export name does not collide with the imported binding:

   ```tsx
   import { HomeView as DefaultHomeView } from '../../default/HomeView';
   import type { HomeViewProps } from '../../core/contract';

   export function HomeView(props: HomeViewProps) {
     return <DefaultHomeView {...props}/>;
   }
   ```

2. **Type-only re-exports** must use `export type` re-export syntax rather than importing into scope first:

   ```ts
   export type { AuthShellProps } from '../../core/contract';
   ```

3. New themed wrappers must follow these patterns from the start. Existing wrappers must be audited for shadowed identifiers.

## Consequences

- Themed wrappers delegate to the imported implementation as intended. No infinite recursion can arise from shadowed imports.
- The delegated component remains in the build output and receives the intended props.
- TypeScript produces no TS2440 errors from conflicting identical type names.
- Reviewers can catch shadowed identifiers by looking for function names that match import names without an `as` alias.
- This rule is mechanical and requires no runtime overhead.

## Rejected alternatives

- **Renaming the local export to differ from the import name** (e.g. `DefaultHomeView` instead of `HomeView`) was rejected because the themed wrapper must match the name expected by the theme registry contract.
- **Suppressing TypeScript errors with `@ts-ignore` or `@ts-expect-error`** was rejected because it masks the problem and does not fix the runtime crash.
- **Restructuring the theme registry to resolve names differently** was rejected because the current contract-based registry design is deliberate and well-understood (ADR-0005).

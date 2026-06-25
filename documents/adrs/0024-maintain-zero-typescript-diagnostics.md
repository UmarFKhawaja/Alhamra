# ADR-0024: Maintain zero TypeScript diagnostics

- Status: Accepted
- Date: 2026-06-25

## Context

The codebase had started to accumulate TypeScript diagnostics that did not block runtime behavior, but still weakened editor feedback, slowed review, and made future refactors harder to trust.

Allowing known diagnostics to remain in place invites more of them. Once the baseline is no longer clean, it becomes difficult to tell whether a new message points to a real regression or just more tolerated noise.

## Decision

The repository maintains a zero-diagnostic TypeScript baseline.

- `tsc --noEmit` must pass with no errors.
- New work must not introduce TypeScript warnings or suppressions as a shortcut around incorrect types.
- Route params, loader data, form state, and database inputs are typed explicitly enough that the compiler can verify them without relying on unsafe assumptions.

If a case cannot be expressed cleanly, the code should be refactored until the types describe the runtime behavior accurately. Leaving a known TypeScript diagnostic in place is a defect.

## Consequences

The type checker becomes a dependable review gate instead of an advisory signal. Refactors should be safer because mismatches are surfaced immediately.

This decision increases the cost of finishing partially typed work. Features are not considered complete until their TypeScript diagnostics are resolved.

Developers may need to introduce small helper functions, narrower data contracts, or better validation at boundaries so that runtime behavior and compile-time expectations stay aligned.

## Related decisions

- [ADR-0015: Standardize ESLint and Stylistic tooling](./0015-standardize-eslint-and-stylistic-tooling.md) — linting and type checking are both treated as code quality gates.
- [ADR-0018: Place mutable form screens on explicit routes](./0018-place-mutable-form-screens-on-explicit-routes.md) — explicit routes make it easier to validate route params and maintain safe loader/action contracts.

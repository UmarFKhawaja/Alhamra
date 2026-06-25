# ADR-0015: Standardize ESLint and Stylistic tooling

- Status: Accepted
- Date: 2026-06-20

## Context

The codebase needed a consistent linting and formatting baseline built on modern flat-config tooling:

- ESLint flat config;
- TypeScript-aware rules through `typescript-eslint`;
- Stylistic formatting rules;
- React Hooks validation;
- import sorting.

Without that baseline, formatting drift and low-signal review comments accumulate quickly across a large component library.

## Decision

The project now uses an ESLint flat configuration with this core stack:

- `@eslint/js`;
- `typescript-eslint`;
- `@stylistic/eslint-plugin`;
- `eslint-plugin-react-hooks`;
- `eslint-plugin-react-refresh`;
- `eslint-plugin-simple-import-sort`.

The project now provides:

- `npm run lint`;
- `npm run lint:fix`;
- `npm run format`.

The configuration intentionally omits content restrictions that depend on application-specific files or conventions outside this repository.

The configuration also disables selected React compiler-style lint rules that currently conflict with the application's proxy-based content editor and existing state synchronization patterns. The shared style, import-order, and TypeScript safety rules remain enforced.

Import ordering is enforced with these groups:

- `react` imports first;
- `react-` prefixed packages second;
- other package imports after that, sorted alphabetically;
- `~/` application modules next;
- `./+...` modules after aliased application imports;
- `../` relative imports ordered from most deeply nested to least deeply nested;
- `./` relative imports ordered from least deeply nested to most deeply nested.

Those imports remain in one contiguous block. Blank lines between import statements are not part of the house style, even when the sort order changes between groups.

## Consequences

Formatting and import ordering are now machine-enforced instead of review-enforced.

Most style corrections can be applied automatically through `npm run lint:fix`.

The lint stack now follows one consistent tooling baseline while still respecting the application's current architectural constraints.

If the application later refactors away from the current reactive editor patterns, the disabled React compiler-style rules should be revisited and potentially re-enabled.

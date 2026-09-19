# ADR-0015: Standardize ESLint and Stylistic tooling

- Status: Accepted
- Date: 2026-06-20

## Context

TypeScript and React codebases benefit from a consistent linting and formatting baseline built on flat-config tooling:

- ESLint flat config;
- TypeScript-aware rules through `typescript-eslint`;
- Stylistic formatting rules;
- React Hooks validation;
- import sorting.

Without that baseline, formatting drift and low-signal review comments accumulate quickly across a large component library.

## Decision

Use an ESLint flat configuration with this core stack:

- `@eslint/js`;
- `typescript-eslint`;
- `@stylistic/eslint-plugin`;
- `eslint-plugin-react-hooks`;
- `eslint-plugin-react-refresh`;
- `eslint-plugin-simple-import-sort`.

Expose the following scripts, or equivalent commands for the project's package manager:

- `npm run lint`;
- `npm run lint:fix`;
- `npm run format`.

Keep the shared configuration independent of application-specific files. Add project-specific restrictions in the project configuration.

Any rule exceptions required by an application's architecture must be narrowly scoped and documented with their rationale. The shared style, import-order, and TypeScript safety rules remain enforced.

Enforce import ordering with these groups, adapting alias and generated-module prefixes to the project:

- `react` imports first;
- `react-` prefixed packages second;
- other package imports after that, sorted alphabetically;
- `~/` application modules next;
- `./+...` modules after aliased application imports;
- `../` relative imports ordered from most deeply nested to least deeply nested;
- `./` relative imports ordered from least deeply nested to most deeply nested.

Those imports remain in one contiguous block. Blank lines between import statements are not part of the house style, even when the sort order changes between groups.

## Consequences

Formatting and import ordering are machine-enforced instead of review-enforced.

Most style corrections can be applied automatically through `npm run lint:fix`.

The lint stack follows one consistent tooling baseline while allowing documented architectural exceptions.

Revisit any disabled rules when the architectural constraints that justified them change.

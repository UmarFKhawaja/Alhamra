# ADR-0016: Match file names to primary exports

- Status: Accepted
- Date: 2026-06-20

## Context

Grouped modules can mix multiple concerns inside one file:

- database schema files that define more than one table;
- query and mutation files that export several unrelated operations;
- helper types embedded inside larger modules instead of living at an obvious import path.

That structure makes it harder to scan the codebase, harder to clean up dormant work, and harder to predict where a given export lives.

The same discoverability problem appears inside larger React component packages when hooks, helper methods, subcomponents, or standalone public types accumulate in shared files whose names do not clearly identify the main export.

This decision complements the component package directories described in [ADR-0001](./0001-package-components-in-pascal-case-directories.md) with a naming and file-boundary rule.

## Decision

Files should be named after the primary export they contain.

Each non-barrel file should have one primary export. Small colocated constants that directly support that primary export may remain in the same file only when they are clearly private implementation detail and are unlikely to be imported elsewhere.

## Database Structure

Organize database modules into these directories under the project's database root, shown here as `app/lib/server/db`:

- `entities/` for table definitions;
- `queries/` for read operations;
- `mutations/` for write operations;
- `types/` for helper types used by those modules.

### Entities

Each database entity lives in its own file under `app/lib/server/db/entities`.

Examples:

- `user.ts` exports `user`;
- `userRole.ts` exports `userRole`;
- `project.ts` exports `project`;
- `projectMember.ts` exports `projectMember`.

Use `entities/` for domain entity definitions so the directory has a clear purpose rather than collecting unrelated schema concerns.

### Queries

Each query file exports one primary query function whose filename matches that function.

Examples:

- `getUserByID.ts` exports `getUserByID`;
- `listUsers.ts` exports `listUsers`;
- `getProjectByID.ts` exports `getProjectByID`.

### Mutations

Each mutation file exports one primary mutation function whose filename matches that function.

Examples:

- `updateUserProfile.ts` exports `updateUserProfile`;
- `setUserPassword.ts` exports `setUserPassword`;
- `upsertProject.ts` exports `upsertProject`.

### Types

Helper types and reusable value sets used by the database layer live in `app/lib/server/db/types`.

Each helper type or reusable value-set file exports one primary object whose filename matches that export.

Examples:

- `ProjectSummary.ts` exports `ProjectSummary`;
- `UserProfileUpdate.ts` exports `UserProfileUpdate`;
- `roleValues.ts` exports `roleValues`;
- `accountStatusValues.ts` exports `accountStatusValues`.

Enum-like value sets used in table definitions, validation, forms, routing, or other application code should live in `db/types` rather than inside entity files.

That includes Drizzle enum value arrays such as role, status, and type values when those values are likely to be reused outside the entity definition itself.

## React Components

React component packages still follow ADR-0001:

- each component lives in its own PascalCase directory;
- `index.ts` remains the package boundary;
- `component.tsx`, `props.ts`, `types.ts`, and `models.ts` remain valid package-level conventions.

This ADR applies to split-out files inside those packages.

When a component package grows beyond its package boundary files, each additional hook, method, subcomponent, or standalone helper type should live in its own file named after its primary export.

Examples:

- `hooks/useImageSelectController.ts` exports `useImageSelectController`;
- `components/CountryPicker/component.tsx` exports `CountryPicker`;
- `methods/getFileExtension.ts` exports `getFileExtension`;
- `types/ThemeRuntimeValue.ts` exports `ThemeRuntimeValue`.

If a public or private type set becomes large enough that `types.ts` or `models.ts` becomes a grab-bag, split those exports into one-type-per-file and use filenames that match the exported type names.

## Barrels

`index.ts` files are allowed as barrel files and package boundaries.

Barrels may re-export several modules, but they should not contain business logic beyond those exports.

## Consequences

Finding an export becomes more predictable because the import path mirrors the export name.

Large grouped modules are discouraged, which makes dormant or half-built work easier to isolate and remove.

Refactors will usually touch more files, and the repository will contain more small modules. This is an intentional trade-off in favor of clarity, reviewability, and local ownership.

This rule works best when export names are stable and well chosen. Renaming an export implies renaming its file as part of the same change.

## Exceptions

`index.ts` remains the only general-purpose exception for barrel exports.

Component package boundary files defined by ADR-0001 remain valid even though names such as `component.tsx`, `props.ts`, `types.ts`, and `models.ts` describe package roles rather than a single export name. Once those files start collecting multiple unrelated concerns, they should be split into export-matching files.

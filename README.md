# Alhamra

`Alhamra` is a collection of reusable architecture guidelines and development how-to guides for TypeScript and React applications.

The documents cover component organization, theme architecture, application behavior, naming conventions, and code quality. They provide decisions and examples that teams can adapt to their own projects.

## Documentation

| Collection | Purpose |
| --- | --- |
| [Architecture Decision Records](documents/adrs/README.md) | Explain architectural decisions, their rationale, consequences, and alternatives. |
| [How-To Guides](documents/howtos/README.md) | Walk through development workflows using the architectural conventions. |

## Getting started

1. Browse the [ADR index](documents/adrs/README.md) and identify the decisions relevant to your project.
2. Read each decision's context and consequences before adopting it. Framework-specific guidance applies when your project uses the named framework or an equivalent capability.
3. Adapt example paths, component names, domain entities, and configuration keys to your project while preserving the intended boundaries and behavior.
4. Use the [how-to guides](documents/howtos/README.md) for implementation steps after checking their prerequisites.

The repository contains Markdown documentation and requires no installation or build step. Code snippets illustrate conventions; their dependencies and surrounding application infrastructure belong to the adopting project.

## Guideline areas

### Component organization

Package components in PascalCase directories with explicit public exports, colocate styles with CSS Modules, and expose semantic appearance props. Keep reusable controls separate from domain workflows and theme composition.

Start with [component packages](documents/adrs/0001-package-components-in-pascal-case-directories.md), [component styles](documents/adrs/0002-colocate-component-styles.md), and the [reusable UI tier](documents/adrs/0019-keep-reusable-ui-in-shared-component-packages.md).

### Themes and presentation

Define colors, typography, shapes, shadows, and gradients through semantic tokens. Select themes through a typed runtime contract, resolve branding at the application root, and let each theme own its shell and view composition.

Start with [semantic design tokens](documents/adrs/0004-use-semantic-design-tokens.md) and the [theme runtime contract](documents/adrs/0005-select-themes-through-a-typed-runtime-contract.md). The [custom theme guide](documents/howtos/create-a-custom-theme.md) brings these conventions together in a worked example.

### Application behavior

Separate workflow controllers from themed views, maintain a canonical rendering path for each screen, and give mutable form screens explicit routes. Use consistent submission feedback and keep data-loading failures distinguishable so fixes address their causes.

Start with [controllers and views](documents/adrs/0006-separate-controllers-from-themed-views.md), [mutable form routes](documents/adrs/0018-place-mutable-form-screens-on-explicit-routes.md), and [root-cause fixes](documents/adrs/0025-prefer-root-cause-fixes-over-fallback-data-paths.md).

### Naming and code quality

Match file names to their primary exports, use consistent acronym and boolean naming, and write source text in US English. Apply a shared linting and formatting baseline and maintain zero TypeScript diagnostics.

Start with [file naming](documents/adrs/0016-match-files-to-primary-exports.md), [linting and formatting](documents/adrs/0015-standardize-eslint-and-stylistic-tooling.md), and [TypeScript diagnostics](documents/adrs/0024-maintain-zero-typescript-diagnostics.md).

## Use cases

### 1. Establish conventions for a new application

Use the ADRs as a starting point for the project's own architectural decisions. Record which conventions are adopted and document any project-specific alternatives.

### 2. Refactor an existing interface

Use the component, controller, and theme boundaries to separate behavior from presentation. Review existing rendering paths and public APIs before moving or consolidating implementations.

### 3. Add a visual theme

Follow the [custom theme guide](documents/howtos/create-a-custom-theme.md) to define tokens, implement contract-bound components, register assets, and configure theme selection in an application with the required runtime.

### 4. Support onboarding and code review

Link contributors to the relevant decision when explaining a convention or reviewing a change. The rationale and consequences provide context beyond a checklist of rules.

## Document conventions

- Add ADRs under `documents/adrs/` using the next four-digit number and a descriptive kebab-case filename. Preserve existing numbers and filenames.
- Add how-to guides under `documents/howtos/` using descriptive kebab-case filenames. Preserve existing guide filenames.
- Update the corresponding directory index when adding a document, and use relative links between documents.
- Keep examples reusable and state framework assumptions, dependencies, and prerequisites explicitly.
- Preserve the distinction between an ADR's status and project adoption. A record's status and date describe the decision; they do not establish that a particular project has implemented it.

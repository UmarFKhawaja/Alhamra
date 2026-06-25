# ADR-0021: Use US English as the standard language for the codebase

- Status: Accepted
- Date: 2026-06-22

## Context

The codebase contained a mixture of US and UK English spellings in documentation, code identifiers, user-facing messages, and enum values. Examples included `cancelled` alongside `canceled`, `catalogue` alongside `catalog`, and `behaviour` alongside `behavior`.

This inconsistency was checked into the repository without any documented preference or enforcement mechanism, leading to mixed spellings within the same file.

## Decision

US English is the standard language for all project text, including:

- prose in documentation and ADRs;
- variable names, function names, type names, enum members, and database identifiers;
- user-facing messages in server responses, toast notifications, form labels, and error text;
- UI copy rendered to the end user.

Variations such as UK English are not supported in any form during development and review. They represent defects and should be corrected when found.

The codebase does not currently implement internationalization (i18n). When i18n is added in future, locale-specific variants such as UK English UI copy may be introduced through translation files or locale-aware message catalogs. Until that time, any locale-dependent text is prohibited. Code identifiers and documentation will remain in US English permanently regardless of any future i18n support.

## Consequences

All existing UK English spellings have been corrected. Reviewers should reject contributions that introduce new UK English text.

Tooling may be added in future to enforce this rule through linting or spell checking. Until then, enforcement is manual during code review.

When i18n is implemented, the localization layer will handle locale-specific spelling for UI copy. Code identifiers, documentation, and developer-facing text will not be affected by locale changes.

## Rejected alternatives

Allowing mixed spellings was rejected because it creates ambiguity about the project's language convention and leads to inconsistent user-facing text.

Mandating UK English was rejected because US English is the dominant convention in the JavaScript and TypeScript ecosystem, and the current codebase already leans toward US spelling in most areas.

## Related decisions

- [ADR-0020: Standardize form error surfacing](./0020-standardize-form-error-surfacing.md) — error messages follow this standard.

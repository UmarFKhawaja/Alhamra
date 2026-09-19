# ADR-0021: Use US English as the standard language for the codebase

- Status: Accepted
- Date: 2026-06-22

## Context

Mixing US and UK English spellings in documentation, code identifiers, user-facing messages, and enum values creates inconsistency. Examples include `cancelled` alongside `canceled`, `catalogue` alongside `catalog`, and `behaviour` alongside `behavior`.

Without a documented preference and enforcement mechanism, mixed spellings can appear within the same file.

## Decision

US English is the standard language for all project text, including:

- prose in documentation and ADRs;
- variable names, function names, type names, enum members, and database identifiers;
- user-facing messages in server responses, toast notifications, form labels, and error text;
- UI copy rendered to the end user.

Use US English for source text during development and review. Variants such as UK English belong in an internationalization (i18n) layer when the product supports those locales; mixed spelling outside that layer is a defect.

Projects with i18n may provide locale-specific UI copy through translation files or locale-aware message catalogs. Projects without i18n use US English for all UI copy. Code identifiers, documentation, and developer-facing text remain in US English regardless of localization support.

## Consequences

Correct inconsistent spellings when found. Reviewers should reject contributions that introduce variants outside an intentional localization layer.

Tooling may be added in future to enforce this rule through linting or spell checking. Until then, enforcement is manual during code review.

When i18n is used, the localization layer handles locale-specific spelling for UI copy. Code identifiers, documentation, and developer-facing text will not be affected by locale changes.

## Rejected alternatives

Allowing mixed spellings was rejected because it creates ambiguity about the project's language convention and leads to inconsistent user-facing text.

Mandating UK English was rejected because US English is the dominant convention in the JavaScript and TypeScript ecosystem, and provides a consistent baseline for source text.

## Related decisions

- [ADR-0020: Standardize form error surfacing](./0020-standardize-form-error-surfacing.md) — error messages follow this standard.

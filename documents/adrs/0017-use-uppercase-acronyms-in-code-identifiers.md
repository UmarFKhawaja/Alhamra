# ADR-0017: Use uppercase acronyms in code identifiers

- Status: Accepted
- Date: 2026-06-20

## Context

The repository already relies on naming consistency across entities, queries, mutations, helper types, and component packages.

That consistency becomes harder to maintain when common domain or technical acronyms are written inconsistently, such as a lower-case acronym form in one place and `vendorSKU` in another.

The codebase already uses uppercase acronyms in many exported TypeScript identifiers, including names such as `userID`, `bodyURL`, and `emailOTP`.

As the domain model expands, especially around purchasing and treatment records, acronym handling should be explicit rather than left to taste.

This decision complements [ADR-0016](/documents/adrs/0016-match-files-to-primary-exports.md): once file names mirror exports, acronym style must also be predictable.

## Decision

In TypeScript and JavaScript identifiers, established acronyms should stay uppercase when they appear as a PascalCase word, or when they are not the first word in a camelCase identifier.

When the acronym is the first word in a camelCase identifier, it should stay lowercase.

Examples:

- use `otp` for a first-word camelCase acronym;
- use `setOTP` when the acronym is a later camelCase word;
- use `vendorSKU`;
- use `userID`;
- use `callbackURL`;
- use `emailOTP`.

This applies to:

- exported constants;
- function and method names;
- type and interface names;
- object property names used in application code;
- filenames when they are meant to match the primary export name.

Examples:

- `otp: string`
- `vendorSKU: string`
- `productPurchaseID: string`
- `SetManageUserPasswordInput`
- `getUserByID.ts`

## Boundaries

This ADR applies to code identifiers, not every naming surface in the repository.

It does not change these established conventions:

- database tables and columns stay `snake_case`, such as `vendor_sku` and `user_id`;
- route segments stay URL-friendly, such as `/manage/user/:userID`;
- prose documentation may use normal human-readable capitalization.

## Consequences

Identifier names become easier to scan because acronyms are treated consistently.

Generated or manually written filenames remain aligned with the exports they contain.

This choice differs from some style guides that prefer `Id`, `Url`, or `Sku`, so contributors should follow the repository rule rather than external defaults when they conflict.

## Exceptions

Do not force all short uppercase sequences to remain capitalized if they are not established acronyms in the project domain.

For example, ordinary words or partial abbreviations should still follow normal `camelCase` or `PascalCase` expectations.

When a new abbreviation is introduced and its status is unclear, prefer writing the term out fully until the abbreviation is stable enough to deserve glossary or ADR-level treatment.

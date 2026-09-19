# ADR-0023: Name boolean identifiers as plain-English propositions

- Status: Accepted
- Date: 2026-06-24

## Context

Boolean variables, state fields, and props can follow different naming conventions. Some use bare adjectives (`active`, `disabled`, `required`) while others use question-like prefixes (`isActive`, `hasError`, `canEdit`) or more domain-specific verb-led forms.

A mixed convention makes boolean intent harder to distinguish from noun-like identifiers that represent objects or string values.

## Decision

Every boolean identifier must read as a plain-English proposition that can be answered with yes or no.

Common forms include:

- `is` — describes a state or property: `isActive`, `isOpen`, `isSlugEdited`
- `has` — describes possession or presence: `hasError`, `hasSelection`, `hasUserAccess`
- `can` — describes capability or permission: `canEdit`, `canView`, `canManageRoleAssignment`
- `requires` — describes a requirement or prerequisite: `requiresApproval`, `requiresAuthentication`

Other verb-led forms are acceptable when they read naturally as a yes/no statement in the domain language.

The proposition shape is never omitted. A boolean named `active` is a defect and should be renamed to `isActive`. A boolean named `approvalRequired` is weaker than `requiresApproval` because it stops reading as a direct question-like proposition.

## Consequences

Boolean intent is immediately visible from the identifier name. Code reviewers can enforce the convention mechanically by asking whether the name reads as a truth-valued English statement.

The convention is binding for all code. New code must follow it, and any existing unprefixed boolean is a defect that must be corrected rather than left for an opportunistic future pass. A reviewer who finds a bare-adjective or noun-form boolean (such as `active`, `collapsed`, `linked`, `uploading`, or `started`) must require it to be renamed before the change merges.

Database column names are exempt. The convention applies to TypeScript identifiers, React state, component props, and function return values.

## Related decisions

- [ADR-0022: Auto-generate slugs from names with manual override](./0022-auto-generate-slugs-from-names.md) — the `isSlugEdited` flag follows this convention.

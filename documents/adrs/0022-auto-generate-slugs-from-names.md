# ADR-0022: Auto-generate slugs from names with manual override

- Status: Accepted
- Date: 2026-06-24

## Context

Entities such as articles and categories require a URL-safe slug for routing and reference. Asking users to manually type a slug alongside every name is error-prone, while silently overwriting a manually chosen slug on every name change is frustrating.

An entity slug also serves as a stable identifier. A name change should update the slug automatically only when the slug has not been intentionally set by the user.

## Decision

Form fields that pair a human-readable name with a machine-readable slug follow this behavior:

1. As the user types in the **Name** field, the **Slug** field is populated with a URL-safe version of the name (`slugify(name)`).
2. If the user **manually edits** the slug, auto-generation stops. Subsequent name changes do not alter the slug.
3. If the user **clears the slug** to an empty value, auto-generation resumes on the next name change.

Keep `slugify` in a shared utility module, such as `app/lib/utils/slugify.ts`. For a project using ASCII slugs, an implementation is:

```ts
function slugify(text: string) {
  return text
    .toLowerCase()
    .trim()
    .replace(/[^a-z0-9\s-]/g, '')
    .replace(/\s+/g, '-')
    .replace(/-+/g, '-');
}
```

Both client-side form logic and server-side upsert mutations use the same utility so that the client preview matches the stored value.

The form widget tracks an `isSlugEdited` boolean state to decide whether slug generation is locked. Boolean flags follow the `is*`, `has*`, or `can*` prefix convention.

## Consequences

Users see a preview of the computed slug as they type the name. Editing the slug is possible but deliberate. Clearing the slug resets to auto-generation.

Every form that pairs a name with a slug must implement this behavior. The shared `slugify` utility removes the need for duplicate logic.

Server-side upserts apply `slugify` when the submitted slug is empty and validate the result before persistence so a name that produces an empty slug cannot cause a blank slug to be stored.

## Rejected alternatives

Always auto-generating the slug without a manual override was rejected because some entities need a curated slug that differs from the name.

Requiring the user to always type the slug manually was rejected because it adds friction to a common workflow.

Persisting a blank slug as a fallback was rejected because every entity requires a non-empty URL-safe identifier.

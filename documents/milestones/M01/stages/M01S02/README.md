# M01S02 — Core Customer and Practitioner Model

## Status

Accepted

## Milestone

[M01 — Foundation](../../README.md)

## Purpose

Create the people and role foundations used by appointments, consultations, treatments, product purchases, and assets.

## Scope

- Create or extend the `user` model
- Add `customer`
- Add `practitioner`
- Add basic role assignment if not already present
- Add list and detail screens needed to review records

## Deliverables

- Drizzle schema and migration for customer and practitioner records
- Validation schemas
- Server-side actions or service functions for create/update
- Admin list and detail screens
- Seed data for at least one customer and one practitioner
- Tests for basic create/update/read behavior

## Suggested data model

```txt
user
  id
  fullName
  emailAddress
  telephoneNumber
  createdAt
  updatedAt

customer
  id
  userID
  dateOfBirth?
  residentialAddress?
  emergencyContactName?
  emergencyContactTelephone?
  notes?
  createdAt
  updatedAt

practitioner
  id
  userID
  displayName
  professionalTitle?
  bio?
  active
  createdAt
  updatedAt
```

## Suggested application surfaces

- Unified admin user list
- Shared admin user detail page
- Basic create/edit forms

## Acceptance criteria

- A user can have a linked customer record
- A user can have a linked practitioner record
- The same user may support more than one role if the business later requires it
- Customer contact data is stored on `user`, not duplicated unnecessarily
- The UI uses customer terminology consistently
- Seed data allows the screens to be reviewed without manual setup
- Any implementation delivered in this stage complies with accepted ADRs, or records an approved deviation

## Out of scope

- Appointment booking
- Consultation records
- Treatment records
- Product purchases
- Photo and video assets
- Advanced permissions

## Implementation notes

- Prefer narrow, boring CRUD here
- Avoid adding treatment-specific fields to `customer`
- Avoid adding appointment-specific fields to `practitioner`
- Customer contact and address details remain on `user`; `customer` only stores customer-specific data that should not be duplicated onto the base account
- The existing manage-user profile flow is the primary detail surface for both customer and practitioner records
- Admin user management should stay unified under the existing user-management table rather than forking role-specific directory screens this early
- Practitioner identity is modeled through `practitioner`, including a separate practitioner-facing display name and active flag
- The generated profile migration also removes the previously deleted dormant `treatment` table because that schema cleanup had not yet been captured in a Drizzle migration file
- Seed data and automated tests should only be added once the repository has a stable stage-level pattern for them; do not add one-off scaffolding here unless it clearly simplifies later stages

## Deviations

Record any changes from this stage plan here.

| Date | Deviation | Reason | Approved By |
|---|---|---|---|
| 2026-06-21 | Stored customer contact and address data on `user` instead of duplicating a `residentialAddress` field onto `customer`. | This matches the approved glossary and the stage acceptance rule to avoid unnecessary duplication. | Project direction |
| 2026-06-21 | Reused the existing manage-user detail flow for customer and practitioner editing instead of creating separate detail routes. | The current account and admin profile surface already provides the right seam for first-pass CRUD without fragmenting the admin UI. | Project direction |
| 2026-06-21 | Allowed the generated migration to also drop the dormant `treatment` table. | The code cleanup had already removed the entity definition; this was the first Drizzle migration generation after that cleanup. | Project direction |

## Change record

Maintain a chronological record of meaningful updates to this document here.

| Date | Change | Reason |
|---|---|---|
| 2026-06-21 | Renamed the planned people entities from profile-oriented names to `customer` and `practitioner`, and aligned the seed branding assets with explicit light and dark image filenames. | Keep the foundation terminology simple and make the baseline seed data match the current branding schema. |
| 2026-06-21 | Recorded that the generated schema migration also removes the dormant `treatment` table. | Make the migration scope explicit during review. |
| 2026-06-21 | Removed the separate customer and practitioner directory screens in favor of the unified users screen. | The same management actions belong on the unified users screen, which also covers owner-only and roleless users. |
| 2026-06-21 | Moved `M01S02` to `In Progress` and recorded the implementation approach for customer and practitioner records. | Implementation has started and the stage now has concrete delivery notes tied to the current codebase. |
| 2026-06-21 | Moved `M01S02` from `In Progress` to `Accepted`. | Application behavior and the stage documentation were reviewed together and confirmed ready for `M01S03`. |
| 2026-06-22 | Added ADR compliance to the stage acceptance criteria. | Make existing architecture decisions an explicit review gate for this stage. |
